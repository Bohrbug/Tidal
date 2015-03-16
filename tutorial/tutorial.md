% Tidal tutorial

Welcome to the Tidal tutorial. Tidal is a mini-language for exploring pattern, designed for use in live coding performance. In this tutorial we'll step through different levels of abstraction, starting with sounds and filters, then sequences of sounds and filters, and moving up to functions for manipulating those sequences, and ending up looking at functions which manipulate other functions. Fun stuff!

# Sounds and effects

With a bit of fiddling, Tidal can be used to pattern the input to any
device which takes MIDI or Open Sound Control input, but the default is the Dirt software sampler. If you followed the install process, you should have Dirt installed and it should be running.

To test it, run the following by typing it into your text editor, holding down ctrl and pressing enter:

```haskell
d1 $ sound "can"
```

You should be able to hear a repeating sample of someone hitting a can. Tidal is designed with repetitive dance music in mind, and will repeat the pattern forever, although you can build a great deal of variety in a single pattern, and also change it while it is running (i.e. live code).

The `can` in the above is the name of the sample you are playing. Well actually it is the name of a folder full of samples. You can find them in the `samples` subfolder of your dirt folder. You can specify a different sample by number, using the colon:

```haskell
d1 $ sound "can:1"
```

Try some different numbers to hear all the different can samples that
come with dirt.

Dirt comes with a wide range of samples to work with, here's some of
them:

```
    flick sid can metal future gabba sn mouth co gretsch mt arp h cp
    cr newnotes bass crow hc tabla bass0 hh bass1 bass2 oc bass3 ho
    odx diphone2 house off ht tink perc bd industrial pluck trump
    printshort jazz voodoo birds3 procshort blip drum jvbass psr
    wobble drumtraks koy rave bottle kurt latibro rm sax lighter lt
    arpy feel less stab ul
```

Replace `can` with one of these to explore.

## Effects

You can also apply a range of effects to change what your sound, er,
sounds like. For example a vowel-like 'formant filter':

```haskell
d1 $ sound "can:1" |+| vowel "a"
```

The `|+|` operator in the above is what binds the sound with the vowel parameter.

Try changing the "a" for other vowels. You can also play the sample faster, which makes it higher in pitch:

```haskell
d1 $ sound "can:1" |+| speed "2"
```

Or slower:

```haskell
d1 $ sound "can:1" |+| speed "0.5"
```

Or even backwards:

```haskell
d1 $ sound "can:1" |+| speed "-1"
```

You can also apply several effects at the same time:

```haskell
d1 $ sound "can:1" |+| vowel "a" |+| speed "-1"
```

Here is the full list of effects you can play with.

Name          | Description
------------- | -----------
accelerate    | a pattern of numbers that speed up (or slow down) samples while they play.
bandf         | a pattern of numbers from 0 to 1. Sets the center frequency of the band-pass filter.
bandq         | a pattern of numbers from 0 to 1. Sets the q-factor of the band-pass filter.
begin         | a pattern of numbers from 0 to 1. Skips the beginning of each sample, e.g. 0.25 to cut off the first quarter from each sample.
coarse        | fake-resampling, a pattern of numbers for lowering the sample rate, i.e. 1 for original 2 for half, 3 for a third and so on.
crush         | bit crushing, a pattern of numbers from 1 for drastic reduction in bit-depth to 16 for barely no reduction.
cutoff        | a pattern of numbers from 0 to 1. Applies the cutoff frequency of the low-pass filter.
delay         | a pattern of numbers from 0 to 1. Sets the level of the delay signal.
delayfeedback | a pattern of numbers from 0 to 1. Sets the amount of delay feedback.
delaytime     | a pattern of numbers from 0 to 1. Sets the length of the delay.
end           | the same as begin, but cuts the end off samples, shortening them; e.g. 0.75 to cut off the last quarter of each sample.
gain          | a pattern of numbers that specify volume. Values less than 1 make the sound quieter. Values greater than 1 make the sound louder.
hcutoff       | a pattern of numbers from 0 to 1. Applies the cutoff frequency of the high-pass filter.
hresonance    | a pattern of numbers from 0 to 1. Applies the resonance of the high-pass filter.
pan           | a pattern of numbers between 0 and 1, from left to right (assuming stereo)
resonance     | a pattern of numbers from 0 to 1. Applies the resonance of the low-pass filter.
shape         | wave shaping distortion, a pattern of numbers from 0 for no distortion up to 1 for loads of distortion (watch your speakers!)
sound         | a pattern of strings representing sound sample names (required)
speed         | a pattern of numbers from 0 to 1, which changes the speed of sample playback, i.e. a cheap way of changing pitch
vowel         | formant filter to make things sound like vowels, a pattern of either a, e, i, o or u. Use a rest (~) for no effect.

