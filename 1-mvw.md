## me&ngular.

If you haven't already, I highly encourage you to check out Angular. The first time I looked at it, I thought, _"ew! All of that 'ng-'' stuff in the markup? Gross!"_ If you're thinking the same thing, __remember this feeling__. Being introduced to something that makes you think in a completely new way doesn't come along often. When it does, embrace it. It will keep you employable, if that's your concern. But more than that, it will expand your comfort zone.

That's what Angular did for me, at least. The problem, however, is the ramping up time. While the documentation and availability of help is great, it will still take time and patience. While I'm far from an expert, I thought I would try to help someone who is starting from where I was just a few weeks ago.

This will be a mini-series on the evolving tale of mine and Angular's relationship.

### Welcome to me&ngular.

## mvw?

The most important thing I've learned about MVC is that nobody knows what it is. I stand behind this statement with about 95% certainty. To me, this isn't a bad thing. MVC is a collection of helpful patterns arranged in a way that urges the separation of concerns. As long as you abide by that interpretation of the architectural pattern, you're good. _Bend it, don't break it, and **ship it**._

Angular seems to agree with that general idea, which is why they've labeled it _M-V-W_. I'll save you the deep Googling; it stands for __Model-View-Whatever__. The developers took bites out of several different patterns and spit out their own definition of what makes a successful web app.

### `m`

I'm pretty confident if you've made it this far in the article, you're familiar with a Model. Angular doesn't deviate from the definition you're familiar with. You might see it referred to as the _single source of truth_ in their documentation. This is a good way to think of it, and we can probably leave it at that for now.

### `v`

Thiiiiis is where things might shake up your system. Close your eyes _--wait until you finish this thought to close your eyes, otherwise this won't work--_ now, remember your early days as a "webmaster." Remember your first `<h1>`? Remember how ugly Times New Roman was on that white background?

Did you ever do a `<font size="3" face="verdana" color="red">`? How nice was it to see in your markup _everything_ a webmaster needed to know about what's going to display in the browser? Pre-stylesheets, you didn't need to dig up a separate CSS file, find the matching selector, and make your changes _nowhere near the actual display element_.

Of course, CSS is a wonderful thing. Using repeatable modules of style rules throughout your site not only offers developer sanity, but improved performance. However, imagine a world where __the convenience of structuring a _document_ high-fived the art of _web application_ architecture__.

#### angular _views_ are that high-five.

When you see a `<p>`, there's your paragraph.

An `<img>`, an image.

Now, a quick test. What do you think when you see:

    <uploader>

If you've worked with HTML forms, and file uploads in particular, you might imagine something like this:

    <form id="image-uploader" name="image-uploader" method="post" action="/where/this/image/needs/to/go">
      <input type="file" name="files">
      <input type="submit" value="upload!">
    </form>

Angular takes advantage of JavaScript being __everywhere__, and lets you sprinkle your own seasoning on top of the official HTML specification using JavaScript, then brings that new spec with it to the compilation process. Let me go over that a little slower this time, because this is what makes Angular unique.

The HTML specification is a big, long rule book that says what you can and can't do with the language if you want your file to be _valid_ and understood by all the different browsers out there. It's what says `<a>` represents an anchor, or link. It's also what says `<uploader>` is _not_ a recognized tag.

Angular stands between you, the developer, and the specification implementer, the browser. If you tell Angular `<uploader>` *is a tag, damn it*, Angular tells the browser, `<uploader>` **is a tag, damn it!**

If you have some kind of crazy, file uploading site, wouldn't it be nice to just say...

    <uploader id="image-uploader" url="/where/this/image/needs/to/go" />

...and have the not-as-pretty `<form>...</form>` stuff generated for you?

Angular gives you this type of control over your HTML with what they call `directives`. Its usefulness is only limited by your imagination. This is the point you start to realize making web applications could have been a lot easier if you had this type of control years ago.

You'll start to notice you're engaged in __a one-sided, declarative conversation with your HTML file.__

Let's look at another quick example of this __declarative__ approach.

Ever try something like this?

    var displayUsers = function () {
      $.post('/api/users', function(res){
        $.each(res.users, function(i, user){
          insertUserBios(user);
        });
      });
    };

    var insertUserBios = function (user) {
      $('.user-bios').append('<div><h1>'+user.name+'</h1><p>'+user.bio+'</p></div>');
    };

    $(displayUsers);

This would hopefully generate something like:

    <section class="user-bios">
      <div>
        <h1>Stephen</h1>
        <p>I am a passionate web developer, loving father, and Mountain Dew enthusiast.</p>
      </div>
      <div>
        <h1>Phoenix</h1>
        <p>Phoenix is a tiny little baby, daughter to a Mountain Dew enthusiast.</p>
      </div>
    </section>

