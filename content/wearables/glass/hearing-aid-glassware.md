{
    "slug": "hearing-aid-glassware",
    "date": "2014-04-13T23:37:00.000Z",
    "tags": [
        "googleglass",
        "Google Glass",
        "glassware",
        "hearing aids"
    ],
    "title": "Hearing Aid Glassware",
    "publishdate": "2014-04-13T23:37:00.000Z"
}Hearing Aid Glassware
=====================




<p><img src="https://31.media.tumblr.com/b43829862c7e14631ebbeb76e401013e/tumblr_inline_n3zuvas4Nu1sq0x3a.jpg" alt=""/></p>

<blockquote>
  <p><small><a href="http://www.soundadvicehearing.com/news/what-happens-when-there-lack-testing-and-education-hearing-industry" target="_blank">Image source</a> </small></p>
</blockquote>

<p>Last night, I received an interesting email from <a href="https://plus.google.com/u/0/101818316891399335943/about" target="_blank">Ajay Kohli</a>. I met Ajay at the CodeChix Glass workshop back in February. He&rsquo;s a Med Student, working at Kaiser, as well as a Glass Explorer. When I met him, he was very excited about the possibilities for using Glass in hospitals to assist physicians with all sorts of things, in particular during surgery. Anyways, his email mentioned that he was working with a cochlea surgeon, and they were discussing a rather interesting application for Google Glass. Apparently, bone conduction is a viable method to enable some people with hearing loss to hear, and there is currently a class of hearing aids that uses bone conduction. What&rsquo;s interesting is that Google Glass uses bone conduction with its built-in speaker. Ajay was wondering if I knew of any Glassware or source code that could enable <a href="http://www.futuristspeaker.com/2013/04/how-google-glass-will-disrupt-the-hearing-aid-industry/" target="_blank">Glass to be used as a hearing aid</a>.</p>

<p>I looked into it a bit, and initially found an old project that didn&rsquo;t work. I figured that it probably wouldn&rsquo;t be much effort to write something to do this, and I was not wrong. Here&rsquo;s the entire code (sans resources) for my hearing aid Glassware:</p>

<p><script src="https://gist.github.com/emil10001/10606008.js"></script></p>

<p>You can check out the full project, and download the apk to try it out for yourself <a href="https://github.com/emil10001/GlassHearingAid" target="_blank">on GitHub</a>.</p>

<h1>Update</h1>

<p><a href="https://plus.google.com/+DavidCallawayi3/about" target="_blank">David Callaway</a> had his father, who is hard of hearing and uses hearing aids, test this out. <a href="https://plus.google.com/110693175237378228684/posts/CrMR4bc6ucQ" target="_blank">According to him, it actually works!</a></p>

<h1>Update 2</h1>

<p>Ajay Kohli used this for actual clinical trials! (I&rsquo;m pretty thrilled that something that I did was studied scientifically.) Looks like Glass won&rsquo;t be replacing hearing aids any time soon:</p>

<blockquote>
  <p>We did a functional gain test (hearing test with and without the glass on) yesterday on a bilateral mild conductive hearing loss.  No <em>significant</em> improvement in hearing with the glass. We did a unilateral conductive hearing loss speech in noise test.  No major improvement with glass as well.</p>
  
  <p>At this point the limitations for glass as a hearing assistance device seem to be 1) available power (gain of making the bone conductor vibrate as fast as possible) and 2) possible processing power, if that processed speech cannot catch up to the live stimulus. So in a controlled audiology setting, these variables would have to be addressed to achieve hearing aid potential for glass.</p>
</blockquote>