# Sequences

You're probably bored of hearing the same sample over and over by now, let's quickly move on to 
sequences. Tidal sequences allow you to string samples together, stretch the sequences out and 
stack them up in a variety of interesting ways, as well as start mixing in randomisation.

You can make a tidal cycle with more than one sample just like this:

```haskell
d1 $ sound "drum drum:1"
```

Kick and snare forever!

You'll notice that however many things you put into a Tidal pattern, it still takes up the same 
amount of time. For example the following fits three sounds into the same cycle duration:

```haskell
d1 $ sound "drum drum:1 can"
```

The `~` symbol represents a rest, or pause, e.g.:

```haskell
d1 $ sound "drum drum:1 ~"
```

You can play around with some more off-kilter patterns, for example this one which has seven steps in it:

```haskell
d1 $ sound "drum ~ can ~ ~ drum:1 ~"
```

## Subdividing sequences

You can take one step in a pattern and subdivide it into substeps, for example in the following the 
three `can` samples are played inside the same amount of time that each `drum` sample does:

```haskell
d1 $ sound "drum drum [can can:4 can:5] drum"
```

As you can see the square brackets give the start and end of a subdivision. Actually you can keep going, 
and subdivide a step within a subdivision:

```haskell
d1 $ sound "drum drum [can [can:4 can:6 can:3] can:5] drum"
```

## Layering up patterns

Square brackets also allow you to specify more than one subpattern, by separating them with 
a comma:

```haskell
d1 $ sound "drum [can cp, can bd can:5]"
```

As you can hear, the two patterns are layered up. Because they are different lengths (one with two 
sounds, the other with three), you can get an interesting polyrhythmic effect. You can hear this better
if you just have a single subdivision like this:

```haskell
d1 $ sound "[can cp, can bd can:5]"
```

If you use curly brackets rather than square brackets the subpatterns are layered up in a different way, 
so that the sounds inside align, and the different lengths of patterns seem to roll over one another:

```haskell
d1 $ sound "{can can:2, can bd can:5}"
```

Again, you can layer up more than one of these subpatterns:

```haskell
d1 $ sound "[can cp, can bd can:5, arpy arpy:2 ~ arpy:4 arpy:5]"
```

And subdivide further:

```haskell
d1 $ sound "{[can can] cp, can bd can:5, arpy arpy:2 ~ [arpy:4 arpy:5] arpy:5}"
```

This can already start getting very complex, and we haven't even got on to functions yet!

## Sequencing niceties and tricks

Staying with sequences for a bit longer, there are a couple of other things you can do.

### Repetition and division

If you want to repeat the same sample several times, you can use `*` to specify how many times. For 
example this:

```haskell
d1 $ sound "bd [can can can]"
```

Can be written like this:

```haskell
d1 $ sound "bd can*3"
```

When live coding saving a little bit of typing helps a lot. You can 
experiment with high numbers to make some strange sounds:

```haskell
d1 $ sound "bd can*32 bd can*16"
```

The above pattern plays the samples so quickly that your ears can't hear the individual sounds any
more, and instead you hear it as an audio frequency, i.e. a musical note.

If you have a pattern that has a repeat that isn't a subpattern, like this:

```haskell
d1 $ sound "bd can can can"
```

You can repeat successive events with `!`:

```haskell
d1 $ sound "bd can ! !"
```

You can also 'slow down' a subpattern, for example this plays the `[bd arpy sn:2 arpy:2]` 
at half the speed:

```haskell
d1 $ sound "bd [bd arpy sn:2 arpy:2]/2"
```

That is, the first cycle you get `bd [bd arpy]` and the second time around 
you get `bd [sn:2 arpy:2]`. This is a little bit difficult to understand, but 
basically if you don't get through a whole subpattern during one cycle, it carries 
on where it left off the next one.

You can get some strange things going on by for example repeating four thirds of a
subpattern per cycle:

```haskell
d1 $ sound "bd [bd arpy sn:2 arpy:2]*4/3"
```

If you like strange time signatures, hopefully you will be having fun with this already.

### Random drops

If you only want something to happen sometimes, you can put a question mark after it:

```haskell
d1 $ sound "bd can? bd sn"
```

In the above, the can sample will only play on average 50% of the time. If you add a question
mark to a subpattern, it applies separately to each element of the subpattern. For example in 
the following sometimes you get no can sounds, sometimes just the first or second, and sometimes
both:

```haskell
d1 $ sound "bd [can can:4]? bd sn"
```

