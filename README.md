cherrypy
========

Fork of CherryPy v3.6.0 to address known SSL bugs.

SSL works in CherryPy < v3.2.5, but is broken in CherryPy >= v3.3.0.  The client refuses connection, but no requests are logged by CherryPy.  Regular HTTP requests work fine.
