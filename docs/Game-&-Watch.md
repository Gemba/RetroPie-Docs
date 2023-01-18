***

![game_and_watch_banner](https://cloud.githubusercontent.com/assets/10035308/13205492/9b6db396-d8a6-11e5-8a74-51b2a74a0f35.png)

***
_Game & Watch is a line of handheld electronic games produced by Nintendo from 1980 to 1991._

***

| Emulator | Rom Folder | Extension | BIOS |  Controller Config |
| :---: | :---: | :---: | :---: | :---: |
| [lr-gw](https://github.com/libretro/gw-libretro) | gameandwatch  | .mgw | none | /opt/retropie/configs/gameandwatch/retroarch.cfg |

## Emulator: [lr-gw](https://github.com/libretro/gw-libretro)

**lr-gw** is a simulator and not an emulator. This means that the games that can be played with it aren't actually the original games, but recreations of the games combined with the original artwork and an image of the handheld.

## ROMS
Accepted File Extensions: **.mgw**

Place your Game & Watch ROMs in
```
/home/pi/RetroPie/roms/gameandwatch
```

Games can be found at the [Libretro Handheld Electronic Games page](https://bot.libretro.com/assets/cores/Handheld%20Electronic%20Game/).

### Video Guide

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/DzbsfCC77IQ" title="RetroPie: Game and Watch emulation on a Raspberry Pi" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; allowfullscreen"></iframe>

## Controls

lr-gw utilises Retroarch configurations

Add custom retroarch controls to the retroarch.cfg file in
```shell
/opt/retropie/configs/gameandwatch/retroarch.cfg
```
For more information on custom RetroArch controls see: [RetroArch Configuration](RetroArch-Configuration)
