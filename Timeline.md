# Introduction #

A Timeline can be used to sequence multiple tweens. They will be automatically delayed so they can be executed one after the other.

  * See the Timeline.java **[source code](http://code.google.com/p/java-universal-tween-engine/source/browse/tween-engine-api/src/aurelienribon/tweenengine/Timeline.java)**

# Using a Timeline #

The following example will move the target horizontal position from its
current location to x=200, then from x=200 to x=0, and finally from
x=0 to x=500, but this last transition will only occur 1000ms after the previous one.

```
Timeline.createSequence()
    .push(Tween.to(myObject, POSITION_X, 0.5f).target(200))
    .push(Tween.to(myObject, POSITION_X, 0.5f).target(0))
    .pushPause(1.0f)
    .push(Tween.to(myObject, POSITION_X, 0.5f).target(500))
    .start(myManager);
```

# Nested timelines #

Timelines can nest tweens but also other timelines. For instance:

```
Timeline.createSequence()
    // First, set all objects to their initial positions
    .push(Tween.set(...))
    .push(Tween.set(...))
    .push(Tween.set(...))

    // Wait 1s
    .pushPause(1.0f)

    // Move the objects around, one after the other
    .push(Tween.to(...))
    .push(Tween.to(...))
    .push(Tween.to(...))

    // Then, move the objects around at the same time
    .beginParallel()
        .push(Tween.to(...))
        .push(Tween.to(...))
        .push(Tween.to(...))
    .end()

    // And repeat the whole sequence 2 times
    .repeatYoyo(2, 0.5f)

    // Let's go!
    .start(myManager);
```

# Options #

Many options can be added to your timelines. Most of these optional methods return the current timeline, so you can chain them in one line.

  * `.addCallback(TweenCallback callback)`: adds a callback to the timeline. There are many available callbacks, to listen for the end of a timelineiteration, for its full completion, etc.
  * `.repeat(int count, int delayMillis)`: repeats the timeline for a given number of times.
  * `.repeatYoyo(int count, int delayMillis)`: repeats the timeline for a given number of times with a yoyo style.

Of course, nested timelines also support all these options.