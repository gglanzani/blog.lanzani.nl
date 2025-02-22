---
title: Customizing the Binepad BNK9 firmware
author: Giovanni Lanzani
date: 2025-02-22T00:00:00Z
snippet: |
  In which I explore how to create and flash a custom firmware in the Binepad BNK9.
url: /2025/customizing-the-binepad-bnk9-firmware
---

Last year, I blogged about the [Binepad BNK8](/2024/binepad-bnk8-macropad/), a macropad by [Binepad]. This week, I received the larger brother, the [Binepad BNK9](https://binepad.com/products/bnk9). It sports 9 buttons and a larger knob[^1].

Since the firmware is [customizable], I started exploring it through [VIA]. I could create new layers, control the light effects, etc. But, once I started adding layers, I had a dilemma:

1. Either keep a key pressed to activate a layer. That, however, required some finger gymnastic, especially if I wanted to use the knob while keeping a key pressed. Or
2. Be left wondering in which layer I was as there is no visual clue.

I then started looking around and soon it turned out I had to write a [qmk] firmware by hand.

The getting started [guide] is straightforward. On macOS, just fire up a terminal and type:

{{< highlight sh >}}
brew install qmk/qmk/qmk
qmk setup  # take note of where the qmk_firware is cloned. It will be your $QMK_FIRMWARE_HOME
qmk compile -kb binepad/bnk9 -km default
qmk config user.keyboard=binepad/bnk9
{{< / highlight >}}

Afterwards, I created a copy of the keymap into `$QMK_FIRMWARE_HOME/keyboards/binepad/bnk9/keymaps/gglanzani`, where `$QMK_FIRMWARE_HOME` is whatever folder [qmk_firmware] was cloned into.

After some trials and errors, I ended up with a `keymap.c`, `config.h`, and `rules.mk` that work the way I want. I've uploaded them to [Github] and you're free to use them.

To compile the custom firmware type in a terminal:

{{< highlight sh >}}
cd $QMK_FIRMWARE_HOME
qmk compile -kb binepad/bnk9 -km gglanzani  # use your own if you don't use my repo!
{{< / highlight >}}

This will create a `binepad_bnk9_gglanzani.uf2` file in your `$QMK_FIRMWARE_HOME` folder.

But how do you get it on your macropad? To do so, disconnect the USB cable, press the knob, and then connect the cable. That will mount an `RPI-RP2` volume on your computer. One you copy the `uf2` file into it,  the volume will unmount and your macropad will be ready to use!

If you look into my [repository], there should be enough comments to understand what's going on an adapt it to your needs!

[Binepad]: https://binepad.com
[customizable]: https://binepad.com/blogs/configurator/bnk9-configurator
[qmk_firmware]: https://github.com/qmk/qmk_firmware/
[VIA]: https://usevia.app/
[qmk]: https://qmk.fm/
[guide]: https://docs.qmk.fm/newbs_getting_started
[Github]: https://github.com/gglanzani/qmk_firmware/tree/master/keyboards/binepad/bnk9/keymaps/gglanzani
[repository]: https://github.com/gglanzani/qmk_firmware/tree/master/keyboards/binepad/bnk9/keymaps/gglanzani

[^1]: For those curious about the quality: the product finish of the knob and the buttons is nice, while the USB-C port feels finnicky at times 
