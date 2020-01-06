# Props with Functional Component

reusing component with a similar layout for different data

## Covers

1. Why Functional Component
2. Passing a value to a functional component

## Why Functional Component

Functional Components is perhaps the easiest way to create a highly reusable component.
Say we want to create a component that can say Hi to many different people.
A naive way is to keep repeating.

```javascript
<div>Hi John Smith</div>
<div>Hi Jane Doe</div>
<div>Hi Peter Pan</div>
<div>Hi Alice</div>
<div>Hi Bob</div>
```

If we want to style the text, we now have

```javascript
<div className="welcome-text">Hi John Smith</div>
<div className="welcome-text">Hi Jane Doe</div>
<div className="welcome-text">Hi Peter Pan</div>
<div className="welcome-text">Hi Alice</div>
<div className="welcome-text">Hi Bob</div>
```

If you want to change the Hi to Hello, you need to search thru all the code and edit them one by one.

```javascript
<div className="welcome-text">Hello John Smith</div>
<div className="welcome-text">Hello Jane Doe</div>
<div className="welcome-text">Hello Peter Pan</div>
<div className="welcome-text">Hello Alice</div>
<div className="welcome-text">Hello Bob</div>
```

If we do not create a reusable component for elements that behaves the same, the code becomes hard to maintain and difficult to tell if the code is behaving consistently.

## Passing value to functional component

Having a reusable component comes with lots of flexibility.
We can group common behaviours to a single place and allow modifying of code in one place.

```javascript
function WelcomeText(props) {
  return <div className="welcome-text">Hello {props.name}</div>;
}

function App() {
  return (
    <div className="App">
      <WelcomeText name="John Smith" />
      <WelcomeText name="Jane Doe" />
      <WelcomeText name="Peter Pan" />
      <WelcomeText name="Alice" />
      <WelcomeText name="Bob" />
    </div>
  );
}
```

!Sidenote Is nice to keep things DRY(Don't Repeat Yourself) and have reusable component but make sure the components should have similar behaviour and not happened to be the same. I.E. if a username and shopping cart item have the same style and format, it would still be better to separate them to use separate components and apply the same style as they work independently and likely to change independently from one another.

Is sometimes more readable to destructure out the value in a Component.

```javascript
function WelcomeText({ name }) {
  return <div className="welcome-text">Hello {name}</div>;
}
```

Destructuing revision:

```javascript
const props = {
  name,
};

const { name } = props;
```

Similarly, we can rename variable to improve clarity when necessary.

```javascript
function WelcomeText({ name: username }) {
  return <div className="welcome-text">Hello {username}</div>;
}
```

## Exercise

1. Pokemon Component

- git clone https://github.com/thoughtworks-jumpstart/pokemon-react-lab
- In App.js, create a component `PokemonCard` to return a Pokemon's image and states.
- Style the Component

![Pokemon Card](/_media/pokemonCard.png ":size=200")

Credits to batch 6 trainee Yun

2. Change content to your repo

- Create a new react app "pokemon gallery" and push it to Github.

```sh
git remote remove origin
git remote add origin <your repo here, i.e. git@github.com:john-smith/pokemon-gallery>
```

- Open package.json, change the name of the package to "Pokemon Gallery."
- commit and push to master

```sh
git push -u origin master
```

3. Moving component to separate file

- Create a new file "components/PokemonCard.js"
- move pokemon card component into the file

4. PokemonGallery

- Create a new file "components/PokemonGallery.js"
- import PokemonCard into the component
- import data from "pokemon/pokemon.js"
- Create a PokemonGallery component that renders out all the pokemon.
- import PokemonGallery from App.js and use the component.

![Pokemon Gallery](/_media/pokemonGalleryDemo.png ":size=600")
