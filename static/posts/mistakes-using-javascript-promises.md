---
title: Mistakes we make using JavaScript Promises 
date: Wednesday, May 29, 2019
description: Mistakes developers often make when using JavaScript Promises and how to avoid them
---

Promises were introduced in ES6 to improve the way we handle asynchronous tasks in JavaScript. A promise is simply an object that serves as a placeholder for an asynchronous result but its implementation helps us solve three problems we experienced using simple callbacks:

**Difficult error handling:** Since asynchronous callbacks are not being executed in the same step as the event loop, we can’t use built-in language constructs such as try/catch statements:

```javascript
try {
    get("http://data.io/user", function() {
        // handle result
    });
} catch (error) {
    // handle error
}
```

**Performing asynchronous code in parallel:** In some cases, we want to perform an action preceded by asynchronous requests that are mutually independent. Running them in parallel is an opprtunity to improve performance but with simple callbacks, it requires some boilerplate code:

```javascript
var users, entries;
 
get("http://data.io/users", function(users) {
    users = users;
    finished();
});
 
get("http://data.io/settings", function(entries) {
    entries = entries;
    finished();
});
 
function finished() {
    if(users != null && entries != null) {
        // perform action
    }
}
```

**Performing asynchronous code in a sequence:** If we have multiple asynchronous requests that are interdependent, we usually end up with code that’s difficult to read:

```javascript
get("http://data.com/user", function(user) {
    get("http://data.com/location" + user.id, function(location) {
        createEntry(user, location, function(response) {
            // handle response
        });
    });
});
```

- - -

The promise syntax immediately addresses the first problem, *difficult error handling*. The `then()` function lets us pass two callbacks to handle resolved promises, one for the fulfilled state (resolved successfully) and one for the rejected state (an error has occurred). As for the other two problems, we still fail to address them at times.

### Mistake 1: Not running asynchronous tasks in parallel

Promises offer useful functions that allow us to process asynchronous code in parallel but a lot of developers don’t take advantage of them, instead, they nest their promises and run their requests in a sequence:

```javascript
get("http://data.com/user").then(user => {
    store.commit("user", user);
    get("http://data.com/settings").then(settings => {
        store.commit("settings", settings);
    });
}).catch(err => {
  // handle error
});
```

In this example, the second promise will not be processed until the first one is resolved, but since they these requests are not mutually dependent, we should process them in parallel using `Promise.all()`:

```javascript
Promise.all([
    get("http://data.com/user"),
    get("http://data.com/settings")
]).then([user, settings] => {
    store.commit("user", user);
    store.commit("settings", settings);
}).catch(error => {
    // handle error
});
```
In this example, both promises will be processed asynchronously and only when both of them are resolved, we handle the result. JavaScript is still single-threaded but since fetch is a primitive handled internally by the browser, it supports multiple fetches at once.

Another related function is `Promise.race()`. Like `Promise.all()`, It takes an array of promises, but `Promise.race()` only handles the first request that’s resolved, regardless of which one it is:

```javascript
Promise.race([
    get("http://data.com/user1"),
    get("http://data.com/user2")
]).then(anyUser => {
    console.log(`${anyUser.name} responded first!`);
}).catch(err => {
    // handle error
});
```

### Mistake 2: Not chaining promises

The thing that makes the `then()` function special is, it always returns a promise. This allows us to chain promises and eliminates callback hell altogether. Unfortunately, nested promises are not uncommon:

```javascript
get("http://data.com/user").then(user => {
	get("http://data.com/location" + user.id).then(location => {
  	    createEntry(user, location).then(response => {
    	    // handle response
        });
    });
}).catch(err => {
	// handle error
});
```

This example looks very similar to the one that’s using callbacks. Let’s see how we can make things a little better by chaining these requests:

```javascript
get("http://data.com/user")
.then(user => get("http://data.com/location" + user.id))
.then(location => createEntry(user, location))
.then(response => { 
    // handle response
}).catch(err => {
    // handle failure
});
```

This example looks much better! It certainly makes the code more readable, but we can do even better.

### Mistake 3: Not adopting new features

Before we get into *async functions*, it’s worth mentioning they are not entirely new.

Promises were the most anticipated feature in ES6 but it wasn’t the only one that helped improve our experience with asynchronous code. Often overlooked, generator functions offered an interesting use case that helped us write asynchronous code that looked very much like synchronous code.

Generator functions are special functions that can return multiple values in a sequence and on-demand. Use cases for generators are very limited in the context of a web app, however, combined with promises, generator functions offer a new way to write asynchronous code.

If we were to write an async handler function that takes a generator as a parameter, our last example would have looked like this:

```javascript
async(function *() {
    try {
        let user = yield get("http://data.com/user");;
        let location = yield get("http://data.com/location");
        let response = yield createEntry(user, location);
        // handle response
    } catch(error) {
        // something went wrong
    }
});
```

If you are curious about implementing an async handler using generators, I encourage you to explore this topic, but in the meantime, you can get started with async functions right away!

The ECMAScript committee was well aware of the benefits of combining generators and promises so they decided to save us the time by introducing async functions in ES2016.

Now, without writing any additional code, we could rewrite our last example using async/await:

```javascript
async function processRequests() {
	try {
        let user = await get("http://data.com/user");;
        let location = await get("http://data.com/location");
        let response = await createEntry(user, location);
        // handle response
    } catch(error) {
        // something went wrong
    }
}
```

It shouldn’t surprise you that both examples look very similar. Under the hood, async functions are essentially a mixture of generators and promises, but now we don’t have to think about the implementation.

If you want to learn more about async functions, a good place to start is [Async functions — making promises friendly](https://developers.google.com/web/fundamentals/primers/async-functions) by Jake Archibald at Google.

- - -

I hope this article highlighted some common issues we can all avoid moving forward. If you’ve got feedback, the best way to reach me is on Twitter [@shlominissan](https://twitter.com/shlominissan).