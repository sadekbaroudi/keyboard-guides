# Keyboard Case Design

## Overview

In this guide, I'll be providing an detailed walkthrough of how I design 3D models for cases, with the intent of 3D printing them. This starts with a completed keyboard pcb from the kicad files, and ends with a fusion 360 model that you can export as an stl/3mf for 3D printing. For this guide, I'll be using the chocolad as an example. As of writing this sentence, I haven't designed the case yet!

https://github.com/jimmerricks/chocolad

## Prerequisites

* You have a basic understanding of kicad
* You have inkscape installed (only used to convert svg to dxf in a way that fusion 360 can consume)
* You have fusion 360 installed

## Follow along

I have included the kicad_pcb file used in this example. So, if you'd like to follow along with the instructions below, you can create the same exact case I did! I find that it's a good way to validate what you're reading, and gives you experience for the next case you make.

Navigate to the [src](src/) directory to see the files.

## Walkthrough

### Kicad

Kicad is a perfect place to start, as it provides you with exact dimensions to create a case. This includes, but is not limited to:

* PCB outline
* Switch locations
* Reset switch location
* On/off switch location (for wireless builds)
* Controller location and dimensions
* Rotary encoders
* OLED
* etc

---

### PCB footprints

So, let's first look at the pcb to make sure it has everything we need to consider when doing the initial sketch for the case. You'll want to hide the copper layers so you can clearly see the pcb and footprints to identify what you care about for the case.

![01-pcb-footprints.png](images/01-pcb-footprints.png)

---

### Case considerations

There are 5 considerations when looking at this particular PCB:  
1) The keyswitch cutouts  
2) The edge cuts of the pcb  
3) The mounting holes for the M2 standoffs  
4) The opening for the controller, trrs, reset switch  
5) The EC11 encoders  

![02-pcb-case-considerations.png](images/02-pcb-case-considerations.png)

---

### Deselect layers

At this point, you want to deselect layers until you have everything you need, while keeping as little showing as possible otherwise. In this case, `User.Eco2`, `Edge.Cuts`, and `F.Courtyard` leaves us with almost everything. The only thing missing is the mounting holes.

![03-deselect-layers.png](images/03-deselect-layers.png)

---

### Update footprints 

There was no good layer to expose the mounting holes without exposing way too much from the pcb. It's important not to expose to much, because we'll be exporting this as an SVG. By leaving too much, you have to do a lot more cleanup in SVG later. This is a tedious process, so let's add some drawings to the mounting hole footprints using one of the layers that we've identified in a previous step.

![04-edit-footprint.png](images/04-edit-footprint.png)

So, here we edit the footprint and add a circle centered around the mounting hole on the `User.Eco2` layer, since we're exporting that already. Do this for each of the mounting holes.

![05-update-footprint.png](images/05-update-footprint.png)
![06-save-footprint.png](images/06-save-footprint.png)
![07-final-pcb-svg.png](images/07-final-pcb-svg.png)

---

### Export SVG

Now, we export an SVG and include only the layers we identified earlier.

![08-export-svg-layers.png](images/08-export-svg-layers.png)
![08-export-svg.png](images/08-export-svg.png)

---

### Save as DXF

You don't have to use inkscape, but I've found that inkscape is very reliable in giving us a usable DXF file for Fusion 360. Use the R14 format, and make sure it's set to mm as the unit.

![09-save-as-dxf.png](images/09-save-as-dxf.png)

---

### Enable capture design history

If you already use Fusion 360, you may already have this enabled, but if you don't, it's really good to enable this before you get started. Otherwise, it will be hard to make adjustments later!

![10-capture-design-history](images/10-capture-design-history)

---

### Import DXF into Fusion 360

Now we go into Fusion 360 and import the DXF we just saved from Inkscape.

![10-insert-dxf.png](images/10-insert-dxf.png)

---

### Check switch hole size

Check the size of the switch holes. For choc switches, they should be 13.95mm. For MX switches, 14mm works great. If you are doing this for a pcb that supports both choc and MX, 14mm is fine. It feels a bit loose for choc switches, but it'll still work fine.

![11-switch-hole-too-small.png](images/11-switch-hole-too-small.png)

---

### Adjust switch holes size

If they are not correct, select all the switch holes, and offset them by whatever distance you need to make them 13.95mm or 14mm.

![12-offset-switch-holes.png](images/12-offset-switch-holes.png)
![13-offset-switch-holes-amount.png](images/13-offset-switch-holes-amount.png)

---

### Switch hole size confirmation

Now confirm that the switch holes are correct!

![14-confirm-switch-dimension.png](images/14-confirm-switch-dimension.png)

---

### Fix edges

Look around the perimeter of the board and make sure it's a continuous shape. If you see anything that needs some adjustment, fix it in the sketch. In this case, there is an extra curve that we don't want, so let's remove that and make it a straight line.

![15-fix-curve.png](images/15-fix-curve.png)

---

### Mounting hole cleanup

