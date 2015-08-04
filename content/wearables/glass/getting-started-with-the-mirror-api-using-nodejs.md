{
    "slug": "getting-started-with-the-mirror-api-using-nodejs",
    "date": "2013-08-04T18:17:58.000Z",
    "tags": [
        "google glass",
        "googleglass",
        "mirror",
        "mirrorapi",
        "nodejs",
        "web development",
        "programming"
    ],
    "title": "Getting started with the Mirror API using node.js",
    "publishdate": "2013-08-04T18:17:58.000Z"
}Getting started with the Mirror API using node.js
=================================================




<p>I recently accepted an invite to purchase an Explorer Edition of Google Glass. Well, actually, the company I work for accepted (man, I love working for <a href="http://talkray.com/" target="_blank">TiKL, Inc.</a>), and I&rsquo;ve been the one working on everything Glass at our office. (In case you&rsquo;re wondering, a write-up on my thoughts about Glass so far is in the works.) So far, my efforts have been focused on building apks to run on Glass, which is incredibly easy if you&rsquo;re and Android hacker like myself. What isn&rsquo;t so easy for someone mainly focused on Android is trying to figure out the <a href="https://developers.google.com/glass/overview" target="_blank">Mirror API</a>.</p>

<p>The Glass team has put out a number of quick-start demos for working with the Mirror API, with a focus on getting over the OAuth2 hump. They demonstrate this in a number of languages, but they haven&rsquo;t put out anything for node.js, which is what I use whenever I need a quick demo server for something. What Google has provided, is a <a href="https://github.com/google/google-api-nodejs-client" target="_blank">github library</a> that provides some simple mechanisms to interact with <strong>all</strong> of their services. It&rsquo;s pretty awesome, and it allows you to pretty easily inspect the JSON data that is returned as the <code>client</code> object, and look at what&rsquo;s available.</p>

<p>I figured out how to do the Mirror stuff by looking at the Google+ example in the googleapis github, implementing that first, so that I could get the authentication working. Then, making a couple of educated guesses about what I might need to change to get the Mirror API endpoints, along with looking at the Mirror documentation for Java and Python. Once I had G+ authentication up, moving it over to getting authenticated with the Mirror API was really easy, and getting valid requests written was not a big leap from there.</p>

<p>I still do not have much done, my main goal was to simply get node.js talking to Glass, which I did. Speaking of, <a href="https://github.com/emil10001/glass-mirror-nodejs-auth-demo" target="_blank">here&rsquo;s the github repo for my demo</a>. Again, all this does right now is allows a user to authenticate, pushes the message &lsquo;hello world&rsquo; to Glass, and requests the user&rsquo;s timeline for the registered app.</p>

<p><img src="https://developers.google.com/glass/images/glass-screens/hello_world_320.png" alt="Hello world on Glass"/></p>

<h2>Instructions</h2>

<p>Follow the instructions in the <a href="https://developers.google.com/glass/overview" target="_blank">Mirror API</a> documentation for registering a new app. It&rsquo;s basically the same flow for starting any new project using a Google API. Just follow the instructions, it&rsquo;s easy. Once you&rsquo;ve registered, pull down <a href="https://github.com/emil10001/glass-mirror-nodejs-auth-demo" target="_blank">my repository</a> and run <code>npm install</code>. This will install the googleapis npm package, as well as express.js if you need it.</p>

<p>Next, copy <code>sample_config.json</code> to <code>config.json</code>, and replace the values with those from the Google Developer&rsquo;s console.</p>

<script src="https://gist.github.com/emil10001/6151154.js?file=config.json"></script><p>Run <code>node app.js</code>. Finally, visit the app in your browser, and authenticate with Google. You should get a push to your Glass with the text 'Hello world&rsquo;.</p>

<h2>Digging in a bit</h2>

<p>Now that you&rsquo;ve seen this working, let&rsquo;s look at some code. This is simply chunks from the <code>app.js</code> file, so if you want to just look at the whole thing at once, <a href="https://github.com/emil10001/glass-mirror-nodejs-auth-demo/blob/master/app.js" target="_blank">here it is</a>.</p>

<p>First up, we&rsquo;ve got some things to require, the <code>googleapis</code>, and the related oauth stuff.</p>

<script src="https://gist.github.com/emil10001/6151154.js?file=app_requires.js"></script><p>We should look at the routes involved here first, before we look at anything specific to the API. Without the routes, you don&rsquo;t really know what&rsquo;s being called, and where the most important parameters come from, so here are the routes:</p>

<script src="https://gist.github.com/emil10001/6151154.js?file=routes.js"></script><p>As you can see, there are only two routes, the root, and the oauth2 callback routes. We start by looking to see if we already have a token, if they don&rsquo;t, we send them to Google to go request one. Otherwise, we use the token we have. If they didn&rsquo;t have a token, they&rsquo;ll return to us on the route oauth2callback (unless you change that route on your config), and we take the code provided in the request data to parse out their token.</p>

<pre><code>**WARNING**: this is not intended for a multi-user case, just for you! Don't use this unmodified!
</code></pre>

<p>We&rsquo;ll provide a couple of generic callbacks, for simplicity. Whenever you&rsquo;re unsure of what to do next, or aren&rsquo;t sure if something&rsquo;s working, dump whatever data that you&rsquo;ve got.</p>

<script src="https://gist.github.com/emil10001/6151154.js?file=generic_callbacks.js"></script><p>Alright, first thing&rsquo;s first, we need to get ourselves a token. This method gets called when the user returns from giving us permission to connect to their account, and will use the provided oauth2Client to pull the token out of the code that is returned to us. We then store that token in the oauth2Client.</p>

<script src="https://gist.github.com/emil10001/6151154.js?file=grabToken.js"></script><p>Once we have a token, we can do stuff with it. First thing to do is request a client that can make requests on the Mirror API, and in the callback, we can use the returned client to make those requests.</p>

<script src="https://gist.github.com/emil10001/6151154.js?file=gotToken.js"></script><p>The following are two simple requests that you can make once you&rsquo;re authenticated and get your client object:</p>

<script src="https://gist.github.com/emil10001/6151154.js?file=insertHello.js"></script><script src="https://gist.github.com/emil10001/6151154.js?file=listTimeline.js"></script><p>I probably don&rsquo;t really need to explain what&rsquo;s going on there, it&rsquo;s incredibly simple. But, in case you like reading things, the first one inserts the text 'hello world&rsquo; into your timeline on Glass. If you&rsquo;re using this with the account that you&rsquo;ve attached Glass to, then that text will show up in your eyeball. It&rsquo;s neat. The second request simply shows the timeline items that your app has generated.</p>

<h2>Notes</h2>

<p>Currently, this is incredibly simple, and as such is completely lacking in style, structure, and functionality. This is intended to demonstrate just two things, first how to authenticate with Google for the Mirror API with OAuth2, using node.js. Second, it begins to show how you can use node.js to make requests with the Mirror API. Think of it as the quick-start for node.js.</p>

<h2>Go get the source</h2>

<p><a href="https://github.com/emil10001/glass-mirror-nodejs-auth-demo" target="_blank">Here it is again, in case you missed it, the github repo that this post was discussing.</a></p>
