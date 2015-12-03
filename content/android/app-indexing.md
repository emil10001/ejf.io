+++
date = "2015-12-03T10:00:03-05:00"
title = "What is App Indexing?"
images = ["https://storage.googleapis.com/ejf-io/auto-complete.png"]
+++

<iframe width="560" height="315" src="https://www.youtube.com/embed/kM1ZwBK2fUA" frameborder="0" allowfullscreen></iframe>

### And, what's the difference between App Indexing and Deep Linking?

**Deep Linking** - Not new. Opening a link from search or any other app in your app. That same link that would normally open a web page, is opening the same (or very similar) content in your app.

**App Indexing** - New-ish. Reporting back to Google what content and web addresses are being opened by users in your app. Allows for auto-complete of searches with app and previously visited content.

Using both together allows Google to put a button on search results that allows users to install and open pages with your app using deep linking.

<img alt="auto complete search" width="50%" src="https://storage.googleapis.com/ejf-io/auto-complete.png">

## How do I enable App Indexing for my app?

1. You have a website with content
1. Verified website's domain in the Google Search Console
1. Have an app that opens content that is the same as your web content
1. Implement an intent-filter for your site in your app
1. Clicking links for your website opens the content in your app
1. AppIndexing API called whenever content in app is loaded, or the user leaves the app
1. Publish app
1. Add the android-app://<package id> version to your Google Search Console, and associate it with your website!
1. Verify domain in Play Console

Aside from the Google Play app publishing process, this should be doable end-to-end in about two hours, if you are starting from scratch with your app. If you already have an app, you should be able to do it in less than 20 minutes.

Let's take a quick look at all the pieces. The code that I'm using comes from the [blog example that I built and have hosted on GitHub](https://github.com/emil10001/ejf.io-blogapp). First, we'll go over the deep-linking part, then we'll look at what needs to be added to make App Indexing work.

### Deep-Linking


AndroidManifest.xml support for deep linking:

    <activity
        android:name=".MainActivity">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="http"
                android:host="ejf.io" />
        </intent-filter>
    </activity>

In onStart of the MainActivity, load up the URL in the webview to make the deep-linking work:

    @Override
    public void onStart() {
        super.onStart();
        mClient.connect();

        Intent intent = getIntent();
        String action = intent.getAction();
        Uri data = intent.getData();

        if (Intent.ACTION_VIEW.equals(action) && null != data)        
            webview.loadUrl(data.toString());
    }

### App Indexing

App's build.gradle:

    dependencies {
        // ...
        compile 'com.google.android.gms:play-services-appindexing:8.3.0'
    }

AndroidManifest.xml support for Google APIs:

    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />

MainActivity get GoogleApiClient for App Indexing:

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // ...
        mClient = new GoogleApiClient.Builder(this).addApi(AppIndex.API).build();
        // ...
    }

I added a listener on my WebView to run some code when the page finishes loading. This is we let the App Indexing API know that we've started viewing the page:

    webview.setWebViewClient(new WebViewClient() {
        @Override
        public void onPageFinished(WebView view, String url) {
            // Define a title for your current page, shown in autocompletion UI
            final String title = view.getTitle();
            Uri webUri = Uri.parse(url);
            Uri appUri = Uri.parse("android-app://io.ejf.blogapp/https/"
                    + webUri.getHost() + webUri.getPath());

            // Construct the Action performed by the user
            Action viewAction = Action.newAction(Action.TYPE_VIEW, title, webUri, appUri);
            // Call the App Indexing API start method after the view has completely rendered
            AppIndex.AppIndexApi.start(mClient, viewAction);
        }
    });

We call the App Index end command in onStop:

    @Override
    public void onStop() {
        // Call end() and disconnect the client
        // Define a title for your current page, shown in autocompletion UI
        final String title = wv.getTitle();
        if (null != wv.getUrl() && null != wv.getUrl()) {
            Uri webUri = Uri.parse(wv.getUrl());
            Uri appUri = Uri.parse("android-app://io.ejf.blogapp/https/" + webUri.getHost() + webUri.getPath());
            Action viewAction = Action.newAction(Action.TYPE_VIEW, title, webUri, appUri);
            AppIndex.AppIndexApi.end(mClient, viewAction);
            mClient.disconnect();
        }
        super.onStop();
    }


## Testing deep-linking and App Indexing

In order to test deep-linking from cli:

    # adb shell am start -a <action> -d <data> <package>
    adb shell am start -a android.intent.action.VIEW -d “http://www.example.com/app/some-great-content” com.example.app

If you have App Indexing set up, you should be able to go to Google Search on your phone, and begin typing the title of the page that you just visited. After just a couple of characters, you should be given an auto-complete result for your app.

## Why do I want App Indexing for my app?

There are two basic benefits of adding in App Indexing. The first is **user acquisition**, the second is **reengagement**.

Google's basic argument is that as the number of available apps grows, and the content accessible in those apps grows, users are going to begin to rely more heavily on search. Users will start with a Google Search to look for content either on the web or on their device. If there are results that are app-based, users may tend to prefer those results over others that are purely web-based. App Indexing puts an app badge, and possible install button (if the user doesn't have the app), on your search result. The other nice thing that it does is allows users to auto-complete a Search with content from an app.

For non-content focused apps, the benefits are obviously a little less clear. However, you could deep-link any pages that you have on your website, serve them up in-app, and then use traditional web SEO to drive installs. This is nice because web SEO is much easier to do than ASO (App Store/Search Optimization).

Some of the numbers that Google has given about success cases for this feature are as follows:

* The Guardian - 4.5% increase in CTR from search results
* Etsy - 11.6% increase in app traffic from referrals
* Tabelog - 9.6% increase in app traffic to restaurant pages

This feature is also really easy to implement for apps that are already serving up content. Adding this feature really shouldn't take more than about 20 minutes.
