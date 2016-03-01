+++
date = "2016-03-01T08:30:49-08:00"
draft = true
title = "New Matter MOD-t Review - a 3D printer"
images = ["https://storage.googleapis.com/ejf-io/3d_printer_collage.jpg"]
+++

<img alt="3d printed watch stand" width="75%" src="https://storage.googleapis.com/ejf-io/finished_stand_print.jpg">

Back in the beginning of November, I received a giant box on my doorstep. It was a 3D printer that I had <a href="https://www.indiegogo.com/projects/new-matter-mod-t-a-3d-printer-for-everyone#/">backed on Indiegogo</a> a year and a half earlier. The printer is a <a href="http://newmatter.com/">New Matter MOD-t</a>. They were a bit late shipping, but that was not exactly unexpected, generally, I'm happy when crowdfunded projects ship at all. Either way, it was here, arriving just in time for me to be super busy with other things. We were on our way out to do something, so all I had time to do was pull it out of the box, and stick it on a shelf.

<img alt="3d printer unboxing collage" width="75%" src="https://storage.googleapis.com/ejf-io/mod-t_unboxing.jpg">

This past weekend, I finally got around to getting the thing actually set up, and running. While the setup was rocky, and the desktop app poor, the overall experience is actually better than expected. I'm very excited to see where New Matter takes this product.

The setup was not easy, the software tools that they provide for Mac don't work well. I was able to get the firmware updated on the machine, but spent about two hours trying to get it to connect to WiFi. There was a bug in their installer and desktop app that shows an error and a 'disconnected' status, even when the WiFi is connected properly. Took me a while to realize that it had actually been connected. One of the problems with this is that it means that it's impossible to complete the setup installation, including calibrating the thing and loading the filament. You can install the desktop app without completing the setup, so I did that.

After getting the desktop app installed, I tried getting on WiFi again (still didn't realize that I was connected), and ran into the same bug described above. I decided to skip that, and try to get the test print going. Looking in the desktop app, they have a button called 'Load Filament', I tried that, and it asked me if I wanted to unload or load the filament. I needed to load filament, and it then gave instructions for first unloading filament, and then a button to press for loading filament. I pressed the button and nothing happened. It took me quite some time to figure out that you needed to restart the printer while on that screen in the app before that button would become active. (Restarting the printer is part of the unload filament process.) Figuring that bit out, I was able to get the filament loaded and the test print going.

<img alt="3d printer collage" width="75%" src="https://storage.googleapis.com/ejf-io/3d_printer_collage.jpg">

This is about the time that I figured out that the printer was really connected, and showing up in the New Matter web app. Excellent! I loaded up some STL files from <a href="http://www.thingiverse.com/">Thingiverse</a> into my New Matter library, and sent them to the printer from the web. I was able to disconnect the printer from my MacBook, and let it run on its own. From here, I was able to basically get what I needed done with little to no issue.

For me, this is where the MOD-t really shines, and I think that New Matter has done a brilliant job. There's no figuring out printer settings, or doing a deep dive into deeply understanding how FDM 3D printers work, you just go to the website, and hit 'print', then press a button on the printer. Simple. The problems that I experienced were all, 100%, on the desktop app side, which are easily fixable with updates.

There were two little hiccups. First, the top part of the watch stand that I was printing kept failing. I needed to edit the STL file to fix it, but it wasn't really New Matter's fault. The file was set up to print with only a single edge on the print bed. I grabbed the Meshmixer app, rotated the part, re-uploaded, and it printed just fine. The other issue was that the New Matter web app doesn't seem to handle printing multiple parts too well (or at all). You can add multiple parts to a thing in your library, but it will only print one of them, without giving you a way to select which one. The workaround is just to upload each part separately, and that's not really an issue.

All in all, I'm very excited about doing more with this thing. I've already started printing a case for the <a href="https://www.adafruit.com/products/3014">PiGrrl 2 project</a> that I'm working on, just waiting for some white filament to arrive for printing certain parts. The watch stand that I printed is great, and I saved myself $15 which is what they go for on Amazon. If you're in the market for a 3D printer, and want something simple and relatively inexpensive, this is a good choice.
