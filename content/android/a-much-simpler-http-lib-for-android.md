{
    "slug": "a-much-simpler-http-lib-for-android",
    "date": "2013-04-27T23:50:59.000Z",
    "tags": [
        "android",
        "android development",
        "programming"
    ],
    "title": "A much simpler HTTP lib for Android",
    "publishdate": "2013-04-27T23:50:59.000Z"
}A much simpler HTTP lib for Android
===================================




<p>A couple of days ago, I wrote up a post about Square&rsquo;s OkHttp library. That was <strong>ok</strong> (pun intended), but <a href="http://kevinsawicki.github.io/http-request/" target="_blank">kevinsawicki&rsquo;s HttpRequest library</a> is much simpler.</p>

<p>This is an excerpt from a demo app using HttpRequest, and it does the exact same thing as my OkHttp demo app:</p>

<script src="https://gist.github.com/emil10001/5475184.js" type="text/javascript"></script><p>Compare that with OkHttp&rsquo;s version:</p>

<script src="https://gist.github.com/emil10001/5453576.js?file=OkDownload.java" type="text/javascript"></script><p>Notice the difference? HttpRequest only needs one line to do it&rsquo;s thing, OkHttp needs a bit more. Now, I believe that OkHttp is intended to be a bit more general purpose, and it&rsquo;s still pre-release, and without documentation. So, to be fair to OkHttp, I&rsquo;m sure that there are things that it does really well, and I also think that it may be more performant than HttpRequest, at least based on the brief presentation that I heard on OkHttp.</p>

<p>Still, if I need to build something quickly, especially just for demonstration purposes, I really appreciate the brevity of HttpRequest.</p>

<p><a href="https://github.com/emil10001/HttpRequestExampleAndroid" target="_blank">Check out the full demo app on github</a>. You can compare that with the <a href="https://github.com/emil10001/OkHttpAndroidExample" target="_blank">demo that I did for OkHttp</a> a few days ago. The <em>only</em> difference is the http library used, everything else is copied.</p>
