---
layout: post
title: The patron happy-sad-ometer prototype
tags:
- python
- patron_satisfaction
- raspberry_pi
---

Some time ago Masud Khokar 
([email](mailto:masud.khokar"), [@mkhokhar](https://twitter.com/mkhokhar))
suggested it would be a nice idea to 
use a Raspberry Pi with a couple of buttons attached as a quick mechanism
for collecting some patron satisfaction data on leaving the library.

'Did you manage to get done what you came to the library for today?'

We procured a Pi, some red and green buttons with LEDs, bit's of wire and 
a break out board. Soldering not necessary, just a little wire stripping.

<!--more-->

The Pi has the Red Hat flavour of Linux installed. A short python program,
using the GPIO library, sets everything up and listens for button
press events. When one is detected, the colour and time are logged to
'/var/local/red_green_log'. The LED in the button is briefly lit up as
a visual indication for the user.

I took a drill and hacksaw to an old pamphlet box to make a housing for
the prototype.

All the bits squash inside.

![The innards](/public/images/hsom_innards.jpg)

Finished!

![Finshed](/public/images/hsom.jpg)

The data is collected in a Postgres database and then can displayed 
and analysed within Tableau.

![tableau analysis](/public/images/hsom_tableau.png)

An interesting little project which we may get round to deploying. 
A  nicer perspex case would be better.

The python code:

{% highlight python linenos %}
#!/usr/bin/env python2.7  
from __future__ import print_function
import RPi.GPIO as GPIO  
import time
import datetime
import os, errno
import sys


RED_IN = 7
GREEN_IN = 8
RED_OUT = 3
GREEN_OUT = 2
CONSOLE_PRINT = False
LOGDIR = '/var/local/red_green_log'


def mkdir_p(path):
    try:
        os.makedirs(path)
    except OSError as exc:
        if exc.errno == errno.EEXIST and os.path.isdir(path):
            pass
        else: raise


def red_setup():
    GPIO.setup(RED_IN, GPIO.IN,
               pull_up_down=GPIO.PUD_UP)
    GPIO.setup(RED_OUT, GPIO.OUT)  


def green_setup():
    GPIO.setup(GREEN_IN, GPIO.IN,
               pull_up_down=GPIO.PUD_UP)
    GPIO.setup(GREEN_OUT, GPIO.OUT)  


def button_pressed(channel):
    if channel == RED_IN:
        colour = "red"
        oc = RED_OUT
    else:
        colour = "green"
        oc = GREEN_OUT

    # A bit of software switch debounce, there is also a bouncetime
    # parameter to add_event_detect which does more or less
    # the same thing I think.
    tm_check = time.time() + 0.2
    while time.time() < tm_check:
        if GPIO.input(channel) == 0:
            return

    if CONSOLE_PRINT:
        print(datetime.datetime.now().strftime('%Y%m%d-%H:%M:%S.%f'),
              colour)
    fname = LOGDIR + '/' + time.strftime('%Y%m%d',time.localtime()) \
            + '_' + colour
    fn = open(fname, 'a')
    print(datetime.datetime.now().strftime('%Y%m%d-%H:%M:%S.%f'),
          colour, file=fn)
    fn.close()

    # Turn the led on
    GPIO.output(oc, True)
    time.sleep(0.3)
    # and off
    GPIO.output(oc, False)


if __name__ == "__main__":
    mkdir_p(LOGDIR)
    GPIO.setwarnings(False)
    GPIO.setmode(GPIO.BCM)  
    green_setup()
    red_setup()

    GPIO.add_event_detect(RED_IN, GPIO.RISING,
                          callback=button_pressed)
    GPIO.add_event_detect(GREEN_IN, GPIO.RISING,
                          callback=button_pressed)

    while True:
        try:  
            time.sleep(10)
        except KeyboardInterrupt:  
            GPIO.cleanup()
            sys.exit()
{% endhighlight %}


