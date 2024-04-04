![](images/cannonball/logo.png)

***
_Cannonball is a program which allows you to play an enhanced version of Yu Suzuki's seminal arcade racer - [OutRun](https://segaretro.org/OutRun) - on a variety of systems._

***

### Port: [Cannonball](https://github.com/djyt/cannonball/wiki/Cannonball-Manual)

### Roms

Place the OutRun Revision B MAME rom in
```
/home/pi/RetroPie/roms/ports/cannonball

```
### Custom music
Cannonball can play WAVs instead of using the inbuilt game music.

Custom WAV files can be copied to `/home/pi/RetroPie/roms/ports/cannonball/music`, then enabled by editing the `config.xml` configuration file. Each track can be configured individually in the `custom_music` section, by specifying the _full_ path to the WAV file and toggling the `enable` flag:

``` xml
 <custom_music>
          <!-- Magical Sound Shower Replacement -->
          <track1 enabled="1">
               <title>MAGICAL SOUND SHOWER REMIX</title>
               <filename>/home/pi/roms/ports/cannonball/music/track1.wav</filename>
          </track1>
          <!-- Passing Breeze Replacement -->
          <track2 enabled="0">
               <title>PASSING BREEZE REMIX</title>
               <filename>/home/pi/roms/ports/cannonball/music/track2.wav</filename>
          </track2>
          <!-- Splash Wave Replacement -->
          <track3 enabled="1">
               <title>SPLASH WAVE REMIX</title>
               <filename>/home/pi/roms/ports/cannonball/music/track3.wav</filename>
           </track3>
          <track4 enabled="1">
               <title>LAST WAVE REMIX</title>
          <filename>/home/pi/roms/ports/cannonball/music/track4.wav</filename>
          </track4>
</custom_music>
```

The `config.xml` file is located in `/opt/retropie/configs/ports/cannonball` or (via file shares) in `\\retropie\configs\ports\cannonball`.

See [HERE](https://github.com/djyt/cannonball/blob/master/roms/roms.txt) for exact rom info.

See [HERE](https://github.com/djyt/cannonball/wiki/Cannonball-Manual) for more complete documentation.
