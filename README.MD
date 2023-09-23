This is a small group of macros that implements Prusa’s Build Sheet system and Adjust Live Z behavior in klipper. Specifically:

You can have as many build sheets as you like, each with a unique human readable name
Each sheet has its own Z offset
A sheet can be installed in the printer and this persists across restarts.
When you “Adjust Live Z” (AKA Baby Stepping) with your klipper UI of choice the offset change is saved instantly to the sheet that is installed.
Anything thats not a true “Adjust Live Z” call wont change the sheet’s z-offset value. You need to set MOVE=1 and ADJUST_Z= in your call to SET_GCODE_OFFSET. This is how Fluidd, Mainsail & Klipper Screen etc do it. All other call patterns wont change the sheet offset.

Requirements:

You have [save_variables] set up
You need to call the APPLY_BUILD_SHEET_ADJUSTMENT in your PRINT_START macro after you calibrate your Z offset. This is when the sheet offset is applied.
Last but most important: Your PRINT_START macro must set your Z Offset. You can do this with a plugin like CALIBRATE_Z 65. Or, you can add a line to your PRINT_START to reset the Z offset to the correct value for your printer: SET_GCODE_OFFSET Z=0.0. This macro doesn’t try to keep track of the state of the z offset, that’s on you. If you don’t do this the APPLY_BUILD_SHEET_ADJUSTMENT call will apply the same adjustment multiple times and you will crash your printer.

Nice To Have:

You’ll probably want to make some small macros for each build sheet you have to make swapping them easy. E.g. :

[gcode_macro INSTALL_TEXTURED_SHEET]
gcode:
    INSTALL_BUILD_SHEET NAME="Textured PEI Sheet"

    In your PRINT_START you can add a call to SHOW_BUILD_SHEET at the start and end. This prints the build sheet name and offset to the console. This can be helpful and remind you if you forgot to swap sheets!
