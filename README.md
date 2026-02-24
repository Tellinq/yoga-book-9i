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
> Provided bluetooth keyboard Type-C port is for power only. you cannot use it for data. Must use a Bluetooth connection. Bluetooth is not supported when powered on.

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

This should not matter (mostly). What will matter is future kernel updates. So far, 6.18 LTS is in a state where a majority are working either out of the box, or with configuration. Please note, this laptop does require a large amount of configuration to make this usable.

Here's my general set of recommendations:

Bare minimum: Distribution with kernel 6.16+. This includes fixes for the speakers.
Recommendation: Distribution with kernel 6.18 LTS or newer. Once [Kernel.org bug 220386](https://bugzilla.kernel.org/show_bug.cgi?id=220386) is fixed in the kernel, my baseline recommendation will change.

The rest of what you're looking for in a distribution is up to you.

## How I test

I test with a Live ISO first. For full testing, I will install a Linux distribution that frequently updates the Linux kernel. This is a high requirement as this is incomplete for support. Improvements will happen throughout time.

## What works

### Faical Recognition

The IR Camera works with [howdy](https://github.com/boltgolt/howdy). Fedora-based distribution users can install [howdy-beta](https://copr.fedorainfracloud.org/coprs/principis/howdy-beta/) with Copr.

> [!NOTE]
> The `device_path` in `/etc/howdy/config.ini` needs to be set to `/dev/video2`.

### Speakers

> [!NOTE]
> For best experience: Use Kernel 6.16+

You will need a listening script that can sync the bass speakers. Alternative, you can individually adjust the bass speakers.

### Variable/Dynamic Refresh Rate/Adaptive Sync

Supported depending on your desktop environment or Window Manager.

### Screen brightness

Technically, the bottom screen will only work out of the box. As this is an OLED screen, using a "fake" brightness such as redshift will get the job done.

However, you can get this working via the necessary kenrel parameters listed https://github.com/blhoward2/yogabook_patches

If you choose to use KDE with this patch, The top screen brightness affects both displays. You can individually control the intel_backlight via scripts. For exampls, DMS can let you select an independent monitor device to control the backlight independently.

Alternatively, Plasma 6+ HDR and GNOME HDR can take advantage of fake brightness. By enabling HDR, you can get the best of both worlds by being able to adjust screen brightness and a higher color range.

### HDR

This is heavily dependent on what desktop environment or window manager you choose to use. If you already have a device or monitor that already supports HDR on Linux, then chances are it will likely work on the Yoga Book 9i as well.

### Automatic rotation

> [!NOTE]
> Requires `iio-sensors-proxy`

If you use KDE Plasma, the top screen will work with automatic rotation so long as "Only in tablet mode" is disabled. Keep in mind with this the screen orientation is offset by 180 degrees.

It is possible to make a script to handle both screens. the hinge and individual screen accel/gyro sensors are exposed and can be used. I will be releasing a script for Plasma, which can be adapted for other window managers and desktop environments accordingly.

### Start-Stop charging

It appears that Yoga devices only support a hard set range of 75% (start) - 80% (stop). Please be mindful of this if you are coming from a device that let you configure this.

This is also applicable to Windows via Vantage.

### Intel graphics (Mesa)

Works fine on Wayland sessions out of the box. However, for X11 sessions, see https://github.com/blhoward2/yogabook_patches to apply the kernel parameters that force Xe graphics.

### Using a Thunderbolt 3/4 dock, USB4 dock

Works as intended. 

Tested using:
- Lenovo Thinkpad Universal Thunderbolt 3 Docking Station
- Lenovo Thinkpad Universal Thunderbolt 4 Docking Station
- Lenovo Thinkpad Universal Thunderbolt 4 Smart Dock
- Dell WD15 Thunderbolt 3 Docking Station
- MOKIN MOUC1705
- Hagibis Pro MC100

Docking stations that support Lenovo Thinkpad Power Buttons will work.

### Secure Boot

Enable third party CAs. You can enroll keys via sbctl.
Note that enabling Setup Mode disables Secure Boot until you manually re-enable yourself in UEFI. User Mode will turn back on once enrolling is complete.

# Touchscreens

See https://github.com/blhoward2/yogabook_patches for applying a patched kernel. Do note, once you remove your fingers from the bottom touchscreen, the screens and stylus will disconnect and reconnect. This can cause a visual stutter.

Note that Window Managers and desktop environments may not support individually mapping to specific outputs on multiple touchscreens/stylus.

Turning off the bottom screen will not keep touch support enabled, unlike on Windows where you can double tap to re-enable the bottom screen.

So far, a maximum of 5 fingers can be used at once. If you attempt to use both screens at the same time, the inputs will go back and forth uncontrollably.

10 fingers on both screens is the current goal.

### Function/Nonstandard key support

The following works:

- Volume Mute
- Volume Up/Down
- Brightness up/down
- External display settings: Binds to `Left Meta + P`
- Settings: Binds to `Left Meta + I`
- Lock Screen: Binds to `Left Meta + L`
- Screenshot: Binds to `Left Meta + Left Shift + S`
- Copilot key: Defaults to `Left Meta + Left Shift + XF86TouchpadOff` (Libinput will show as `Left Meta + Left Shift + F23`).
    - This will not behave like a modifier key. For example, trying to use `Left Meta + Left Shift + XF86TouchpadOff + L` will not work (tested on Niri). It might be possible to work around this, but I have not attempted.
    - You can can remap using [Input Remapper](https://github.com/sezanzeb/input-remapper).
    - I have not had much luck with getting this to work on Plasma 6.

Using Lenovo's bluetooth leyboard specifically also forcefully has additional Function bindings:

- FN + Q: Cycle System Performance Mode (does not do anything like the dedicated key)
- FN + R: Cycle refresh rates (does not actually do anything)
- FN + T: Binds to `Printscreen`

The following are not recognized:

- Touchpad toggle
- Night mode
- Music profile cycle
- Customizable Lenovo key
- Open collaboration app
- Open User Center app
- Open AI Experience
- Cycle System Performance Modes

### Proximity sensor/user presence sensing

> [!NOTE]
> Requires `iio-sensors-proxy`

Currently not detected by iio-sensors-proxy. Requires manual configuration.

I don't see desktop environments or window managers taking advantage of this, given how few devices include proximity sensors.

What works:
- If someone is nearby (yes or no). I have also found this to also be determined if you look at the display specifically
- Distance ranging from 0-10000, not sure what metric this is on
- Attention detection (a separate sensor that will flip from 0 (not looking at the display) and 100 (looking at the screen). I find this to be unreliable, but it does povide results

With all that said, this is enough data we can grab to make a script that handles multiple actions (such as adjusting screen brightness, toggling the display on/off, locking the screen, pausing media)

I have a script in the works that adapts to KDE Plasma screen brightness. My main objective is to start a "not in focus" timer IF the user is also idle as the sensor can be convinced you are away even if you are using the device. This is also true for Windows but they seem to have and check if you are idle for a few seconds before actually starting the away timer

### Ambient Light Sensor

> [!NOTE]
> Requires `iio-sensors-proxy`

Works if your window manager or desktop environment supports it. Research how you can use it on your desktop environment.

> [!WARNING]
> Most of the time I cannot get this to work. I did have a one off chance where I managed to get this to work. I'm not sure what triggered this. However, if yours is not stuck at 120 lux, you can use this as intended.

### External keyboard brightness

This cannot be configured with software yet. You can however turn it on/off/auto using `FN + Space`

### Power Profiles

> [!NOTE]
> Requires `power-profiles-daemon`

Directly adjusts firmware System Performance Mode

Power Save: Battery Saving
Balanced: Intelligent Cooling
Performance: Extreme Performance

## Others without comment
- Camera
- Microphone support
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

### Detecting physical keyboard attached on bottom display

Libinput does not detect this. This might change if the bottom screen has full touch support. I will revisit this only when the bottom screen has full multi touch support.

If you're wanting to simply turn off the bottom display, you can change the state of either display as you please with manual scripts. I will look into providing some if requested via an Issue.

### Haptics

Don't exactly have a reason to use this yet unless I can get a full size virtual keyboard that is tolerable. Don't have a way to thoroughly test this yet.


