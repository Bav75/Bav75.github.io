---
layout: post
title:      "Understanding State And The "setState" Method"
date:       2019-09-29 20:56:29 +0000
permalink:  understanding_state_and_the_setstate_method
---


Today we'll be taking a closer look at an important aspect of React and React Components, namely State, and how to properly work with it via the setState method. As a quick primer, State is essentially an object which lives within your application's components (similar to props), and which contains data describing what should be currently rendered on your DOM. Whenver changes occur to your component's state, React is able to detect this and make adjustments to the DOM accordingly. This functionality is incredibly powerful and allows us to build dynamic and responsive applications. 

In order to better understand State, let us first look at how to create a component capable of containing State:

```
class MovieTheater extends React.Component {
 constructor() {
  super()
	
	this.state = {
	 tickets: 0,
	 movies: []
	}
	
	render() {
	 return (<Movie tickets={this.state.tickets}/>)
	}

 }

}
```

In the above example you'll notice we did a few things, namely we: 
1. Created a class which extends React.Component
2. Added a class constructor and a call to super to allow us to use the <this> keyword within the constructor 
3. Set the initial state of the component with assignment to an object literal (with keys of tickets and movies) 

With this initial block of code we now have state configured in our MovieTheater component and are able to dynamically control rendering of our child components (<Movie />)  based on changes in State to MovieTheater. As you'll see in the render method of the above component, we're able to pass the state of MovieTheater down to our Movie component via a props element named tickets (which holds the value of this components tickets state). With this setup we can imagine a few scenarios where we render different text in our movie component based on the number of tickets remaining in the MovieTheater (think "Sorry we're out of tickets" when the tickets property of MovieTheater's state is set to 0).

With the initial setup out of the way, let's take a look at just how we would go about changing our component's state. First, it should be mentioned that we should never change the state of our object's directly, as demonstrated below: 

```
// Do not do this 
this.state.tickets = 20 
```

We mentioned previously that React is able to identify changes to our component's State and re-render our DOMs accordingly; React is able to do all of this because of the setState method. By bypassing that method and assigning new values to our State object directly, React never realizes it needs to re-render the DOM. With that said, the proper ways to modify our component's state are outlined below: 


```
// scenario 1 
this.setState({
 tickets: 20 
})

// scenario 2
this.setState((state, props) => ({
 tickets: state.tickets - props.sales 
}))
```

The setState method works under two conditions: 1. when passed an object literal, 2. when passed a callback function. In the first scenario, we provided setState with an object literal containing the key/value pair we wanted to update within our state object. It should be noted that the setState method does a shallow merge on our existing state object, meaning it will copy over the first layer of values in our State object and overwrite a matching key (if found) or add a new key/value pair to the state object. This is great when working with a State object which doesn't contained nested properties as we only need to provides setState with the key/value pairs we want to update. In nested scenarios however, setState will override nested properties, however this can be addressed by integrating the spread operator into your setState calls. 

Scenario 2 addresses an important fact of the setState method and React state in general: setState is an asynchronous function (therefor State is updated asynchronously) and so you should not rely on the values of your previous State for calculating the value of your new State. By passing the setState method a function, it allows us to ensure we're using the the correct values to set our component's state; in this scenario we are passing in the previous state as an argument to get our previous number of tickets and hence perform the right calculation. In this situation, using this.state.tickets instead of state.tickets may have resulted in the wrong calculation due to the way State is being updated. 

To recap, State is an incredibely powerful and useful aspect of working in React. In order to make use of State we create classes which inherit from React.Component and declare our initial state within our Component's class constructor. We can alter that state by using the setState method and passing in an object literal or a function. 
