# Notes on this fork

This is a fork of [i3-gaps](https://github.com/Airblader/i3), which in turn is a
fork of the [i3wm](https://github.com/i3/i3) tiling window manager. The original
README is below.

This fork adds handling for KDE Plasma, allowing i3 to be used as the window
manager for the Plasma desktop. While there are lots of instructions floating
around (e.g. [this
tutorial](https://userbase.kde.org/Tutorials/Using_Other_Window_Managers_with_Plasma#I3_configuration)),
using vanilla i3 and adding lots of rules for handling notifications does not
work well. The underlying problem is that i3 does not support some of the
`_NET_WM_*` atoms used by Plasma, so you can't even write good rules. This fork
adds the necessary handling.

In short, with this fork:

- The `for_window` rules that various tutorials provide for handling
  notifications **are not needed**. Notifications are placed according to KDE
  settings.
- It is **not necessary** to kill the Plasma desktop window, and it's possible to
  let Plasma handle the desktop background, have widgets on the desktop, have
  icons, etc; whether you actually want that with a tiling WM is up to you 😉.
- The Plasma widget selection window behaves properly instead of being
  borderline unusable.

## Branches

I intend to provide a branch with KDE-specific changes on top of each tag (i.e.
release) of upstream i3-gaps, from 4.20 onward. There is no master branch. If
you wish to play with a development version of i3, I suggest you cherry-pick the
commit with KDE changes from the latest release. I'm keeping all KDE-specific
changes squashed in a single commit to make this straightforward.

## Configuration

As noted above, **no special configuration is needed**. Although if you use
tools like `kcalc` you may want to add rules to make them floating, but that's a
matter of preference.

## Multiple monitors

If you often connect and disconnect monitors, Plasma sometimes gets confused and
stops providing correct size for Plasma pop-ups. The symptom is "cropped",
unusable pop-ups. It's not clear yet why this happens, but it is easily avoided
by restarting Plasma. It's a good idea to have a key binding on hand, e.g.

```
bindsym Mod4+m exec kquitapp5 plasmashell && kstart5 plasmashell
```

If you use `krunner`, restart it as well.

**This does not affect static setups.**

## Credits

KDE support is due to:

- Marius Muja
- [sLite](https://github.com/sLite)
- [Paul Du](https://github.com/PJK136)
- [Howard Cheung](https://github.com/h0cheung)
- [Jure Varlec](https://github.com/exzombie)

----------------------------

# i3-gaps

## What is i3-gaps?

i3-gaps is a fork of [i3wm](https://www.i3wm.org), a tiling window manager for X11. It is kept up to date with upstream, adding a few additional features such as gaps between windows (see below for a complete list).

![i3](http://i.imgur.com/y8sZE6o.jpg)

## How do I install i3-gaps?

Please refer to the [wiki](https://github.com/Airblader/i3/wiki/installation).

## Where can I get help?

For bug reports or feature requests regarding i3-gaps specifically, open an issue on [GitHub](https://www.github.com/Airblader/i3). If your issue is with core i3 functionality, please report it [upstream](https://www.github.com/i3/i3).

For support & all other kinds of questions, you can ask your question on [GitHub Discussions](https://github.com/i3/i3/discussions).

# Features

## i3

### gaps

*Note:* In order to use gaps you need to disable window titlebars. This can be done by adding the following line to your config.

```
# You can also use any non-zero value if you'd like to have a border
for_window [class=".*"] border pixel 0
```

Gaps are the namesake feature of i3-gaps and add spacing between windows/containers. Gaps come in two flavors, inner and outer gaps wherein inner gaps are those between two adjacent containers (or a container and an edge) and outer gaps are an additional spacing along the screen edges. Gaps can be configured in your config either globally or per workspace, and can additionally be changed during runtime using commands (e.g., through `i3-msg`).

*Note:* Outer gaps are added to the inner gaps, i.e., the gaps between a screen edge and a container will be the sum of outer and inner gaps.

#### Configuration

You can define gaps either globally or per workspace using the following syntax. Note that the gaps configurations should be ordered from least specific to most specific as some directives can overwrite others.

```
gaps [inner|outer|horizontal|vertical|top|left|bottom|right] <px>
workspace <ws> gaps [inner|outer|horizontal|vertical|top|left|bottom|right] <px>
```

The `inner` and `outer` keywords are as explained above. With `top`, `left`, `bottom` and `right` you can specify outer gaps on specific sides, and `horizontal` and `vertical` are shortcuts for the respective sides. `<px>` stands for a numeric value in pixels and `<ws>` for either a workspace number or a workspace name.

#### Commands

Gaps can be modified at runtime with the following command syntax:

```
gaps inner|outer|horizontal|vertical|top|right|bottom|left current|all set|plus|minus|toggle <px>

# Examples
gaps inner all set 20
gaps outer current plus 5
gaps horizontal current plus 40
gaps outer current toggle 60
```

With `current` or `all` you can change gaps either for only the currently focused or all currently existing workspaces (note that this does not affect the global configuration itself).

You can find an example configuration in the [wiki](https://github.com/Airblader/i3/wiki/Example-Configuration).

### Smart Gaps

Gaps can be automatically turned on/off on a workspace in certain scenarios using the following config directives:

```
# Only enable gaps on a workspace when there is at least one container
smart_gaps on

# Only enable outer gaps when there is exactly one container
smart_gaps inverse_outer
```

### Smart Borders

Smart borders will draw borders on windows only if there is more than one window in a workspace. This feature can also be enabled only if the gap size between window and screen edge is `0`.

```
# Activate smart borders (always)
smart_borders on

# Activate smart borders (only when there are effectively no gaps)
smart_borders no_gaps
```

### Smart Edge Borders

This extends i3's `hide_edge_borders` with a new option. When set, edge-specific borders of a container will be hidden if it's the only container on the workspace and the gaps to the screen edge is `0`.

```
# Hide edge borders only if there is one window with no gaps
hide_edge_borders smart_no_gaps
```

## i3bar

### Bar Height

The height of an i3bar instance can be specified explicitly by defining the `height` key in the bar configuration. If not set, the height will be calculated automatically depending on the font size.

```
bar {
    # Height in pixels
    height 25
}
```
