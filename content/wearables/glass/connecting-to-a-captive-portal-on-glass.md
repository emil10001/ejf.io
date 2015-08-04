{
    "slug": "connecting-to-a-captive-portal-on-glass",
    "date": "2013-10-23T02:26:00.000Z",
    "tags": [
        "glass",
        "googleglass"
    ],
    "title": "Connecting to a captive portal on Glass",
    "publishdate": "2013-10-23T02:26:00.000Z"
}Connecting to a captive portal on Glass
=======================================




<p>I figured out a work-around for wifi networks that have a specific type of captive portal. I&rsquo;m actually at an Informal Glass Office Hours right now at the <a href="http://www.hackerdojo.com/" target="_blank">Hacker Dojo</a>, who happen to have a captive portal. The Hacker Dojo&rsquo;s wifi is set up such that you can connect to their network, then when you visit the Dojo&rsquo;s web page, it redirects you to the captive portal, where all you need to do is click the &lsquo;agree&rsquo; button. If your captive portal requires you to log in or type something, it won&rsquo;t work.</p>

<p>If you happen to be at the Hacker Dojo, or a similar place where the captive portal is easy to reach, and only requires you to click an 'agree&rsquo; button, here are the steps to take:</p>

<ol><li>While tethered, or on your own connection, search for &ldquo;hacker dojo&rdquo;, make sure that the web page shows up in the results</li>
<li>Connect to the wifi</li>
<li>Visit the &ldquo;hacker dojo&rdquo; search results bundle, and open the hackerdojo.com card, then open webpage</li>
<li>Scroll down to the 'agree and continue&rsquo; button, and click it</li>
<li>&hellip;</li>
<li>Profit!</li>
</ol><p>Replace &ldquo;hacker dojo&rdquo; with whatever network you are trying to connect to. Some networks will redirect you from any page, so the initial search doesn&rsquo;t matter as much. Hope this helps!</p>

<h2>Update</h2>

<p>The redirect should work for any non-HTTPS page. So, as long as you have a search result for an insecure page, you should be good to go. I didn&rsquo;t realize that because Gmail, Google, G+ and Facebook are all HTTPS, and that&rsquo;s where I usually start my browsing.</p>
