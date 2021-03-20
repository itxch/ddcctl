# ddcctl: DDC monitor controls for the OSX command line

Adjust your external monitors' built-in controls from the OSX shell:

- brightness
- contrast

And _possibly_ (if your monitor firmware is well implemented):

- input source
- built-in speaker volume
- on/off/standby
- rgb colors
- color presets
- reset

# PBP Mode

This is a fork I made to try and enable DPB mode (and select input) on a Dell U4919DW.
And it works :)

## Added two flags:

| VCP Code  | flag        | args  | description                                                                    |
| --------- | ----------- | ----- | ------------------------------------------------------------------------------ |
| E9 (0xE9) | -pbp        | 0-255 | This either enables or disables PBP mode. On my machine, 0 is off and 36 is on |
| E8 (0xE8  | -pbp-screen | 0-255 | Selects the input, uses same values as Input Sources below                     |

# How to Find your -pbp flag?

**Only use these steps if using `-pbp 36` doesnt work.**

1. Nagivate to the directory where ddcctl is.
2. Ensure PBP mode is not on.
3. Run `./ddctl -d 1 -D > normal.txt` - this will dump all the VCP codes into a text file
4. Once it has stopped running enable PBP mode on your monitor, and hook up a 2nd device - make sure the 2nd device does not go to sleep
5. On the first device in the same directory run `./ddctl -d 1 -D > pbp.txt` and wait for it to complete
6. Go to [diffchecker](https://www.diffchecker.com/).
7. Paste each of the respective files content into each side and press find differences
8. It will show the VCP Codes that changed, i.e. responsible for enabling PBP mode.
9. Now there is a matter of adding that flag to ddctl. If anyone actually reads this and wants help doing that, drop me a PM on twitter [@itxchh](https://twitter.com/itxchh)

## Download Binaries

Head to [Releases](https://github.com/kfix/ddcctl/releases) and from the
[latest release](https://github.com/kfix/ddcctl/releases/latest) download
[`ddcctl_binaries.zip`](https://github.com/kfix/ddcctl/releases/latest/download/ddcctl_binaries.zip)
archive

## Build from Source

- install Xcode
- run `make`

# Usage

Run `ddcctl -h` for some options.  
[ddcctl.sh](/scripts/ddcctl.sh) is a script I use to control two PC monitors plugged into my Mac Mini.  
You can point Alfred, ControlPlane, or Karabiner at it to quickly switch presets.

# Input Sources

When setting input source, refer to the table below to determine which value to use.  
For example, to set your first display to HDMI: `ddcctl -d 1 -i 17`.

| Input Source                    | Value |
| ------------------------------- | ----- |
| VGA-1                           | 1     |
| VGA-2                           | 2     |
| DVI-1                           | 3     |
| DVI-2                           | 4     |
| Composite video 1               | 5     |
| Composite video 2               | 6     |
| S-Video-1                       | 7     |
| S-Video-2                       | 8     |
| Tuner-1                         | 9     |
| Tuner-2                         | 10    |
| Tuner-3                         | 11    |
| Component video (YPrPb/YCrCb) 1 | 12    |
| Component video (YPrPb/YCrCb) 2 | 13    |
| Component video (YPrPb/YCrCb) 3 | 14    |
| DisplayPort-1                   | 15    |
| DisplayPort-2                   | 16    |
| HDMI-1                          | 17    |
| HDMI-2                          | 18    |
| USB-C                           | 27    |

# Credits

`ddcctl.m` sprang from a [forum thread](https://www.tonymacx86.com/threads/controlling-your-monitor-with-osx-ddc-panel.90077/page-6#post-795208) on the TonyMac-x86 boards.

`DDC.c` originated from [jontaylor/DDC-CI-Tools-for-OS-X](https://github.com/jontaylor/DDC-CI-Tools-for-OS-X), but was reworked by others on the forums.

A few forks have also backported patches, which is _nice_ :ok_hand:.
