# How to use the Tween Engine with pure Android applications #

The only requirement of the Universal Tween Engine to be able to compute anything is that **the Tweens and Timelines you create are updated regularly (on each frame)**.

In most situations, you should use a `TweenManager` to handle the update process of all your tweens. Therefore, all you need to do is to call the `.update()` method of the manager on each frame. For android games, this is really easy to do since every game has a "game loop", an infinite loop which recomputes and redraws the world on each frame. Therefore, all you need to do is to insert the manager update call in this loop, and voilà. However, pure android UI applications do not expose their loop, so you cannot just insert your update call anywhere.

What you should do however is to create a separate thread with an infinite loop inside it, and put the `manager.update()` method inside it. It should work like a breeze :)

First you need to create a manager somewhere:
```
private final TweenManager tweenManager = new TweenManager();
```

Also, you'll need a variable to be able to stop the animation thread when your application will close or pause:
```
private boolean isAnimationRunning = true;
```

Then you need to create the thread (in the Activity constructor for instance, or better: in the `onResume()` method):
```
new Thread(new Runnable() {
    private long lastMillis = -1;

    @Override
    public void run() {
        while (isAnimationRunning) {
            if (lastMillis > 0) {
                long currentMillis = System.currentTimeMillis();
                final float delta = (currentMillis - lastMillis) / 1000f;

                runOnUiThread(new Runnable() {
                    @Override public void run() {
                        tweenManager.update(delta);
                    }
                });

                lastMillis = currentMillis;
            } else {
                lastMillis = System.currentTimeMillis();
            }

            try {
                Thread.sleep(1000/60);
            } catch(InterruptedException ex) {
            }
        }
    }
}).start();
```

Then you can create tweens anywhere in your android UI code. The update call is surrounded with a `view.post(...)` as you can see, so the update will be done in the main Android UI thread and therefore you won't have any synchronization issue.

All you'll have to do to stop the animation thread when closing the application (or better: when pausing the application) is to set `isAnimationRunning` to false.

I never tried the engine with anything else than games, but this solution should be working without any problem. If it is not the case, please report :)

To get you started, asramzero contributed a nice project example: https://docs.google.com/open?id=0B8tFsMh6a8xISVhYb3FENXRaWEE