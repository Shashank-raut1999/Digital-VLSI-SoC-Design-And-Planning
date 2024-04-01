# Day 4 :  Pre-layout timing analysis and importance of good clock tree

## Our task is to find the errors in the layout and then fix them so that this layout can be plugged in the *picorv32a* process flow :
- While doing placement, we need the layout inforamtion which is present in **lef** file. So we need to get this lef file first.
## Guidelines to be followed while making a standard cell :
1. The input and the output ports must lie on the intersection of the vertical and the horizontal tracks.
2. The width of the standard cell must be in the odd multiple of the track pitch and height must be in the odd multiples of the vertical pitch.

