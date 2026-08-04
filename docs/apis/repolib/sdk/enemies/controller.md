# Creating the Controller Script

This guide demonstrates how to write a custom script in a similar style to R.E.P.O., while aiming to flexibly accomodate your own ideas. You may choose to follow this guide as closely or loosely as you want.

## State management

R.E.P.O.'s enemies use a simple form of a [finite state machine](https://en.wikipedia.org/wiki/Finite-state_machine), which controls the flow of their actions based on certain conditions.

Start by defining an `enum` containing all your enemy's states, and a variable holding the current state:

```C#
public class YourEnemy : MonoBehaviour
{
    // ···
    
    // [!code focus:15]
    // This is an example of the bare minimum required for a basic enemy.
    private enum State
    {
        Despawn,
        Spawn,
        Idle,
        Roam,
        Investigate,
        Notice,
        Chase,
        Attack,
        Stun,
        Leave
    }
    private State currentState;
    
    // ···
}
```

skibidibadabble



yabbadabadabadoo

::: tip TIP
While `Vector3.Distance` is useful, it *can* incur a small performance penalty due to calculating a square root to return the final distance. Few calculations like this may seem insignificant at first, but they can add up to bigger losses when done very frequently per frame, and by many different scripts running in tandem.\
Instead, you can utilize the `sqrMagnitude` between the two vector positions, and compare to a squared value:
```C#
public PlayerAvatar playerTarget;
public float distance;

private void DistanceExample()
{
    Vector3 playerPos = playerTarget.transform.position;
    
    if (Vector3.Distance(transform.position, playerPos) < 2f) // [!code --]
    {
        // Slightly inefficient due to the square root calculation.
    }
    
    if ((transform.position - playerPos).sqrMagnitude < distance * distance) // [!code ++]
    {
        // Small optimization with sqrMagnitude.
    }
    
    if ((transform.position - playerPos).sqrMagnitude < 2f * 2f) // 4f [!code ++]
    {
        // Hard-coded numbers are also applicable. They are automatically
        // evaluated to the squared value at compilation, adding no overhead.
        // You may also write single numbers, if preferred.
    }
}
```
:::
