# Havok Animation Exporter

**Havok Animation Exporter (HAE)** is a tool that allows you to generate skeleton and animation data for Sonic games that use Havok. (It may also work for non-Sonic games.)
This is a fork that adds Root Motion export support for Havok Version 5.5.0.

## Usage

```
HavokAnimationExporter [source] [destination] [options]
```

### Options

* `-s` or `--skl`
    * Path to the skeleton HKX file when generating animation data.  
Example: `-s chr_Sonic_HD.skl.hkx`

* `-u` or `--uncompressed`

    * Outputs uncompressed animation data. Disabled by default.  
Enable this if spline-compressed animations cause flickering in-game.

* `-f` or `--fps`

    * Frames per second for animation generation. Defaults to 60.  
Example: `-f 60`

* `-w` or `--windows`

    * Converts output for Windows.  
Enabled by default for Havok 2010 2.0 and Havok 2012 2.0.  
Available as an option for Havok 5.5.0.

* `-x` or `--xbox360`

    * Converts output for Xbox 360.  
Enabled by default for Havok 5.5.0.

* `-p` or `--ps3`

    * Converts output for PS3.

* `-w` or `--wiiu`

    * Converts output for Wii U.  
Available only for Havok 2012 2.0.

* `-t` or `--tagfile`

    * Saves the output in tagfile format.  
Tagfiles work on all platforms.  
Available for Havok 2010 2.0 and Havok 2012 2.0.

* `-m` or `--motion`

    * Include Root Motion Data when exporting animations
Available only for Havok 5.5.0.

* `-ac` or `--armatureChild`

    * Name of a bone that's a child to the armature for root motion exports.
Will default to searching for `Reference` if not given.
Available only for Havok 5.5.0.

### Examples

To generate skeleton data, simply provide the FBX file:

```
HavokAnimationExporter chr_Sonic_HD.fbx chr_Sonic_HD.skl.hkx
```

To generate animation data, also specify the skeleton HKX file using `--skl`:
```
HavokAnimationExporter --skl chr_Sonic_HD.skl.hkx sn_idle_loop.fbx sn_idle_loop.anm.hkx
```

Alternatively, you can use the included `.bat` files from the release packages and adjust the skeleton HKX file paths in them as necessary.

## Versions

The tool supports multiple Havok SDK versions. Currently, 3 SDKs are implemented:

### Havok 5.5.0 (Sonic Unleashed)

By default, output files are saved in Xbox 360 format. You can export for Windows or PS3 by providing the respective command-line option.

Skeleton files in Windows or Xbox 360 format are required for animation conversion. Xbox 360 skeletons are automatically endian-swapped without requiring you to do it manually.

> [!WARNING]
> Unleashed Recompiled requires files in Xbox 360 format. Do not export for Windows.

#### Adding New Bones

Sonic Unleashed, unlike later games, is not compatible with existing animations when new bones are added to the original game skeletons.

To solve this, add the `@NEW` tag to the names of new bones.
HAE will push these new bones to the end of the bone list, preserving the rest pose for existing animations while allowing custom animations to target the new bones.

> [!WARNING]
> You must use the `@NEW` tag when exporting both skeletons and animations.
If the tag is missing during animation export, HAE will not be able to locate the new bones.

#### Root Animation

This fork allows for Root Animation Data to be included in exports.
As an example for creating an animation with Root Motion for the Werehog you must:
* First animate the motion you want onto the reference bone (E.G animate the reference bone moving forward if you want the character to move forward during the animation).
* Add an animation to the root armature (in this case `EvilRoot`) with the exact opposite motion (E.G If `Reference` is moved forward, move the armature backwards the exact same amount so it looks like the character isn't moving at all).
* Finally convert the .FBX to .HKX using the exporter while using the `-m` or `--motion` option.

When creating an animation that has a rotation motion, only animate the rotation onto the root armature without adding animation to the Reference bone.
If you're making an animation for a rig that doesn't have a `Reference` bone, use the `--armatureChild` option to specify a different bone that is a child to the armature root.

### Havok 2010 2.0 (Sonic Generations)

By default, output files are saved in Windows format. You can export for Xbox 360 or PS3 by providing the respective command-line option. Alternatively, saving in tagfile format will produce a cross-platform file.

Skeleton files must be either in Windows format or tagfile format for animation conversion.

### Havok 2012 2.0

#### Sonic Lost World

By default, output files are saved in Windows format. You can export for Wii U by providing the respective command-line option. Alternatively, saving in tagfile format will produce a cross-platform file.

Skeleton files must be either in Windows format or tagfile format for animation conversion.

#### Sonic Forces

By default, the game cannot load files saved directly by HAE. You must explicitly save the resulting files in tagfile format. Alternatively, you can port your HKX files to Sonic Forces' format using [TagTools.](https://github.com/blueskythlikesclouds/TagTools)

Before generating animation data, you must backport your skeleton HKX file using [TagTools.](https://github.com/blueskythlikesclouds/TagTools) **Be careful not to accidentally use the backported HKX file in-game.** 

If you forward-port your custom skeleton file with [TagTools](https://github.com/blueskythlikesclouds/TagTools), also **ensure that you do not provide the forward-ported HKX file to this tool**.