Now we need to adjust the mounting hole size to be correct. Most boards use M2 standoffs, so the hole for the screws here should have a diameter of 2.5mm. Draw a line down the center to identify the center point. Then place a point in the center. Once you do that, you can delete the existing circle and line.

![16-mounting-line.png](images/16-mounting-line.png)
![17-center-point.png](images/17-center-point.png)
![18-delete-circle.png](images/18-delete-circle.png)

---

### Create new mounting holes

Now that you have a point, draw a center circle around that point, with a diameter of 2.5mm. Repeat this for all of the mounting holes on the board.

![19-create-mounting-hole.png](images/19-create-mounting-hole.png)

---

### Case outline offsets

At this point, we want to select the perimeter of the case, and offset it by about 1mm. If you don't do this, the pcb may not fit in the case, as you want to leave room for error from the PCB manufacturing process.

![20-offset-pcb-inner-wall.png](images/20-offset-pcb-inner-wall.png)

In the last step, we created the case **inner** wall. Now we need to create the outer wall. You should do the same exact thing you did in the previous step, but increase the offset value. If you want a 2mm thick wall in your case, select 3mm. If you want larger, increase the value as much as you like!

![21-offset-pcb-outer-wall.png](images/21-offset-pcb-outer-wall.png)

---

### Switch plate extrude

At this point, we want to extrude the plate. You should select everything but the switch holes, mounting holes, and any other area that protrudes above the switch plate. In this case, we want to exclude the region that has the controller, TRRS, and reset switch. This should generally be a thickness of 1.6mm. You can go thicker, but I don't recommend going higher than 2mm.

![22-extrude-plate.png](images/22-extrude-plate.png)

---

### Wall extrude

Now you want to extrude the walls in the region we offset earlier. The height of this is very important. You don't want to go too short, but you don't want to go much taller than you need. My general rule of thumb is:
* For choc switches, 3mm if soldered, and 4.5mm if hotswap
* For MX switches, 6.5mm if soldered, 8mm if hotswap

You can experiment with values until you get what you're looking for.

![23-extrude-walls.png](images/23-extrude-walls.png)

---

### Add style and texture

I usually like to add some texture to the outline, but this is a stylistic preference, so feel free to adapt it, or skip this step if you don't mind the sharp edges.

![24-chamfer.png](images/24-chamfer.png)
![25-chamfer-complete.png](images/25-chamfer-complete.png)

---

### Adjust for other components (measure and draw)

So, admittedly, I forgot to account for the depth of the trrs cable in the case. That said, this actually serves as a great example to illustrate how to solve for it. I measure from a known point on the pcb outline, and look at the Y values (in this case). I check to the bottom edge of the TRRS port, and the top edge of the TRRS port. Then I offset by a couple of mm or so to make sure there is enough clearance for the TRRS cable end.

![26-trrs-measure-part1.png](images/26-trrs-measure-part1.png)
![27-trrs-measure-part2.png](images/27-trrs-measure-part2.png)
![28-trrs-lines-part1.png](images/28-trrs-lines-part1.png)
![29-trrs-lines-part2.png](images/29-trrs-lines-part2.png)

---

### Adjust for other components (extrude)

Now that we have drawn the lines and created a region to extrude, we extrude down to give room to plug in the cable.

![30-trrs-extrude.png](images/30-trrs-extrude.png)

---

### Create the bottom plate sketch

Last step is to create the bottom plate, which you will use to screw in to the body using the mounting holes. This part is very easy! You go back and edit the original sketch. Then you copy the border of the case and the mounting holes.

![31-copy-bottom-plate-sketch-part1.png](images/31-copy-bottom-plate-sketch-part1.png)
![32-copy-bottom-plate-sketch-part2.png](images/32-copy-bottom-plate-sketch-part2.png)

---

### Extrude the bottom plate

Once you have the plate sketch, you extrude by whatever thickness you like (2mm is a very good sweet spot, but I sometimes do 1.6 for a slighly lower profile keyboard)

![33-extrude-bottom-plate.png](images/33-extrude-bottom-plate.png)

---

### Chamfer mounting holes (optional)

If you are using countersunk screws, you can optionally decide to chamfer the mounting to help accommodate that. It helps to create a flush bottom plate. I generally consider this unnecessary, since you will usually use bump ons, which protrude farther than M2 screw heads. But if you'd like to experiment with this, see the images below as an example. The example is using a 1.6mm bottom plate, which I generally wouldn't recommend if doing this. I'd suggest using at least a 2mm thickness bottom plate.

![34-chamfer-mounting-holes-part1.png](images/34-chamfer-mounting-holes-part1.png)
![34-chamfer-mounting-holes-part1.png](images/34-chamfer-mounting-holes-part2.png)

---

### Complete case

You are done! Export this as an stl, print it, and do a test fit. If you need to make adjustments, you already know all the steps above. Go back into Fusion 360 and edit using the timeline.

![34-completed-case-part1.png](images/34-completed-case-part1.png)
![35-completed-case-part2.png](images/35-completed-case-part2.png)
