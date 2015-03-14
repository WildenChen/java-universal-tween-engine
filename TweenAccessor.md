# Introduction #

The **TweenAccessor** interface lets you interpolate **any attribute** from **any object**. Just implement it as you want and register it to the engine.

  * See the TweenAccessor.java **[source code](http://code.google.com/p/java-universal-tween-engine/source/browse/tween-engine-api/src/aurelienribon/tweenengine/TweenAccessor.java)**

Here is the content of this interface (without the javadoc):

```
public interface TweenAccessor<T> {
    public int getValues(T target, int tweenType, float[] returnValues);
    public void setValues(T target, int tweenType, float[] newValues);
}
```

# How to implement that #

**1.** In the `getValues()` method, you need to modify the "returnValues" float array that is given in parameters with values corresponding to the given tweenType. For instance, if tweenType corresponds to a rotation, you need to return the current value or your object rotation:

```
public int getValues(Sprite target, int tweenType, float[] returnValues) {
    switch (tweenType) {
        case ROTATION:
            returnValues[0] = target.getRotation();
            return 1;
    }
}
```

Note that you have to return the amount of modified array cells (in the case of rotation, there is only 1 modification).

**2.** In the `setValues()` method, you need to update your object attributes with the given values:

```
public void setValues(Sprite target, int tweenType, float[] newValues) {
    switch (tweenType) {
        case ROTATION:
            target.setRotation(newValues[0]);
            break;
    }
}
```

# Example #

For a complete implementation example, please refer to the GetStarted page.