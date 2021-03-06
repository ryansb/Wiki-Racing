Wiki-Racing
===========
This project's goal is to be able to find the best path between any two wikipedia articles.
It runs a frontend and backend side that tries to find the path between any two articles.
If the front end can not find the path in the given time period (60 seconds), then it is
moved to the backend and given 10 minutes to find the path. Once the path is found, it is
added to the database of found paths. When there are no failed atempts in the backend, it
picks commonly used start and end points and tries to find a path between them.

Dependicies:
- python2 (v2.7.3):
	The program uses cPickle, an C implementation of Pickle, which is not supported in
	python3.

- Bottle (v0.12-dev):
	It is a lightweight (single file), fast, WSGI web framework that runs completly in
	python. Bottle has support for templates, routing, and multiple different types of 
	servers. It defaultly uses wsgiref WSGIServer, a non-threading HTTP server, which can
	cause the server to be slow. It handles all the request that the server gets and 
	calls all the important	methods when needed.

- Paste (v1.7.5.1):
	This is not required but it fixes the problem of the server not being able to handle
	multiple request at the same time. This is the suggested server to run instead of the
	default but, others could be cherrypy, rocket, waitress, etc.

- Spin.js

Files:
- parser.py:
	Reads the .xml file from wikipedia and pulls out the article title and all
	the links in it. It saves each (title, link) pair as a list using
	cPickle.

- pathFinder.py:
	Holds the methods used by the server to load the articles into memory and find
	the best path between any two articles. The data for each class is stored in
	a python dictionary with the key being the article name and the value being a
	string of all the links that the article has. The pathfinder method has instance
	list for distances and parents of each article so that mult. connections
	do not interfer with each other.	

- server.py:
	Uses the bottle python web server framework to handle requests. The main path is
	'/path' but everything gets redirected to /path as well. The server handles 404 and
	500 errors. It uses a templates system to help display the path to the user. It imports
	the methods from pathFinder.py.

- backend.py:
	The code that runs in the backend of the site. When an attempt to find a path fails, it
	is given to the backend and given 10 minutes to find the path. The backend uses threading
	to allow for multiple paths to be searched for at the same time. When there are no failed
	attempts to search for, it generates some by looking at commonly used start and end
	locations.

- pathViewer.tpl:
	The template file used for the bottle server. It displays the result from the give input
	in a list.

- index.tpl:
	The template for the start page as long as the error message page.