{
    "slug": "going-from-google-glass-mirror-api-quick-start-to",
    "date": "2014-11-03T16:57:12.000Z",
    "tags": [
        "java",
        "python",
        "googleglass",
        "Google Glass",
        "Mirror API",
        "mirrorapi"
    ],
    "title": "Going from Google Glass Mirror API Quick Start to Product",
    "publishdate": "2014-11-03T16:57:12.000Z"
}Going from Google Glass Mirror API Quick Start to Product
=========================================================




<p>At this point, you should have either the Java or the Python Mirror API Quick Start up and running. If you do not, please <a href="http://www.recursiverobot.com/post/82000012312/a-high-level-look-at-the-mirror-api" target="_blank">check out this post</a>, and <a href="http://www.recursiverobot.com/post/82000172161/google-glass-mirror-api-java-quick-start" target="_blank">this post</a> (apologies that I don&rsquo;t have one for Python, just Java). This post is going to discuss how to use the quick start projects to begin making progress on your product.</p>

<p>I&rsquo;m going to talk about this in the context of a service that I&rsquo;m working on building up, Hearty.io. I think that it&rsquo;s important to understand the product, and the goals of the product when approaching development. I also think that it is especially important with wearables to understand your product, and how that&rsquo;s going to work with wearables.</p>

<h2>Hearty.io</h2>

<p><img src="https://raw.githubusercontent.com/emil10001/Hearty.io/master/raw_assets/icon/color_icon.png" height="400px" width="400px"/></p>

<p>Let&rsquo;s get started by taking a look at what we have for a service, understanding it, and thinking about how we can use the Mirror API to extend it and offer something useful to our users. One note, Hearty.io is currently very rough and is missing quite a bit. What we will do here, is to define a current, fictional set of functionality, things that we can assume are already working. Again, to be clear, the following is a fictionalized description fo the service, that we will use for our purposes.</p>

<p>Hearty.io is a fitness service with several components. There is an Android component, an Android Wear component, and Glassware. There is also a backend service that keeps all of the data synchronized.</p>

<h3>Android</h3>

<p>The Android app does automatic activity recognition, and tracking. It uses Google Play Services to determine what the user&rsquo;s current activity is, and then automatically tracks times when the user is active. The Android app posts a notification during times that it is tracking with the current activity, and the length of time tracked.</p>

<p>The Android app also syncs with the server, and displays data from all three sources.</p>

<h3>Android Wear</h3>

<p>The Wear app does step counting, and syncs directly to Android using the Android Wear Data APIs. The Android notifications for Activity tracking show up on the watch, and there is a Wearable action that would allow the user to &lsquo;ignore&rsquo; the current activity.</p>

<h3>Glass</h3>

<p>The Hearty.io Glassware uses the GDK and talks to a Bluetooth Heart Rate Monitor. The user launches the Glassware by saying 'Ok Glass, show my heart rate&rsquo;. When Hearty launches, if there is no device paired, it redirects into a pairing UI. Once paired, Hearty.io inserts a LiveCard that displays the users&rsquo;s current heart rate. The Glassware syncs the user&rsquo;s heart rate directly with the server.</p>

<h3>Server</h3>

<p>The server just keeps track of three things for each user, each day&rsquo;s step count, number of active minutes, and average heart rate.</p>

<h3>New Mirror functionality</h3>

<p>The most obvious thing that I can think to do with Mirror here is to send the daily summary to the user on a daily basis. We also want to display that data in a dashboard for the user.</p>

<p>There are a few things to consider here, beyond simply what we want to build. The primary consideration should be if it&rsquo;s something that makes sense, and would be a good experience on Glass. Mirror can be used to do things that don&rsquo;t result in a good experience. For example, news content is tricky to consume on Glass. Reading lots of text on a screen, or spending up to a minute listening is not ideal. WinkFeed does a great job of providing a good experience for news, mainly because of its Pocket integration. Sometimes, a clever feature can help your Glassware to go from a mediocre experience to a great one. Take a look at the <a href="https://developers.google.com/glass/design/principles" target="_blank">Glass Design Principles</a> and think about whether what you&rsquo;re doing is in line with those principles.</p>

<h3>What we are going to build</h3>

<p>Let&rsquo;s build the following, a web service that uses a fake database, displays the dummy data to the user on a web app, and then allows the user to send that data to Glass.</p>

