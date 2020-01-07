# Http Request

## Covers:

1. Overview
2. [Optional] Fetch API
3. Axios
4. Handling Error
5. Waiting for an item to load
6. [Optional] Common Config

## Overview

In an application that does server-side rendering, an endpoint call typically fetches the full page which includes the HTML page View and all the data that is already embedded inside. Moving to Single Page Application, we already have all the HTML and Javascript code in the browser all we need is the data. Fetching data only not only reduces the size of data transfer over the network but also introduce other issues such as the need for a loader and error handling.

In this course, we are going to use JavaScript Object Notation(JSON) to transfer data over the network. You can read more about JSON in https://en.wikipedia.org/wiki/JSON. In JSON, our data are group into objects and array. The object should contain many key-value pairs. Accepted values are string, boolean, number and null.

```javascript
{
  "firstName": "John",
  "lastName": "Smith",
  "isAlive": true,
  "age": 27,
  "address": {
    "streetAddress": "21 2nd Street",
    "city": "New York",
    "state": "NY",
    "postalCode": "10021-3100"
  },
  "phoneNumbers": [
    {
      "type": "home",
      "number": "212 555-1234"
    },
    {
      "type": "office",
      "number": "646 555-4567"
    }
  ],
  "children": [],
  "spouse": null
}
```

## [Optional] Fetch API

Fetch API is the browser default API to handle HTTP request and response. Fetch allows us a quick and easy way to retrieve data from the server without building an `XMLHttpRequest` object.

```javascript
fetch("https://jsonplaceholder.typicode.com/todos/2")
  .then(response => response.json()) // convert data to json
  .then(json => console.log(json)); // log data
```

The above code assumes that the server is not going to throw an error.
We also see the line `.then(response => response.json())`, which gets the data in the body of the response and translates it to a JSON format for us.
There are other possible types such as `Body.blob()` for images, `Body.text()` for plain text and `Body.arrayBuffer()` for array buffer. You will most likely use `json()` in most cases.

You can also face error when sending a request.

```javascript
const checkResponse = res => {
  if (!res.ok) {
    throw Error(res.statusText);
  }
  return res;
};

fetch("https://jsonplaceholder.typicode.com/todos/2")
  .then(checkResponse) // check if response is okay
  .then(response => response.json()) // convert data to json
  .then(json => console.log(json)) // log data
  .catch(err => console.log(err)); // log error if an error was thrown
```

Server sometimes can return you an invalid response. Is good to check your response before using them.

Fetch allows you to take in a config object, within the config object you can also use other methods such as `POST` and `PATCH`.

```javascript
fetch(url, {
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    headers: {
      'Content-Type': 'application/json'
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    body: JSON.stringify(data) // body data type must match "Content-Type" header
  });
  return await response.json(); // parses JSON response into native JavaScript objects
}
```

`method`, `headers`, `body` are standard config options you might use. There are other more advanced configurations.

## Axios

Axios is a JS library that provides an even more straightforward way to an HTTP request.

Features

- Make XMLHttpRequests from the browser
- Make HTTP requests from node.js
- Supports the Promise API
- Intercept request and response
- Transform request and response data
- Cancel requests
- Automatic transforms for JSON data
- Client-side support for protecting against XSRF

Installation

```sh
npm install axios
```

fetching data

```javascript
axios("https://jsonplaceholder.typicode.com/todos/2").then(res =>
  console.log(res.data),
); // log data
```

### Using in React

```javascript
import React from "react";
import axios from "axios";

export default class Todolist extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      todos: [],
    };
  }

  componentDidMount() {
    axios("https://jsonplaceholder.typicode.com/todos").then(res => {
      this.setState({
        todos: res.data,
      });
    });
  }

  render() {
    return (
      <div>
        {this.state.todos.map(todo => (
          <div key={todo.id}>{todo.title}</div>
        ))}
      </div>
    );
  }
}
```

In React, fetching data in `componentDidMount` ensures that when data return, our component is ready to use the data. Using `this.setState`, we can update the data.

