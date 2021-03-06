# Pi-Apps   ![logo](https://github.com/Botspot/pi-apps/blob/master/icons/logo-64.png?raw=true)
## Raspberry Pi App Store for Open Source Projects

There are many open-source, community-developed software projects for Raspberry Pi, yet very few people know about them. Pi-Apps aims to improve this, functioning as a software catalog and standardizing installation.

### To install Pi Apps
```
git clone https://github.com/Botspot/pi-apps
/home/pi/pi-apps/install
```
The install script ensures YAD is installed, creates two menu buttons, and an autostarted updater. Nothing is modified outside your home directory.
### To run Pi Apps
Menu -> Accessories -> Pi Apps, or type `./pi-apps/gui`.
### Useful links
- [Pi-Apps Discord server](https://discord.gg/PCgG2v)
- [Send Botspot a donation](https://paypal.me/josephmarchand)
- [View changelog](https://github.com/Botspot/pi-apps/blob/master/CHANGELOG.md)
- [Report an error](https://github.com/Botspot/pi-apps/issues/new)
- [Leepspvideo Pi-Apps walkthrough](https://www.youtube.com/watch?v=zxyWQ3FV98I)
- [ETA Prime's Pi-Apps & Minecraft Java video](https://www.youtube.com/watch?v=oqNWJ52DLes)
### Basic usage
- This is the **main window**:  
![main window](https://github.com/Botspot/pi-apps/blob/master/icons/screenshots/main%20window.png?raw=true)  
Use the main window to quickly browse the selection of apps and easily install them.  
- If you double-click an app, or select and app and click Details, you will see the **Details window**.  
![details](https://github.com/Botspot/pi-apps/blob/master/icons/screenshots/details%20window.png?raw=true)  
- The **updater window** may pop up when you launch Pi-Apps:  
![updates](https://github.com/Botspot/pi-apps/blob/master/icons/screenshots/updates%20available.png?raw=true)  
Unless you have a very good reason not to, clicking 'Update now' is strongly recommended.  
- Pi-Apps **Settings** can be configured by launching Menu -> Preferences -> Pi-Apps Settings.  
![settings](https://github.com/Botspot/pi-apps/blob/master/icons/screenshots/settings.png?raw=true)  
- If you click **New App** in Settings, you can easily create your own Apps with a wizard-style sequence of windows.  
![create app](https://github.com/Botspot/pi-apps/blob/master/icons/screenshots/create%20app.png?raw=true)  
It helps you select an icon, create & debug install/uninstall scripts, write a description, and more.  
### Donations raised so far:
**$0**
### Terminal usage
 - The `manage` script is similar to `apt-get` - it handles installing apps, uninstalling them, keeping them updated, and more. `Manage` does not include a GUI, though in some cases a dialog will appear to ask you a question.
   - To **install** an app, run this:
`/home/pi/pi-apps/manage install Zoom`
   - To **uninstall** an app:
`/home/pi/pi-apps/manage uninstall Zoom`
   - To **update** a single app:  
`/home/pi/pi-apps/manage update Zoom`
Note that if an app is up-to-date, no files will be moved around.
   - To **check** all apps for updates:  
 `/home/pi/pi-apps/manage check-all`
 This command will return a list of updatable apps, separated by the `|` character.
   - To **update all** apps:
  `/home/pi/pi-apps/manage update-all`
 - To **list** all apps:
 `ls /home/pi/pi-apps/apps`
 Note that this will also list the `template` app, which contains the default install & uninstall scripts. Please don't try to install it.
### How it works
 - Each 'App' is simply a small `install` script, `uninstall` script, two icon sizes, and two text files containing the description and a website URL.
 - Each App is stored in its own separate directory. `/home/pi/pi-apps/apps/` holds all these app directories. The Zoom app, for example, would be located at `/home/pi/pi-apps/apps/Zoom/`.
 - Because of the contained nature of each app folder, it's really easy to 'package' your own apps: just put the folder in a ZIP file and send it to friends. (or upload it as a [new issue](https://github.com/Botspot/pi-apps/issues/new) so your app can be added to Pi-Apps)
 - When you click Install, the selected App's `install` script is executed.
 - When you click Uninstall, the selected App's `uninstall` script is executed.
### Directory tree
 - `/home/pi/pi-apps/` This is the main folder that holds everything. In all scripts, it is represented as the `${DIRECTORY}` variable.
   - `COPYING` This file contains the GNU General Public License v3 for Pi-Apps.
   - `createapp` GUI script - this is run when you click "Create App" in Settings.  
   ![create app](https://github.com/Botspot/pi-apps/blob/master/icons/screenshots/create%20app.png?raw=true)
   - `gui` The main GUI window. This script  is responsible for displaying the App list and the Details page.
   ![main window](https://github.com/Botspot/pi-apps/blob/master/icons/screenshots/main%20window.png?raw=true)
   - `install` This script is used to install Pi-Apps. Adds a couple menu launchers, and makes sure YAD is installed.
   - `manage` This script handles installing, uninstalling, and updating Apps. It does not check or update any files outside the `apps/` directory.
   - `pi-apps.desktop` This file is a .desktop launcher, exactly the same as the main Pi-Apps launcher in Menu.
   - `pkg-install` If an App requires some `apt` packages in order to run, its `install` script will run `pkg-install`. Pkg-install records which app installed what (in the installed-packages folder BTW), so when you uninstall an App, those packages will be removed.
   - `preload` This script generates the app list for the `gui` script. If no files have been modified since last launch, `preload` won't regenerate the app list, but instead will return a previously saved version of the list. This approach reduces Pi-Apps's launch time by around 1 second.
   - `purge-installed` This does exactly the opposite of `pkg-install` This script is run when an App is being uninstalled. Purge-installed will uninstall all packages the app installed.
   - `README.md` You are reading this file right now!
   - `settings` This GUI script is executed when you launch 'Pi-Apps Settings' from the Menu.
   ![settings](https://github.com/Botspot/pi-apps/blob/master/icons/screenshots/settings.png?raw=true)
   - `uninstall` Uninstalls Pi-Apps and removes the menu launchers. Asks permission to uninstall YAD.
   - `updater` This GUI script is executed every time  the `gui` script is launched. Updater first compares today's date against the `last-update-check` file. If it's time to check for updates, `updater` first checks for App updates, then checks for other files/folders that have been modified or created. If anything can be updated, a dialog will open and ask permission to update:  
   ![updates](https://github.com/Botspot/pi-apps/blob/master/icons/screenshots/updates%20available.png?raw=true)
   - `data/` This folder holds all local data that should not be overwritten by updates.
     - `settings/`This stores the current settings saved by the 'Pi-Apps Settings' window. Each file contains one setting. For example, the file `settings/Preferred text editor` contains "geany" by default.
     - `status/` This folder stores all installation information for all apps.
     If you install Zoom, then the `status/Zoom` file will be created, containing "installed". Installed apps will have this status icon in the app list: ![installed](https://github.com/Botspot/pi-apps/blob/master/icons/installed.png?raw=true)  
     If installation was unsuccessful, then the file will contain "corrupted". The corresponding icon looks like: ![corrupted](https://github.com/Botspot/pi-apps/blob/master/icons/corrupted.png?raw=true)  
     If the app has been uninstalled successfully, the icon is ![uninstalled](https://github.com/Botspot/pi-apps/blob/master/icons/uninstalled.png?raw=true)  
     If the app has never been installed or uninstalled, then its `status` file will not exist. The icon for that is: ![none](https://github.com/Botspot/pi-apps/blob/master/icons/none.png?raw=true). Notice the slight amount of red in the center. That's how you can tell the difference.  
     - `update-status/` This folder keeps track of which apps can be updated. Each file's name is of an app, so `update-status/Zoom` stores the update status of the Zoom app. This folder is refreshed whenever `~/pi-apps/manage check-all` is run.
     "latest" means that app is up to date.
     "new" means that app is new from the repository. (in other words, it does not exist locally)
     "local" means that app does not exist on the repository.
     "updatable" means the repository's version and the local version don't match.
     - `preload/` This directory is used by the `preload` script to improve Pi-Apps' launch time.
       - `timestamps` This file stores timestamps for the most recently modified app, the most recently modified setting, and the most rencently modified status file.
       If any of these entries don't match when `preload` is called, then the app list will be regenerated.
       - `LIST` This file holds the app list. The entire file's contents is piped into the YAD dialog box.
     - `installed-packages/` This keeps track of any/all APT packages each app installed. This folder is written to from the `pkg-install` script.
     For example, if Pi Power Tools installs `xserver-xephyr` and `expect`, then the `installed-packages/Pi Power Tools` file will contain "xserver-xephyr expect".
     - `hidelist` This file contains app names that should be hidden from the app list. `template` should always be there. If your Pi runs TwisterOS, then `hidelist` will contain several more app names, like balenaEtcher, for example.
     - `last-update-check` This contains a date in numeric form. (Jan. 1 would be `1`, Dec. 31 would be `365`.) The `updater` script uses this file to keep track of when updates were last checked.
   - `etc/` This folder is basically an extension of the main `pi-apps/` folder. Its contents don't need to clutter up the main directory, but they can't go in `data/` because these files should be kept up-to-date.
     - `setting-params/` This stores the GUI entries for the Settings window. For example, if I wanted to add a new setting called "Auto donate" with 'Yes' and 'No' parameters, I'd create a new file called `setting-params/Auto donate` and it would contain this:
     ```
     #Donate automatically to Botspot every time Pi-Apps is launched
     Yes
     No
     ```
     Now, the next time Settings is opened, you will see:  
     ![auto-donate](https://i.ibb.co/nzBNgFT/auto-donate.png)
     What's the point? Basically, it allows for a more elegant way to add new settings. With this approach, it's a lot harder to screw up than with manually editing a bash script.
     - `git_url` This simple file stores this link: https://github.com/Botspot/pi-apps
     If you fork this repository and make changes, you will want Pi-Apps checking for updates from your repository, not this main one. Simply change the URL in this file to switch to your repository.
   - `icons/` This stores all the icons that are embedded into various dialogs.
     - `screenshots/` Stores screenshots of various dialogs, mainly used as an image hosting service, though I suppose they could come in handy if an offline help dialog was made.
   - `update/` This folder holds the latest version of the entire Pi-Apps repository. It's contents is re-downloaded every time you check for updates. It is used to compare file hashes, detect when an app or file can be updated, and is used to copy new file versions into the main `pi-apps/` directory during an update.

### Q&A with Botspot
 - Why did you develop Pi-Apps?  
> For a long time I have been saddened by how few people are aware of open-source RPi software projects. Many of these projects are extremely useful and beneficial, but there has never been a good way to distribute them.  
> The repositories don't host them, and they usually aren't advertised very well, so how will people find them?  
> Most people never find them.  
> One day I realized: Why not make my own app store that specializes in all the community RPi software projects out there? It will help more users find the software, and at the same time it would provide a super simple way to install them.  
> (Which would you rather do - click a shiny Install button, or manually type 11 commands?)
 - How long did it take to program this?  
> About two weeks of nearly non-stop coding. It was fun, but excruciating at the same time.
