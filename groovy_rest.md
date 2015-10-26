# Take a Groovy REST!

by: Guillaume Laforge

* 2nd Edition of Groovy in Action releaed, `ctwjavaone` to get 40% off
* Star wars API :) http://swapi

## Intro to REST

* Coined by Roy Fielding, Representational State Transfer
* Architectural style, not a framework or anything else.
* Resources as URIs
* HATEOAS: Adds hyperlinks to connect states
* **GET** gets resources, **POST** for creating new one, **PUT** replaces what's there

## Restlet Framework

* Can be used as a server, and also as a client for REST APIs.
* Starts very fast as it's lightweight

## Ratpack

* Toolkit for writing asynchronous web apps
* Very different from MVC-style frameworks
* Can decide different outputs based on content-type headers. Pretty cool.

## REST Clients

* On the web-browser side: Raw AJAX, jQuery, AngularJS, Restful.js, Grooscript
* You can do a get request on a resource with one line: `getText()` on a URL object.
* Used Spock to test GET loop, and JSON persing
* Used wslite library to do a functional test that hits the API
* Retrofit is an Android friendly library. Marshals and unmarshals objects for you.
* Talked about DHC, a Chrome client for RESTful API testing.
 
