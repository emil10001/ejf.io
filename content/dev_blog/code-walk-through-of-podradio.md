{
    "slug": "code-walk-through-of-podradio",
    "date": "2013-06-15T23:25:00.000Z",
    "tags": [
        "angularjs",
        "html5",
        "ajax",
        "javascript",
        "webdevelopment",
        "webdev",
        "yeoman",
        "podcast"
    ],
    "title": "Code walk-through of podrad.io",
    "publishdate": "2013-06-15T23:25:00.000Z"
}Code walk-through of podrad.io
==============================




<p>Over the last couple of weeks, I&rsquo;ve been learning about AngularJS, and I&rsquo;ve had this idea of building out a podcast client web-app that I could use to consume podcasts while on my Chromebook Pixel. For those who don&rsquo;t know, <a href="http://angularjs.org/" target="_blank">AngularJS is a client-side JavaScript framework</a> that intends to simplify Ajax style web-apps. If you haven&rsquo;t taken a look at it before, you might want to head over to their site and click through the <a href="http://docs.angularjs.org/tutorial" target="_blank">tutorial</a> or at least look at the <a href="http://docs.angularjs.org/misc/started" target="_blank">getting started page</a>. In this post, I&rsquo;m going to walk through how I did this, with a focus on the code. I&rsquo;m not going to post a step-by-step, but I&rsquo;ll attempt to give you enough code to be able to figure out what&rsquo;s going on.</p>

<h2>Why?</h2>

<p>I listen to a lot of podcasts, and have been disappointed that there wasn&rsquo;t something in the Chrome web store for this already. While I know that there are a couple of web-app clients out there, I haven&rsquo;t found one so far that I&rsquo;ve really liked. So, I decided to build my own.</p>

<p>After about 24 hours of work, I got to a point where I thought that I could <a href="http://podrad.io/" target="_blank">push it up to my server without getting hate-mail</a>. I deployed it and it&rsquo;s going ok so far. I&rsquo;ve been able to use it a little bit to listen to a couple of episodes, and it seems to be working for me.</p>

<h2>Prerequisites</h2>

<p>You&rsquo;ll need to have a few things installed before getting started:</p>

<ul><li>NodeJS (and NPM)</li>
<li><a href="http://yeoman.io/" target="_blank">Yeoman</a></li>
<li>Grunt</li>
<li>Bower</li>
<li><a href="http://compass-style.org/install" target="_blank">Compass</a> (requires Ruby and Gems)</li>
</ul><p>Follow the instructions on the <a href="http://yeoman.io/" target="_blank">Yeoman project site</a> for getting the prerequisites installed.</p>

<h2>Generating the project</h2>

<p>Again, check out the <a href="http://yeoman.io/" target="_blank">Yeoman project site</a>, but basically, all you need to do is <code>yo angular:app</code> inside a directory to have it generate a new project for you. However, for the purposes of this post, just go ahead and clone the <a href="https://github.com/emil10001/podrad.io" target="_blank">podrad.io github repo</a>. (At the time of this writing, the current commit is <a href="https://github.com/emil10001/podrad.io/tree/10f18e9ee4bf6ed1d4a0b48b969c0e981164ce49" target="_blank">10f18e9</a>.)</p>

<h2>Onto the code!</h2>

<p>At a very high level, there will be a couple of files that lend the most structure to the project, those are the index.html and app.js files.</p>

<h3>index.html</h3>

<p>The index.html file is just what you&rsquo;d expect it to be, it&rsquo;s the starting point of the app. Here&rsquo;s the index.html file from podrad.io with all of the auto-generated code stripped out, and just the important changes left in:</p>

<script src="https://gist.github.com/emil10001/5788653.js?file=index.html"></script><p>The first thing that you&rsquo;ll notice is that I added a CDN source for <a href="http://twitter.github.io/bootstrap/" target="_blank">Bootstrap</a> and <a href="http://fortawesome.github.io/Font-Awesome/" target="_blank">Font Awesome</a>. I did this for a couple of reasons. First, you sort of get Bootstrap by default when you generate an AngularJS project with Yeoman and accept all the defaults. It&rsquo;s also quite useful for providing some basic styling, especially if you&rsquo;re like me and are allergic to CSS (not that I dislike it, I&rsquo;m just bad at it). Font Awesome is used because it provides vector images, and I have a really high res screen. CDNs are used for a couple reasons, first, it&rsquo;s better for client caching. Second, because I couldn&rsquo;t figure out how to properly compile my scss files, and just wanted to get things moving along.</p>

<p>Next in the index file are a couple of AngularJS directives, ng-app and ng-view. These tell AngularJS about the scope, and what it&rsquo;s supposed to control, ng-view is controlled by the routing in app.js.</p>