In the render function, we then use `map` to render out the list of elements.

# Handling Error

Handling error in Axios is simple. Axios function returns a Promise and Rejects when the status code is 4XX or 5XX, the typical error code of the server. In `fetch`, we need to throw Error manually whereas, in Axios, it throws `Error` for us. As a result, we can use `catch` directly.

```javascript
axios("https://jsonplaceholder.typicode.com/todos")
  .then(res => {
    this.setState({
      todos: res.data,
    });
  })
  .catch(error => {
    console.error(error);
    this.setState({ errorMessage: "Please Try Again" });
  });
```

# Waiting for an item to load

Is always good to tell the user that something has happened after he initiated an action.

We can build our loader, or we can use an external package to help us.
For simplicity, we are going to use a package to help us.

```
npm install react-loading
```

src/components/Loader.js

```javascript
import React from "react";
import Loader from "react-loader-spinner";

export default () => <Loader type="ThreeDots" color="#00BFFF" />;
```

src/containers/TodoPage.js

```javascript
import React from "react";
import axios from "axios";
import Loader from "../components/Spinner";

export default class Todolist extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isLoading: false,
      todos: [],
      errorMessage: null,
    };
  }

  componentDidMount() {
    this.getData();
  }

  getData() {
    this.setState({
      isLoading: true,
    });

    axios("https://jsonplaceholder.typicode.com/todos")
      .then(res => {
        this.setState({
          isLoading: false,
          todos: res.data,
        });
      })
      .catch(error => {
        console.error(error);
        this.setState({ isLoading: false, errorMessage: "Please Try Again" });
      });
  }

  printTodos() {
    return this.state.todos.map(todo => <div key={todo.id}>{todo.title}</div>);
  }

  render() {
    return <div>{this.state.isLoading ? <Loader /> : this.printTodos()}</div>;
  }
}
```

Try it out, and now you see the loader for a split second. Our server is local and doesn't require to navigate thru the long underground sea cable to reach the server. A real API call takes much longer, especially if it requests a complex query thru a database.

To simulate a longer wait time, we can add a timeout or even better make use or browser network feature. On Chome, open the developer tool, navigate to the `network` tab, under you should see an `online` dropdown. Click on it and select `Slow 3G`, refresh again and now you can see.

## [Optional] Common Config

Axios allow us to do a standard config which can be convenient to reduce repetitive code to config Axios to do the same thing.

Create a new file utils/axios.js

utils/axios.js

```javascript
import axios from "axios";

const instance = axios.create({
  baseURL: "https://jsonplaceholder.typicode.com",
  timeout: 1000,
});

instance.defaults.headers.post["Content-Type"] = "application/json";

export default instance;
```

This consist of some of the more common settings.
The above setting sets the baseUrl, now we can just pass in the path when we do Axios call, and everything should work the same.
We also shorten the timeout, so API fails faster if the server keeps us wait for too long, default is `0` which waits till the connection get dropped.
On POST request, we set the content-type to `application/json` the most common setting.

## Exercise

Create a comment searcher

The API `https://jsonplaceholder.typicode.com/comments` returns an array of comments.

- Each "comment" is related to a "post".
- Each post can have many comments.
- post is represented by `postid`
- comment has an id, represented by `id`

1. Create a component `container/CommentsSearch.js` to print comments based on an input id.

- create an input box that takes in a post id
- create a submit button
- on clicking submit button fetch the comments based on the postid `https://jsonplaceholder.typicode.com/comments?postId=1`
- feel free to add some styles to make it looks beautiful.

2. Add a loader

- when the postId changes, it takes some time to load the data
- show a loader before the data returns

3. Handle no data

- when data is empty such as when user type in 999 as the postid: `https://jsonplaceholder.typicode.com/comments?postId=999` shows a
- "no comments available for postid: <postid>"

4. [optional] create a `utils/axios` to configure the baseUrl of Axios.

5. [optional] display the post followed by the comments

- post can get retrieve from `https://jsonplaceholder.typicode.com/posts/<postid>.`
