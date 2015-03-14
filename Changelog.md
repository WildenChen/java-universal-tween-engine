6.3.3
  * ! fixed a bug allowing users to manipulate tweens/timelines when pooled,
  * ! fixed a bug in timeline preventing some tweens to start under special condition,
  * ! fixed a bug in setCallback which reset the triggers to 'complete'

6.3.2
  * ! Fixed the GWT module definition

6.3.1
  * ! Fixed an issue with infinitely repeated tweens/timelines firing the complete/back\_complete events.

6.3.0
  * + **Greatly improved precision and reliability of callbacks**, especially for timeline children. Every single callback should now be correctly fired, even if the update delta is very big (due to some lag for instance)!
  * + **Added a GWT module definition** to the library!
  * + Library should now properly support multi-threading (but is still not thread-safe, by design).
  * + Added many methods to TweenManager to list its objects.
  * ! Removed java raw types from the API (sorry 'bout that)

6.2.0
  * + Added a lot more precision to timings by changing ints to floats.
  * + Added paths for tweens through the 'waypoint()' method, no more linear transitions!
  * + Added bezier paths (this is the default if you use waypoints).
  * + Added TweenPaths static collection for paths implementations.
  * + Added TweenEquations static collection for easing equations.
  * + Added delay() to timelines too.
  * + Added a completely new demo to promote the engine!
  * `*` Improved performances by removing a lot of useless computations.
  * `*` Greatly reduced the memory footprint of each tween object.
  * `*` Object pooling is now enforced everywhere.
  * `*` `[`api break`]` Callback registration was changed to reduce memory footprint.
  * ! fixed a few potential bugs with kill() method and with timelines.

6.1.1
  * ! fixed a bug preventing timelines from being removed when using TweenManager.killTarget().

6.1.0
  * + Added Tween.cast() to let you cast your targets to the class or interface you want, in order to use a specific TweenAccessor.
  * + Added pause()/resume() methods to Tween, Timeline and TweenManager.
  * + Added MutableFloat and MutableInteger as quick tweenable primitive types.
  * ! Fixed many minor bugs.

6.0.0
  * + **Zero allocation**! Use the engine safely in Android!
  * + Very **robust** engine, shouldn't lost a single millisecond in updates.
  * + You can now **apply tweens directly on your objects**, there is no need that they implement some interface!
  * + As a result, the Tweenable interface is no more. It has been replaced by a **TweenAccessor** that can be registered statically to the engine.
  * + Repetitions can now be played in **'yoyo' mode**: every odd iteration is played backwards.
  * + **Callbacks** were all changed, you have BEGIN, START, END, COMPLETE, and similar ones for backwards playing.
  * + TweenGroup is no more: welcome **'Timeline'**.
  * + Timelines can be repeated, yoyo style too.
  * + Nested timelines can be repeated too!
  * + Timelines now support callbacks! And all the callbacks used in Tween are working in Tiemlines!
  * + Several protections were added to prevent you from misusing the engine.
  * + **Extensive javadoc** was added to every public classes and methods!