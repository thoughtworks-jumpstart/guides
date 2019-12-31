# Props with Functional Component

reusing component with similar layout for different data

## Covers

1. Why Functional Component
2. Passing value to functional component

## Why Functional Component

Functional Components is prehaps the easiest way to create highly reusable component.
Say we want to create a component that can say Hi to many different people.
A naive way is to keep repeating

```javascript
<div>Hi John Smith</div>
<div>Hi Jane Doe</div>
<div>Hi Peter Pan</div>
<div>Hi Alice</div>
<div>Hi Bob</div>
```

If we want style the text we now have

```javascript
<div className="welcome-text">Hi John Smith</div>
<div className="welcome-text">Hi Jane Doe</div>
<div className="welcome-text">Hi Peter Pan</div>
<div className="welcome-text">Hi Alice</div>
<div className="welcome-text">Hi Bob</div>
```

If you want to change the Hi to Hello, you will have to search thru all the code and edit them one by one.

```javascript
<div className="welcome-text">Hello John Smith</div>
<div className="welcome-text">Hello Jane Doe</div>
<div className="welcome-text">Hello Peter Pan</div>
<div className="welcome-text">Hello Alice</div>
<div className="welcome-text">Hello Bob</div>
```

Slowly, this will make the code hard to maintain and difficult to tell if the code is behaving consistenly.

## Passing value to functional component

Reusable component comes with lots of flexibility.
We can group common bahaviors to a single place and allow change to be done in one place.

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

!Sidenote: Is good to keep things DRY(Don't Repeat Yourself) and have reusable component but make sure the components should have similar behaviour and not happened to be the same. I.E. if a username and shopping cart item have the same style and format, it would still be better to separate them to use separate components and apply the same style as they work independently and likely to change independently from one another.

When a component have multiple property. Is sometimes more reable to destructure out the value in a Component.

```javascript
function WelcomeText({ name }) {
  return <div className="welcome-text">Hello {name}</div>;
}
```

Notice the bracket surrounding the name. This is javascript destructuring.

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

### Exercise

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
git remote add origin <your repo here i.e git@github.com:john-smith/pokemon-gallery>
```

- Open package.json, change name of package to "Pokemon Gallery"
- commit and push to master

```sh
git push -u origin master
```

3. Moving component to seperate file

- Create a new file "components/PokemonCard.js"
- move pokemon card component into the file

4. PokemonGallery

- Create a new file "components/PokemonGallery.js"
- import PokemonCard into the component
- import data from "pokemon/pokemon.js"
- Create a PokemonGallery component that renders out all the pokemons.
- import PokemonGallery from App.js and use the component.

![Pokemon Gallery](/_media/pokemonGallery.png ":size=600")
