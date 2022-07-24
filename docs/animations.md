# .animation files

.animation files are files used by the engine to animate textures.
They can be opened in any text editor. If you want to use a graphical user interface to build animations you can use [Keristero’s sprite tool](https://keristero.github.io/spritesheet-tool/), but read on if you want to know the technical details.

Animation files will have one or more animation states defined in them.

```
animation state="IDLE_U"
```

**animation**: Indicates the start of a new animation

**state**: Name of the animation, in this case IDLE_U. If its a player animation, you'll need to use the standard player states.

```
frame duration="0.05" x="0" y="0" w="100" h="100" originx="50" originy="100" flipx="1" flipy="0"
```

**frame**: Indicates a new frame in the animation. Must come after an animation state

**duration**: The amount of time in seconds to hold the frame

**x**: Starting point on the sheet for the upper left corner of your sprite’s x

**y**: Starting point on the sheet for the upper left corner of your sprite’s y

**w**: Width of the frame

**h**: Height of the frame

**originy**: y coordinate within the frame, used to align the placement of the frame

**originx**: x coordinate within the frame, used to align the placement of the frame

**flipy**: Flip along the y axis 0=false 1=true. Optional

**flipx**: Flip along the x axis 0=false 1=true. Optional

flipx and flipy are not required but can be useful to cut down on workload for sprites that can be flipped!

point label="Head" x="29" y="13"

**point** : Used to assign overlay points. Zero or more can be defined for a frame. Must come after a frame
**label**: Name of the point. See Battle Animations for points used by battles
