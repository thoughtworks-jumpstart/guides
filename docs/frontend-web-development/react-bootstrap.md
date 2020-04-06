# React Bootstrap

React bootstrap is one of the more popular UI Component Library.
Here we are going to show you how to set it up quickly.

## Other common component libraries

1. React Material-UI
2. Semantic UI React

## Should you use a UI Component Library?

UI Components like bootstrap and material can make development much faster and keep a standardise styling. They provide a consisted layout with well-tested style and behaviour with many support the older browser. They are an excellent choice if you don't have an opinion on how to style your component and want to get something out and pretty fast. If you have a very different conclusion from any UI component library, overwriting the styles and maintain the version of the component library will be a nightmare. So do check with your UX if they can align their designs to the component library to get the benefit of using one.

## Setting up bootstrap in 3 mins

```sh
npm install react-bootstrap bootstrap
```

import the stylesheet

In App.js

```javascript
import "bootstrap/dist/css/bootstrap.min.css";
```

Now you can import the components to use

```javascript
import { Container, Row, Button, Col } from "react-bootstrap";

function App() {
  return (
    <Container fluid className="justify-content-center">
      <Row className="header">
        <Col className="text-center" xs={12}>
          Header
        </Col>
      </Row>
      <Row className="buttons">
        <Col className="text-center" xs={12} md={6}>
          <Button variant="warning" onClick={() => alert("warning")}>
            alert
          </Button>
        </Col>
        <Col className="text-center" xs={12} md={6}>
          <Button variant="secondary">does nothing</Button>
        </Col>
      </Row>
    </Container>
  );
}

export default App;
```

Even though you are using a component library, you might still need to style the behaviour outside of what the component support.

App.css

```css
.header {
  height: 2em;
  background-color: pink;
  color: white;
  line-height: 2em;
  font-size: 1.5em;
  font-weight: bold;
}

.buttons {
  margin-top: 1em;
}
```
