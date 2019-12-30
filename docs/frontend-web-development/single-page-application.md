# Single Page Applicaiton

Introduction to single page application(SPA)

## Covers

1. Motivation of SPA
2. Overview of SPA
3. Pros and Cons of SPA

## Motivation of SPA

Just a while back, web application used to be dreadful to use. Every action by a user would require the browser to refresh. The page would go white, and the new content would be shown to the user again.

Example of a web app that requires the browser to reload: https://forums.xamarin.com/

As you can probably tell, the user experience is not that good. The load time takes longer, the screen goes blank before new content is loaded and native applications were still the preferred way to interact with any software. Many attempts by web developers made to improve this experience with some degree of success such as Iframes(website in another website), Java applets, Flash in website each bringing security or performance issue.

The introduction to Asynchronous JavaScript And XML(AJAX) in the 2000s changes everything. When AJAX got famous, and browser support for DOM manipulation techniques improves, web developers now have a tool to change part of the content of a website rather than causing a full page to reload.

Web application has improved significantly since, reaching the standards of even beyond
comparing to Native applications. The benefits of having a fast, responsive and always up to date software makes browsing a joy. SPA prevents users from even noticing all the controlling logic behind the scene.

## Overview of SPA

Before we talk about SPA, we might first want to understand the [Model-View-Controller](https://en.wikipedia.org/wiki/Model-view-controller) model.

Components:

- View, the presentation that users see.
- Model, the data that end-user would see
- Controller, logics that updates the model also controls what the user can or cannot do

When a user loads a page, the View would take the information it needs from the model and present it to the user. When a user does an action, the controller will determine can the action be done(check for permissions, validation) then execute the action most likely to update the model. The View would take the updated model and present the newly updated View to the user.

## Server-Side Rendering

On a traditional web application, the browser takes the HTML file from the Server. The Server would have to build the HTML file with all the information. The browser would then fetch this HTML file.

Browser

- HTML, construct the DOM and present to the user

Server

- Presentation Layer(View)
  - Controllers the view to show to the user.
- Controller
  - Expose API to the user
  - Handles the Business logic
- Model
  - Update and fetch latest data typically from a Database

Single Page Application

- On a SPA, the Server would send a single HTML file with all necessary CSS and JS bundle. The
  browser would change the View-based on user interactions and model data fetched from the Server.

Browser

- Presentation Layer
  - html, construct the DOM and present to the user
  - view controller that fetch data from Server

Server

- Controller

  - Expose API to the user
  - Handles the Business logic

- Model
  - Update and fetch latest data typically from a Database

## Pros and Cons of SPA

### Pros

- Browser doesn't reload
  - only parts of the content within a page is being change
- Faster loading time
  - Transferring the data with the html page will have a smaller file size
  - only load new necessary data
- More responsive
  - Changes on the view can be done within the browser
- Code Seperation
  - Presentation and Business logic can be entirely separated
  - Ease of deployment
  - Ease of development
  - Opportunity for different views(browser, desktop native, mobile native apps)

### Cons

- Longer initial load time if JS bundle is very large
