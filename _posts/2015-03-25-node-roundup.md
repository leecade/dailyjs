---
layout: post
title: "Node Roundup: Node 0.12.1, io.js 1.6.2, DIY, Typescript-Deferred"
author: Alex Young
image: "/images/posts/diy-build.png"
categories:
- node
- libraries
- modules
- iojs
- npm
- build
---

###Node 0.12.1

Yesterday [Node 0.12.1](http://blog.nodejs.org/2015/03/23/node-v0-12-1-stable/) was released.  It has OpenSSL fixes for security vulnerabilities that include a DoS.  Two fixes are ranked as high, and nine are marked as moderate.

io.js just hit version 1.6.2.  The recent io.js releases have had tls and http fixes, but there are other very interesting changes if you dig into the [changelog](https://github.com/iojs/io.js/blob/v1.x/CHANGELOG.md).  For example, the Node binary can now be invoked with [a list of modules to preload](https://github.com/iojs/io.js/pull/881):

{% highlight text %}
iojs -r foo -r /bar/baz.js -r quux
{% endhighlight %}

This is equivalent to:

{% highlight javascript %}
require('foo');
require('/bar/baz.js');
require('quux');
{% endhighlight %}

The reason I like this is it means environment-specific modules can be loaded without changing your code.  You could use this to load modules that instrument the runtime with debugging and logging functionality.

The npm blog (yet again) has a useful post for client-side developers: [Using Angular's new improved Browserify support](http://blog.npmjs.org/post/114584444410/using-angulars-new-improved-browserify-support):

> With the recent release 1.3.14 instantaneous-browserification, Angular introduced better support for those using Browserify. Thank you to everyone who worked on that issue, and especially to Ben Clinkinbeard for his unflagging dedication in getting it in.

I prefer reading AngularJS code with CommonJS modules, so it would be cool to see AngularJS module maintainers using Browserify for modules on npm.

###DIY: JavaScript that Compiles to a Makefile

![DIY](/images/posts/diy-build.png)

I've used Gulp and Grunt to manage client-side builds, but as someone who used to write makefiles I'll admit to wishing everyone just used `make`.  Well, now maybe we can, with Vittorio Zaccaria's [DIY](http://www.vittoriozaccaria.net/diy/) (GitHub: [vzaccaria/diy](https://github.com/vzaccaria/diy), License: _BSD_, npm: [diy](https://www.npmjs.com/package/diy-build)).

Here's a sample configuration file:

{% highlight javascript %}
generateProject(_ => {
  _.collectSeq("all", _ => {
    _.collect("build", _ => {
      /* specify how to build files */
    })
    _.collect("deploy", _ => {
      /* specify how to deploy files */
    }
  })
})
{% endhighlight %}

DIY is a DSL for describing build processes.  The `generateProject` function is used to wrap calls to various methods that describe build steps.  The final makefile is generated by using [babel](https://www.npmjs.com/package/babel), so you just have to type `babel configure.js | node`.  Once you've got a makefile you can invoke the targets that were defined by calls to the `collect` method.

This module gets top marks for using Babel and for generating something that's friendly for Unix grumps like me.

###Typescript-Deferred

What if you want TypeScript-friendly promises? Typescript-Deferred (GitHub: [DirtyHairy/typescript-deferred](https://github.com/DirtyHairy/typescript-deferred), License: _MIT_, npm: [typescript-deferred](https://www.npmjs.com/package/typescript-deferred)) by Christian Speckner is a Promises/A+ implementation written in TypeScript.  The author says nobody needs another promises implementation, but this implementation is pretty cool: it has no dependencies, and fully implements the specification.

If, like me, you're a TypeScript tourist, then here's what strongly typed promises look like:

{% highlight javascript %}
// A promise for a number that resolves to 10
var promise: tsd.PromiseInterface<number> =
    tsd.when<number>(10);

// A promise that adopts the state of some other thenable that wraps a value
// of type sometype
var promise: tsd.PromiseInterface<number> =
    tsd.when<sometype>(someThenable);
{% endhighlight %}

Christian suggests that this will be useful if you want to embed a small promises implementation into your own libraries.