### Enter Bjorklund (and Euclid)

The If you give two numbers in parenthesis after an element in a pattern, then Tidal will
distribute the first number of sounds equally across the second number of steps:

```haskell
d1 $ sound "can(5,8)"
```

Now it isn't possible to distrute three elements equally across eight discrete steps, but the
algorithm does the best it can. The result is a slightly funky bell pattern. Try this one:

```haskell
d1 $ sound "can(5,8)"
```

Bjorklund's algorithm wasn't made for music but for an application in nuclear physics, which is exciting. More exciting still is that it is similar to the one of the first known algorithms written in Euclid's book of elements in 300 BC. You can read more about this in the paper [The Euclidean Algorithm Generates Traditional Musical Rhythms](http://cgm.cs.mcgill.ca/~godfried/publications/banff.pdf) by Toussaint. Examples from this paper are included below, although some require rotation to start on a particular beat - see the paper for full details and references.

Pattern | Description
------- | -----------
(2,5)   | A thirteenth century Persian rhythm called Khafif-e-ramal. 
(3,4)   | The archetypal pattern of the Cumbia from Colombia, as well as a Calypso rhythm from Trinidad. 
(3,5)   | When started on the second onset, is another thirteenth century Persian rhythm by the name of Khafif-e-ramal, as well as a Rumanian folk-dance rhythm.
(3,7)   | A Ruchenitza rhythm used in a Bulgarian folk-dance. 
(3,8)   | The Cuban tresillo pattern discussed in the preceding.
(4,7)   | Another Ruchenitza Bulgarian folk-dance rhythm.
(4,9)   | The Aksak rhythm of Turkey. 
(4,11)  | The metric pattern used by Frank Zappa in his piece titled Outside Now.
(5,6)   | Yields the York-Samai pattern, a popular Arab rhythm, when started on the second onset.
(5,7)   | The Nawakhat pattern, another popular Arab rhythm.
(5,8)   | The Cuban cinquillo pattern.
(5,9)   | A popular Arab rhythm called Agsag-Samai. 
(5,11)  | The metric pattern used by Moussorgsky in Pictures at an Exhibition.
(5,12)  | The Venda clapping pattern of a South African children’s song.
(5,16)  | The Bossa-Nova rhythm necklace of Brazil.
(7,8)   | A typical rhythm played on the Bendir (frame drum).
(7,12)  | A common West African bell pattern. 
(7,16)  | A Samba rhythm necklace from Brazil. 
(9,16)  | A rhythm necklace used in the Central African Republic. 
(11,24) | A rhythm necklace of the Aka Pygmies of Central Africa.
(13,24) | Another rhythm necklace of the Aka Pygmies of the upper Sangha.

# Functions

Up until now we have mostly only been working with building sequences, although this has included polyrhythm, and some simple algorithmic manipulation that is built into Tidal's pattern syntax. Now though it is
time to start climbing up the layers of abstraction to see what we can find on the way.

First, lets have a closer look at the functions we have been using so far.

## Sending patterns to Dirt

`d1` is a function that takes a pattern as input, and sends it to dirt. By default there are ten of them defined, from `d1` to `d10`, which allows you to start and stop multiple patterns at once.

For example, try running each of the following four lines in turn:

```haskell
d1 $ sound "bd sn"

d2 $ sound "arpy arpy:2 arpy"

d1 $ silence

d2 $ silence
```

The first line will start a bass drum - snare pattern, the second start a slightly tuneful pattern, 
then the third swaps the first pattern with silence (so it stops), and the four swaps the second 
with silence (so everything is silent).

It's important to notice here that the Tidal code you type in and run with `ctrl-enter` is changing patterns which are running in the background. The running patterns don't change while you are editing the code, until you hit `ctrl-enter` again. There is a disconnect between code and process that might take a little getting used to.

## The dollar

You might wonder what that dollar symbol `$` is doing. If you are not wondering this, you are safe to skip this explanation.

The dollar actually does almost nothing; it simply takes everything on its right hand side, and gives it to the function on the left. If we take it away in the following example, we get an error:

```haskell
d1 sound "bd sn"
```

That's because Tidal^[well, Tidal's underlying language, Haskell] reads from left to right, and so gives the `sound` to `d1`, before `sound` has taken `"bd sn"` as input, which results in confusion. We could
get the right behaviour a different way, using parenthesis:

```haskell
d1 (sound "bd sn")
```

This makes sure `sound` gets its pattern before it is passed on to d1. The dollar is convenient though
because you don't have to match the closing bracket, which can get fiddly when you have lots of patterns
embedded in each other.

```haskell
d1 $ sound "bd sn"
```

# Meta-functions

