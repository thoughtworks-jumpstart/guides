# Mongoose with Express.js

When adding a database to Express.js that the backend server will interact with, many of our route handlers will become more like controllers that interact with the database.

For example, route handlers that GET a single resource can be a controller named `findOne` that finds a single resource in the database.

Similarly, when you POST a single resource in a route handler, it can be a controller named `createOne` that creates a single resource in the database.

It is convention to put these controllers into a `controllers` folder. Controllers are these handlers interacting with the database that is linked to the routing in the routers.

Refactor the controllers into the folder.

In the MVC design pattern, controllers is the middleman between Express.js routes and mongoose models.

![MVC diagram](https://mdn.mozillademos.org/files/14456/MVC%20Express.png)
