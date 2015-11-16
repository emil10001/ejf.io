+++
date = "2015-11-16T07:46:57-08:00"
title = "Managing Finances (Android Intents Part 5)"
images = ["https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/financedemo2.png"]
+++

This is the fifth post in a series about sharing information between apps. ([Series Table of Contents](/android/intents/toc).) In this post, we're going to create another demo of an Intents based API.

<iframe width="560" height="315" src="https://www.youtube.com/embed/w4PmOTXKfmM" frameborder="0" allowfullscreen></iframe>

## Series Table of Contents

[Follow the series on the Table of Contents page](/android/intents/toc).

## Finance Management App

Let's imagine a scenario, Mohammed has built an investing app. It allows users to buy stocks and build up a portfolio. His app is only available on smartphones. There's no web version of the app, and no public-facing APIs available from our servers.

Now, let's imagine that his friend Susan is building an Android app to help people manage their finances. She wants to give her users the most complete picture that she can. She already has bank and credit card integrations done, and now she's looking to build a view of users' investments. She knows that a lot of her customers are also customers of ours. She asks Mohammed if he can open up an API for her.

All she really needs is a current dollar value of the investments, along with the amount of cash invested. Mohammed decides that he can open up an intent API for her in our app.

*For demo purposes, I'm not actually going to build out a stock app, I'm just going to build something that handles the Intent request, and returns a response.*

Mohammed wants to have some amount of security, so he comes up with a scheme for calculating a client id and using a shared signature, to try to prevent random, unauthenticated apps from requesting data. Here is what the request looks like:

    private void requestStockInfo() {
        Intent intent = new Intent(STOCKY_RECEIVER_ACTION);
        // calculate client_id, based on known algorithm
        intent.putExtra(CLIENT_ID, Hasher.hashIt("stocky_app" + getPackageName()));
        intent.putExtra(CLIENT_SECRET, CLIENT_SECRET);
        startActivityForResult(intent, REQUEST_CODE);
    }

**Disclaimer: Please do not actually do this with a financial app unless you consult with a security expert first. I would assume that this could be made to be safe, but I am not a security expert. The point is that there are ways of adding layers of security.**

<img alt="finance demo 1" width="50%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/financedemo1.png">

> Activity before request to Stocky

Within Stocky, Mohammed's stock buying app, the intent is handled, checked and result returned:

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Intent intent = getIntent();
        String caller = Hasher.hashIt("stocky_app" + getCallingPackage());
        Intent sIntent = new Intent();

        if (Constants.STOCKY_RECEIVER_ACTION.equals(intent.getAction())
                && intent.hasExtra(Constants.CLIENT_ID)
                && intent.hasExtra(Constants.CLIENT_SECRET)) {

            Bundle extras = intent.getExtras();
            String clientId = extras.getString(Constants.CLIENT_ID);

            if (!caller.equals(clientId)) {
                setResult(RESULT_CANCELED, sIntent);
                finish();
                return;
            }

            // hardcoded information, I'm not building a full app here
            sIntent.putExtra(Constants.VALUE, 9830.00);
            sIntent.putExtra(Constants.INVESTED, 10000.00);
            setResult(RESULT_OK, sIntent);
        } else {
            setResult(RESULT_CANCELED, sIntent);
        }

        finish();
    }

Then, back in Susan's finance app, she handles the result:

    @Override
    public void onActivityResult( int requestCode, int resultCode, Intent data ) {
        if (RESULT_OK != resultCode)
            return;

        Bundle extras = data.getExtras();

        double value = 0L;
        double invested = 0L;
        if (extras.containsKey("VALUE"))
            value = extras.getDouble("VALUE");
        if (extras.containsKey("INVESTED"))
            invested = extras.getDouble("INVESTED");
        double gains = value - invested;
        // do something with the data
        stockInfo(value, invested, gains);
    }

<img alt="finance demo 2" width="50%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/financedemo2.png">

> Activity after result from Stocky

That's it! [There is a fully working demo in our repository](https://github.com/emil10001/AndroidIntentExamples/tree/step8). ( `git checkout -b step8 step8` )

## Conclusion

Today, we looked at another example of using public APIs on the phone. We thought through a new example of something that might be useful to do, and built a demo of that.

I have some ideas for the next few posts, but I don't want to spoil the surprise (which is to say, I have no idea how I'm going to write up the next few posts).

Keep an eye on the [Table of Contents](/android/intents/toc) for the latest entries in the series.
