---
title: Flash the Firmware of the Preonic from Keyboardio
tags: ["chrysalis", "customization", "firmware", "keyboardio", "preonic", "programming", "tutorial"]
author: Giovanni Lanzani
date: 2025-08-06
snippet: |
  I recently bought the Preonic keyboard, as my two Model 01 were not really portable.

  The Preonic is a super keyboard that is customizable and one of the first things I wanted to do was change the layout a bit. The suggested way is to use Chrysalis but it requires Chrome-based browsers, which I don't use. I resorted to the terminal.
url: /2025/flash-the-firmware-of-the-preonic-from-keyboardio
---

I recently bought the [Preonic] keyboard, as my two [Model 01] were not really portable.

The Preonic is a superb keyboard that is highly customizable and connects either by wire or bluetooth. I couldn't pass the opportunity.

Since it's customizable, one of the first things I wanted to do was change the layout a bit. The suggested way is to use [Chrysalis] but it requires Chrome-based browsers, which I don't use. So, I resorted to the terminal.

As there were no step-by-step instructions, this post documents how I've done it.

### Getting started

The first step is to clone the repository and set things up. From your terminal, type:


{{< highlight sh "style=github">}}
git clone https://github.com/keyboardio/Kaleidoscope
cd Kaleidoscope
export KALEIDOSCOPE_DIR=${HOME}/git/Kaleidoscope  # or, if you use fish, use set -x KALEIDOSCOPE_DIR ${HOME}/git/Kaleidoscope
make setup # this is done the first time
{{< / highlight >}}

### Customizing the layout

If everything goes well, we should be ready to customize our layout. To do so

{{< highlight sh >}}
# for some reason, it's not where their other firmwares are
cd plugins/Kaleidoscope-Hardware-Keyboardio-Preonic/examples/Devices/Keyboardio/Preonic
vim Preonic.ino # edit the layout with whatever editor you have
make compile
{{< / highlight >}}

If everything went well (e.g., you didn't mess up), we should be ready to flash the layout.

### Flashing the layout into the Preonic

To flash the layout, we need to put the keyboard in bootloader model (these instructions minus the last step come from their website): 

- First, make sure the power switch on your keyboard is turned off (with the white side of the power switch on the back hidden and the black side of the switch visible)
- Disconnect the keyboard's USB cable from the computer.
- Hold in the "Hyper" key in the bottom left corner of the keyboard.
- Plug the USB cable back into the computer.
- The butterfly on the top of the keyboard should glow green.
- You can now release the "Hyper" key.
- In the terminal, type `make flash`

That's it! You should now have a working layout. If you want to generate an SVG of the layout, you can type

{{< highlight sh >}}
python generate_layout.py
{{< / highlight >}}

There are a couple of quirks in the python file, but it will output something that looks like

{{< figure src="/images/layout_preonic.svg" width="90%" alt="The layout as produced by a modified version of the generate_layout.py file" >}}

Well done, Jesse and Kaia!

[Preonic]: https://www.kickstarter.com/projects/keyboardio/preonic?ref=7jsno1
[Chrysalis]: https://chrysalis.keyboard.io/
[firmware]: https://github.com/keyboardio/Kaleidoscope
[Model 01]: https://www.kickstarter.com/projects/keyboardio/the-model-01-an-heirloom-grade-keyboard-for-seriou
