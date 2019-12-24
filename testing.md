---
layout: ordered_page
title: Testing
subtitle: Making sure it actually works
prevUrl: /assembly
prevTitle: Final Assembly
nextUrl: /bluetooth
nextTitle: Bluetooth
---

[//]: # (This will be the tutorial for testing the final product.  I'll go over what we're testing and highlight some of the issues I ran into.)

### Goal

Test the final product for playability.

### The Problem

Okay, so we finally have a finished MIDI accordion... but does it actually work?  We'll need to test the following:

1. Do all keys sound?
2. Are all keys mapped correctly to their proper pitches?
3. Do any keys stick?
4. Are there latency issues?

### Solution

Start by booting up all the software and getting the [final project](https://github.com/bvavra/MIDI_Accordion/tree/master/MIDI_Accordion) running on the accordion:

1. Start your virtual MIDI port (if it's not running already).
2. Open your MIDI playback software of choice and verify it's hooked up to your virtual MIDI port.
3. Upload the [final project](https://github.com/bvavra/MIDI_Accordion/tree/master/MIDI_Accordion) code to your Arduino (if it's not on there already).
4. Start your Serial-to-MIDI conversion software and connect it to your Arduino and virtual MIDI port.

Now it's time to start testing.  It's easier if you already have the accordion open and still have access to pressing all of the buttons so you can easily switch between key testing and circuit testing.

#### 1. Do all keys sound?

Press all of the keys one by one and make sure each one sounds.  All the melody keys should play a single pitch, all the bass root/third buttons (first two rows) should play two pitches, and all the bass chord buttons should play three pitches. 

*Note: You don't have to play each of the 120 bass buttons - 12 root buttons and a few random chord buttons should suffice.*

If a key or button doesn't sound, try the following:

- Open the accordion and verify everything is wired to the silent key correctly.
- Use your multimeter to verify connectivity on each of the 4 pins on the silent key's opto-interruptor.

If a key or button makes too many sounds, try the following:

- Verify that the opto-interruptor's cathode pin is wired directly to the corresponding transistor.
- Use your multimeter to verify there is no connectivity between the collector pin of that opto-interruptor and any other opto-interruptors.
- Try altering the code to increase the delayMicroseconds amount between each port read (start by doubling it).  If this makes a difference, the opto-interruptors may be reading light interference.  Try covering the sensors or closing the accordion and then try pressing the key again with the original delay.  If it works now, you're good to go.  If not, you may have to either keep the increased delay (which may prove to be unacceptable in the long run), or replace the opto-interruptors with a different component.  [Don't say I didn't warn you](../opto-interruptor).


#### 2. Are all keys mapped correctly to their proper pitches?

Unless you followed my designs verbatim, chances are you will have to alter the code to remap which pitch is played for each key and button.  This is where your music theory skills come into play.  There area few ways to do this, but here's the general idea:

1. For each key you press, determine which number is associated with the expected pitch for that key.  I [drew up a diagram](https://github.com/bvavra/MIDI_Accordion/blob/master/pdf/design/treble_pitch_mapping.pdf) that maps which right hand keys have which pitch numbers (from the perspective of the inside of my accordion, which may or may not help you).
2. Press that key and see what pitch is actually played.  You can either use your ear to determine this, or you can turn DEBUG mode on in the code and view the serial output to view the numeric pitch mapping for each key you press.
3. For each pitch mapping that is incorrect, go into the code and swap the positions in the 2D array of mapped pitches for the expected pitch and the sounded pitch.
4. Rinse and repeat on both hands until all pitches are mapped correctly.

[//]: # (TODO - maybe include some visual examples)


#### 3. Do any keys stick?

These will either become painfully obvious to you while doing Step 1, or they will sneak up on you over hours of play testing.  This happened to several keys of mine - each of which were a result of me not gluing my sensor blockers in the exact place needed to avoid collisions.  However, before going in, taking out the circuit, ripping out the blocker, and regluing it in the right place, see if you can't bend it around the obstacle first.  If that doesn't work, then yeah, do all that.


#### 4. Are there latency issues?

So this is a big one that's kind of hard to quantify.  There will *always* be latency - the question is, "How much latency can you take before the instrument becomes unplayable, and has your MIDI accordion passed that threshold?"  I don't have the tools to measure the exact timespan from key input to sound output, but I believe you should be able to acheive a latency less than 20ms, [which can be considered an acceptable amount for keyboard players](http://music.stackexchange.com/questions/30323/when-does-audio-latency-matter-and-not-matter).  For Windows users, you may need a piece of software called [ASIO4All](http://www.asio4all.com/), which can help reduce the amount of latency your audio driver creates when generating sound from MIDI data.  Ideally you would want a sound card that already has an ASIO driver, which would reduce the latency even further.

### Play Testing

From this point on, it's just a matter of making minor tweaks here and there until find yourself tweaking no more.  Play a lot of different kinds of tunes that exercise various parts of your accordion to make sure everything feels right.  Play fast, play slow, play chords, play everything: this is your instrument now.

Congratulations on making your own MIDI Accordion!  From here all other pages are bonus tutorials for optional improvements you can make to add additional features like wireless capabilities or dynamic volume control with the bellows.