Instead of going to some separate JavaScript file to structure your presentation markup, wouldn't it make more sense to start from where the presentation markup goes?

    <section class="user-bios">
      <div ng-repeat="user in users">
        <h1>{{user.name}}</h1>
        <p>{{user.bio}}</p>
      </div>
    </section>

You can then leave it to the JavaScript to get you the `users` data, and expose it to your view with the help of something called the `$scope`. This creates a much nicer development environment for the _designer_ and the _developer_ to work side-by-side.

There's a lot more to the Angular `view` than just trivial time-savers. Hopefully by seeing those two examples, you can start to imagine the complex and powerful architecture you can arrange for your application. Angular's views will help your development thought process stay in the right place, not letting your presentation get lost in the weeds of some complex JavaScript file.

Speaking of complex JavaScript, let's see if we can figure out what those zany Angular guys mean by that `w`hatever in M-V-W.

### `w`

**Like, omg, just-- w/e.**

If you've played around with the many different MVC frameworks out there, you may have said something like that when trying to figure out what goes where. The Angular devs seemed to appreciate the multiple personality disorder, MPD, MVC has taken on, and thought they would be honest about their contribution to the confusion.

My take on the `w`hatever is a hybrid of tools and patterns. Controller, ViewModel, Observer, and the scaffolding for you to implement your own preferred solutions to the problems you'll face.

The `controllers` of your app, will be bound with the `views`, the .html template files. So, if you're making a page which lists a quick bio about each of your users, you might have a `UserBiosCtrl` controller file, and a corresponding `user-bios.html` file.

Within `user-bios.html`, you will have access to `UserBiosCtrl`, and have a free-flowing data pipe from one to the other.

_user-bios.html_

    <section class="user-bio">
      <div ng-repeat="user in users">
        <h1>{{user.name}}</h1>
        <p>{{user.bio}}</p>
      </div>
    </section>

_UserBiosCtrl.js_

    var UserBiosCtrl = function ($scope, Users) {
      $scope.users = Users.getAllUsers();
    };

I hid a little bit of magic in here, but you can probably start connecting the dots. Let's go over some of this new magic.

- Do I have to do a function call `UserBiosCtrl()` when my page loads or something?

- What the heck is `$scope`, and why the dollar sign? Are we using some kind of jQuery madness now?!

- `user`? Where am I getting this `user` from? Am I doing that outside of Angular, and then just feeding it in when I call the controller?

The easy answer to all of this is a quiet, creepy "shhhh... we've got you covered."

Let's take a quick walk through some of the very basics of how Angular works. As you've probably read, it's not just a library, it's a framework. Part of that responsibility is routing your user where they need to go. So, that means regardless of where a visitor would like to begin in your app, you will always start from a single file: index.html. And index.html will contain the Angular JavaScript file in addition to your custom JavaScript files.

Your custom JavaScript, which could span several .js files, will be responsible for telling Angular what to do. Before you can do that, you have to know how Angular wants you to speak to it. Angular can "inject" several helper functions inside of your JavaScript, which demand your respect of their API. These are crucial pieces of Angular's approach: __plan ahead and be consistent__.

One of those very helpful, injectable function is something called `$routeProvider`. When you're bootstrapping, or configuring, your application, you can ask for this function, and Angular simply goes, "oh yeah, $routeProvider. Cool. Here ya go." The technical name for this is...

#### dependency injection.

`$routeProvider` is meant to guide your user throughout your application, which should explain some of that magic we saw above. This is the glue that holds our template and Controller together:

    $routeProvider.when('/users', {
      controller: UserBiosCtrl,
      templateUrl: 'user-bios.html'
    });

Let's peep on `UserBiosCtrl` again:

    var UserBiosCtrl = function ($scope, Users) {
      $scope.users = Users.getAllUsers();
    };

So, now we know by just asking for things in our `function (/** arguments */)`, we get them. Let's see we get by saying we want `function ($scope, Users)`.

- `$scope` is an object that you can add your own properties and datasets on to, which are then available within the "scope" of the corresponding template.

- `Users` is something I haven't explained. And, for the sake of simplicity, I'll leave it unexplained. Just know this: I defined an extra piece of functionality that I want to make accessible from my controllers, and I told Angular about it. That's it! Angular took care of the magic and injected it into my controller for me to use however my template sees fit.

There are a lot more things that go into making an Angular application come together, and it's something I look forward to explaining in the coming articles.  For now, it's...

### play time.

When I was starting with Angular, dependency injection and custom directives were the most intriguing things. I encourage you to set up your first application and explore these things, if you haven't already. You'll have no choice but to become familiar with some of the principles and features we've talked about.

### to be continued...

If you're not yet inspired to give Angular a try, please stay tuned for the next edition of __me&ngular__.

##### need more now?
- [AngularJS](http://angularjs.org/)
- [AngularJS Blog](http://blog.angularjs.org/)
- [AngularJS Mailing Group](https://groups.google.com/forum/?fromgroups#!forum/angular)
