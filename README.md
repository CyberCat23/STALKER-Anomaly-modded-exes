# STALKER-Anomaly-modded-exes

Here is list of exe files for Anomaly 1.5.2 that contains all engine patches by community required for some advanced mods to work.

# Read the instructions PLEASE!!!
 ![image](https://media.moddb.com/images/members/1/772/771864/profile/image.png)

Download STALKER-Anomaly-modded-exes.zip archive.
Unpack all directories directly into your Anomaly game folder, overwrite files if requested.
Delete shader cache in launcher before first launch of the game with new exes. You only have to do it once.

# List of patches

* DLTX by MerelyMezz with edits and bugfixes by demonized, differences compare to original:

  * Attempting to override sections no longer crash the game, but prints the message into the log. All sections that triggers that error will be printed
  * Duplicate section errors now prints the root file where the error happened for easier checking mod_... ltxes
  * Print of class and script errors in console
  * DLTX received possibility to create section if it doesn't exists and override section if it does with the same symbol `@`.
  Below is the example for `newsection` that wasn't defined. Firstly its created with one param `override = false`, then its overriden with `override = true`

  ```
  @[newsection]
  override = false

  @[newsection]
  override = true
  
  ```

  * DLTX received possibility to add items to parameter's list if the parameter has structure like 
  
  ```name = item1, item2, item3```
  
    * `>name = item4, item5` will add item4 and item5 to list, the result would be `name = item1, item2, item3, item4, item5`
    * `<name = item3` will remove item3 from the list, the result would be `name = item1, item2`
    * example for mod_system_...ltx: 
    
    ```
      ![info_portions]
      >files                                    = ah_info, info_hidden_threat

      ![dialogs]
      >files                                    = AH_dialogs, dialogs_hidden_threat
      
      ![profiles]
      >files                                    = npc_profile_ah, npc_profile_hidden_threat
      >specific_characters_files                = character_desc_ah, character_desc_hidden_threat
    ```

  * Here you can get the LTXDiff tool with set of scripts for converting ordinary mods to DLTX format (https://www.moddb.com/mods/stalker-anomaly/addons/dltxify-by-right-click-for-modders-tool).

* DXML by demonized

  * Allows to modify contents of loaded xml files before processing by engine by utilizing Lua scripts
  * For more information see DXML.md guide.

* Possiblity to unlocalize Lua variables in scripts before loading, making them global to the script namespace by demonized

  * For unlocalizing a variable in the script, please refer to documentation in test file in `gamedata/configs/unlocalizers` folder

* Doppler effect of sounds based on code by Cribbledirge and edited by demonized.

  * To edit effect strength, type in console `snd_doppler_power [0, 5]`. Choosing power 0 will disable the feature

* True First Person Death Camera, that will stay with player when he dies and will react accordingly to player's head transforms, by demonized, with possibility to adjust its settings.
  * Known bugs:
    * If the player falls with face straight into the ground, the camera will clip underground due to model being clipped as well with
    
  * Additional console commands
    * `first_person_death` // Enable First Person Death Camera
    * `first_person_death_direction_offset` // FPD Camera Direction Offset (in DEGREES)
    * `first_person_death_position_offset` // FPD Camera Position Offset (x, y, z)
    * `first_person_death_position_smoothing` // FPD Camera Position Change Smoothing
    * `first_person_death_direction_smoothing` // FPD Camera Direction Change Smoothing
    * `first_person_death_near_plane_offset` // FPD Camera Near Plane Offset


* Additions to demo_record commands by demonized

  * New console commands:
    * `demo_record_blocked_input 1` will start demo_record but you won't be available to move it or stop it, its intended for manipulation via scripts executing console commands below. The console and Esc key are available
    * `demo_record_stop` will stop all launched `demo_record` commands, including with blocked input ones 
    * `demo_set_cam_direction <head, pitch, roll>` will set the direction the camera is facing and its roll. The parameters are in RADIANS, beware. Use this with `demo_set_cam_position <x, y, z>` to manipulate camera via scripts

* Added CGameObject::NetSpawn and NetDestroy callbacks to Lua (file callbacks_gameobject.script), to register callback use

  ```lua
  RegisterScriptCallback("game_object_on_net_spawn", function(obj))
  RegisterScriptCallback("game_object_on_net_destroy", function(obj))
  ```

* Added `gameobjects_registry` table in `callbacks_gameobject.script` that contains all online game objects and updates accordingly. Additionally, a global iterator `game_objects_iter` added that will go through all online game objects

  ```lua
  for obj in game_objects_iter() do
    printf(obj:section())
  end
  ```

* Pseudogiant stomps now can kill and damage any object, stalker or mutant, instead of only actor. New callbacks provided for pseudogiants attacks in callbacks_gameobject.script

  * To disable new functionality, type in console `pseudogiant_can_damage_objects_on_stomp 0`

* Additional edits and bugfixes by demonized
  
  * Additional callbacks and exports described in `lua_help_ex.script`
  * Restored "Fatal Error" MessageBox popup in case of encountering fatal engine errors like it was on Windows 7 or lower
  * In case of typical first person model/animation errors, the game will print the section that has defined model
  * MAX_TRIS const increased from 1024 to 16384
  * Enabled death animations for CWeaponAutomaticShotgun class
  * Fixed sorting news in News Tab in PDA
  * Added getting material of ray_pick() result with all of its properties
  * Added `bone_direction()` function for game objects
  * Added `level.get_music_volume()` and `level.set_music_volume()` functions to adjust music volume in runtime without messing with console commands
  * Added `on_loading_screen_key_prompt` callback for when loading screen happens and "Press Any Key to Continue" prompt appears
  * Added `on_loading_screen_dismissed` callback for when player dismisses loading screen after "Press Any Key to Continue" pressed
  * Added `on_specific_character_dialog_list` callback that allows to manipulate available actor dialog list defined in characted_desc...xml files in `<actor_dialog>` tags
  * Added `on_specific_character_init` callback that allows to manipulate information about specific character such as rank, money, faction, etc.
  * Added `bullet_on_impact` and `bullet_on_remove` callbacks, please refer to `callbacks_gameobject.script` file for available info about new callbacks
  * Potential fix for stuck monsters from OGSR Engine repo in `control_animation_base_accel.cpp`

* Fixes and features by DPurple

  * Fix of using `%c[color]` tag with multibyte font causing unexpected line ending by DPurple
  * Ability to autosave the game before crash occurs, can be disabled with console command `crash_save 0` and enabled with `crash_save 1`. Maximum amount of saves can be specified with command `crash_save_count <number>`, where number is between 0 to 20 (default is 10)

* Shader Scopes by CrookR and enhanced by Edzan, comes ready to use in the archive, delete old version first

  * Dynamic zoom is disabled by default for alternative sights (can be enabled by adding scope_dynamic_zoom_alt = true to the weapon section). For example, if you take SVD Lynx or SVD PMC with March Tactical (or other sights with adjustable zoom) and switch to alternate sight, they wont have dynamic zoom anymore
  * Possibility to set alternative sight crosshair and zoom_factor with `scope_texture_alt = <path to texture>` and `scope_zoom_factor_alt = <number>` parameters in weapon ltx
  * The main sights with dynamic zoom and binoculars now normally remember their state.
  * Added console command sds_enable [on (default)/off] to enable/disable Shader Based 2D Scopes.
  * Added sds_speed_enable [on (default)/off] console command to disable/enable mouse speed (sensitivity) effect of scope_factor when aiming.
  * Added console command sds_zoom_enable [on (default)/off] with which you can disable /enable correction of max. zoom with scope_factor, if this option is enabled then max. zoom will be such as prescribed in settings regardless of scope_factor value, if this option is disabled then max. zoom will be sum of value prescribed in settings and the increase that gives scope_factor.

  Above mentioned options are applicable only for scopes which have prescribed values in file scoperadii.script

  * Added alternative zoom control (toggle with new_zoom_enable [on/off (default)]
    * Minimal zoom is equal to either mechanical zoom or the one prescribed in section min_scope_zoom_factor.
    * The step of zoom adjustment is more precise. Also, it's possible to adjust the step of zoom with the console command zoom_step_count [1.0, 10.0], this option is also applicable to the binoculars.
  * In the new version all implementations from fakelens.script have moved directly to the engine. fakelens.script remained as a layer between the engine and scopeRadii.script

* All settings can be edited from the game options in "Modded Exes" tab
![image](http://puu.sh/JC40Y/9315119150.jpg)

## Below are the edits that are supplemental to the mods, the mods themselves **are not included**, download the mods by the links. If mods in the links provide their own exes, you can ignore them, all necessary edits are already included on this page. 

* BaS engine edits by Mortan (https://www.moddb.com/mods/stalker-anomaly/addons/boomsticks-and-sharpsticks)

* Screen Space Shaders by Ascii1457 (https://www.moddb.com/mods/stalker-anomaly/addons/screen-space-shaders)

* Heatvision by vegeta1k95 (https://www.moddb.com/mods/stalker-anomaly/addons/heatvision-v02-extension-for-beefs-nvg-dx11engine-mod/)

## Optional patches (not used in exes)

* vs2022.diff: Visual Studio 2022 solution and repo edits for building with VS2022 by mrnuku/xray-monolith and edited by demonized (the exes in this repo's archive are still build with VS2015)

## How to make my own modded exe?

Patches folder contains diffs used to create new exes, they are used to compile your own exes if you need that

How to compile exes:

1. Git clone XRay-Monolith engine repository https://bitbucket.org/anomalymod/xray-monolith.git
2. Open cloned repo in git console or in application of your choice (Sourcetree (recommended), Github Desktop)
3. Make new branch from "master" branch
4. From newly created branch make additional branches for each .diff file in "patches" folder of this repo that you want to apply
5. Apply patches in those branches via `git apply <path_to_patch>` command
6. Merge those branches back into your branch created in p.3
7. To compile the engine open the solution in VS2015, select all projects and configurations in Batch build and start a build.

## Changelog
**2023.03.25**:

* Potential fix for stuck monsters from OGSR Engine repo in `control_animation_base_accel.cpp`
* Added `bullet_on_impact` and `bullet_on_remove` callbacks, please refer to callbacks_gameobject.script file for available info about new callbacks

**2023.03.19**:

* Added true first person death camera (enabled by default), that will stay with player when he dies and will react accordingly to player's head transforms. Additional console commands
  * `first_person_death` // Enable First Person Death Camera
  * `first_person_death_direction_offset` // FPD Camera Direction Offset (in DEGREES)
  * `first_person_death_position_offset` // FPD Camera Position Offset (x, y, z)
  * `first_person_death_position_smoothing` // FPD Camera Position Change Smoothing
  * `first_person_death_direction_smoothing` // FPD Camera Direction Change Smoothing
  * `first_person_death_near_plane_offset` // FPD Camera Near Plane Offset

[![Watch the video](https://img.youtube.com/vi/Jm-DRNqnak0/default.jpg)](https://youtu.be/Jm-DRNqnak0)

* Fixed heatvision effects not applied to the player hands
* Added options menu for modded exes settings
  * From options menu you can adjust all added modded exes parameters, such as for Shader Scopes, Sound Doppler, FPD Camera and so on

![image](http://puu.sh/JC40Y/9315119150.jpg)

* Added `level.get_music_volume()` and `level.set_music_volume()` Lua functions to adjust music volume in runtime without messing with console commands

**2023.03.15**:

* Stability updates to heatvision sources
* DXML 3.0 update:
  * DXML now uses own storage for callbacks to ensure they are fired accordingly to registering order
  * Added `insertFromXMLFile` function to read contents of xml file to insert into xml_obj
  * Added optional parameter useRootNode to `insertFromXMLString` and `insertFromXMLFile` functions that will hint DXML to insert contents from a root node of parsed XML instead of the whole file (default: false) 

**2023.03.11**:

* Added heatvision support by vegeta1k95 (https://www.moddb.com/mods/stalker-anomaly/addons/heatvision-v02-extension-for-beefs-nvg-dx11engine-mod/)
* Fixed too big FOV when using shader scopes with `new_zoom_enable` command enabled

**2023.03.09**:

* Added possibility to unlocalize Lua variables in scripts before loading, making them global to the script namespace
  * For unlocalizing a variable in the script, please refer to documentation in test file in `gamedata/configs/unlocalizers` folder
* Fixed the bug where `scope_factor` settings were applied to disabled shader scopes or scopes without defined radius for shader effect
* Fixed non-working adjustable scopes upgrade for weapons

**2023.03.05**:

* Fixed the bug where the weapon with attached adjustable scope and grenade launcher will allow to zoom in with GL. To explicitly enable zooming with active grenade launcher, for whatever reason, add `scope_dynamic_zoom_gl = true` in weapon section in its .ltx file
* Possibility to add shader scopes to alternative sights
  * `scope_dynamic_zoom_alt = true` will enable adjustable scope for alt. sight
  * `scope_texture_alt = <path to texture>` will allow to specify what crosshair to use for alt. sight
* Correct zoom_factor calculation for adjustable scopes with shader scopes enabled, you wont get any extra zoom from shader on top of engine FOV
* `scope_factor` console command that changes zoom by shader scopes now works in real time
* Lowered chromatic abberation and scope blur effect, increasing the quality of image  

**2023.02.20**:

* New SSS update
* New demo-record.diff that contains these changes

  * New console commands:
    * `demo_record_blocked_input 1` will start demo_record but you won't be available to move it or stop it, its intended for manipulation via scripts executing console commands below. The console and Esc key are available
    * `demo_record_stop` will stop all launched `demo_record` commands, including with blocked input ones 
    * `demo_set_cam_direction <head, pitch, roll>` will set the direction the camera is facing and its roll. The parameters are in RADIANS, beware. Use this with `demo_set_cam_position <x, y, z>` to manipulate camera via scripts

**2023.02.18**:

* New SSS update

**2023.02.16**:

* Added `gameobjects_registry` table in `callbacks_gameobject.script` that contains all online game objects and updates accordingly. Additionally, a global iterator `game_objects_iter` added that will go through all online game objects

  ```lua
  for obj in game_objects_iter() do
    printf(obj:section())
  end
  ```

* Pseudogiant stomps now can kill and damage any object, stalker or mutant, instead of only actor. New callbacks provided for pseudogiants attacks in callbacks_gameobject.script

  * To disable new functionality, type in console `pseudogiant_can_damage_objects_on_stomp 0`

**2023.01.28**:

* DLTX received possibility to create section if it doesn't exists and override section if it does with the same symbol `@`.
Below is the example for `newsection` that wasn't defined. Firstly its created with one param `override = false`, then its overriden with `override = true`

```
@[newsection]
override = false

@[newsection]
override = true

```
* Added `bone_direction()` function for game objects
* Updated `lua_help_ex.script` with new functions available

**2023.01.23**:

* MAX_TRIS const increased from 1024 to 16384

**2023.01.13**:

* Fix corrupted print of duplicate section

**2023.01.06**:

* In case of typical first person model/animation errors, the game will print the section that has defined model

**2023.01.03**:

* Added CGameObject::NetSpawn and NetDestroy callbacks to Lua (file callbacks_gameobject.script), to register callback use

  ```lua
  RegisterScriptCallback("game_object_on_net_spawn", function(obj))
  RegisterScriptCallback("game_object_on_net_destroy", function(obj))
  ```

* DXML will no longer process translation strings of non eng/rus languages, they aren't supported yet
* New lua_help_ex.script file where new engine exports will be described
* Exported additional CWeapon functions considering weapon's RPM, handling and recoil
* Exported functions to get and set actors walk accel and walkback coeff

  ```lua
  db.actor:get_actor_walk_accel()
  db.actor:set_actor_walk_accel(float)
  db.actor:get_actor_walk_back_coef()
  db.actor:set_actor_walk_back_coef(float)
  ```
  * DLTX received possibility to add items to parameter's list if the parameter has structure like 
  
  ```name = item1, item2, item3```
  
    * `>name = item4, item5` will add item4 and item5 to list, the result would be `name = item1, item2, item3, item4, item5`
    * `<name = item3` will remove item3 from the list, the result would be `name = item1, item2`
    * example for mod_system_...ltx: 
    
    ```
      ![info_portions]
      >files                                    = ah_info, info_hidden_threat

      ![dialogs]
      >files                                    = AH_dialogs, dialogs_hidden_threat
      
      ![profiles]
      >files                                    = npc_profile_ah, npc_profile_hidden_threat
      >specific_characters_files                = character_desc_ah, character_desc_hidden_threat
    ```

* Exported distance_to_xz_sqr() function of Fvector
* Redesigned duplicate section error, it will additionally print what file adds the section in the first place in addition to the file that has the duplicate
