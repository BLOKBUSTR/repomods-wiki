# Creating Enemies with REPOLib SDK

::: info NOTE
**This guide assumes you have set up a REPOLib Unity project for REPOLib modding, *and* a HarmonyX project for custom scripting.\
If not, follow [HarmonyX Project Setup](./../../../../harmonyx.md) and [Unity Project Setup](./../../../../unity.md) first.**
:::

Creating custom enemies in R.E.P.O. is a complex and intricate task, requiring lots of attention and care. This guide aims to provide a basic overview for how to set up the most necessary components and scripts for your enemy to function.

## 1. Create the Enemy Prefab

Create a Unity prefab for your Enemy. This process is elaborated in the [Enemy Prefab](prefab.md) page.

**Vanilla Enemies** are located in `Assets/REPO/Game/Resources/enemies`. You can simply copy a prefab to your mod folder to use as a template.

::: tip TIP
Since this guide may not cover *every* aspect of creating a custom enemy, feel free to study and reverse-engineer vanilla enemies' prefabs and scripts yourself to learn how their systems and components work altogether.
:::

## 2. Create the EnemySetup Asset

1. Create an `EnemySetup` asset by right-clicking and choosing `Create > Other > Enemy Setup`. The default name of the asset is `Enemy - _____`; replace the underscores with your enemy's name.
2. Configure the fields:
   - *Leave the `Spawn Objects` list empty.*
   - `Levels Completed Condition`: Whether your enemy should only spawn within a specific range of levels. Configure the following `- Min` and `- Max` sliders to set the range.
   - `Rarity Preset`: The rarity of your enemy. There are currently only two presets: "**Very Common**" for all typical enemies, and "**Very Rare**" for enemy groups.
   - `Runs Played`: How many runs have to be played before your enemy can spawn (in other words, how many times you've seen the Game Over screen).

## 3. Create the REPOLib Enemy Asset

1. Right click in your mod folder (or any subfolder) and choose `Create > REPOLib > Enemy`. Configure the fields:
   - `Setup`: The reference to the `EnemySetup` asset you created prior.
   - `Spawn Objects`: A list of prefabs that will be spawned when your Enemy is chosen. Usually, you'll only need to add your single Enemy prefab here. There are a couple cases where you'd want to assign multiple prefabs to this list:
      - Grouped enemies such as Gnomes or Bangers. These also come with their own Director, which coordinates all such enemies.
      - Enemy Groups, which contain multiple instances of the same enemy that rarely replace one Difficulty 3 spawn.
