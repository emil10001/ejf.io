{
    "slug": "aws-s3-utils-for-nodejs",
    "date": "2014-03-20T18:11:46.000Z",
    "tags": [
        "aws s3 nodejs"
    ],
    "title": "AWS S3 utils for node.js",
    "publishdate": "2014-03-20T18:11:46.000Z"
}AWS S3 utils for node.js
========================




<p>I just published <a href="https://www.npmjs.org/package/s3-utils" target="_blank">my first package on npm</a>! It&rsquo;s a helper for S3 that I wrote. It does three things, it lists your buckets, gets a URL pair with key, and deletes media upon request.</p>

<p>The URL pair is probably the most important, because this allows you to have clients that put things on S3 without those clients having any credentials. They can simply make a request to your sever for a URL pair, and then use those URLs to put the thing in your bucket, as well as a public GET URL, so that anyone can go get it out.</p>

<pre><code>var s3Util = require('s3-utils');
var s3 = new s3Util('your_bucket');
var urlPair = s3.generateUrlPair(success);
/**
    urlPair: {
        s3_key: "key",
        s3_put_url: "some_long_private_url",
        s3_get_url: "some_shorter_public_url"
    }
*/
</code></pre>

<p>Deleting media from your S3 bucket:</p>

<pre><code>s3.deleteMedia(key, success);
</code></pre>

<p>Or just list your buckets:</p>

<pre><code>s3.listBuckets();
</code></pre>

<p>I had previously written about <a href="http://www.recursiverobot.com/post/75281512146/demos-using-aws-with-node-js-and-an-angularjs-frontend#s3" target="_blank">using the AWS SDK for node.js here</a>. It includes some information about making sure that you have the correct permissions set up on your S3 bucket, as well as how to PUT a file on S3 using the signed URL.</p>
