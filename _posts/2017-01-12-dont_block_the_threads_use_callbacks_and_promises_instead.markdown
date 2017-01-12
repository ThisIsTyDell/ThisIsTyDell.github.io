---
layout: post
title:  "Don't Block the Threads, Use Callbacks and Promises Instead"
date:   2017-01-12 03:22:52 +0000
---

## Async Callbacks and Promises


If you have ever written any JavaScript before then, you've run into both synchronous and asynchronous code. Synchronous JavaScript runs one line at a time completing each task before moving onto the next. Synchronous JavaScript is easy to write and easy to read, since it's written and executed in a linear format. This easy of use does come at a cost, for more complex applications. While nothing entirely wrong with this approach, depending on your application's needs this may not be the best way to compose your code. Reason being is that JavaScript is single threaded, so it can only work on one task at any given moment in time.

Each time a function executes, it must be run entirely before moving on to the next. For simple operations or just returning a value this is fine and wouldn't cause any noticeable drag at all to your program. What if your app is more complicated than just returning a value or simple math, and you have to retrieve from the server or a 3rd party API? This type of task may take a long time and would lock up the application until the job is complete. The user of your app would not be able to do anything else until that is complete. That can be frustrating to users and cause an unpleasant experience due to UI issues. 

So there just has to be a better way right? Yes, with Asynchronous JavaScript, or Async JS for short. Async JS allows the JS engine to do task switching. Task switching is awesome because instead of the system waiting for the function to complete, the system can switch to another task and start working on that task. Asynchrony can is possible multiple different ways, but this post will focus on Callbacks and Promises, each having its set of pros and cons and best use cases. 

> "Don't Block the Threads, Use Callbacks and Promises Instead"

We know JavaScript allows us to pass in strings and objects as parameters, but you can also pass in functions as well. That's what's known as a callback. A callback is a higher-order function that takes advantage of the ability to pass functions around just like variables. The callback function is not executed inside of the parameter itself, but rather "called back" later inside of the containing function. The callback has access to the containing function's variables. These characteristics allow us to write more elegant code by abstracting "things" away and keeping the separation of concerns convention in check.

Callbacks can be written either synchronously or asynchronously. Implementing callbacks asynchronously allows our code to make our request and continue doing other things until the request completes. When the request is complete, the callback takes over and can operate over the returned data, doing whatever you tell it.

Here is an example of a callback used with jQuery. (In this case, the callback function will is labeled "cb" for reference)

```
function cb(data){
   console.log(data); //this will print to the console second.
}

$.get("someUrlOrDataPath.json", cb);
console.log("test"); // this will print to the console first
```
In the above example jQuery sends a get request to the URL. While waiting for the data to return from the server, the next line item will run. In this case "test" will be shown in the log. After the data from the server is retrieved, we can then call the callback and pass the data from the request. After that the callback can do whatever you want, in this case, the data is being printed to the console.

Callbacks are great, but be careful when using multiple nested callbacks. You've probably heard of it before. CALLBACK HELL. It can get messy real quick and become more error prone. Don't end up in callback hell. Here's a contrived example using AJAX to illustrate callback hell. 

```
$.ajax({
  type: "GET",
  url: "someUrlPathOne.json",
  success: function(data){
    console.log(data);

    $.ajax({
      type: "GET",
      url: "someUrlPathTwo.json",
      success: function(data){
        console.log(data);

        $.ajax({
          type: "GET",
          url: "someUrlPathThree.json",
          success: function(data){
            console.log(data);
          },
          error: function(jqXHR, textStatus, error){
            console.log(error);
          }
        });
      },
      error: function(jqXHR, textStatus, error){
        console.log(error);
      }
    });
  },
  error: function(jqXHR, textStatus, error){
    console.log(error);
  }
});
```

This looks like a nightmare to debug. Callback after callback after callback.  It's hard to work with because things are scattered. This can be refactored to make debugging and reading easier without getting lost in the nest of a nest of nests. Here's an example of the above, refactored to avoid callback hell.

function handleError(jqXHR, textStatus, error){
console.log(error);
}

```
function handleError(jqXHR, textStatus, error){
  console.log(error);
}

$.ajax({
  type: "GET",
  url: "someUrlPathOne.json",
  success: cbOne,
  error: handleError
});

function cbOne(data){
  console.log(data);

  $.ajax({
    type: "GET",
    url: "someUrlPathTwo.json",
    success: cbTwo,
    error: handleError
  });
}

function cbTwo(data){
  console.log(data);

  $.ajax({
    type: "GET",
    url: "someUrlPathThree.json",
    success: function(data){
      console.log(data);
    },
    error: handleError
  });
}
```

The example above looks a little cleaner and easier to work with. We no longer have that triangle of death. We aren't diving deep into the nested callbacks. Instead, we are going down the code doing one thing after another. This code appears to be written in a linear or synchronous fashion making it simpler to follow, but yet it is still asynchronous. This is a better way to arrange our code to avoid callback hell and readability issues, but it still seems a bit complicated for just three tasks. However, there is an even better way to achieve the same functionality. I promise.

We can use promises for more complicated situations that involve a lot of asynchronous callbacks. We can use promises to organize our code better and make it easier to maintain. A promise is an object that represents an action that has not finished yet but will at some point later. It is a placeholder for the result of an asynchronous task.

For example, in an asynchronous HTTP request, as soon as that request is made, it returns a promise object right away before the data is retrieved and brought back to us. Within that promise object, we can register callbacks that will run when that request completes. Let's refactor the previous example above, but this time using promises.

```
function get(url){
  return new Promise(function(resolve, reject){
    var xhttp = new XMLHttpRequest();
    xhttp.open("GET", url, true);
    xhttp.onload = function(){
      if (xhttp.status == 200){
        resolve(JSON.parse(xhttp.response));
      } else {
        reject(xhttp.statusText);
      }
    };
    xhttp.onerror = function(){
      reject(xhttp.statusText);
    };
    xhttp.send();
  });
}

var promise = get("someUrlPathOne.json");
promise.then(function(dataOne){
  console.log(dataOne);
  return get("someUrlPathTwo.json");
}).then(function(dataTwo){
  console.log(dataTwo);
  return get("someUrlPathThree.json");
}).then(function(dataThree){
  console.log(dataThree);
}).catch(function(error){
  console.log(error);
});
```

The above example looks a lot less cluttered and easier to work with than the callback hell we experienced earlier. It gets even better, though. jQuery comes with a promise interface. It abstracts a lot of it out, though. Here is a quick example.

```
$.get("someUrlPathOne.json").then(function(dataOne){
  console.log(dataOne);
  return $.get("someUrlPathTwo.json");
}).then(function(dataTwo){
  console.log(dataTwo);
  return $.get("someUrlPathThree.json");
}).then(function(dataThree){
  console.log(dataThree);
});
```
Now compare this to the first example when we were in CALLBACK HELL just to do the same thing. This is even easier to read and work with. However, since everything is abstracted away, you will need some knowledge of jQuery and Promises to debug if you run into errors. However, with the readability of this last example, you may be less error prone than CALLBACK HELL. 

This post was intended to give you an overview of async JS. So we've gone through the benefits of asynchronous JS. We don't want to have to block the system just to wait for data to load. Instead, we'd rather put the request in a queue and then run callbacks when the requests finish. Also looked at different ways achieve async JS with callbacks and promises.

We only skimmed the surface, stay tuned to this blog for more detail in the future on callbacks, promises and various other topics. So remember, Don't Block the Threads, Use Callbacks and Promises Instead!

