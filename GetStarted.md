# The different steps #

The most common way to create an interpolation is the following:

  1. **Implement the TweenAccessor interface** for your objects, and **register the implementations**, so the engine will know them,
  1. **Create a TweenManager**, to easility handle their life-cycles and updates of your tweens and timelines,
  1. **Create some tweens** with `Tween.to()`, `Tween.from()` or `Tween.set()` and **add them to the manager**,
  1. (repeat) **Call the `update(int delta)` method** of your manager periodically.

# Example (with code) #

Let's say that we have some particles drawn, and we want to move them around with acceleration and/or deceleration. We'll use the Tween Engine of course. But before any tween can be applied on our particle, we must tell the engine how to tween those particles: **we need to make the particles tweenable !**

## The Particle class ##

This Particle class is very simple (for the example) and only defines a position with an "x" and an "y" field, like this:

```
public class Particle {
    private float x, y;
    
    public float getX() {
        return x;
    }
    
    public float getY() {
        return y;
    }

    public void setX() {
        this.x = x;
    }

    public void setY() {
        this.y = y;
    }
}
```

## Making the particles tweenable ##

The recommended way is to **create a new class which implements the TweenAccessor interface**.
Of course, you can also directly implement this interface in the Particle class, but the philosophy of the Tween Engine is to let your code unchanged.

```
// We implement TweenAccessor<Particle>. Note the use of a generic.

public class ParticleAccessor implements TweenAccessor<Particle> {

    // The following lines define the different possible tween types.
    // It's up to you to define what you need :-)

    public static final int POSITION_X = 1;
    public static final int POSITION_Y = 2;
    public static final int POSITION_XY = 3;

    // TweenAccessor implementation

    @Override
    public int getValues(Particle target, int tweenType, float[] returnValues) {
        switch (tweenType) {
            case POSITION_X: returnValues[0] = target.getX(); return 1;
            case POSITION_Y: returnValues[0] = target.getY(); return 1;
            case POSITION_XY:
                returnValues[0] = target.getX();
                returnValues[1] = target.getY();
                return 2;
            default: assert false; return -1;
        }
    }
    
    @Override
    public void setValues(Particle target, int tweenType, float[] newValues) {
        switch (tweenType) {
            case POSITION_X: target.setX(newValues[0]); break;
            case POSITION_Y: target.setY(newValues[0]); break;
            case POSITION_XY:
                target.setX(newValues[0]);
                target.setY(newValues[1]);
                break;
            default: assert false; break;
        }
    }
}
```

Before anything else, don't forget to **register this new TweenAccessor implementation to the engine**, else it won't know it:

```
Tween.registerAccessor(Particle.class, new ParticleAccessor());
```

## Let's move the particles! ##

It's over, we can now easily apply interpolations directly on our particles.

In the initialization code:
```
// We need a manager to handle every tween.

manager = new TweenManager();

// We can now create as many interpolations as we need !

Tween.to(particle1, ParticleAccessor.POSITION_XY, 1.0f)
    .target(100, 200)
    .start(manager);
    
Tween.to(particle2, ParticleAccessor.POSITION_XY, 0.5f)
    .target(0, 0)
    .ease(Bounce.OUT)
    .delay(1.0f)
    .repeatYoyo(2, 0.5f)
    .start(manager);
```

In a loop (usually the render loop in games):
```
manager.update(ellapsedSeconds);
```