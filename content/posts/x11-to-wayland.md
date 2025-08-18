+++
date = '2023-08-31T10:19:13+02:00'
draft = false
title = 'X11 to Wayland'
tags = ["Linux"]
+++

In this post I will update my journey of moving from X11 to wayland.
Currently I am running bspwm with polybar and everything works really well.
It's fairly minimalistic and very performant.

Several distributions are shipping with wayland instead of X11 such as Debian, Fedora, Manjaro and Ubuntu.
Some time ago there were not many choices for tiling window managers, though Sway was one of the first.
Now we have more choices and one that interests me the most is hyprland.
To replace polybar I will use waybar.

Installing the packages is simple

`pacman -S hyprland`\
`pacman -S waybar`

### Lockscreen

In X11 `betterlockscreen` is a great looking lockscreen and works well with xidlelock,
as an alternative for wayland I'll be using swaylock(-effects) and swayidle.

### sxhkd replacement

With hyprland you don't need a different tool for shortcuts, it's baked in.
The nice thing in sxhkd were the keychord, this can be achieved in hyprland using [submaps](https://wiki.hyprland.org/Configuring/Binds/#submaps).

### Clipboard

Instead of `xclip` wayland has `wl-clipboard`.

### Lockscreen and idle

For the lockscreen you can use `swaylock` and for idle timeout there is `swayidle`.

### Wallpaper

`hyprpaper`

### Screenshot

`grim` together with `slurp` to select areas of the screen.

### Application launcher

[`Walker`](https://github.com/abenz1267/walker) instead of `rofi`.
There are also builds of `rofi` compatible with wayland.
But personally `Walker` is nice application launcher.

### Configuration

You can create configs in `$XDG_CONFIG_HOME` or `~/.config/`.

#### Hyprland

<https://wiki.hyprland.org/Configuring/Configuring-Hyprland/>

#### Hyprpaper

<https://github.com/hyprwm/hyprpaper>

```shell
~/.config/hypr/hyprpaper.conf
```

#### Waybar

<https://github.com/Alexays/Waybar/wiki/Configuration>

```shell
~/.config/waybar/config
```

```shell
preload = /path/to/image.png
#if more than one preload is desired then continue to preload other backgrounds
preload = /path/to/next_image.png
# .. more preloads

#set the default wallpaper(s) seen on inital workspace(s) --depending on the number of monitors used
wallpaper = monitor1,/path/to/image.png
#if more than one monitor in use, can load a 2nd image
wallpaper = monitor2,/path/to/next_image.png
# .. more monitors
```

#### Swaylock-effects

```shell
$XDG_CONFIG_HOME/swaylock/config
```

#### Swayidle

```shell
$XDG_CONFIG_HOME/swayidle/config

timeout 300 'swaylock'
timeout 600 'hyprctl dispatch dpms off' resume 'hyprctl dispatch dpms on'
before-sleep swaylock
```

This config sets an timeout for 300 seconds which then activates `swaylock`.
After 600 seconds it turns off the screen, if one were to resume it turns the screen back on.
Finally before suspending, `swaylock` is always activated.

### Neat to have in hyprland

Changing capslock to control and setting autorepeat rate is much easier.
This can be set directly in the hyprland config.

```config
input {
    kb_options = ctrl:nocaps
    repeat_rate = 50
    repeat_delay = 200
}
```

Before you would have to add a `keyboard-layout.conf` file in `/etc/X11/xorg.conf.d`

```config
Section "InputClass"
    Identifier       "keyboard_layout"
    MatchIsKeyboard  "on"
    Option           "XkbLayout"  "us"
    Option           "XkbOptions" "ctrl:nocaps"
    Option           "AutoRepeat" "200 10"
EndSection
```
