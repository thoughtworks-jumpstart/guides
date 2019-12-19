# Express.js router

For a very small app, we can define all the API endpoints in one file, like what we did in the previous examples. However, as our application grows, it's a good idea to break the application into smaller modules. In terms of the API end points, it would be better to group API endpoints by their nature.

Each Router is like a mini-app, which means it can contain its own API endpoints (together with many other things such as middleware and error handlers, as you will notice later.)


## Router example

For example, if we are implementing an API for the library system, the API endpoint for users and books can be put into their own module. This is illustrated in the example express_basic_example_with_router.js.

Create a router file for the users

Load the router module in the app