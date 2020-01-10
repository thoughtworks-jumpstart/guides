# Express.js parameter processing

If we want to process parameters before calling the handler function, we can add callback functions that will be called when a request has a certain parameter.

## Application-level

`app.param`

## Router-level

Note that the routers will not be affected by `app.param`.


`router.param`
