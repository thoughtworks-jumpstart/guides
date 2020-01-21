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

## Brainstorming

- Take a piece of A4 paper, tear into half, split it into 4 parts
- Draw out an idea on each part
- Don't be afraid of judgement. Don't think that your idea is silly
- Brainstorm as many ideas as possible for now: Quantity is better than quality when it comes to brainstorming
- Consider techniques such as **SCAMPER**

## Prioritisation

- Let us take a step back and consider which ideas can be priortised based on desirability and feasibility. (We usually consider viability for a sustainable business model but for now let's do this for free)
- **Desirability** is delivering a product that your customer / target audience really needs. What is the value that you bring to your customer? Are you solving their pain points? Is this a nice to have or must have?
- **Feasibility** is given your own capabilities and strengths, does the idea help to leverage on your strengths?

Could be useful to use prioritisation matrices to decide on which are the best ideas. You could draw a chart based on User Value vs Feasibility.

See [this website for more information on prioritisation matrices](https://www.nngroup.com/articles/prioritization-matrices/).

## Product design

- Design brief
- Mood board
- It can be words
- Rough sketches
- Mutliple workflow diagrams
- UI sketch
- Prototype on [Figma](https://www.figma.com/) or [Sketch](https://www.sketch.com/)
- Present the idea to your friends or intended users and listen to what they want to say with an open mind.

## Create design brief / mood board

See [exercises on Figma](https://www.figma.com/file/cBhTeRdS3e1XqN6CzXGgA3/Design-Brief-and-Mood-Board-Exercise?node-id=0%3A1).

## Presenting your ideas

Give yourself only 5-7 minutes.

- Grab their attention with a great start - Why does your presentation matter to the audience? What does your idea solve?
- Summarise your idea in 60 seconds - Do you have a Unique Selling Proposition? “What makes you different from the competition?”
- Three minutes of content, split into points
- End off your presentation with a short summary again and a call to action - Should they give feedback? Should they start using your application?

## Effective feedback

- Don't be defensive and feel like you need to justify your ideas when people are commenting, allow them to comment
- Give more description instead of judgement. For example, say "Your UI seems easy to use" instead of just "Your UI is good"
- State your feelings and observations instead of blaming the product or person. Your feelings are always true but blaming the product / person could stem from misunderstandings.
- Provide a balance of positive and negative feedback

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
