# Introduction #

A Tween defines an interpolation that will modify one or many attribute(s) of an object during a specified time. This modification can be done **smoothly** and with **various effects** easily, this is the main purpose of Tweens !

  * see the Tween.java **[source code](http://code.google.com/p/java-universal-tween-engine/source/browse/tween-engine-api/src/aurelienribon/tweenengine/Tween.java)**

# Different tweens for different goals #

The easiest way to create a tween is to use one of the static constructors:

  * `public static Tween to(Tweenable target, int tweenType, int duration)`
  * `public static Tween from(Tweenable target, int tweenType, int duration)`
  * `public static Tween set(Tweenable target, int tweenType)`
  * `public static Tween call(IterationCompleteCallback callback)`

Some explanations:
  * `Tween.to()` creates the most common tween. It interpolates between the current values and the target values.
  * `Tween.from()` creates a reverse tween. It interpolates between the target values and the current values.
  * `Tween.set()` defines an instantaneous tween. It will apply the target values directly, without interpolation.
  * `Tween.call()` does not define an interpolation, but helps you to create a simple timer.

Of course, `Tween.set()` and `Tween.call()` are best used with a delay applied to them. See below for options such as delays.

# Don't forget the targets ! #

The previously listed tweens are useless if you do not specify their target values. Just call the `.target()` method on your tweens. Since there are many overloads for this method, it is not included in the static constructors. Just don't forget it ;-)

The method returns the current tween, so you can easily chain it:

```
Tween.to(...).target(x, y, z);
```

# Options #

Many options can be added to your tweens. Most of these optional methods return the current tween, so you can chain them in one line.

  * `.ease(TweenEquation equation)`: sets the easing function of the tween.
  * `.delay(int delay)`: adds a delay to the tween.
  * `.setCallback(TweenCallback callback)`: sets a callback to the tween. There are many available callbacks, to listen for the end of a tween iteration, for its full completion, etc.
  * `.repeat(int count, int delay)`: repeats the tween for a given number of times.
  * `.repeatYoyo(int count, int delay)`: repeats the tween for a given number of times with a yoyo style.

# Combined Tweens #

Combined tweens are tweens that act on more than one attribute at a time. Just return a number higher than 1 in the `getValues()` method of the TweenAccessor interface for some tween type, and you can use Tween.to()/.from()/.set() with many attributes at a time.

```
Tween.to(myObject, MY_COMBINED_TYPE, 1.0f).target(value1, value2, value3);
```

Its a very powerful feature, especially useful when you want to tween, for instance, the position of an object as a whole XY position, and not with two tweens (one for X, one for Y).