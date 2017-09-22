# Italian layout variant based on US Dvorak layout for Kinesis Advantage

This repository provides a _US variant_ layout for XKB that supports Italians deadkeys (i.e. vowels with diacritics) especially meant for [Kinesis Advantage keyboard](http://www.kinesis-ergo.com/shop/advantage-for-pc-mac/).

This is the original Dvorak US layout:

![Kinesis Advantage Dvorak US layout](http://drive.google.com/uc?id=0B8TPut6hfwH0ei1GS3ZkSGxpSVk)

and this is the Dvorak US layout with Italian variant:

![Kinesis Advantage Dvorak US layout Italian variant](http://drive.google.com/uc?id=0B8TPut6hfwH0WUNxZkxnV3ptVFE)

Even if everything in this repository is meant for Kinesis Advantage, I'm sure that you can learn from here everything you need to build, remap and customize your own keyboard layout for Linux.

If you need some help to understand better how all the parts are working, please have a look at [my guide on Medium.com](https://medium.com/@damko/a-simple-humble-but-comprehensive-guide-to-xkb-for-linux-6f1ad5e13450#.gnyfmp6y4)

## Install

### Before installing this repo

Run a backup of your current `/usr/share/X11/xkb` configuration directory

    su -
    cd /usr/share/X11/
    tar zcf previous-xkb.tgz xkb

So that you can roll back any time with this command:

    su -
    cd /usr/share/X11/
    rm -fR xkb
    tar zxf previous-xkb.tgz

### In case of troubles

If you have already modified your `/usr/share/X11/xkb` and you don't trust it anymore you can replace it with the .tgz file that you can see in this repo which comes with Debian testing (@ September 2017):

    su -
    cd /usr/share/X11/
    rm -fR xkb
    tar zxf {path}/xkb-original.tgz

### Now you can install

Be aware that this repository has two branches:

* master
* hack

**hack** is the branch you might be interested in and contains the modified files. **master** contains the original files, you know, in case of need and for [comparison](https://github.com/damko/xkb_kinesis_advantage_dvorak_layout/compare).

    git clone git@github.com:damko/xkb_kinesis_advantage_dvorak_layout.git
    cd xkb_kinesis_advantage_dvorak_layout
    git checkout hack
    sudo cp -fR * /usr/share/X11/xkb/

These commands will apply the changes:

    setxkbmap -model kinesis -layout us -variant kinesis_adv_dvorak_it -option
    setxkbmap -model kinesis -layout us -variant kinesis_adv_dvorak_it -option -option "lv3:rwin_switch"

You will end up with a standard American Dvorak layout with all the additional vowels with diacritics required for the Italian language.

## About vowels with diacritics

To type **à** hold the SUPER_R (the key with the windows flag on the right thumb-block of your Kinesis) and hit _a_

To type **À** hold the SUPER_R + SHIFT (any) and hit _a_

Of course the same goes for each key having more hosting more than 2 characters (3rd level characters) like 1, 4, 5 etc.

## Multiple layouts

If you want to load an additional layout, let's say the Russian one providing Cyrillic, run these commands

    setxkbmap -model kinesis -layout us,us -variant kinesis_adv_dvorak_it,rus -option
    setxkbmap -model kinesis -layout us,us -variant kinesis_adv_dvorak_it,rus -option "lv3:rwin_switch,grp:alt_space_toggle"

You can switch between the Italian or the Russian layout by hitting ALT+SPACE

If you want to load the US layout and the Italian variant run these commands:

    setxkbmap -model kinesis -layout us,us -variant ,kinesis_adv_dvorak_it -option
    setxkbmap -model kinesis -layout us,us -variant ,kinesis_adv_dvorak_it -option "lv3:rwin_switch,grp:alt_space_toggle"

## Test and run

Run

    setxkbmap -print -verbose 10

to check that all the parameters where loaded correctly by XKB.

You should see something like this (depending on what you have set in the `setxkbmap` command):

    Setting verbose level to 10
    locale is C
    Trying to load rules file ./rules/evdev...
    Trying to load rules file /usr/share/X11/xkb/rules/evdev...
    Success.
    Applied rules from evdev:
    rules:      evdev
    model:      kinesis
    layout:     us,us
    variant:    kinesis_adv_dvorak_it,rus
    options:    caps:none,shift:both_capslock,lv3:rwin_switch,grp:alt_space_toggle
    Trying to build keymap using the following components:
    keycodes:   evdev+aliases(qwerty)
    types:      complete
    compat:     complete
    symbols:    pc+us(kinesis_adv_dvorak_it)+us(rus):2+inet(evdev)+group(alt_space_toggle)+level3(rwin_switch)+capslock(none)+shift(both_capslock)
    geometry:   kinesis(model100)
    xkb_keymap {
        xkb_keycodes  { include "evdev+aliases(qwerty)" };
        xkb_types     { include "complete"  };
        xkb_compat    { include "complete"  };
        xkb_symbols   { include "pc+us(kinesis_adv_dvorak_it)+us(rus):2+inet(evdev)+group(alt_space_toggle)+level3(rwin_switch)+capslock(none)+shift(both_capslock)"    };
        xkb_geometry  { include "kinesis(model100)" };
    };

Test your layout by typing some text and, if everything is working as expected, make the modifications permanent by editing the `/etc/default/keyboard` in this way:

    XKBMODEL="kinesis"
    XKBLAYOUT="us,us"
    XKBVARIANT="kinesis_adv_dvorak_it,rus"
    XKBOPTIONS="lv3:rwin_switch,grp:alt_space_toggle"

Reboot your machine, run again `setxkbmap -print -verbose 10` and test again by typing some text. Everything should work as expected.