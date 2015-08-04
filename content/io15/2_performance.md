{
    "slug": "battery",
    "date": "2015-05-28T00:11:00.000Z",
    "tags": ["io15","googleio"],
    "title": "Battery Performance on Android",
    "publishdate": "2015-05-28T00:11:00.000Z"
}

Battery Performance for Android
=====

> Colt McAnlis | +ColtMcAnlis | @duhroach

\#perfmatters

The more things you do, the more battery you use.

A study by Purdue University showed that only 25-30% of battery usage is for intended use. The other 70% is being eaten by other things like GPS, Ads, etc.

### Defer work until the best time for battery

## Battery Problems

### Networking

There's a round trip that may happen if you wake up the chip to do a network request. If you wake up the device to make a network request, it sends a packet to the server, then waits for some period of time for the server to send a response. There is a 20-60 second keep awake time that happens during these events.

Batch & Delay - Delay XHR requests until you can do them all together, then get only one wait time. This can result in a 5-6x increase in battery life.

Check out Battery Historian, so that you can take a look at what's waking up the chip, and hitting the battery.

### Wakelocks

Don't do that! They don't get released, and make Colt bald. Instead, try `AlarmManager.setInexact()`.

### Location

Very expensive. Using Cell Network Provider instead of GPS, but less accurate. Figure out which is necessary for what the user actually needs.

Turn down resolution while the phone is sleeping. Using passive providers can also help.

### Job Scheduler

Use this too!
