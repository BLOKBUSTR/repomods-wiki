# Creating the Animator Script

In game design, it is advisable to keep your logic and visuals separate.\
Usually, the logic must only be concerned about *its own functions* that the player does not acknowledge on the surface, and must not know anything about the visuals. On the other hand, the visuals may know *everything* about the current states of the logic, and *acts accordingly*. The two must not interfere with each other.

Albeit, R.E.P.O. does not strictly adhere to this standard. However, there are a few sensible exceptions:
- Loom's arms track the player's position when she is about to clap them.
