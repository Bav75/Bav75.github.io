---
layout: post
title:      "Destructuring Destructuring (Pun intended) "
date:       2019-09-28 18:02:05 +0000
permalink:  destructuring_destructuring_pun_intended
---


Now that we've gotten the bad jokes out of the way, today we'll be discussing the destructuring assignment syntax in ES6 and cover a few general use cases. For those unfamiliar with destructuring assignemnt, it's a concise syntax you can use to extract values from arrays and objects into variables. Initially I found the syntax offputting and vague, however, as I've practiced integrating the syntax into my code more and more, I've actually grown to appreciate how lightweight it is (as opposed to dot notation). Below I'll walk through a few examples demonstrating how you can make use of destructuring assignment. 


# Array Destructuring 
One of the simplest use cases for destructuring assignment is when used in conjunction with an array. Take for example the below code: 

```
let arr = [1, 2, 3];

let value1 = arr[0];
let value2 = arr[1];
// ...repeat for all values in array 

```

Traditionally, in order to extract specific values from your array into a separate variable, you'd need to select values by their index in the array (or make use of some combination of array methods such slice, splice, pop, shift, etc...). While it gets the job done, you end up repeating a lot of the same code. With destructuring assignment we're able to cut down this code dramatically and make it much more legible:

```
let arr = [1, 2, 3];

let [val1, val2, val3] = arr;

console.log(val1) // 1;
console.log(val2) // 2;
```

Destructuring assignment (with an array) allows you to specify the variable name you'd like to assign to each element in the array you're targetting. In cases where you don't want to grab every value in the array, you're able to skip elements in the array with a comma (,) as demonstrated below:

```
let arr = [1, 2, 3];

let [val1, , val3] = arr;

//skip value 2 and assign only values 1 and 3

```
# Object Destructuring
Destructuring assignment can also be used with objects to great effect. For this next portion we'll cover object destructuring in the context of React props. As I entered the React portion of the Flatiron curriculum, I quickly became introduced to props and their importance in building a React application. Depending on the type of component / underlying data we're working with we may find ourselves in situations where our props are complex or nested (as in the example case below):

```
import React, { Component} from 'react'

export default class AmusementPark extends Component {

park = {
 rides: [{ride1: ...info}, {ride2: ...info}],
 staff: {security: [...securityTeam], engineers: [...engineeringTeam], hosts:[...hostTeam]},
}
}
```

Lets say in this example we wanted to access information on ride1 from our props object which was passed into a newly created Ride component 

```
render() {

return(
<React.Fragment>
 <Ride park={this.park} />
</React.Fragment>
 )}
```

Traditionally we could access information embedded in our props object via dot notation

```
const Ride = props => {

 const rides = props.rides
 const ride1Info = rides[0].ride1
 .... //repeat for other rides 
}
```

After a while this can get quite repetitive, particularly for larger props. Alternatively, we could make use of destructuring assignment to instantly assign the data we want from our props object to our desired variables within our Ride component

```
const Ride = ({rides, rides: {ride1, ride2, ride3}) => {
 console.log(rides) // [{ride1:...info}.....]
 console.log(ride1) // ...info 
 ....
}
```

Let's breakdown that destructuring line a bit more in detail - {rides, rides: {ride1, ride2, ride3}}. The first rides value within the destructuring syntax corresponds with the rides key within our props object. The rides value we specified in our destructuring syntax will be matched against the key within our props object and will now be available as a rides **variable** within our component containing the value of the rides key within props. In addition to grabbing the entire value of the rides key from our props object, we also grabbed the values of each individual ride within the rides key (rides is an array of objects). 

As you can see, object destructuring really starts to shine when working with nested or deeply nested structures. To take this even one step further, you can also rename variables within the destructuring syntax; you are not bound to making use of the original variable name provided by the keys within the object you're destructuring. Let's take a look at the below example: 

```
const Ride = ({rides : amusements, rides: {ride1 : amusement1, ride2 : amusement2, ride3: amusement3}) => {
 console.log(amusements) // [{ride1:...info}.....]
 console.log(amusement1) // ...info 
 ....
}
```

In this scenario, I was able to rename the value originally associated with the rides key to a variable named amusements. This variable is now avaiable to me within my component under this name (note that rides would not be avaiable in this scenario as I changed the variable name during the destructuring assignment). The same also applies for each individual ride object which was renamed to amusement. 

While destructuring assignment initially takes some work to understand, it pays off tremendously as the data structures you're working with grow in complexity and size. Hopefully a few of these examples have inspired you to make use of destructuring assignment within your own projects! 