<p>Wiring this up to our real service should be something that we know how to do, so we are not going to cover that here. We are also not going to do the work of automating this to run on a daily basis, that should be fairly straightforward.</p>

<p>I&rsquo;ll cover Java first, if you&rsquo;re more interested in Python, feel free to skip down to that section.</p>

<h2>Java</h2>

<p>I&rsquo;m going to assume that you&rsquo;ve already been introduced to the quick start code, and so I&rsquo;m going to skip the overview here. What we need to know is where we need to go in and what to modify.</p>

<h3>JSP</h3>

<p>The <code>index.jsp</code> file is what is used to generate the main web view. We are going to start by adding a section at the top with some dummy, hardcoded data. Place this above where the timeline cards are shown.</p>

<pre><code>&lt;div class="span4"&gt;
  &lt;br&gt;
  &lt;table class="table"&gt;
      &lt;tbody&gt;
      &lt;tr&gt;
          &lt;th&gt;Heart Rate&lt;/th&gt;
          &lt;td&gt;
            77
          &lt;/td&gt;
          &lt;th&gt;Steps&lt;/th&gt;
          &lt;td&gt;
              17,311
          &lt;/td&gt;
          &lt;th&gt;Active Minutes&lt;/th&gt;
          &lt;td&gt;
              89
          &lt;/td&gt;
          &lt;td&gt;
          &lt;/td&gt;
      &lt;/tr&gt;
      &lt;/tbody&gt;
  &lt;/table&gt;

&lt;/div&gt;
</code></pre>

<p>That will give us a table, without any actual data. Let&rsquo;s add a button at the top that uses the same sort of paradigm that all of the other buttons do on this page.</p>

<pre><code>&lt;form action="&lt;%= WebUtil.buildUrl(request, "/") %&gt;" method="post"&gt;
    &lt;input type="hidden" name="operation" value="insertHeartyData"&gt;
    &lt;button class="btn btn-block" type="submit"&gt;
        Insert a bundle with Hearty Data&lt;/button&gt;
&lt;/form&gt;
</code></pre>

<p>Pretty easy, right? Notice that the value of the input is <code>insertHeartyData</code>, that&rsquo;s going to be important when we go to the servlet.</p>

<p>Now, let&rsquo;s make a quick side-trip, and add our dummy database.</p>

<p><strong>FakeDatabase</strong>:</p>

