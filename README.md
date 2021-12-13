# Dell XPS 9500 macOS Monterey

This is an OpenCore EFI that allows you to install and boot macOS Monterey on your Dell XPS 15 9500 (2020).

Thanks to @zachs78 and @thefiredragon for previous repos I built this from.

<b>OpenCore Version:</b> 0.7.4

<b>macOS Version:</b> Monterey 12.0.1

---

## Functional Status

|Function / Hardware|Status|
|-|-|
|iGPU UHD630 Acceleration|Working|
|CPU Power Management|Working - idles at 800MHz, boosts to max Turbo frequency|
|Laptop Keyboard|Working|
|Laptop Trackpad|Working|
|Laptop Headphones Jack|Working|
|Built-in Speakers|Working|
|Built-in Mic|Working|
|Hotkeys for audio|Working|
|USB 3.x|Working|
|USB 2.0|Working|
|Fingerprint Sensor|Not working|
|Touchscreen|Working|
|SD Card Slot|Not Working|
|Screen brightness|Working, hotkeys fn+S/fn+B to decrease/increase brightness|
|Built-in Wifi|Working|
|Built-in Bluetooth|Semi-working, AirDrop is not currently working|
|Dell USB3.1 dock|Working|
|RTL8153 USB Ethernet on Dell dock|Working|
|Other peripherals on Dell dock|Working|
|Built-in webcam|Working|
|Sleep|Dell BIOS bug (see [below](https://github.com/zspherez/Dell-XPS-9500-4K-UHD-Monterey#s3-sleep-issue-with-xps-9500-and-other-modern-laptops---credit-to-zachs78) to block sleep)|

---

## BIOS Settings

Disable the following
 - VT-d
 - TPM
 - Secure boot
 - Disable CFG Lock (via modGRUBShell)

---

## Important

This EFI was created from scratch using Dortania's vanilla laptop guide with additional info from the Comet Lake setup https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/coffee-lake-plus.html#starting-point

You are strongly advised to read the guide fully before using the EFI in this repository. Do not use the EFI as is. You will need to learn how to replace the serial number with GenSMBios and disable cfg lock at the very least.

---

## How to disable CFG Lock

This is specific to XPS 15 9500 only (along with its sibling models and previous gen).

Select the modGRUBShell option at startup (OpenCore boot selection page).
At the grub prompt, enter the following commands (the first line unlocks CFG, the second line unlocks overclocking).

```
setup_var CpuSetup 0x3e 0x0
setup_var CpuSetup 0xda 0x0
exit
```

Restart your laptop and boot into the BIOS. Do a factory reset. Now your CFG lock will be disabled. You can confirm that by running the VerifyMSR2 option.

If you update your BIOS, you may need to do this again but so far Dell has been kind to us.

---

## <a name="sleep"></a>S3 sleep issue with XPS 9500 and other "modern" laptops - credit to @zachs78

Thanks to Microsoft, we are losing the ability to really put our laptops / desktop to sleep. In their infinite wisdom, they decided that we need our laptops and desktops to be "instant on" because we need to receive notifications such as emails and phone calls (hey Satya, heard of a smartphone?)

Manufacturers such as Dell and Lenovo have very stupidly followed Microsoft's advise, by gradually removing S3 sleep mode, breaking compatibility with other OSes such as Linux and Mac OS.

The bigger issue here is that "modern standby" just does not work on Windows itself. The laptop never sleeps if you have background apps running - these days, they *always* receive notifications/messages. We'll need to close the apps to prevent this and if you do, you might as well shutdown. Care to address the white elephant, Satya (not blessed with high IQ but this is common sense, surely)? We close the lid because we don't want to be disturbed. If it's urgent, perhaps pick up the phone? `¯\_(ツ)_/¯`

TL;DR Sleep is broken for Dell XPS 9500 on Windows too. We can still opt-in to use S3 on Windows but Dell has chosen not to implement (or very incompetent at implementing) a working version of S3 resume in their BIOS for XPS 9500. When you resume, you'll get the same behavior as Catalina - Dell logo that never goes away until you hard reset.

To avoid this behavior, we run the following command to block sleep from occurring in the first place. 
```
sudo pmset -a disablesleep 1
```
If you do not mind losing sleep in Windows as well, an alternative method is to enable "Block Sleep" in the BIOS Setup.

---

## Brightness hotkeys

fn+S and fn+B hotkeys are currently used to adjust brightness.

---

## Undervolting

This EFI comes preinstalled with VoltageShift kext. To undervolt, visit https://github.com/sicreative/VoltageShift (skip the kext loading part).
