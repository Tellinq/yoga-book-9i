# yoga-book-9i
Findings for the Lenovo Yoga Book 9i Gen10.

I will accept PRs for the Yoga Book 9i Gen8, Gen9, and other future revisions if they come!

# Enter the UEFI

Enter the Novo menu in one of two ways
1. Power off the device
   - Upon power on, repeatedly flip the camera kill switch
   - Use a paper clip or similarly sized tool to press the Novo button

If you have a wired keyboard, you can press:
- F2 (Setup)
- F12 (One time boot menu)

> [!NOTE]
> Provided bluetooth keyboard Type-C port is for power only. you cannot use it for data. Must use a Bluetooth connection. Bluetooth is not supported when powered on

# Notable features

- User Presence sensing exists. You can [read more here](https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/sensors-presence-sensing)
- All Yoga Book 9i software features can be controlled via User Center. You can read the list of features [here](https://download.lenovo.com/pccbbs/pubs/yogabook_9_13imu9/user_guide/en/help-center-user-center-app.html)

# Power on

- You can have the laptop turn on the moment you open the lid. This is enabled by default, but you can toggle via Lenovo Vantage or in the UEFI Setup
- Touch is supported until you reach an OS that does not share the same support (Windows Boot Manager Menu, Limine, GRUB, etc.)

# If you choose to reinstall Windows via an official ISO

- All drivers can be installed in tandem through SDIO, Windows Update, and Lenovo Vantage
- Pen targets the opposite screen however. Tried using the drivers provided in the OEM install without luck. could be me using SDIO first, not sure.
- Installing User Center will restore full software experience

# Linux support

This is not a guide. This simply lists what works and what limitations exist out of the box. I will provide resources for working features when possible or if requested. If you own a Gen8, Gen9, or Gen10 device and you have additional information or a discrepency, please open an issue with your findings.

## What distribution should I use?

This is a blanket statement I put on any device out there. If there are device parity limitations, then my recommendation will always be any distribution that allows for a rolling release (Fedora and Arch to name base distributions).

I do not test Debian installs. Especially those that use an older kernel.

The lowest kernel install I have tested is 6.14. I will continue to update device parity as newer kernel releases come out. If and when there is full device parity, then I will mark a base kernel to recommend.

TDLR: Anything that allows the latest kernel is recommended. The rest of what you're looking for in a distribution is up to you.

## How I test

I test with a Live ISO first. for full testing, I will install a Linux distribution that frequently updates the Linux kernel. This is a high requirement as this is incomplete for support. Improvements will happen throughout time.

## What works

### Faical Recognition

The IR Camera works with [howdy](https://github.com/boltgolt/howdy). Fedora-based distribution users can install [howdy-beta](https://copr.fedorainfracloud.org/coprs/principis/howdy-beta/) with Copr.

The `device_path` in `/etc/howdy/config.ini` needs to be set to `/dev/video2`.

### Speakers

- Kernel <6.15 speakers will not work
- Kernel 6.15.X: There is no speaker bass
- Kernel 6.16+: Bass speakers work, but are stuck at 100%. Rest of the speakers can be controlled

What needs to be tested: Check PCM and see if a softmixer can work around this

### Variable refresh rate

If you use Plasma 6+, you can use Adaptive Sync.

### Screen brightness

Technically, the bottom screen will only work out of the box. As this is an OLED screen, using a "fake" brightness such as redshift.

Plasma 6+ HDR can take advantage of this. By enabling HDR, you can get the best of both worlds by being able to adjust screen brightness and a highee color range.

### HDR

This is heavily dependent on what desktop environment or window manager you choose to use. If you already have a device or monitor that already supports HDR on Linux, then chances are it will likely work on the Yoga Book 9i as well.

### Start-Stop charging

It appears that Yoga devices only support a hard set range of 75% (start) - 80% (stop). Please be mindful of this if you are coming from a device that let you configure this.

This is also applicable to Windows via Vantage.

### Intel graphics (Mesa)

Works fine on Wayland sessions. I could never enter in an X11 session via GNOME or KDE. My recommendation anyways is to consider using Wayland for laptops.

I do not intend to look further into X11.

### Using a Thunderbolt 3/4 dock, USB4 dock

Works as intended. Tested using Lenovo Thinkpad Universal Thunderbolt 4 Docking Station, various hubs, and an older Dell WD15 thunderbolt 3 dock

Thinkpad docking station power button will work. Haven't 100% tested on Linux despite confirming on Windows. I expect this should work just fine. While not as needed as a traditional Thinkpad with a power button on the keyboard deck, it can be potentially more convenient depending where you have the Yoga Book 9i docked

## Others without comment
- Camera
- Microphone support
- Adaptive Sync/Variabl
- Wi-Fi
- Bluetooth
- Suspend/Sleep
- Power off
 
## Not working

### Tablet mode

Not detected as is. Possible getting around as when you arent in a laptop orientation, there is indication that a "touchpad" is turned off

I am currently finding a way to manually override this as the hinge sensor is exposed and can be used to determine what angle warrants tablet mode.

### User Center software experience

Lenovo does not make a User Center application that works with Linux. This is expected as this has never been advertised as such. I'm only reporting this here to save people from asking.

### Maximum finger support

Partially tested with Libinpurt Debug GUI

Cant fully test. top screen seems to detect my fingers but not all 10. Bottom can intermittiently detect one finger, but it can be considered non existent in real world use case

### Function key support

For universal keys, works just fine. As anticipated, none of the lenovo special keys can be used or bound to KDE shortcuts out of the box

### Detecting physical keyboard attached on bottom display

Libinput does not detect this. This might change if the bottom screen has full touch support. I will revisit this only when the bottom screen has full multi touch support.

If you're wanting to simply turn off the bottom display, you can change the state of either display as you please with manual scripts. I will look into providing some if requested via an Issue.

### Copilot key

While technically bindable, I will mark this as not working fully as I can never get this to bind on any of my Copilot key devices

### External keyboard brightness

Cannot configure in Plasma. Must use built in function

### Automatic rotation

Requires iio-sensors-proxy.
If you use KDE Plasma, the top screen will work with automatic rotation so long as "Only in tablet mode" is disabled. Keep in mind with this the screen orientation is offset by 180 degrees.

It is possible to make a script to handle both screens. the hinge and insividual screen accel/gyro sensors are exposed and can be used. I will be releasing a script for Plasma, which can be adapted for other window managers and desktop environments accordingly.

### Proximity sensor/user presence sensing

Requires iio-sensors-proxy

Currently not detected by iio-sensors-proxy. Requires manual configuration.

I don't see desktop environments or window managers taking advantage of this, given how few devices include proximity sensors.

What works:
- If someone is nearby (yes or no). I have also found this to also be determined if you look at the display specifically
- Distance ranging from 0-10000, not sure what metric this is on
- Attention detection (a separate sensor that will flip from 0 (not looking at the display) and 100 (looking at thr screen). I find this to be unreliable, but it does povide results

With all that said, this is enough data we can grab to make a script that handles multiple actions (such as adjusting screen brightness, toggling the display on/off, locking the screen, pausing media)

I have a script in the works that adapts to KDE Plasma screen brightness. My main objective is to start a "not in focus" timer IF the user is also idle as the sensor can be convinced you are away even if you are using the device. This is also true for Windows but they seem to have and check if you are idle for a few seconds before actually starting the away timer

### Ambient Light Sensor

This is exposed. Seems to be stuck at a value of 120. I cannot seem to configure this.

## Untested

- Power profiles
  - Plasma does provide a slider via Power Profiles Daemon. However, I haven't entirely tested the behavior. These have the possibility of not working.
- Secure boot
  - Didnt exactly let me enroll any keys in Ventoy despite enabling 3rd party CAs in the UEFI

## Other notes
- Default scaling on Plasma 6.3+ vs Windows
   - Windows is 200% as with most 2800x1800 and 2560x1600 14-16" displays
   - KDE 6.3+ will default to 160% scaling on both displays
- Default screen
    - Top screen is the default


