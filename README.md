# yoga-book-9i
Documentation for the Lenovo Yoga Book 9i Gen10.

I will accept PRs for the Yoga Book 9i Gen8, Gen9, and other future revisions if they come!

To-do

- User Prescence exists
- Power on when opening the lid exists. It's an option in the UEFI and in software
- I did not see an option to power on when AC adapter is plugged in. Curious if it does as I have modern Thinkpads such as the T16 Gen 3 which has this feature in the UEFI

To-do with a Windows reinstallation - restoring to OEM defaults

- Check available drivers on the internet vs what is currently only available on the OEM install
- Check what drivers can install via Windows update vs other party solutions (Lenovo Vantage/Commercial Vantage, SDIO, etc.)

To-do for Linux support

I will be testing a Live ISO first. for full testing, I will install a Linux distribution that frequently updates the Linux kernel. This is a high requirement as this is incomplete for support. Improvements will happen throughout time

- IR Webcam support - test with howdy-beta
- Camera support
- Microphone support
- Speaker support
   - Not present in 6.14. untested in further revisions
- "Tablet mode"
- "Software experience" missing
- Maximum finger support (test with libinput debug gui)
- function key support
- copilot key
   - Untested. should be bindable
- keyboard brightness
   - Cannot configure in Plasma. Must use built in function
- Screen brightness
- HDR support
   - Supported on both screens
- Adaptive sync support
   - KDE feature. Supported
- default scaling on KDE vs Windows
   - Windows is 200% as with most 2800x1800 and 2560x1600 14-16" displays
   - KDE 6.3+ will default to 160% scaling on both displays
- Start-Stop charging support
   - Tested on Kernel 6.14 - a toggle exists. No option to configure further is present. Unknown if what is done in Windows Lenovo Vantage exists
- WiFi and Bluetooth support
    - Both are supported out of the box
- Intel graphics via Mesa
    - Seems to work. I cannot seem to get a Plasma X11 session. Plasma Wayland seems to work fine as Wayland for these devices should be used anyways
- detecting physical keyboard and turning off display
- "merging the two screens together" support
    - This isnt a real thing anyways. this is just software in Windows that makes a window overflow the top taskbar and into the second acreen. could be possible with some software tweaks
- Suspend/sleep
- Power profiles (wattage and fan speeds)
    - There is a slider in Plasma. whether they actually work I have not tested
- Variable refresh rate
    - As with other Windows laptops, this is not present in Linux. I expect this to remain until there is similar support with other Windows laptops
- Automatic rotation
    - Seems to work on the top screen. however since screen orientation is flipped, this isnt ideal. i wouldnt enable it
- Secure Boot support
    - Didnt exactly let me enroll any keys in Ventoy despite enabling 3rd party CAs in the UEFI
- Using a Thunderbolt 3/4 dock, USB4 dock
- Ambient brightness sensor support
- Default screen
    - Top screen is the default


