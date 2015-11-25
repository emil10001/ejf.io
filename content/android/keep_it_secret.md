+++
date = "2015-11-23T11:42:50-08:00"
draft = true
title = "Keep it Secret, Keep it Safe"

+++

<iframe width="560" height="315" src="https://www.youtube.com/embed/fcWVV0Hafuk" frameborder="0" allowfullscreen></iframe>

http://developer.android.com/training/articles/security-ssl.html

*Protecting the network traffic in your app.*

* High level TLS, and why you should use it
* Using TLS, checking your work
* Mistakes and misconfigurations

## Why

The network is not to be trusted. This has always been true, but especially for mobile devices.

* Devices connect to many different networks
  * Coffee shop wifi
  * Anyone can run an open wifi
* Devices do sensitive transactions wirelessly

Canonical examples of sensitive traffic

* login for banking
* credit card details
* private personal photos
* emails
* sensitive web traffic
* health information

On the server-side, you should secure your traffic. All traffic must be protected.

* Modify non-sensitive traffic (e.g. Time Warner)
  * inject exploits
  * modify content
  * replace images
* Tracking and snooping

**Goal** the network cannot affect the security of your device.

## Enter TLS

* Transport Layer Security
  * Previously known as SSL
  * Usable with any protocol
* Establishes an end-to-end channel between two peers
  * Integrety
  * Confidentiality
  * Hard part - knowing you're talking to the right peer
* ...?

## Using TLS

Using standard HTTP libs, replace `http://` with `https://`. Use SSLSocket for socket connectisons.

    HostNameVerifier.verify

ssllabs has a good intro for server-side.

## Checking your work

* Hardcoded URLs - what about redirects?
* Server provided - maybe you forget on the server?
* Third-party code - how do you check them?

New feature in Marshmallow.

* Strict mode clear text detection
* usesClearTextTraffic flag in manifest

## Strict Mode

Uses packet inspection to catch all non-TLS traffic. Useful during development to make sure that all traffic is going over TLS. Some false-positives, e.g. HTTP proxies, protocols with STARTTLS logic, or secure traffic that doesn't begin with a TLS Hello.

## usesClearTextTraffic

AndroidManifest.xml:

    <application android:usesClearTextTraffic="false">

Supported:
* HttpsUrlConnection
* OkHttp, apache

## Why block vs upgrade

* Older devices left out.
* Upgrade logic isn't always well defined for non-HTTP protocols

Blocking will fail quickly, and obviously. Fixes will be true for all devices.

## Mistakes and misconfigurations

Using the default implementation is a good idea. However, you can override the defaults with insecure code.

Legacy servers, with legacy clients choosing what cert to present for a connection.

TLS with Server Name Identifier (2.3+) extension solves some problems around knowing who you're talking to.

Some internet routing may not support all of the TLS protocols, and may downgrade to SSLv3.

How do you know if you trust this chain of certificates? What ships with your devices?

How can you trust the CAs on the device? (e.g. Lenovo.)

Your app can use its own CA, which may make sense for various applications. However, the example code out there may not be great. [Documentation](http://developer.android.com/training/articles/security-ssl.html#UnknownCa).

## What Google's doing

*Vulnerability alerts and blocks*

Google is going to be scanning the Play Store, running static analysis on apps, looking for vulnerabilities. They will alert developers, and remove apps that have not been fixed. Here's the slide that discusses that:

<img alt="vulnerable apps" widt="75%" src="https://storage.googleapis.com/ejf-io/android_dev_summit/security_vulnerability_reports.jpg">
