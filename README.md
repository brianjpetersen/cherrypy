cherrypy
========

Temporary fork of CherryPy v3.6.0 to address known SSL bugs.

SSL works in the CherryPy v3.2.x, but is broken in CherryPy >= v3.3.0 at least through v3.6.0.  The client refuses connection, but no requests are logged by CherryPy.  This behavior is consistent across clients.  Regular HTTP requests work fine.

This problem is described in issues #1293 and #1298.  The problem was apparently introduced to address another issue of CherryPy HTTPS not working on PyPy (issue #1263).  A patch is proposed which appears to fix the problem in cPython without adversely impacting PyPy deployments.

This project is an exact clone of v3.6.0 except for the following change:

diff -r 080abbbd67ba cherrypy/wsgiserver/wsgiserver2.py
--- a/cherrypy/wsgiserver/wsgiserver2.py    Thu Mar 06 16:23:42 2014 -0500
+++ b/cherrypy/wsgiserver/wsgiserver2.py    Wed Mar 19 07:32:17 2014 -0700
@@ -1324,8 +1324,8 @@
     def __init__(self, server, sock, makefile=CP_fileobject):
         self.server = server
         self.socket = sock
-        self.rfile = makefile(sock._sock, "rb", self.rbufsize)
-        self.wfile = makefile(sock._sock, "wb", self.wbufsize)
+        self.rfile = makefile(sock, "rb", self.rbufsize)
+        self.wfile = makefile(sock, "wb", self.wbufsize)
         self.requests_seen = 0

The project will remain active until the issue is addressed in CherryPy (it is currently referenced in three blocking issues, but several releases have been made without fixing it.
