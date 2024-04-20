# Linux on the Lenovo 82Y8 Yoga Pro 7 14APH8 (Gen8) with Ryzen 7 7840HS and Nvidia GTX 4050

# Finding a distro
Manjaro was the first distro that worked out of the box with the GPU and prime. Tried KDE Neon and EndeavourOS before but
both had issues with GPU/Prime. I installed Manjaro with the proprietary GPU drivers and can run applications with 
`prime-run x` now.

# Additional setup steps on Manjaro
## Audio
Connected headphones didn't work. Switching over to pipewire by installing `manjaro-pipewire` fixes this. This also enables you to use more modern bluetooth audio codecs. The 4 speakers seem to work fine on Kernels >= 6.6 now.

## Sound quality of speakers
The speakers sound pretty flat per default, even with the tweeters and the woofers both emitting sound. This can be fixed with easyeffects by following this guide: https://wwmm.github.io/easyeffects/guide_1.html

I've also added the profile to this repo: [speakers profile](./easyeffects/Speakers.json)

Remeber to setup easyeffects to disable this profile on all other outputs in 

# Power management
Per default Manjaro uses the power-profiles-daemon, I found [tlp](https://linrunner.de/tlp/index.html) to offer slightly more
power savings on battery (maybe 1-2W with same applications open). Performance on mains was unaffected. I've set the CPU power policy up a notch as I didn't notice any worse battery life. You can do that with tlpui or in the tlp.conf: `CPU_ENERGY_PERF_POLICY_ON_BAT=balance_performance` 

TLP breaks powerdevil which let you set the performance profile, that didn't really have a big effect anyway.

# Biometric auth

Installed howdy and used with these two links: https://wiki.archlinux.org/title/Howdy

https://gist.github.com/pastleo/76597c6ae8f95bb02982fea6df3a3ade

## Fix white flickering after suspend/resume on KDE
After suspending the laptop or connecting/disconnecting external displays sometimes one or more displays would flicker white on refresh or become completely white with just the cursor moving. The issue is described here: https://gitlab.freedesktop.org/drm/amd/-/issues/2354

Setting `amdgpu.sg_display=0` as additional kernel parameter seems to fix this issue. To set this in grub, append `amdgpu.sg_display=0` to the line `GRUB_CMDLINE_LINUX_DEFAULT` in `etc/default/grub`. Afterwards run `grub-mkconfig -o /boot/grub/grub.cfg`.

# TODO
- Prime is a bit finicky and typical use cases like steam don't automatically select the correct GPU. Also the gamescope microcompositor is currently broken on Nvidia
- Automatically set the clean profile in easyeffects on new audio outputs
- Volume controls with headphones do not work, adjusting 'bass speaker' in `alsamixer`fixes this. Probably has to do with the 4 channels used in speaker mode
  
