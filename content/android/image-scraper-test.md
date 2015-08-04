{
    "slug": "image-scraper-test",
    "date": "2013-03-04T03:23:47.000Z",
    "tags": [],
    "title": "Image scraper test",
    "publishdate": "2013-03-04T03:23:47.000Z"
}Image scraper test
==================




<p>The following blog post was written over a year ago. The app discussed, Pin’d, has since been completed, published, and removed from the Google Play Store. I have decided not to modify the post, because it still contains lots of useful information. I also <a href="https://github.com/emil10001/ImageScraper" target="_blank">open sourced</a> this image scraper since the original post.</p>

<p>I&rsquo;m excited, I just got my image scrapper up and running for my Pinterest Android app, Pin&rsquo;d. I wrote it as a separate project, and the coding was a couple fast hours yesterday at the coffee shop, and then an hour debugging this morning. It works well enough to dump it into my Pin&rsquo;d project, and then I will simply chop it up and make it a bit more robust and stable. Currently, it only builds images based off of a full path, and also only for images that are within an img tag. I will probably keep the tag requirement, but I might be able to do something about using relative paths (really, it&rsquo;s written in, I just need to test it). Oh, and if you&rsquo;re really confused about what I&rsquo;m talking about because I started this blog post mid-way through my stream of consciousness, I should probably give a bit more context. Basically, the experience for sharing content from the web in my Pin&rsquo;d app will be the default &lsquo;Share&rsquo; action from your normal web browser. Go to a web page you want to share, click Menu -&gt; Share and select Pin&rsquo;d from the list. Depending on what Pinterest&rsquo;s API offers, I might do the same for allowing uploads from the Gallery or simply snapping a photo with the camera directly (this, too is already written). Anyways, once you click select Pin&rsquo;d as your share action, it will take you to my app which will scrape the images off of the website and allow you to select one that you&rsquo;d like to share. Select the image and it will build the request and send the information to Pinterest. See below the fold for a couple of images.</p>

<p><img src="http://files.feigdev.com/imgscrape_1_share.png" alt="Screenshot 1"/><img src="http://files.feigdev.com/imgscrape_2_view.png" alt="Screenshot 2"/></p>
