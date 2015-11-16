+++
date = "2015-11-16T06:13:04-08:00"
title = "Editing Images (Android Intents Part 4)"
images = ["https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/edited_file.png"]

+++

This is the fourth post in a series about sharing information between apps. ([Series Table of Contents](/android/intents/toc).) In this post, we're going to look at an example of an app that provides a public API. Our Aviary example offers a solid example of why this sort of thing might be beneficial, so let's get right into it!

<iframe width="560" height="315" src="https://www.youtube.com/embed/YcAbmKIxHK4" frameborder="0" allowfullscreen></iframe>

## Series Table of Contents

[Follow the series on the Table of Contents page](/android/intents/toc).

## Aviary

[Aviary](https://play.google.com/store/apps/details?id=com.aviary.android.feather) (acquired by Adobe not too long ago) is a photo editing app on Android that [provides an SDK](https://developers.aviary.com/docs/android) to allow developers to use Aviary's image editing tools within their app. The SDK works on Intents being fired at some Aviary provided Activities that you're supposed to include in your app. This means that you've got to bundle Aviary's SDK into your app, and add UI components which may complicate things for you (especially when you want to change compatibility libraries).

Luckily, [Aviary also provides an external Intents API](https://developers.aviary.com/docs/android/setup-guide#nosdk) that works as long as you've got the Aviary app installed. The external API is really nice because you don't even need credentials to send images to Aviary to be edited.

While the external API is fairly simple. The example app is going to be sending an Intent and expecting a response. This is a bit different from what we've seen before.

[Aviary's documentation](https://developers.aviary.com/docs/android/setup-guide) actually gives a lot of information about how to do this, so I'll just give highlights, and a [link to the demo repository at this stage](https://github.com/emil10001/AndroidIntentExamples/tree/step7).

Our example uses a file that I've included in our resources:

    String destFileName = "startup_unicorn.jpg";
    File file = resToFile(R.drawable.startup_unicorn, destFileName);
    log.d("image file: %s", file.getPath());
    Uri uri = Uri.fromFile(file);

Once we have our file available in external storage, we can fire off our Intent:

    Intent newIntent = new Intent( "aviary.intent.action.EDIT" );
    newIntent.setDataAndType(uri, "image/*");
    newIntent.putExtra("app-id", getPackageName());
    startActivityForResult( newIntent, 1 );

That should send us off to Aviary, and wen we come back, we will need to handle the response:

    @Override
    public void onActivityResult( int requestCode, int resultCode, Intent data ) {
        if( resultCode != RESULT_CANCELED && requestCode == 1) {
            Bundle extras = data.getExtras();
            Uri imageUri = data.getData();
            File outFile = new File(getExternalFilesDir(null), "startup_unicorn_edited.jpg");
            if (null != extras) {
                if (extras.getBoolean("bitmap-changed"))
                    saveFile(imageUri, outFile);
            }
        }
    }

Now, we can see our file if we navigate to our demo app's cache directory, shown in the image below:

<img alt="aviary edited image" width="50%" src="https://s3.amazonaws.com/ejf3-public/hosted_files/ejf_io/intents/edited_file.png">

To see a fully working demo of this, [check out step 7 in the demo repository](https://github.com/emil10001/AndroidIntentExamples/tree/step7). ( `git checkout -b step7 step7` )

## Conclusion

Today, we looked an example of using public APIs on the phone. We reviewed a real-world, working example with Aviary. Keep an eye on the [Table of Contents](/android/intents/toc) for the latest entries in the series.