<p>You can also see that I added a few extra libraries to help me along. One for angular-bootstrap, which provides the accordion effect on the single pod view. Another called X2JS, which parses XML into JSON format. And, finally, an MD5 library, so that I can easily make hashes of things. That&rsquo;s basically it for the index.</p>

<h3>app.js</h3>

<p>Moving on to app.js, mine is very simple, there&rsquo;s very little routing that needs to happen here (as of now). Basically, we just want to use the main view for everything, and while it&rsquo;s defining a controller, that controller doesn&rsquo;t actually get used. That&rsquo;s an error, and I&rsquo;ll probably refactor that out.</p>

<script src="https://gist.github.com/emil10001/5788653.js?file=app.js"></script><h3>Main view</h3>

<p>The main.html file simply pulls in additional views and defines controllers for those views. It also declares that the second view, onepod.html is not visible if there is not a podId in the scope.</p>

<script src="https://gist.github.com/emil10001/5788653.js?file=main.html"></script><h3>Pods</h3>

<p>In pods.html, we can see a couple of input fields being defined and bound to models in the scope. When the button is pressed, the addPod function in the controller is called, defined by <code>ng-submit</code>. Then, in the next chunk, we display all of the podcasts that the user has subscribed to, and provide a couple of simple actions that the user can take (either select or delete).</p>

<p>To generate the list of podcasts, use <code>ng-repeat="item in Array"</code>. Filtering the list is easy too, I&rsquo;m filtering based on the contents of the search box, and also sorting the list on the fly.</p>

<script src="https://gist.github.com/emil10001/5788653.js?file=pods.html"></script><p>In the controller, all of the work is done to populate those models with content, or handle the content coming in from the view when the user wants to add a new podcast. We also provide a few default podcasts for casual viewers, just so that they have something to look at if they find the site and are utterly confused.</p>

<script src="https://gist.github.com/emil10001/5788653.js?file=pods.js"></script><p>One more thing to note here is that I&rsquo;m using the LocalStorage HTML5 API to persist data. I reviewed and implemented about a half dozen different solutions before choosing this one. I found issues with nearly all of the other solutions that I had tried, most of them being either lack of documentation, lack of development or poor browser support. While it may not scale well, LocalStorage was what I needed to get me up and running.</p>

<h3>OnePod</h3>

<p>When the user selects a podcast in the pods view, the onepod view comes alive and displays some basic information about the podcast, along with a handful of the most recent episodes. Users can change the number of episodes they see by entering a number in the input box and clicking the reload button.</p>

<p>I mentioned that I was using the accordion element from angular-bootstrap, and you can see that here. What&rsquo;s also done here is the parsing of the podcast feed data. Since I&rsquo;m able to iterate over an array, it is easy to simply pull out the bits that need to be displayed from the JSON provided. This means that I don&rsquo;t need to write a separate parser, I can do that straight in the view.</p>

<script src="https://gist.github.com/emil10001/5788653.js?file=onepod.html"></script><p>This script is a bit more interesting, and uses a couple of libraries, and has an interesting work-around to a problem I ran into. First, X2JS, podcasts are defined as an XML standard, related to RSS. JSON is a bit easier to manipulate than XML using JavaScript, so I decided to use X2JS to do the conversion for me.</p>

<p>When I started out trying to parse the feeds, I was making the request straight to the podcast feed url, and parsing it from there. This worked fine for the first feed that I tried, since the HTTP response headers were set properly, allowing cross-site requests. Another provider, however, had failed to set that header, and my requests were failing. After searching for a bit, I landed on a service that Google offers to make this sort of request easy, and AngularJS provides some hooks to make it even easier. The strategy is that you call the Google service with your feed location as an encoded uri argument, and also provide a standard callback to Google. In AngularJS, you can use the JSONP HTTP request, and everything works well.</p>

<p>One cool feature about the Google service is that it can translate the XML response into JSON for you. However, podcasts contain certain elements that the Google service didn&rsquo;t handle, namely their media. So, I had to get the results as XML and then do the conversion with X2JS.</p>

<script src="https://gist.github.com/emil10001/5788653.js?file=onepod.js"></script><h3>Gotchas</h3>

<p>I was having an issue trying to compile this app for deployment, something about <code>.jshintrc</code> getting an end of file error. The problem was that Yeoman does not to auto-generate a .jshintrc, even though Grunt requires that file for compilation. I eventually got a working .jshintrc built, however it doesn&rsquo;t seem to be correct, or it might be ok, and my Grunt config file is screwed up. The issue that I&rsquo;m seeing is that my scss stylesheets are not being compiled into CSS for deployment.</p>

<script src="https://gist.github.com/emil10001/5788653.js?file=jshintrc.js"></script><h2>Conclusion</h2>

<p>I think that about covers it, at least for the code aspect. I&rsquo;m planning to continue building out this project, hopefully as I discover issues as I use it. Overall, I really enjoyed building this, and while there were some pain points along the way, I&rsquo;ll be using AngularJS again in the future.</p>
