# Creating the Enemy Prefab

[Blender]() is **highly** recommended if you want to create your own model for your enemy.\
You can also ask a friend for assistance, or obtain a model online (**ensuring you follow its license correctly**).

::: warning NOTE
Metallic material properties work a bit differently in R.E.P.O. They are used as intentional darkening on certain parts
of a texture. You can find good examples of this on Loom, Birthday Boy, Bangers, and most other enemies.

> Screenshot

If you accidentally use a normal Metallic texture the way you'd expect, your model may appear much darker than intended,
or black altogether.

::: tip TIP
To preview a Metallic/darkening texture accurately in Blender, you can connect it to the "Fac(tor)" property on a Mix Shader
node, with one input connected to your Principled BSDF, and the other unconnected. The unconnected socket will simply
appear as pure black.

> Screenshot

:::

↑ Interesting...??
