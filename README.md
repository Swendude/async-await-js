# Navigator

- [Synchronous and Asynchronous Code](#synchronous-and-asynchronous-code)
- [AJAX](#ajax)
- [Promise](#promise)
- [Async functions](#async-functions)
- [Await](#await)
- [Error Handling with try ... catch](#error-handling-with-try-and-catch)
- [Chain Promises](#chain-promises)
- [Running Promises In Sequence](#running-promises-in-sequence)
- [Running Promises In Parallel](#running-promises-in-parallel)

## Synchronous and Asynchronous Code

First of all we need to know that there are `synchronous` and `asynchronous` code.

Most of the code is Synchronous code and that mean the code is executed line by line in the exact order that we defined in our our code.
So each line of code wait for previous line to finish

```js
const p = document.querySelector(".p");
p.textContent = "My Name is Omar!";
alert("Text set!");
p.style.color = "red"; // will start after alert finish
```

But imagine that execution would have to wait

for example, for a five second timer to finish. That would just be terrible, right?
Because meanwhile, nothing on the page would work during these five seconds.

And so that's where asynchronous code comes into play.

```js
const p = document.querySelector(".p");
setTimeout(function () {
  p.textContent = "My Name is Omar!";
}, 5000);
p.style.color = "red";
```

Now, anyway, the first line of code is still synchronous here, and we also move to the second line in a synchronous way.
But here we encountered the set timeout function, which will basically start a timer in an asynchronous way.
So this means that the timer will essentially run in the background without preventing the main code from executing.
We also register a callback function, which will not be executed now, but only after the timer has finished running.
Now this callback function that I just mentioned is asynchronous JavaScript.
And it is asynchronous because it's only going to be executed after a task that is running in the background finishes execution.
And in this case, that is the timer.
So this callback that we just talked about is registered, and then we immediately move on to the next line.
So the main code is not being blocked and execution does not wait for the asynchronous timer to finish its work.
And that's the big difference between synchronous and asynchronous code. So previously we had to wait for the user to click on the alert window to continue execution. And again, that's because alert is blocking synchronous code, but now with this timer, the callback is actually asynchronous. And so it's only going to be executed after the timer has finished. And so therefore we say, that it's non-blocking code because in the meantime, the rest of the code can keep running normally.

So:

- Asynchronous code it is executed **after a task that runs in the "background" finishes**
- Asynchronous code is **non-blocking**.
- Execution doesn't wait for an asynchronous task to finish it's work

So in summary, asynchronous programming is all about coordinating the behavior of our program over a certain period of time. And this is essential to understand.
So asynchronous literally means not occurring at the same time.
And so that's what asynchronous programming is all about.

Callback functions alone do not make code asynchronous, that's essential to keep in mind. Only some of them like setTimeout

```js
[1, 2, 3].map((v) => v * 2);
//the call back inside the map dos NOT automatically make code asynchronous
```

`Asynchronous behavior` in JavaScript like `Ajax` calls. And Ajax calls are probably the most important use case

[Back to top](#navigator)

## AJAX

So Ajax stands for **A**synchronous **J**avaScript **A**nd **X**ML, Allows us to communicate with remote web servers in an **asynchronous way.** With AJAX calls, we can **request data** from a web server dynamically.
Without reloading the page so that we can use that data in our application dynamically.

So let's say that we have our JavaScript application running in the browser, which is also called the Client.
And we want the application to get some data from a web server.
And let's say the data about countries that I was talking about earlier.

So with Ajax,

We can do an HTTP request to the server, which has this data.
And the server will then set back a response containing that data that we requested.
And this back and forth between Client and server all happens asynchronously in the background.
And there can even be different types of requests, like get requests to receive data or post requests to send data to a server.
AJAX calls types: `GET`, `POST`, `PUT/PATCH`, `DELETE`

To use AJAX calls there is a javascript build in function called `fetch`, or use more modern libraries like `axios`

[Back to top](#navigator)

## Promise

The `Promise` object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.
It is used basically as a placeholder for the future result of an asynchronous operation.

Ex: The `response` of ajax call is a promise.

A Promise is in one of these states:

- _pending_: initial state, neither fulfilled nor rejected.
- _fulfilled_: meaning that the operation was completed successfully.
- _rejected_: meaning that the operation failed.

```js
const axios = require("axios"); // Make a get request for country data
const request = axios.get("https://restcountries.com/v2/name/netherlands");
console.log(request); // Promise { <pending> }
```

In this case axios is building a promise for getting data from API.

[Back to top](#navigator)

## Async/Await

since `ES2017`, there is now an even better and easier way to consume
promises, which is called a `sync` `await`. It's surprisingly easy to understand and use.

## Async functions

Let's start with the `async` keyword. It can be placed before a function, like this:

```js
async function functionName() {
  return 1;
}
```

The word "async" before a function means one simple thing: a function always returns a promise. Other values are wrapped in a resolved promise automatically.

For instance, this function returns a resolved promise with the result of `1`; let's test it:

```js run
async function functionName() {
  return 1;
}

functionName().then(alert); // 1
```

...We could explicitly return a promise, which would be the same:

```js run
async function functionName() {
  return Promise.resolve(1);
}

functionName().then(alert); // 1
```

So, `async` ensures that the function returns a promise, and wraps non-promises in it. Simple enough, right? But not only that. There's another keyword, `await`, that works only inside `async` functions, and it's pretty cool.

[Back to top](#navigator)

## Await

The syntax:

```js
// works only inside async functions
let value = await promise;
```

The keyword `await` makes JavaScript wait until that promise settles and returns its result.

We can have one or more await statements. And we can use the promise returned from the axios function and so let's now, use or API again, to search for a country basically.

```js
const axios = require("axios");

async function whereAmI(country) {
  const response = await axios.get(
    `https://restcountries.com/v2/name/${country}`
  );
  console.log(response);
}

whereAmI("netherlands");
```

We await axios to get the data response then just assign this value to a response.
Before we had to mess with callback functions and that was true in callback hell, but also by consuming promises with the then method.
But now with a sync await, that is just completely gone.
Now, before you start using a sync await all over the place, you need to first understand that a sink await is in fact, simply syntactic sugar over the then method in promises.

So of course behind the scenes, we are still using promises. We are simply using a different way of consuming them here.

[Back to top](#navigator)

## Error Handling with try and catch

It works with async/await. So with async/await, we can't use the catch method that we use before, because we can really attach it anywhere, right.
So instead, we use something called a `try` `catch` statement.
And the try catch statement is actually used in regular JavaScript as well.
It's been in the language probably since the beginning. So try catch has nothing to do with async/await.

A `catch-block` contains statements that specify what to do if an exception is thrown in the try-block. If any statement within the try-block (or in a function called from within the try-block) throws an exception, control is immediately shifted to the catch-block. If no exception is thrown in the try-block, the catch-block is skipped.

The `finally` block will always execute before control flow exits the try...catch...finally construct. It always executes, regardless of whether an exception was thrown or caught.

You can nest one or more try statements. If an inner try statement does not have a catch-block, the enclosing try statement's catch-block is used instead.

We can still use it to `catch errors` in async functions. But before we do that,let's look at a more simple example, just to see how try catch works.
So we can basically wrap all our code in a try block. And so JavaScript will then
basically try to execute this code.

```js
async function whereAmI(country) {
  try {
    const request = await axios.get(
      `https://restcountries.com/v2/name/${country}`
    );
    const data = request.data[0];
    console.log(data);
  } catch (err) {
    console.error(`${err 💥}`);
  }
}

whereAmI('netherlands');
```

[Back to top](#navigator)

## Chain promises

Let's see other example how to chain promises in async call along with error handling.

So in this example there are two axios AJAX calls, one to Geocode API to get all data related to specific location by entering two parameters (latitude and longitude) and in data object we will find city and country of the location. The second call to restcountries API is depending on data return from first promise.
As we see we wrapped the logic inside async function in `try` block including api calls, also use catch to handle error.
Finally block is optional to use, the code will be executed in it as last in the async call

```js
const axios = require("axios");

const whereAmI = async function (latitude, longitude) {
  try {
    const resLocation = await axios.get(
      `https://geocode.xyz/${latitude},${longitude}?json=1`
    );
    const data = resLocation.data;
    if (resLocation.status !== 200)
      throw new Error(`Problem with geocoding ${resLocation.status}`);

    console.log(`You are in ${data.city}, ${data.country}`);

    const resCountry = await axios.get(
      `https://restcountries.com/v2/name/${data.country}`
    );
    if (resLocation.status !== 200)
      throw new Error(`Problem with geocoding ${resCountry.status}`);

    // console.log(resCountry.data[0]);
  } catch (err) {
    console.error(`${err.message} 💥`);
  } finally {
    console.log("finally do something optional");
  }
};
whereAmI(52.50177, 13.40483);
whereAmI(52.36039, 4.89688);
console.log("FIRST");
/** OUT PUT
  FIRST
  You are in Amsterdam, Netherlands
  You are in Berlin, Germany
  finally do something optional
  finally do something optional
     */
```

[Back to top](#navigator)

## Running Promises In Sequence

Now, if we want to do multiple promises and we write them like in the next example. They will be called after each other and wait, so that means they will spend a lot time comparing if can call them in `parallel` all in one at the same time

```js
const get3Countries = async function (country1, country2, country3) {
  try {
    const resCountry1 = await axios.get(
      `https://restcountries.com/v2/name/${country1}`
    );
    const resCountry2 = await axios.get(
      `https://restcountries.com/v2/name/${country2}`
    );
    const resCountry3 = await axios.get(
      `https://restcountries.com/v2/name/${country3}`
    );
    const [data1] = resCountry1.data;
    const [data2] = resCountry2.data;
    const [data3] = resCountry3.data;

    console.log([data1.capital, data2.capital, data3.capital]);
  } catch (err) {
    console.error(`${err}`);
  }
};

get3Countries("netherlands", "canada", "portugal");

//[ 'Amsterdam', 'Ottawa', 'Lisbon' ]
```

[Back to top](#navigator)

## Running Promises In Parallel

To Run Promises in `parallel` javascript come up with `promise.all` combinator function.
Now, this function here takes in an array of promises, and it will return a new promise, which will then run all the promises in the array at the same time.

```js
const get3Countries = async function (country1, country2, country3) {
  try {
    const data = await Promise.all([
      await axios.get(`https://restcountries.com/v2/name/${country1}`),
      await axios.get(`https://restcountries.com/v2/name/${country2}`),
      await axios.get(`https://restcountries.com/v2/name/${country3}`),
    ]);

    const capitals = data.map((country) => country.data[0].capital);
    console.log(capitals);
  } catch (err) {
    console.error(`${err}`);
  }
};

get3Countries("netherlands", "canada", "portugal");

//[ 'Amsterdam', 'Ottawa', 'Lisbon' ]
```

So as we saw promises are running in parallel, no longer in sequence now.
`Promise.all` sends an `array` and receive an `array`
If one of the promises get reject, all the promises get rejected because the are in parallel.
Great, so whenever you have a situation in which you need to do **multiple asynchronous operations** at the same time, and operations that don't depend on one another, then you should always, `always` run them in parallel, just like we did here using promise.all.

[Back to top](#navigator)