<pre><code>public class FakeDatabase {
    private static final ConcurrentHashMap&lt;String, HeartyData&gt; heartyData = new ConcurrentHashMap&lt;String, HeartyData&gt;();

    private static Random rand = new Random(System.currentTimeMillis());

    public static void generateFakeUserData(String userId) {
        HeartyData data = new HeartyData();
        data.activeMinutes = rand.nextInt(110);
        data.stepCount = rand.nextInt(25000);
        data.heartRate = rand.nextInt(50) + 50;
        heartyData.put(userId, data);
    }

    public static HeartyData getUserData(String userId) {
        return heartyData.get(userId);
    }

    private FakeDatabase(){ // not allowed to instantiate }
}
</code></pre>

<p>Ok, now that we have our database, we have to get data in there, so what we&rsquo;ll do for that is to add the following to the NewUserBootstrap:</p>

<pre><code>public static void bootstrapNewUser(HttpServletRequest req, String userId) throws IOException {
    // snip ...

    FakeDatabase.generateFakeUserData(userId);

    // snip ...
}
</code></pre>

<p>Back to the jsp, let&rsquo;s use our &ldquo;real&rdquo; data there:</p>

<pre><code>  &lt;div class="span4"&gt;
    &lt;form action="&lt;%= WebUtil.buildUrl(request, "/") %&gt;" method="post"&gt;
        &lt;input type="hidden" name="operation" value="insertHeartyData"&gt;
        &lt;button class="btn btn-block" type="submit"&gt;
            Insert a bundle with Hearty Data&lt;/button&gt;
    &lt;/form&gt;
    &lt;br&gt;
    &lt;table class="table"&gt;
        &lt;tbody&gt;
        &lt;tr&gt;
            &lt;th&gt;Heart Rate&lt;/th&gt;
            &lt;td&gt;
              &lt;%= FakeDatabase.getUserData(userId).heartRate %&gt;
            &lt;/td&gt;
            &lt;th&gt;Steps&lt;/th&gt;
            &lt;td&gt;
                &lt;%= FakeDatabase.getUserData(userId).stepCount %&gt;
            &lt;/td&gt;
            &lt;th&gt;Active Minutes&lt;/th&gt;
            &lt;td&gt;
                &lt;%= FakeDatabase.getUserData(userId).activeMinutes %&gt;
            &lt;/td&gt;
            &lt;td&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;/tbody&gt;
    &lt;/table&gt;
&lt;/div&gt;
</code></pre>

<p>That about does it for the viewing side, let&rsquo;s try to make this do something.</p>

<h3>Servlets</h3>

<p>The quick start uses a Servlet for handling all of the <code>POST</code> requests from the web page, as well as all of the subsequent Mirror requests. We can copy one of the other insert methods, and modify it a bit for our purposes.</p>

<pre><code>if (req.getParameter("operation").equals("insertHeartyData")) {
    LOG.fine("Inserting Hearty Timeline Item");

    String bundleId = String.valueOf(System.currentTimeMillis());

    TimelineItem timelineItem = new TimelineItem();
    Attachment bundleCover = new Attachment();

    String imgLoc = WebUtil.buildUrl(req, "/static/images/hearty_640x360.png");

    URL url = new URL(imgLoc);
    bundleCover.setContentType("image/png");

    timelineItem.setText("Hearty.io");
    timelineItem.setBundleId(bundleId);
    timelineItem.setIsBundleCover(true);

    TimelineItem timelineItemHeart = new TimelineItem();
    timelineItemHeart.setText("Heart Rate: " + FakeDatabase.getUserData(userId).heartRate);
    timelineItemHeart.setBundleId(bundleId);

    TimelineItem timelineItemSteps = new TimelineItem();
    timelineItemSteps.setText("Steps: " + FakeDatabase.getUserData(userId).stepCount);
    timelineItemSteps.setBundleId(bundleId);


    TimelineItem timelineItemActivity = new TimelineItem();
    timelineItemActivity.setText("Active minutes: "  + FakeDatabase.getUserData(userId).activeMinutes);
    timelineItemActivity.setBundleId(bundleId);

    // Triggers an audible tone when the timeline item is received
    timelineItem.setNotification(new NotificationConfig().setLevel("DEFAULT"));

    MirrorClient.insertTimelineItem(credential, timelineItem, "image/png", url.openStream());
    MirrorClient.insertTimelineItem(credential, timelineItemHeart);
    MirrorClient.insertTimelineItem(credential, timelineItemSteps);
    MirrorClient.insertTimelineItem(credential, timelineItemActivity);

    message = "A timeline item has been inserted.";
}
</code></pre>

<p>That&rsquo;s a big chunk of code, but there are lots of repeated bits in there. Let&rsquo;s break it apart and look at it piece by piece.</p>

<p>There is a repeated pattern, that I&rsquo;m going to show here:</p>

<pre><code>TimelineItem timelineItemHeart = new TimelineItem();
timelineItemHeart.setText("Heart Rate: " + FakeDatabase.getUserData(userId).heartRate);
timelineItemHeart.setBundleId(bundleId);

MirrorClient.insertTimelineItem(credential, timelineItemHeart);
</code></pre>

<p>The above is the basics for inserting an item into the timeline. Notice that the <code>bundleId</code> is set as well, that allows this <code>Card</code> to be bundled with other Cards with the same <code>bundleId</code>.</p>

<p>You&rsquo;ll notice that the full block of code that was posted just repeats that pattern a few times to create four cards, a cover and three children.</p>

<h3>Wrapping up with Java</h3>

<p>From here, what I did was to move the Hearty.io pieces out of <code>MainServlet</code> and into both its own Servlet, and its own jsp file. When you do this, you&rsquo;ll need to modify <code>web.xml</code> to map the new path to the correct servlet. You can see the <a href="https://github.com/emil10001/mirror-quickstart-java-hearty-demo" target="_blank">final version of the code here</a>.</p>

<h2>Python</h2>

<p>As with Java, I&rsquo;m going to assume that you&rsquo;ve already been introduced to the quick start code, and so I&rsquo;m going to skip the overview here. What we need to know is where we need to go in and what to modify.</p>

<p>As a note, this is going to be very similar to the Java portion. Partly because it does the same thing, partly because the quick start code structures are fairly similar, and mostly because I&rsquo;m lazy.</p>

<h3>HTML Template</h3>

<p>The <code>index.html</code> file is what is used to generate the main web view. We are going to start by adding a section at the top with some dummy, hardcoded data. Place this above where the timeline cards are shown.</p>

<pre><code>&lt;div class="span4"&gt;
  &lt;br&gt;
  &lt;table class="table"&gt;
      &lt;tbody&gt;
      &lt;tr&gt;
          &lt;th&gt;Heart Rate&lt;/th&gt;
          &lt;td&gt;
            77
          &lt;/td&gt;
          &lt;th&gt;Steps&lt;/th&gt;
          &lt;td&gt;
              17,311
          &lt;/td&gt;
          &lt;th&gt;Active Minutes&lt;/th&gt;
          &lt;td&gt;
              89
          &lt;/td&gt;
          &lt;td&gt;
          &lt;/td&gt;
      &lt;/tr&gt;
      &lt;/tbody&gt;
  &lt;/table&gt;

&lt;/div&gt;
</code></pre>

<p>That will give us a table, without any actual data. Let&rsquo;s add a button at the top that uses the same sort of paradigm that all of the other buttons do on this page.</p>

<pre><code>&lt;form action="/hearty" method="post"&gt;
    &lt;input type="hidden" name="operation" value="insertHeartyData"&gt;
    &lt;input type="hidden" name="heart_rate" value="77"&gt;
    &lt;input type="hidden" name="step_count" value="17,311"&gt;
    &lt;input type="hidden" name="active_minutes" value="89"&gt;
    &lt;button class="btn btn-block" type="submit"&gt;
        Insert a bundle with Hearty Data&lt;/button&gt;
&lt;/form&gt;
</code></pre>

<p>Pretty easy, right? Notice that the value of the input is <code>insertHeartyData</code>, that&rsquo;s going to be important when we go to the handler. Also notice that I&rsquo;ve got several input values, these are for passing values from the web frontend to the backend. There&rsquo;s probably a way to attach this to a session, but I don&rsquo;t know Python that well, and again, lazy.</p>

<p>Now, let&rsquo;s make a quick side-trip, and add some sort of data generator.</p>

<p><strong>Hearty</strong>:</p>

<pre><code>class Hearty(object):
  def __init__(self):
    self.heart_rate = random.randrange(40,135,1)
    self.step_count = random.randrange(2000,28000,1)
    self.active_minutes = random.randrange(17,132,1)
</code></pre>

<p>Ok, now that we have our data, we have to get data in there, so what we&rsquo;ll do for that is to add the following in the get method:</p>

<pre><code>@util.auth_required
def get(self):
  # snip ...
  self.hearty = Hearty()
  self._render_template(message)
</code></pre>

<p>And our render template method:</p>

<pre><code>def _render_template(self, message=None):
  """Render the main page template."""
  template_values = {'userId': self.userid,
                     'hearty': self.hearty }
  # snip ...
</code></pre>

<p>This means that every time we refresh the page, we will see new values.</p>

<p>Back to the template, let&rsquo;s use our &ldquo;real&rdquo; data there:</p>

<pre><code>&lt;div class="span4"&gt;
    &lt;form action="/hearty" method="post"&gt;
        &lt;input type="hidden" name="operation" value="insertHeartyData"&gt;
        &lt;input type="hidden" name="heart_rate" value="{{ hearty.heart_rate }}"&gt;
        &lt;input type="hidden" name="step_count" value="{{ hearty.step_count }}"&gt;
        &lt;input type="hidden" name="active_minutes" value="{{ hearty.active_minutes }}"&gt;
        &lt;button class="btn btn-block" type="submit"&gt;
            Insert a bundle with Hearty Data&lt;/button&gt;
    &lt;/form&gt;
    &lt;br&gt;
    &lt;table class="table"&gt;
        &lt;tbody&gt;
        &lt;tr&gt;
            &lt;th&gt;Heart Rate&lt;/th&gt;
            &lt;td&gt;
                {{ hearty.heart_rate }}
            &lt;/td&gt;
            &lt;th&gt;Steps&lt;/th&gt;
            &lt;td&gt;
                {{ hearty.step_count }}
            &lt;/td&gt;
            &lt;th&gt;Active Minutes&lt;/th&gt;
            &lt;td&gt;
                {{ hearty.active_minutes }}
            &lt;/td&gt;
            &lt;td&gt;
            &lt;/td&gt;
        &lt;/tr&gt;
        &lt;/tbody&gt;
    &lt;/table&gt;

&lt;/div&gt;
</code></pre>

<p>That about does it for the viewing side, let&rsquo;s try to make this do something.</p>

<h3>Handlers</h3>

<p>The quick start uses a handler for handling all of the <code>POST</code> requests from the web page, as well as all of the subsequent Mirror requests. We can copy one of the other insert methods, and modify it a bit for our purposes.</p>

<pre><code>  def _insert_hearty_item(self):
    """Insert a Hearty.io timeline item."""
    logging.info('Inserting hearty timeline item')

    bundle_id = `random.random()`

    body = {
        'notification': {'level': 'DEFAULT'},
        'text': 'Hearty.io python',
        'isBundleCover': True,
        'bundleId': bundle_id
    }
    heart_rate = self.request.get('heart_rate')
    step_count = self.request.get('step_count')
    active_minutes = self.request.get('active_minutes')

    body_a = {
        'text': 'Heart Rate: ' + heart_rate,
        'bundleId': bundle_id
    }
    body_b = {
        'text': 'Steps: ' + step_count,
        'bundleId': bundle_id
    }
    body_c = {
        'text': 'Active Minutes: ' + active_minutes,
        'bundleId': bundle_id
    }

    media_link = util.get_full_url(self, "/static/images/hearty_640x360.png")
    resp = urlfetch.fetch(media_link, deadline=20)
    media = MediaIoBaseUpload(
        io.BytesIO(resp.content), mimetype='image/png', resumable=True)

    # self.mirror_service is initialized in util.auth_required.
    self.mirror_service.timeline().insert(body=body, media_body=media).execute()
    self.mirror_service.timeline().insert(body=body_a).execute()
    self.mirror_service.timeline().insert(body=body_b).execute()
    self.mirror_service.timeline().insert(body=body_c).execute()
    return  'A timeline item has been inserted.'
</code></pre>

<p>That&rsquo;s a big chunk of code, but there are lots of repeated bits in there. Let&rsquo;s break it apart and look at it piece by piece.</p>

<p>There is a repeated pattern, that I&rsquo;m going to show here:</p>

<pre><code>heart_rate = self.request.get('heart_rate')
body_a = {
    'text': 'Heart Rate: ' + heart_rate,
    'bundleId': bundle_id
}
self.mirror_service.timeline().insert(body=body_a).execute()
</code></pre>

<p>The above is the basics for inserting an item into the timeline. Notice that the <code>bundleId</code> is set as well, that allows this <code>Card</code> to be bundled with other Cards with the same <code>bundleId</code>.</p>

<p>You&rsquo;ll notice that the full block of code that was posted just repeats that pattern a few times to create four cards, a cover and three children.</p>

<p>The only other thing going on here is adding the image to the bundle cover:</p>

<pre><code>media_link = util.get_full_url(self, "/static/images/hearty_640x360.png")
resp = urlfetch.fetch(media_link, deadline=20)
media = MediaIoBaseUpload(
    io.BytesIO(resp.content), mimetype='image/png', resumable=True)

# insert the cover body with the media into the timeline
self.mirror_service.timeline().insert(body=body, media_body=media).execute()
</code></pre>

<h3>Wrapping up with Python</h3>

<p>From here, what I did was to move the Hearty.io pieces out of <code>main_handler</code> and into both its own handler, and its own template html file. When you do this, you&rsquo;ll need to modify <code>main</code> to map the new routes to the correct handler. You can see the <a href="https://github.com/emil10001/mirror-quickstart-python-hearty-demo" target="_blank">final version of the code here</a>.</p>

<h2>Moving forward</h2>

<p>This post should have given you some idea of how to take the Mirror API Quick Start and start taking steps towards morphing it into your own project. You will obviously want to have your own service, with your own data, and a real database. The idea here was to start turning knobs and seeing what happens when we change things. Really, the same approach can be taken with most sample code, where you start with what&rsquo;s given, and poke at it, while reading the documentation, to figure out how to do what <em>you</em> want to do.</p>
