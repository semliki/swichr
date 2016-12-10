Swichr
======

Swichr is a small bash script that allows you to easily switch between colorschemes by using a system of templates. Gone are the days of editing every configuration file manually!

This script is inspired by [dkeg's](https://github.com/dkeg) script [gtkColor](https://github.com/dkeg/inspin/blob/master/gtkColor). The color scheme layout is also inspired by his color scheme files [(see here)](https://github.com/dkeg/crayolo).


Usage
-----

`git clone https://github.com/illumni/swichr.git`

```
swichr -s [COLORSCHEME] [OPTIONS]...
Change color schemes easily using a templating system.

  -s     color scheme name
  -c     set a custom color scheme directory
  -b     set a custom base directory
  -t     set a custom template directory
  -w     set a custom wallpaper directory
  -k     view the currently installed scheme
  -d     debug mode
  -h     display this help text
```


Colorscheme
-----------

The first step is to make a colorscheme file. There are a couple included in this repo. The main layout should be:

```
! -- Author: [author name]
! -- Title: [colorscheme title]
! -- Created: [date/year created]
! -- Wall: [wallpaper name here]

#define bg      #000000
#define fg      #000000
#define hl      #000000
#define na      #000000
#define icbs    #000000
#define icfr    #000000
#define icbk    #000000

#define blk     #000000
#define bblk    #000000
#define red     #000000
#define bred    #000000
#define grn     #000000
#define bgrn    #000000
#define ylw     #000000
#define bylw    #000000
#define blu     #000000
#define bblu    #000000
#define mag     #000000
#define bmag    #000000
#define cyn     #000000
#define bcyn    #000000
#define wht     #000000
#define bwht    #000000
```

It's pretty much just a .Xresources file with some extra jazz. You can use whatever variable names, but be aware that you'll also have to change them in the script itself.

The two variables `hl` and `na` correspond to highlight and inactive. These options are really only for window manager configurations (or whatever else you might like to use them for).

The variables `icbs`, `icfr`, and `icbk` correspond to icon colors. See the [example template](https://github.com/illumni/swichr/blob/master/templates/example-template) for more information.

The `Wall` variable in the header corresponds to the wallpaper for the colorscheme. You don't have to use paths for this, as the paths will be configured in the script.


Templates
---------

Templates can be anything. GTK themes, window manager configurations, xresources files, etc. The template system is very simple and easy to use.

A template should have `#? dest /filled/template/destination/here` somewhere in the file. It shouldn't matter whether it's at the bottom or the top, so long as it's in there. If it isn't, the template won't be moved to the destination.

In order to actually use the variables from the colorschemes you'll have to put the variables in the template in place of where you'd want a color to be. For example with a snippet of my i3 configuration file:

```
#? dest /home/illu/.i3/config

## i3 config ##
## by illu   ##

### Variables ###

# Modifiers

    set $mod    Mod4
    set $mod1   Mod1

# Colors

    set $fgcolor    $fg_swichr
    set $bgcolor    $bg_swichr
    set $focused    $hl_swichr
    set $inactiv    $na_swichr
    set $wall       $wall_swichr
```

This works with any of the colors. If you want red, you'd use `$red_swichr`.

All variables except for `$wall_swichr` should be in hex format. If you use a window manager (like spectrwm) that doesn't use hex, you may be out of luck. In the future I may add support for other color systems like that, but for now hex is the only option. As with the colorschemes, I will include a couple examples in the repo.

You can also use this with svg-based icons. I use flattrcolor from the [wpgtk](https://github.com/deviantfero/wpgtk) repository. I'll include a template for this type 
of file. Here's a snippet of how to use it:

```xml
    <path
       style="fill:$icon_front_swichr;fill-opacity:1;fill-rule:evenodd;stroke:none;stroke-width:1px;stroke-linecap:butt;stroke-linejoin:miter;stroke-opacity:1"
       d="M 39 15 C 39 15 35 15 33 19 L 29 19 L 4 19 L 2 19 C 2 19 1.8680203 19.016906 1.7109375 19.058594 C 1.6649814 19.072636 1.6190155 19.081363 1.5761719 19.101562 C 1.3019415 19.211398 1 19.44477 1 20 L 1 22 L 1 23 L 1 57 C 1 57.55397 1.44603 58 2 58 L 62 58 C 62.55397 58 63 57.55397 63 57 L 63 18 L 63 16 C 63 15.44603 62.55397 15 62 15 L 61 15 L 39 15 z "
       transform="translate(384.57143,483.798)"
       id="folderFront" />
```


Script Configuration
--------------------

Swichr's configuration is very simple. The only thing you should have to configure is the base directory that swichr will be using (swichr doesn't have to be IN this directory though)

Snippet from the script:

```
# Main directories the script will use
BASEDIR=/home/illu/.colorschemes
COLORDIR=$BASEDIR/colors
TEMPLATEDIR=$BASEDIR/templates
WALLPAPERDIR=$BASEDIR/walls
```

If swichr doesn't see these directories, it will automatically create them.

Swichr also has a configuration option to setup a "linked script", which is a script to run after swichr runs. This could be linked to a screenshot tool, a color printing tool, or maybe a refresh utility used to refresh the gtk/icon/other themes without doing it manually.

You would use it like so:

`LINKEDSCRIPT=/path/to/script`
`LINKEDSCRIPT=scrot -f`
`LINKEDSCRIPT=archey3`

Other
-----

- Swichr will automatically backup a destination file if it already exists.
- All variables are required in a colorscheme file, but if you don't want to use a specific variable, you don't have to put them into the template(s).
    - The wallpaper variable has to exist, but you don't have to call to it. If it doesn't exist, the script won't work. (A planned feature is to make wallpaper optional)
- Swichr will output some variables into a file called `.current` in your home folder. It's use to print the current scheme. You can also use this file to help integrate swichr's changes into other files (i.e, a script that will automatically update the gtk theme or change the wallpaper)

The `.current` file should look something like:

```
Base dir: /home/illu/.colorschemes
Curr Scheme: /home/illu/.colorschemes/colors/haus
Title: haus
Author: illu
```
