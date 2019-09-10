---
layout: post
title:  "ajax success/done 차이점"
categories: Jquery,ajax
tags: Jquery,ajax,promise
author: moai
---
출처 : [https://stackoverflow.com/questions/8840257/jquery-ajax-handling-continue-responses-success-vs-done][출처]  

`success` has been the traditional name of the `success` callback in jQuery, defined as an option in the ajax call. However, since the implementation of $.Deferreds and more sophisticated callbacks, `done` is the preferred way to implement success callbacks, as it can be called on any deferred.

For example, success:  

```javascript
$.ajax({
  url: '/',
  success: function(data) {}
});
```




For example, done:  

```javascript
$.ajax({url: '/'}).done(function(data) {});
```

**<span style="color:red">The nice thing about done is that the return value of `$.ajax` is now a deferred promise that can be bound to anywhere else in your application.</span>**  So let's say you want to make this ajax call from a few different places.  
Rather than passing in your success function as an option to the function that makes this ajax call, you can just have the function return `$.ajax` itself and bind your callbacks with `done`, `fail`, `then`, or whatever.  
Note that always is a callback that will run whether the request succeeds or fails. done will only be triggered on success.

For example:  

```javascript
function xhr_get(url) {

  return $.ajax({
    url: url,
    type: 'get',
    dataType: 'json',
    beforeSend: showLoadingImgFn
  })
  .always(function() {
    // remove loading image maybe
  })
  .fail(function() {
    // handle request failures
  });

}

xhr_get('/index').done(function(data) {
  // do stuff with index data
});

xhr_get('/id').done(function(data) {
  // do stuff with id data
});
```

An important benefit of this in terms of maintainability is that you've wrapped your ajax mechanism in an application-specific function. If you decide you need your `$.ajax` call to operate differently in the future, or you use a different ajax method, or you move away from jQuery, you only have to change the `xhr_get` definition (being sure to return a promise or at least a done method, in the case of the example above). All the other references throughout the app can remain the same.

There are many more (much cooler) things you can do with `$.Deferred`, one of which is to use pipe to trigger a failure on an error reported by the server, even when the `$.ajax` request itself succeeds. For example:
```javascript
function xhr_get(url) {

  return $.ajax({
    url: url,
    type: 'get',
    dataType: 'json'
  })
  .pipe(function(data) {
    return data.responseCode != 200 ?
      $.Deferred().reject( data ) :
      data;
  })
  .fail(function(data) {
    if ( data.responseCode )
      console.log( data.responseCode );
  });
}

xhr_get('/index').done(function(data) {
  // will not run if json returned from ajax has responseCode other than 200
});
```


[출처]:https://stackoverflow.com/questions/8840257/jquery-ajax-handling-continue-responses-success-vs-done