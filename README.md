# CadPad
A portable macropad designed for use with common CAD software, such as Fusion and SolidWorks.

## Table of Contents
- [Components](#Components)
- [Wiring](#Wiring)
- [Firmware](#Firmware)
- [Case](#Case)

## Components
This board uses relatively standard parts and tools, a good portion of which can be found at [Typeractive](https://typeractive.xyz).
### Parts
1. 10x Kailh Choc White keyswitches
2. 10x MBK Choc keycaps
3. nice!nano v2
4. 110mAh battery, no connector
5. Reset button
6. Power switch
7. 18 ga. solid core copper wire
8. 30 ga. wire
9. Header pins (may be included with the nice!nano)
10. 10x 1N4148 diodes
11. 10x M2x5 screws
12. 5x M2x6 standoffs
13. Heat shrink tubing (probably need 3/32" or 2.4mm)
14. Electrical tape
15. 6x2mm bumper feet (optional)

### Tools
1. Soldering iron (I use the Pinecil)
2. Solder
3. Screwdriver
4. Tweezers (not necessary but VERY helpful)
5. Computer
6. USB-C cable
7. 3D Printer (again, not strictly necessary but good to have to manufacture the case)
8. Drill and pliers (good to have for straightening wire)
9. Lighter/heat source
10. Sharpie

## Wiring
At a high level, wiring involves enableing the board to form a loop of current that can flow in one direction when a key is pressed. The following is how to achieve that.

Print out the keyplate, this is used to hold everything in place while we solder. Cut about 45cm of wire, create a small loop at one end, and place it in the drill.
Then hold the other end with pliers, and while applying some force, turn on the drill until the wire is straight. 

Place your keycaps into the keyplate (any orientation of the keyplate
is fine, as long as the switches all face the same direction). Insert the header pins into the slots and solder the nice!nano to them. Take care to remember the board's orientation, pinouts can be found 
[here](https://nicekeyboards.com/docs/nice-nano/pinout-schematic/). Cut five pieces of copper wire just short enough to span the rightmost pins in each column, and with the pins towards the bottom,
solder them to each right side pin. It should look something like this: 
![IMG_5481](https://github.com/user-attachments/assets/3228ce63-f4fb-436a-ad1f-9d79e15fb6a5)

Tin the remaining pins with solder,and clip the leg of each diode with no black line to about 5mm (IMPORTANT: DIODES ARE POLARIZED COMPONENTS. 
PAY ATTENTION TO THE DIRECTION). Solder the clipped leg of each diode to a tinned pin. 

Cut two pieces of copper wire wide enough to span the diode rows. For the top row, simply bend each
diode over the copper wire and solder in place. For the bottom row, use a sharpie or other marker to mark where the row line makes contact with each column. Wrap each mark with a small
bit of electrical tape and seal with heat shrink. Solder into place, ensuring that the overlap lies on the heat shrink to prevent electrical shorts.

Solder the positive terminal (red lead) of the battery to the power switch. On the one from typeractive, use the third pin along the bottom row. Then solder the output of the switch to
the RAW pin on the nice!nano using a bit of 30 ga. wire. On the switch from typeractive, this is the second pin along the bottom row.

Solder one side of the pushbutton to the RST pin and the other to GND. On the switch from typeractive, use one pin close to the button and the one below it.

Solder each row and column (each piece of copper) to its own unique GPIO pin on the headers using the 30 ga. wire. While the position of the wire on the copper doesn't matter, take note of which pins you use, you'll need them later. Nest the battery between the
header pins, and if necessary, secure with a piece of double sided tape. Glue the power switch and reset buttons to the underside of the case. Their positioning is not particularly important.

Here is a reference picture of a completed board: 
![IMG_5486](https://github.com/user-attachments/assets/47fe7e28-ed2f-4125-9353-87cd94ae7987)

## Firmware
This board uses ZMK, which allows it to operate both wirelessly and over USB. I like to use terminal and GitHub actions to create my ZMK firmware. 

To start, follow the instructions [here](https://zmk.dev/docs/user-setup-cli) to create a new keyboard. Make sure to make it your default repo. For the controller board select pro_micro. In a code editor, open the files that 
ZMK has created locally on your computer. You'll want to edit keyboard-layouts.dtsi, keyboard.conf, keyboard.overlay, keyboard.zmk.yml, keyboard.keymap, and build.yaml, where "keyboard" in
the file names is a stand in for whatever name you chose during setup. A note when editing the .overlay file, the way the board is wired is col2row, but the pin labels for the nice!nano 
are different than what ZMK expects. Just use the numbers from the corresponding green labels in the pinout linked above. The rows should run top to bottom as you go down the code, and the 
columns will run right to left. Use the firmware files here as reference, but don't copy them, it probably won't work.

Once you've edited all your files, go back to terminal and run the following in order:

```zmk cd```

```git add .```

```git commit -m "COMMIT MESSAGE HERE"```

```git push```

Now go back to GitHub, and in your repo click on the actions tab. The firmware should be building there. Once complete, plug the nice!nano into your computer and double click the reset button.
Once the board mounts, drag and drop the firmware file onto the board. Wait for it to auto-eject, pair your board, and you're done!

## Case
The case is a simple sandwich design created in Autodesk Fusion. To assemble, insert a screw through each hole in the keyplate and attach a standoff to each. Then, screw the bottom plate into 
the standoffs. Attach the bumper feet and keycaps, and the keyboard is fully assembled!
