# overview
this is my config for recording csgo demos with HLAE. it is made to be quick and easy to use, with plenty of customization options. it records at a high framerate and interpolates the frames to create a motion blur effect (similar to sony vegas resample or after effects frame blending).

the "raw" version of the cfg is functionally the same but doesn't apply motion blur to the rendered clips.

# requirements
- [HLAE (with FFMPEG)](https://github.com/advancedfx/advancedfx/releases)

 ***use the installer and tick the checkbox that says "reinstall FFMPEG"**

# how to use
## basic instructions

1. download adam.cfg from the [releases](https://github.com/abandonedpools/hlae-cfg/releases) tab
2. place adam.cfg in your config folder (steamapps/common/Counter-Strike Global Offensive/csgo/cfg)
2. launch csgo with HLAE
2. open the demo and type "exec adam" in console
3. go to the round/tick you want to record and press <kbd>o</kbd> on your keyboard to start recording (you can change this bind in the cfg file)
4. once you are done, press <kbd>o</kbd> again to stop recording

you should execute the config every time you launch a demo or else some commands will not be executed properly. (delag and such)

by default the recordings will be saved in a folder named "untitled_rec" in your Counter-Strike Global Offensive folder.

if you do not see an mp4 file or encounter other issues please refer to the [bottom of this page](#troubleshooting).

## simple command shortcuts
aliases you can type in console to quickly execute/toggle commands.

| alias | command(s) | description |
| --- | --- | --- |
| localplayer | mirv_deathmsg localplayer xTrace | (toggle) highlight kills of the player currently being spectated. **(may not work with pov demos)** |
| block | mirv_deathmsg filter add attackerMatch=!xTrace victimMatch=!xTrace block=1 lastRule=1 | (toggle) block kills that are not from the player you are currently spectating **(may not work with pov demos)** |
| lifetime | mirv_deathmsg lifetimeMod 10 | (toggle) make highlighted kills last longer in the killfeed |
| clearmsg | mirv_deathmsg filter clear; mirv_deathmsg lifetimeMod default; mirv_deathmsg localplayer default | clear all the killfeed filters (last 3 commands) |
| id | mirv_listentities isplayer=1 | show player XUID's in console |
| noflash | mat_suppress effects/flashbang.vmt; mat_suppress effects/flashbang_white.vmt | disable flashbangs |
| radar | toggles cl_drawhud_force_radar (0/-1) | toggle the radar off/on |
| chat | toggles cl_chatfilters (0/1) | toggle the chat off/on |
| hud | toggles cl_draw_only_deathnotices (0/1) |toggle the hud off/on (minus the crosshair and killfeed) |
| commands | n/a | print all the commands shown above in the console |


### note:
`localplayer` and `block` **do not** work with mirv_pov/pov demos.  you will need to type the commands in the console and replace `Trace` with your XUID, like [this](https://github.com/advancedfx/advancedfx/wiki/Source%3Amirv_deathmsg#how-to-block-everything-except-a-specific-player).

## customizing blur/video settings
| line | command | default value | description |
| --- | --- | --- | --- |
| 10 | host_framerate | 1080 | this is how many frames per second the game will run at during recording, higher will make the blur look smoother and **will not** make the file size bigger, but it will take longer. 900 and up should look good |
| 25 | -crf | 9 | video quality ([what is crf?](https://trac.ffmpeg.org/wiki/Encode/H.264#crf)) |
| 30 | mirv_streams settings edit blur exposure | 0.65 | this is how much blur the recording will have, this setting is good if you render your final video at 24-30 fps but you might want more blur for lower framerate fragmovies |
| 31 | mirv_streams settings edit blur fps | 60 | this is the fps that your recording will be. 60 is good enough for most fragmovies if you render at 30fps but you can make it higher if you need to. this setting will also affect how much blur your video has. (e.g. 60 fps 1 exposure = 120 fps 2 exposure = 240 fps 4 exposure... etc.) |

### note:
the last two settings do not appear in the "raw" version of the cfg since it does not have a sampler, the output fps is instead determined by the `host_framerate` value which is defaulted to 60. note that higher framerates may require you to adjust the bitrate (crf value), leading to larger files.

## optional commands
these commands are already in the cfg file but are disabled by default, to enable them remove the "// " in front of them.

| line | command(s) | description |
| --- | --- | --- |
| 37 | demo_index 1 | makes navigation inside of demos less laggy, disabled by default because it can sometimes cause bugs |
| 39 | mp_display_kill_assists 0 | removes assists from killfeed |
| 41 | mirv_streams record voices 1;snd_setmixer voip vol 0 | record in-game voice chat in separate audio files (doesn't work with pre-2015 voice codec) |
| 43 | mirv_deathmsg filter add noscope=0 thrusmoke=0 attackerblind=0 | remove smoke, noscope and blind icons from the killfeed |
| 45 | cl_show_observer_crosshair 0 | disable observer crosshair |
| 47 | mirv_streams record name "[path]" | custom recording path (records faster if you save to an SSD) |
| 49 | mat_postprocess_enable 0; mat_colorcorrection 0; mat_disable_bloom 1 | disable post processing effects |

# troubleshooting
## no video file
if you do not see a video file, make sure you ticked the "reinstall FFMPEG" checkbox when installing HLAE. you must use the installer, not the portable zip.

## error when launching HLAE
if you get an error like this when launching it means your HLAE is either out of date or a game update broke certain HLAE features. note that if you just click OK the game *might* still launch and you *might* still be able to record demos if none of the features used here are affected.

![Error box with text: "Problem in C:\source\advancedffx\AfxHookSource\adresses.cpp:289"](https://media.discordapp.net/attachments/893330030770405437/1085619085372563537/unknown.png?width=360&height=137)

## error messages when executing the config
if you see error messages in the console after executing the config, such as `AFXERROR: There is no recording setting named blur.` or `Error: invalid streamName.`, dont be worried they are normal and don't affect anything.

## other issues
if your issue isn't listed here you can create an issue in the [issues](https://github.com/abandonedpools/hlae-cfg/issues) tab, or politely ask for help in [my discord](https://discord.gg/ATHaaNuxwU) or the [hlae discord](https://discord.gg/NGp8qhN)
