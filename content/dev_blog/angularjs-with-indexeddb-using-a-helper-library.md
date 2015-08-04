{
    "slug": "angularjs-with-indexeddb-using-a-helper-library",
    "date": "2013-06-24T17:02:00.000Z",
    "tags": [
        "angularjs",
        "javascript",
        "indexeddb",
        "webdevelopment",
        "webdev"
    ],
    "title": "AngularJS with IndexedDB using a helper library",
    "publishdate": "2013-06-24T17:02:00.000Z"
}AngularJS with IndexedDB using a helper library
===============================================




<p>In working on my podcast app, <a href="http://podrad.io" target="_blank">podrad.io</a>, I wanted to be able to persist data, so that users could keep their subscriptions between visits, without losing anything, and without my needing to do anything server-side. When I was writing podrad.io, however, my goal was to get something written as quickly as I could, get it out, and then iterate on it as I have time. With those goals in mind, I used LocalStorage for persistence, it was simple, and did the job. Now, I&rsquo;m going back and looking at what needs to be added or fixed, and the data persistence is top on my list.</p>

<h2>HTML5 client-side data persistence</h2>

<p>While I&rsquo;m sure most of you know this already, there are three competing APIs for doing data persistence client-side in HTML5, LocalStorage, IndexedDB and WebSQL. LocalStorage is supported everywhere, but it only allows the mapping of strings, which means that 
you are serializing and de-serializing data whenever you want to touch the disk. That&rsquo;s fine for really small amounts of data, but it won&rsquo;t scale, unless you want to design around this odd constraint.  WebSQL and IndexedDB both have support in various browsers, however neither of them are supported everywhere, and WebSQL is being deprecated in favor of IndexedDB. WebSQL is a limited SQL implementation that uses sqlite3 (again, it&rsquo;s not part of the HTML5 spec, so it&rsquo;s a non-starter). IndexedDB is an indexed NoSQL object store, meaning that you create a data-store and put your objects there. As I mentioned, IndexedDB isn&rsquo;t fully supported yet, however there is a <a href="http://www.html5rocks.com/en/tutorials/indexeddb/todo/" target="_blank">polyfill lib that helps here</a>.</p>

<h2>IndexedDB</h2>

<p>If you look at <a href="http://www.html5rocks.com/en/tutorials/indexeddb/todo/" target="_blank">some code that uses IndexedDB</a>, one of your first reactions will likely be, &lsquo;gee, that&rsquo;s an unnecessarily verbose API!&rsquo; That was my first reaction too. My second reaction was that the code linked above doesn&rsquo;t work, fantastic. As I was googling around for a fix, I stumbled upon <a href="https://github.com/jensarps/IDBWrapper" target="_blank">IDBWrapper</a>, a wrapper class for IndexedDB. The API for the wrapper is quite simple, and easy to understand; to me, it&rsquo;s what the IndexedDB API should have looked like - or at least closer to that.</p>

<h2>Sample app</h2>

<p>I wrote a sample todo list app (in AngularJS) that uses some of IDBWrapper&rsquo;s functionality.</p>

<h3>Generic bits, index.html and app.js</h3>

<p>Starting with the index file:</p>

<script src="https://gist.github.com/emil10001/5851271.js?file=index.html"></script><p>I cut out all of the redundant, and auto-generated bits, and just left this with the bare-bones for us to look at. There are a couple of interesting pieces here. First, I used Font Awesome for the icon font, I&rsquo;ve found that to be easier to deal with that Bootstrap&rsquo;s font, and it scales better. Next, you&rsquo;ll see that I included the IndexedDBShim, and according to the documentation there, that&rsquo;s all that I should need to do to use it. However, I&rsquo;m not sure if IDBWrapper will use it or not, I need to look into that. Finally, I installed IDBWrapper with Bower, so it&rsquo;s in my components directory.</p>

<p>App routing in app.js:</p>

<script src="https://gist.github.com/emil10001/5851271.js?file=app.js"></script><p>No surprises here, just your standard app.js file.</p>

<h3>Main view and controller</h3>

<script src="https://gist.github.com/emil10001/5851271.js?file=main.html"></script><p>We&rsquo;ve got two basic parts the input and the display for our todo list. The input is a basic form that populates <code>$scope.itemname</code> in our controller, and calls <code>$scope.addItem()</code> when submitted. The view part simply iterates over the items array and displays the contents of each object, using <code>ng-repeat</code>. We also have a delete button that calls <code>$scope.deleteItem(id)</code> on the list item that we want to remove. Pretty standard stuff.</p>

<script src="https://gist.github.com/emil10001/5851271.js?file=main.js"></script><p>The controller handles those calls for <code>addItem</code> and <code>deleteItem</code>, as well as does our querying of the data store. Very simply, we start out by initializing the data store, then getting any existing objects within it, and update the view with those. We need to call <code>$scope.$apply()</code> in the <code>getItemsSuccess</code> callback because otherwise, our view does not get updated. <a href="http://jimhoskins.com/2012/12/17/angularjs-and-apply.html" target="_blank">Here&rsquo;s a write-up</a> on why that is. As you can see, the IDBWrapper API is quite straight-forward, and easy to use.</p>

<h2>Conclusion</h2>

<p>That was all pretty simple stuff, I liked the wrapper, and will probably use it in the future. One thing that I&rsquo;m not sure on is whether or not it will use the shim if it&rsquo;s available, and I&rsquo;ll be looking into that shortly. <a href="https://github.com/emil10001/angularjs-indexeddb-sample" target="_blank">Full code available on GitHub</a>.</p>
