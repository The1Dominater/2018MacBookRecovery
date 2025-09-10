**Problem:**

Currently my 2018 MacBook Pro 13"(4-Thunderbolt USB-C) Model A1989 with a T2 security chip will not boot even into recovery mode. The closet I can get is to booting is the Apple Logo with progress bar stuck at about 90%. At this point it hangs(I let it sit for 3 hours) with the fans on full blast.

**What I have tried:**
 1. **Software Update:** I replaced the damaged screen on the Mac using a genuine MacBook Pro replacement screen(so now the warranty is void if it wasn't before). When I finished I turned the Mac on and used it for several days before I realized I could upgrade macOS from Mojave to Sequoia. Through *System Preferences > Software Update* I selected upgrade, but despite downloading the upgrade repeatedly failed. Upset I used terminal to run `sudo softwareupdate -l` which showed me there was a security update for Mojave available so I used `sudo softwareupdate -i <name of update>`. No problems downloading and installing the update, so I ran 'sudo halt' to shutdown the Mac and restart. 
 2. **Recovery Mode:** I still could not install the Sequoia (or Sonoma) upgrade through *System Preferences* so I booted into recovery mode to see if I could reinstall Mojave to fix the issue. Once in recovery mode I was offered to reinstall Catalina(the macOS version after Mojave) so I attempted this even though it said it would erase the disk. I walked away for awhile, and I came back and it was in the language selection screen for setting up the Mac. I left it for a day because I was going to give the Mac to my brother so I figured he could go through the setup menu and personalize it himself.
 3. **NVRAM and SMC**: I tried [resetting the NVRAM][1] using `ctl + opt + R + P` after pressing the power button. I tried [resetting the SMC][2] using `opt + cmd + R-shift` and then pressing the power button for 7 seconds. 
 4. **DFU mode:** Upon returning to the Mac to set it up, it booted to a the flashing ? folder. I restarted it in recovery mode again which brought me to the spinning globe icon and I connected to the internet. I again was presented with the reinstall prompt and seeming no way to get around it so I tried reinstalling again. However, this turned my Mac into a brick which would not power on, no fans, no logo, and no trackpad clicking. After some googling I attempted to follow these guides in this order to revive/restore from DFU mode using my mom's 2018 MacBook Air 13" Model A1932 running macOS Sonoma 14.7.8:
- Apple: [https://support.apple.com/en-us/108900][4]
- Mr. Macintosh: [https://mrmacintosh.com/how-to-restore-bridgeos-on-a-t2-mac-how-to-put-a-mac-into-dfu-mode/][5]
- Wiki: [https://logi.wiki/index.php/DFU_Mode_Restore_(Macs)][6]

This first time I saw the affected MacBook Pro in DFU mode on the host machine a phone download image popped up briefly and finished like when you flash iPhone in recovery mode. I repeated the revive/restore process again. I am confident I was able to get the MacBook Pro into DFU mode as I could see it in both *Finder* and the *Apple Configurator 2(v2.18)*. When I tried revive/restore in *Finder* I was able to find these logs in *Console > Log Reports > iBridgeUpdater.log* 

Full Logs Here: [https://github.com/The1Dominater/2018MacBookRecovery.git][3]

- I tried both the Apple provided usb-c to usb-c cable as well as the Thunderbolt(I know Apple said it was not supported but Mr. Macintosh said it worked) 
- I tried with both Macs connected to power(as Apple recommended), the affected connected to power and the unaffected not connected to power, and neither connected to power.
- I downloaded different version of the `.ipsw` files from Mr. Macintosh's website and tried them to no avail(Either they were not signed correctly or they were not the right version for my Mac).
- I went to `~/Library/Group Container/K36BKF7T3D.com.apple.configurator/Library/Caches/Firmware` and removed the downloaded firmware file and repeated the process.
- I tried downgrading the macOS version of the host MacBook Air to Mojave because Mr. Macintosh says the macs need to be the same version for the DFU restore mode to work, but I could not get the MacBook Air to downgrade(potentially the issue?).

It kept failing with the error: `The Mac "Mac" could not be updated. An unknown error occurred(2015).`

![2015 Error][7]

 5. **Battery Disconnect**: I unscrewed the back plate and disconnected the battery, and then held down the power button for 30-secs to attempt to clear the RAM(probably pointless because this seems like either a T2 chip problem or a storage device defect) and any other residue power on the board. After attempting to restore from *Apple Configurator* at last the Apple logo reappeared, but the restore failed with error after error:

a. `AMRestoreErrorDomain error 75 - Failed to handle message type StatusMsg (Missing data volume)) [AMRestoreErrorDomain - 0x48 (75)]` which according to the Wiki meant: *75 - T2 booted in recovery mode (Revive wont work).*

![75 Error][8]

b. `Failed to create new state machine for restore [com.apple.MobileDevice.MobileRestore - 0xFB1 (4017)]` which I think was just because I was being impatient and tried to start another upload of the firmware while it was stuck on the Apple logo loading bar(I can now get the Apple Logo to appear at will. I can turn the Mac off and then restart the restore process in *Apple Configurator* to get it to return to the logo)

![4017 Error][9]

c. `(AMRestoreErrorDomain error -1 - Failed to handle message type StatusMsg (Generic error))[AMRestoreErrorDomain - 0xFFFFFFFFFFFFFFFF (-1) [sys = 0x3F, sub = 0xFF, code = 0x3ff (16383)]]` which I assume means I am screwed...

![16283 Error][10]

**Conclusion:**
MacBook Pro was bricked after attempting to do a combo upgrade. Tried Recovery Mode, NVRAM & SMC reset, and DFU restore. Going to take it to Apple Repair Specialist to see if it can be fixed even though it is not under warranty, and will post an update here on the results.

**UPDATE** I took it to a shop and they told me the motherboard was damaged. To replace it would be $400, but a used one is only $250 so I think it's time to say goodbye.


  [1]: https://support.apple.com/en-us/102539
  [2]: https://support.apple.com/en-us/102605
  [3]: https://github.com/The1Dominater/2018MacBookRecovery.git
  [4]: https://support.apple.com/en-us/108900
  [5]: https://mrmacintosh.com/how-to-restore-bridgeos-on-a-t2-mac-how-to-put-a-mac-into-dfu-mode/
  [6]: https://logi.wiki/index.php/DFU_Mode_Restore_(Macs)
  [7]: DFU-Errors/IMG_6558.jpg
  [8]: DFU-Errors/IMG_6560.jpg
  [9]: DFU-Errors/IMG_6561.jpg
  [10]: DFU-Errors/IMG_6562.jpg
