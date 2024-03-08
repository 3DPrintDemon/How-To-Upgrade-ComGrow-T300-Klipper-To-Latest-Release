# How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release

[<img width="171" alt="kofi_s_tag_dark" src="https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/489afbbc-6f59-419a-8737-165c20351858">](https://ko-fi.com/3dprintdemon)

## Don't forget if you like & use this project you can buy me a beer/coffee to say thanks. [https://ko-fi.com/3dprintdemon](https://ko-fi.com/3dprintdemon)

## IF THIS PROCESS IS DONE INCORRECTLY IT WILL BRICK YOUR PRINTER! BEFORE YOU ATTEMPT THIS THE OUTCOME IS TOTALLY ON YOU! 

## I HIGHLY RECOMMEND YOU MAKE A WORKING BACKUP OF YOUR PRINTER &/OR A CLONE OF YOUR EMMC!
## YOU HAVE BEEN WARNED! 

### If you need some macros be sure to check out these!
https://github.com/3DPrintDemon/Demon_KLIPPER_Essentials
***********************************************************************************************************************************************************

before you update anything on the T300 you absolutely MUST be 100% sure that the kernel is locked/frozen to stop it being updated when you run any updates or installers.
Also note this is NOT for beginners, it requires some knowledge of the system you're working on to achieve success! Please be careful!

## NOTICE BEFORE YOU START!!

At the time of writing the T300 is set to use a different `virtual_sdcard` location than what is now REQUIRED in the latest version of Klipper. When updating the system the new versions all use the correct newly required location, but the printer's touchscreen does NOT! This is due to the screen's own Comgrow firmware, this means the screen continues to use the old now dead directory. This fact has been passed on to Comrgrow & hopefully they will be making some changes to correct this in their firmware. This is not user configurable.

If you wish to use the screen to start prints you will manually need to drop files into this dead folder for the screen to use.
```
/home/mks/gcode_files
```
Otherwise simply start prints from your PC or mobile device directly in `Mainsail` while we wait for Comgrow to adjust their screen's firmware.

If you find this unacceptable DO NOT DO THIS UPDATE RIGHT NOW!


## START WITH A FULL BACKUP & DOWNLOAD FILES

Start first by making a clone of your EMMC.
Now go into the `Machine` tab in `Mainsail` & selecting the cog in the top corner of the `config files` section & selecting to show hidden files.
Then click the top checkbox on the left to highlight all files & then press the `Download` button. Make sure that it actually completes the download as some browsers will block it unless you allow popups for that address. Chough chough - Firefox!

![ShowAll](https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/96d79aa0-e243-416b-a8f3-00ea2609ea5b)

Once that is done continue on....


Now you have to log into your T300 using SSH. Use `Terminal` on a Mac or `Putty` on a PC.

once logged in paste in:
```
sudo armbian-config
```
It will bring up this blue background menu:

![armbian-config 1](https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/2dc0d51e-13dd-4b5f-ab87-6d0a38203015)



- Press enter to confirm choices & the keyboard arrow keys to navigate. 
- Choose the first option `System and Security`
- Press enter

Now it will load a red background menu BE CAREFUL HERE YOU CAN BREAK THINGS!!! Thats why its red!

![armbian-config 2](https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/e1c9a0a8-5f38-4e83-973d-91fd8c40b6ce)

- Press enter on the top line to change it so the screen reads like the image above if it is not already!
- YOUR KERNEL IS NOW FROZEN!
- DON'T PRESS ON ANYTHING ELSE
- Move down to `OK` to confirm the choice & go back the blue menu.
- Move over to `EXIT` on the blue menu.
- Press Enter to exit

I find its good to do it this way as you can see its done, just don't be tempted to click other stuff!


Now you "should" be able to run the following commands WITHOUT Bricking your printer! Keep 'em crossed!

## BE SURE YOU WANT TO DO THIS! THIS IS THE POINT OF NO RETURN!
This will update all your system components to the latest available versions for this image.


Now that the "Buster" branch has been removed from all online mirrors these update commands below will fail due to this old & outdated system being discontiued, you will not be able to run these commands. You will get a 404 error. So skip this section & move on. 

I HOPE YOU MADE A BACKUP OR CLONE!!!!
```
sudo apt-get update
```
```
sudo apt-get upgrade
```

After it completes enter
```
sudo reboot
```
Here you need your rabbits foot & to keep your fingers crossed as the new updated system attempts to start! 

If it fails to start you'll need to look at unbricking & reinstalling your image.
If it works like mine did GREAT well done, the tricky bit is over!

## CROWSNEST
A quick note on `Crowsnest` before we go further, the latest version is NOT compatible with this old Buster image used on this T300 printer. DO NOT UPDATE IT! IT WILL NOT WORK HERE!
It is very importatnt to leave `Crowsnest` exactly as it is!!

## UPDATING KLIPPER, MAINSAIL & MOONRAKER

Now on SSH run 
```
./kiauh/kiauh.sh
```
If you get a error does not exist type thing & Kiauh is not already installed go here for install instructions: https://github.com/dw-0/kiauh

If it is already installed it should now bring up a request for an update, type `y` to update Kiauh

You can try to update `Klipper`, `Moonraker` & `Mainsail` These updates will probably fail.

This is because at the time of writing on my system these items had been locked with permission/user/group changes & it was not possible to update them, it turns out you can remove them though, so it was quicker & easier to simply remove them in `Kiauh` & reinstall the new versions a-fresh! Then you know there's no old stuff floating about in there also. BUT READ ON BEFORE YOU DO THIS!!!

However, if your system is the same as mine `Mainsail` will need an extra command to allow you to modify, remove or update it in any way.

To check this back out & quit `Kiauh` then type
```
ls -l
```
![root root](https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/8e5d5623-80f8-4b00-a44c-5a7e16e669a0)


If it says `Mainsail` is `root root` the you need to change that to say `mks mks` as well as all files & subdirectories within.

![mks mks](https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/ff8dd0f0-6ce4-4638-a3a3-7912b2ec809d)

To make the change in user & group paste in this

```
sudo -R chown mks:mks mainsail
```
This is the recursive form of the command & should change the user & group of the `Mainsail` directory & all files & subdirectories within & change them back to your `mks` user profile & allow you to remove or update them.
That `chown` command would probably work for the lock on `Klipper` & `Moonraker` too, but I chose to remove & reinstall them instead of messing about with it all as they could be deleted without modification, where as `Mainsail` could not.

Your choice if you try the `chown` command on `klipper` & `Moonraker` or not.

IF YOU CHOOSE TO TRY THE `chown` command & elect to `UPDATE` your installs now is the time for that & then jump down past the next steps to the `ERROR TIME` title.

### TO REMOVE THE INSTALLS YOU NEED TO HAVE MADE THAT DOWNLOAD BACKUP AT THE START BEFORE YOU DO THIS NEXT STEP OR YOU'RE IN TROUBLE!!

### NOTE IF YOU REMOVE YOUR INSTALLS YOU WILL LOOSE YOUR PRINTER.CFG FILE AND ALL DATA IN YOUR CONFIG FOLDER!! DONT BE THAT PERSON! MAKE SURE YOU HAVE THAT BACKUP NOW!!

IF YOU CHOOSE TO REMOVE: Select option `3 Remove` in `Kiauh`. Then remove `Klipper`, `Moonraker` & `Mainsail`. Also `Fluidd` if you wish.


Now head back to `Kiauh` option `1 Install` & install fresh versions of `Klipper`, `Moonraker`, & `Mainsail`

```
sudo reboot
```



Get back to `Mainsail` & go back to the `Machine` tab, & copy all your backed up files back into your new & now empty config folder.

`Restart` Klipper now.

## ERROR TIME 
Your version of klipper will only be partially updated & you'll probably get a big red warning saying `PROTOCOL ERROR!`
& saying you need to update your MCU's!

Don't worry we're dealing with that in a minute!

## UPDATE MANAGER

Now, back into `Mainsail`. You should have access to the `UPDATE MANAGER` in the `Machine` tab.
- Click the circle arrow button in the top of that section the get the latest update info to make sure thats working.
- Click update on your components if needed
- NOTE THIS CAN TAKE A LONG TIME - 10-20 minutes in some cases! Wait for it to complete if updates are required

## UPDATING THE HOST MCU RPI & MCU FIRMWARE

Now your system is updated we need to clear those MCU errors & get this system working again.
First up get back in to SSH & enter the below commands to update the MCU RPI to the same firmware version as the new Klipper.

```
cd ~/klipper/
make menuconfig
```
In the menu, set "Microcontroller Architecture" to "Linux process," then save and exit.
```
sudo service klipper stop
make flash
sudo service klipper start
```
To be safe
```
sudo reboot
```

This will probably remove the big red error you had before but we still need to update the printer's mainboard. LAST STEP!
Back into SSH again

```
cd ~/klipper/
make menuconfig
```
but this time change the options to these...

![T300 Klipper MCU Settings ](https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/4c8c8760-31f4-43d3-b550-9489fe4cbf80)

I am aware this chip setting normally requires a 64kib bootloader but this machine uses 32kib, & a setting of 64kib did not work on my unit.
Also my printer.cfg said at the top to use USART1 - this is incorrect, it is the above USB setting!

Save & exit

now simply type 
```
make
```
This will now build your new MCU firmware

Now use a `FTP CLIENT` to log onto the printer & pull the `Klipper.bin` file from the `klipper/out` folder & download it to your computer.

REMANE THE FILE to...

```
comgrow.bin
```

- Copy the firmware onto a Micro SD card
- SHUTDOWN the printer
- Remove the bottom cover of the printer to access the mainboard
- Locate the SD card slot on the board
- Insert the sd card into the reader until it clicks in
- Power on the printer
- The MCU firmware will update on boot & can be checked in the `Machine` tab of `Mainsail`
- Power off the printer & remove the SD card
- Replace the bottom cover
- Power the printer back on
- You're all done. WELL DONE!!

Your `Machine` tab should now look like this!

![T300 Machine Tab](https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/2c4074ae-3a21-47fc-8713-456192af9801)

## END NOTES:

I elected to comment out the stock `Macro.cfg` on my new updated system as I much prefer to include the `mainsail.cfg` & copy out the `[gcode_macro _CLIENT_VARIABLE]` that is within the file & use all the custom settings there alongside my own macros, those are linked below.

copy these out to your new `macros.cfg` that you should create, remove the hashes to comment them in so the macro is active & then set values to your preference, find them in the `mainsail.cfg`. Dont use these AND the Comgrow ones! Make sure nothing is duplicated also.

![mainsail](https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/8765d7bb-d678-460e-b72e-b86ce30bac2d)


I hope this helps you update your printer/s! Happy printing!

### If you need some macros be sure to check out these!
https://github.com/3DPrintDemon/Demon_KLIPPER_Essentials


[<img width="171" alt="kofi_s_tag_dark" src="https://github.com/3DPrintDemon/How-To-Upgrade-ComGrow-T300-Klipper-To-Latest-Release/assets/122202359/489afbbc-6f59-419a-8737-165c20351858">](https://ko-fi.com/3dprintdemon)

## Don't forget if you like & use this project you can buy me a beer/coffee to say thanks. [https://ko-fi.com/3dprintdemon](https://ko-fi.com/3dprintdemon)

