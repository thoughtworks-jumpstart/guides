# Planning for a new UI Project

## Define problem statement

"A problem statement is a concise description of an issue to be addressed or a condition to be improved upon. It identifies the gap between the current state and desired state of a process or product. Focusing on the facts, the problem statement should be designed to address the 5 W's – who, what, where, when, and why. The first condition of solving a problem is understanding the problem, which can be done by way of a problem statement." - Wikipedia

- Brief description of the problem
- What is the problem
- Who faces the problem
- When and where the problem occurs
- How the problem occurs
- Come out with a problem statement you can relate to

## Idea generation

1. Brainstorm different ideas that can help with the problem
2. Come out with workflow the user might use
3. Prioritise the user workflow in terms of the value
4. Decide on the most simple workflow that brings value to the user

## Product design

- it can be words
- sketches
- workflow diagrams
- one drawing that represents the UI
- multiple drawing that represents the workflow
- Prototype on [Figma](https://www.figma.com/) or [Sketch](https://www.sketch.com/)
- Present the idea to your friends or intended users and listen to what they want to say with an open mind.

## Research on libraries

There are lots of libraries that can make up the building blocks of your projects.
Try looking for a library that might make your life easier.
Check out `https://www.npmjs.com/`.

Some helpful library: [Moment](https://www.npmjs.com/package/moment), [Lodash](https://www.npmjs.com/package/lodash), [Faker](https://www.npmjs.com/package/faker), [Charts](https://www.npmjs.com/package/react-chartjs-2), [Map](https://www.npmjs.com/package/google-map-react)

After reading the introduction to the library and think that can help you, you can try them out on a new `create-react-app` before implementing into your system.

## Public APIs

Here is a list of APIs which you can get data for your front-end projects.

- [Data.gov (Singapore)](https://data.gov.sg/developer)
- https://www.quora.com/What-are-some-cool-fun-random-APIs-such-as-BreweryDB
- https://any-api.com/
- https://github.com/toddmotto/public-apis
- https://github.com/abhishekbanthia/Public-APIs
- https://www.computersciencezone.org/50-most-useful-apis-for-developers/
- [NASA APIs](https://api.nasa.gov/)
- [Quandl financial data](https://github.com/normanjoyner/node-quandl)

### How to get started with these Web APIs

Steps:

- Find/locate the documentation for the API (e.g. https://www.thecocktaildb.com/api.php). To find this, you may have to google around
- Read the API docs and...
  - get a general understanding of where things are in the docs
  - try to see if there are examples of URLs that you can try out
  - find the answers to the following specific questions:
    - Does the API respond in JSON format?
      - If no, it's probably time to find another API
    - Do I need an API key to make requests to the API?
      - If yes, register for an API key (you may have to fill your name, email and even application name or URL)
  - What is the base URL? (e.g. https://www.thecocktaildb.com/api/json/v1/1/)
  - What are the parameters I can use? (e.g. `/cocktails`, `/cocktails/42`, `/ingredients`, `/ingredients/42`)
  - What are the query strings I can use? (e.g. `?s=margarita`, `i=vodka`, `c=Cocktail`, apiKey=YOUR_API_KEY)

### Managing the API Keys

**Don't check in the API key into your public code repository (like Github). It's a secret!!!**

If you need to sign up to get an API key, remember to follow these rules when using the key:

1. Treat the key as a secret, like you would a password
2. Don't hard-code the API key with the code (it can be read by any visitor to the site!)
3. For local development, keep your API keys in an .env.local file in your code
4. Add the .env.local file to .gitignore to prevent it from being accidentally committed
5. For build/deployment on the web servers, add the API key as a config or environment variable on the server where your app is built and deployed (e.g. heroku, netlify, etc).
   Lab
