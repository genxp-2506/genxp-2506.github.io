---
title: Sound Files
---
## NOTE:
To use the sound library, we have to make sure the [p5.sound library](https://p5js.org/reference/#/libraries/p5.sound) is included in our project's `index.html` file after the p5.js file. It will look something like this:

```html
<script src="https://cdn.jsdelivr.net/npm/p5@1.7.0/lib/p5.js"></script>
<script src="https://cdn.jsdelivr.net/npm/p5@1.7.0/lib/addons/p5.sound.js"></script>
```

## Playing Sounds
Let's load an audio file using the [`loadSound()`](https://p5js.org/reference/#/p5/loadSound) function and test it. This is similar to how we have loaded images, text files, datasets, etc using the `preload()` function.

The `loadSound()` function returns a [`SoundFile`](https://p5js.org/reference/#/p5.SoundFile) object, and we can use its [`play()`](https://p5js.org/reference/#/p5.SoundFile/play) function to play the song whenever we click the mouse:

{% include p5-editor.html id="aLnj1hrsv" %}

What happens if we click the mouse multiple times? It sounds like the `song` object starts playing the file multiple times over itself.

There are two ways we can fix this.

First, we can specify the file's [`playMode()`](https://p5js.org/reference/#/p5.SoundFile/playMode) to be either `restart` or `untilDone`, and that will make it only play one instance of the file, either by starting the song over or not doing anything until it plays until the end:

{% include p5-editor.html id="yD2bhgfPO" %}

The other way is to check first if the object is already playing the file and only call `play()` if it's not. We can also use [`isPlaying()`](https://p5js.org/reference/#/p5.SoundFile/isPlaying) to change the background color as an indication of the play state:

{% include p5-editor.html id="cKAbzWY2a" %}

Since we're already checking whether the sound is playing when we get a mouse click, we can also easily pause the song on alternate clicks. This way the canvas acts like a play/pause button. Every time there's a click, pause if it's playing, and continue playing if it's paused.

{% include p5-editor.html id="0cHibgOcU" %}

## Seeing Sound

One more last thing, hopefully a fun one.

Let's visualize the songs while they're playing.

The p5.js `SoundFile` object doesn't seem to have any function or variable that tells us the exact values of the samples that are currently playing. The closest thing it has is a [`getPeaks()`](https://p5js.org/reference/#/p5.SoundFile/getPeaks) function that gives us a simplified, resampled, version of values for the entire sound file.

The default length of the array returned by `getPeaks()` is $$5$$ times the canvas `width`, so no matter how long the song is, the length of the peaks array will always be the same. The numbers in this array represent samples, which are like the pixels of sound files. In this case they have been normalized to a range of $$[-1, 1]$$, where numbers with larger absolute values represent louder samples.

We can use this to map the sample values of the current song from $$[-1, 1]$$ to values between $$[-width, width]$$ that we can use to draw some ellipses:
```js
map(songPeaks[i], -1, 1, -width, width);
```

Once we have that array of ellipse diameters, we just have to know which one to draw at each frame. That's this code here in `drawWave()`:
```js
let tPos = song.currentTime() / song.duration();
let dIdx = floor(tPos * diameters.length + DELAY);
dIdx = constrain(dIdx, 0, diameters.length - 1);
let diam = diameters[dIdx];
ellipse(width / 2, height / 2, diam);
```

The `tPos` variable uses the song's [`currentTime()`](https://p5js.org/reference/#/p5.SoundFile/currentTime) and [`duration()`](https://p5js.org/reference/#/p5.SoundFile/duration) functions to determine how far we are along the song that is playing. Its value will always be between $$[0, 1]$$, and represents the percentage of the track that has been played.

The `dIdx` variable turns the `tPos` value into an index value by multiplying the percentage by the length of the diameter array. `DELAY` is a constant that we added to account for delays between the screen and the computer's speakers. We can play with that variable in the beginning of the file to shift which index we use at a given frame to account for delays when using bluetooth headphones or speakers.

This value then goes through `floor()` and `constrain()` in order to guarantee that we are always using a whole-number integer (no decimal) that is within the array's bounds.

After that we use it to index into our arrays of diameters and draw an ellipse whose size is proportional to the volume of the current sample being played:

{% include p5-editor.html id="5DyVqATW_" %}

Try it out, and remember to adjust the `DELAY` value to get better synchronization if using bluetooth speakers/earbuds or if the browser is overloaded.
