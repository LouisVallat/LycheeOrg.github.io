<style>
    .docs_main h1+ul ul li {
        display: inline-block;
        width: 7em;
    }
</style>

## Master branch

- `fixes` #565 : No Dropbox Import with Lychee 4.0.0
  > The CSP was a bit too tight, preventing the execution of the script from dropbox.

## Version 4

### v4.0.5

Released May 15, 2020

- `new` : Add RSS module.
  > we provide a RSS feed, available at the `/rss` address.
  > it will contains the last `rss_max_items` (default: 100) over the last `rss_recent_days` (default: 7) days.
- `fixes` #557 : report files not imported by sync
  > When using the command `lychee:sync` if a file is not imported for some reason, the Log will contains more detailed information instead of just an error message.
- `fixes` #550 : Version 4.0.4 database is not updated
  > Simply forgot to add the bump version.
- `fixes` #460 : Wrong rotation of photos when imported from server via symlink.
  > thumbnails are now also rotated in the proper direction.

### v4.0.4

Released May 11, 2020

- `new` : More idiot proof safeties (and avoid some 404 errors complaints).
  > - We add a check in the public directory that composer has indeed been run.
  > - We add a check that if apache is being used, the rewrite module is enabled (avoid silly 404 error when redirected to `/install`).
- `new` : Better support for SQLite
  > We create a file in `database/database.sqlite` to avoid issues in the case where some user decides to use this type of database without giving a path. 
- `fixes` #539 : Invalid exif geolocation causes db error
  >  We now check that the latitude is between -90 and 90 degrees, we also check that the longitude is between -180 and 180 degrees. Any other values are discarded.
- `fixes` #533 : Add LYCHEE_UPLOADS_URL to secure-headers.php
  > This allows the use allow images to be loaded from another server (CDN).
- `fixes` #529 : Footer linkable.
  > With the update to Laravel v7, the footer was not html html tags anymore. We fix this. This allows the use of links in the footer, for e.g. legal stuff in Germany.
- `fixes` #525 : The config from the v3 was not migrated
  > We now also migrates most of the configuration from the v3 if the `lychee_settings` table if found.


### v4.0.3

Released April 29, 2020

- `fixes` #498
  > remove Lychee-front version number alltogether
  > add Diagnostic information:
    - Composer install type
    - release type (git vs release)
    - add button to allow migration for users using the release channel instead of master.
- `fixes` #508
  > Diagnostic was checking the existence of mysqli only if postgresql is not used. With the added support of SQLite we now thoroughly check each possibilities.
- `fixes` #510
  > add SQLite3 to the travis build check.

### v4.0.2

Released April 22, 2020

- `fixes` #488
  > Error in the migration which made the script some files were existing.
- `fixes` #485
  > Align lychee-front version number, remove error when not provided.
- `fixes` #487
  > add missing files (css) to the installer.

### v4.0.1

Released April 19, 2020

- `fixes` #481 
  > Decrease the size of the released archive by 82%.
- `fixes` missing download button when album is downloadable, does not have pictures but subalbums

### v4.0.0

Released April 18, 2020

[Upgrade from version 3.x &#187;](https://github.com/LycheeOrg/Lychee/wiki/Upgrade-from-version-3)

#### New

- `new` : uses the new Laravel backend
  > Better security for access control & more flexibility.
- `new` : broader database support
  > With Laravel we are now Database agnostic; you can choose between MySQL, SQLite and PostgreSQL.
- `new` : introduce the sub-albums
  > You can now create albums within albums.
- `new` : introduce multi user system (in addition to admin)
  > The admin can arbitrarily change passwords, lock an account (user cannot change his password), give rights to the user to upload pictures and create albums.
- `new` : sharing between users
  > Users can share albums with each other. *Interface still needs improvements*. Note that you are only give READ access, not write access.
- `new` : Frame module
  > By enabling the Frame module in the advanced settings, a user can display his *starred* pictures as a slideshow.
- `new` : Landing page module
  > Allows the user to have a landing page instead of directly arriving at the gallery if enabled.
- `new` : Image symbolic module
  > To prevent full pictures from being directly available, a module is available to make the image link hard to guess. *This will induce a slow down when the image is generated*
- `new` : use XCRF cookies
  > All requests to the server require an encrypted cookie in order to prevent cross-site request forgeries.
- `new` : *"one click update"*
  > Admins can update their installation in one click in the Diagnostic interface if their installation has been done with `git`.
  > This also support composer updates but this is more risqu&eacute;.
- `new` : Full HiDPI support
  > Optional HiDPI support for preview images can be enabled in the advanced settings.
- `new` : Improved sharing and downloading
  > Sharing and Visibility were split into two menus to make them easier to use; one can now download an image in any of the available sizes; multiple photos or albums can be downloaded in one go.
- `new` : Improved Smart albums
  > _Recent_ and _Starred_ can now be enabled in public mode using advanced settings; the age qualifying for _Recent_ can be adjusted.
- `new` : Continuous Integration and Code Coverage
  > While this is not visible to the users, we now test our builds before placing them in the **master** branch. This increases the stability of our builds. Our test suite currently covers 50% of our code.
- `new` : Image wraparound configurable
  > Wraparound from the last to first image when selecting _Next_ in photo view can now be disabled in the advanced settings.
- `new` : Support for `cn`, `cz`, `nl`, `en` (default), `fr`, `de`, `el`, `it`, `ru`, `sk`, `es`, `sv`
- `new` : Support for GPS coordinates
  > Decode the GPS data from the picture
- `new` : Display on map with OpenStreetMap
  > optionally display where on the map the picture has been taken  
  > optionally add a global map to see where all your pictures have been taken.
- `new` : Live photos
  > Add support to live photos, also extract the video from the photo.
- `new` : Support 32 bits version
  > Add support to 32 bits version of PHP (even though we don't like it).
- `new` : Provide command line access from server side
  > This should replace the use of `lycheeupload`, `lycheesync`, `lychee-create-medium`
  > available commands are:
  >  ```
  >  lychee:exif_lens            Get EXIF data from pictures if missing                                          
  >  lychee:reset_admin          Reset Login and Password of the admin user.                                     
  >  lychee:logs                 Print the logs table.                                                           
  >  lychee:diagnostics          Show the diagnostics informations.                                              
  >  lychee:decode_GPS_locations Decodes the GPS location data and adds street, city, country, etc. to the tags  
  >  lychee:generate_thumbs      Generate intermediate thumbs if missing                                         
  >  lychee:video_data           Generate video thumbnails and metadata if missing                               
  >  lychee:sync                 Sync a directory to lychee                                                      
  >  lychee:npm                  Launch npm on the public/src folder                                             
  >  lychee:takedate             Make sure takedate is correct.                                                  
  >  ```
  > use `php artisan lychee:<command> -h` for more informations about the command.
  > For example: `php artisan lychee:logs` will display the last 100 logs (requires the database).

#### Fixes

- `fixes` : Improved _Import from Server_
  > User can now select whether to delete originals right in the dialog box; status updates are provided throughout the input process; improvements were made to prevent PHP and HTTP timeouts.
- `fixes` : Improved support for video files
  > Next/prev buttons no longer cover the video player; extracted preview image now retains the video aspect ratio; basic video metadata is extracted and displayed in the Info sidebar.
- `fixes` : Multiselect improvements
  > Shift-click is now supported for selecting ranges; clicking on the background clears selection; Ctrl-click for unselecting has been fixed.
- `fixes`: Sidebar improvements
  > The Info sidebar no longer overlaps content; the displayed text can be selected and copied.
- `fixes`: Simplified password protection of albums
  > Password-protected albums stay unlocked for the duration of the viewing session.

---

## Version 3

### v3.2.16

Released June 17, 2019

- `new` : hides lychee version number by default (https://github.com/LycheeOrg/Lychee-Laravel/issues/282)

### v3.2.15

Released June 16, 2019

- `update` : improve stability when getting bad EXIF data ( https://github.com/LycheeOrg/Lychee-Laravel/pull/205 )
- ` fixes` : ignore bad shutter data (#240)
- `update` : switch the git commit format number to only 7 characters in Diagnostics
- `fixes` : takedate format string (#215, #256) 
- `new` : add setting to allow public search (#262)
- `fixes` : wrong version number displayed in Lychee (#268)
- `update` : credits.

### v3.2.14

Released March 28, 2019

- `Updates` Add primary key to settings table (#221)
- `Updates` Add git source commit/branch/repo to Diagnostics page
- `Updates` Use better lens tags from exiftool if present (if exiftool is enabled) (#235)
- `Updates` Accept larger input from exiftool
- `Updates` Make photo/album IDs more consistent
- `New` Add fullscreen support to album and photo views (#228)
- `Updates` Use sortingAlbums and sortingPhotos even if logged out.
- `Updates` Hide passwords. Add password confirmation.

- `Fixes` #220, #222, #234, 'F' and 'f' hotkey behaviour and some spelling mistakes (#229)

### v3.2.13

Released February 20, 2019

- `New` Add "unjustified" layout
- `Updates` Improve Diagnostics page
- `Fixes` #194, #196, #205, #208

### v3.2.12

Released February 12, 2019

- `New` Add usage of exiftool to get exif tags from camera #189 
Using exiftool for getting exif tags make available a lot more tags than the built-in functionality in PHP. Using exiftool will make eg. lens info available without having to rely on that users have exported a raw from Ligthroom.
**This setting needs to be enabled via the `more` menu as it makes system calls.**
- `Fixes` image size missing from about #188

### v3.2.11

Released February 3, 2019

- `New` Add description overlay and takestamps overlay in addition to Exif. Closes #167
- `New` Add Setting to remove script execution time limit during imports. Fixes #177
  This setting can only be activated via the `More` setting and at user owns risks.

### v3.2.10

Released January 19, 2019

- `New` Switch to InnoDB engine. Closes #169
- `New` Add setting to decide whether to delete photos from source when imported. Fixes #173
- `New` Use existing albums (if available) when importing from server
- Remove '[Import] ' prefix for albums created by import

### v3.2.9

Released January 9, 2019

Nothing major here. Just a bunch of small bug fixes

**WARNING**: Lychee now requires PHP 7.1 ( http://php.net/supported-versions.php )

- `Fixes`  Cross-Origin Request Blocked: https://lycheeorg.github.io/update.json ( #121 )
 The server is now doing the check for update (on `Session::init`) if this one fail, the user will do an ajax request to check if an update is available.
- `Fixes` Syntax Error in Session.php ( #153 )
- `Fixes` Small bugs ( #136 ,  #157 , #159 ,  #163 , #166 )
- `Fixes` lychee uploading pics into uploads/thumb folder only  ( #148 , #165 )
- `Updates` German translations ( #161 )

### v3.2.8

Released December 26, 2018

- `Fixes` Site broken (#157)

- `New` Admins can now access all settings via `Settings -> more` at the bottom of the page.
**WARNING**: it is now easier to break your installation.
- `New` Admins can now create a specific `user.css` file that will be loaded in addition to the `main.css` one. This css file can be modified at the bottom of the `Settings` screen.
- `New` Admins can now define the default size for their medium and small images (via the advanced settings).

- `Fixes` Turn off zoom-in animation when switching photos (#154)
- `Fixes` Setting // Image Size Definition // Small, Medium, large // With Default values (#152)
- `Fixes` "Display EXIF data overlay" can toggled with click on image (#151)
- `Fixes` "play-icon.png" should be in "lychee-front/images" not root (#150) It is now placed in `dist`
- `Fixes` Shutter speed for long time exposures is displayed as fraction (#149)
- `Fixes` [Wish] Custom Size Image Creation (#141)
- `Fixes` New album doesn't show after create unless you refresh page (#135)
- `Fixes` Unable to edit settings table in database (#80)
- `Fixes` Hover-Over Blue Border/Square Highlights (#51)

### v3.2.7

Released December 11, 2018

- `New` Album-level licenses
(**WARNING**: All photo-level licenses will be reset when this update is applied.)
- `New` Added script to generate "small" size files for existing images
- `Improved` Update link on login dialog directs to the [Releases](https://github.com/LycheeOrg/Lychee/releases) page rather than the Readme.
- `Fixes` Center align play icon in video thumbnail ([#133](https://github.com/LycheeOrg/Lychee/issues/133))
- `Fixes` Missing "small" folder in 3.2.6 release ([#146](https://github.com/LycheeOrg/Lychee/issues/146))
- `Fixes` Other minor bugfixes

### v3.2.6

Released November 30, 2018

- `New` Default Creative Commons license field in Settings. Applies to new uploads only.
- `Fixes` Misspelling of 'Starred' Smart Album (English)
- `Fixes` Albums not showing when 'Move' was selected on a single photo ([#129](https://github.com/LycheeOrg/Lychee/issues/129))
- `Fixes` Previously set license saved in License field ([#120](https://github.com/LycheeOrg/Lychee/issues/120))

### v3.2.5

Released November 26, 2018

- `New` Creative Commons licenses available as photo metadata ([#71](https://github.com/LycheeOrg/Lychee/issues/71))
- `New` "Copy to..." option is now available

### v3.2.{3,4}

Released November 22, 2018

- `Fixes` [#112](https://github.com/LycheeOrg/Lychee/issues/112), [#111](https://github.com/LycheeOrg/Lychee/issues/111) (quick fix), [#110](https://github.com/LycheeOrg/Lychee/issues/110), [#109](https://github.com/LycheeOrg/Lychee/issues/109), [#105](https://github.com/LycheeOrg/Lychee/issues/105), LycheeOrg/Lychee-front [#16](https://github.com/LycheeOrg/Lychee/issues/16).
- `New` small pictures (for the justified-layout)
- `New` Lens information
- `New` Displaying EXIF data as an overlay in the image view.

### v3.2.2

Released November 21, 2018

- `New` German translations (#104)
- `New` support for justified-layout (#95)

Justified Layout is available as an option in the settings. It will only works with Imagick and medium.
Lychee-front will require a npm install (only for devs).

### v3.2.1

Released November 20,2018

- `Fixed` small bugs
- `Fixed` SQL updated not applied
- `New` Swedish support (#101)
- `New` multi selection with CTRL (#36)
- `New` Content Security Policy via .htaccess  (#91, #92)

### v3.2.0

Released November 12, 2018

* `Fixes` Picture ordering bug.
* `New` Panel for settings.
* `New` Allow video upload. (#4)
* `New` localization (so far in English, French, Dutch and Simplified Chinese) (#48, #53, #54, #55, #87, #94)

[OPTIONAL] In order to have Thumbnail for video you will need to use [composer](https://getcomposer.org/):
```
cd Lychee
composer update
```

### v3.1.6

Released March 20, 2017

- `Fixed` Downloading a SmartAlbum results in crash (#652)
- `Fixed` htaccess IfModule for PHP7 (#653)

### v3.1.5

Released October 25, 2016

- `New` Hide mouse pointer in full screen mode (#620)
- `Improved` Smoothing rotation of album (#626)

### v3.1.4

Released August 28, 2016

- `Fixed` Search stopped working because of an undefined index error (#605)
- `Fixed` Better next/previous photo check to prevent an error when opening an album with only one photo

### v3.1.3

Released August 22, 2016

- `Improved` rotate and flip images with GD based on EXIF orientation (Thanks @qligier, #600)
- `Improved` enter/leave fullscreen-mode by (not) moving the mouse for one second (Thanks @hrniels, #583)
- `Improved` Prefetch the medium photo instead of the big one (Thanks @Bramas, #446)
- `Improved` Added "session" to required extensions (#579)
- `Improved` Added warning if Imagick is not installed/enabled (Thanks @hrniels, #590)
- `Fixed` Don't assume that gd_info exists when running diagnostics (Thanks @hrniels, #589 #565)
- `Fixed` Sidebar showing up in smart albums when navigating back from the photo-view

### v3.1.2

Released June 12, 2016

- `Improved` Added indexes to SQL fields to improve query execution time (Thanks @qligier, #533)
- `Improved` Protocol-relative URLs for open graph metadata (#546)
- `Improved` Remove metadata from medium-sized images and thumbnails (Imagick only) (#556)
- `Improved` Reduce quality of medium-sized images (Imagick only) (#556)
- `Improved` orientation-handling with Imagick (#556)

### v3.1.1

Released April 30, 2016

- `New` share button when logged out (#473)
- `New` Import of IPTC photo tags (Thanks @qligier, #514)
- `New` Added reset username and password to FAQ (#500 #128)
- `Improved` Removed will-change from the main image to improve the image rendering in Chrome (#501)
- `Improved` scroll and rendering performance by removing will-change
- `Improved` Open Facebook and Twitter sharing sheet in new window
- `Improved` EXIF and IPTC extraction (Thanks @qligier, #518)
- `Fixed` broken URL in Update.md (#516)
- `Fixed` error 500 on database connect error (Thanks @tribut, #530)

### v3.1.0

Released March 29, 2016

**Warning**: It's no longer possible to update from Lychee versions older than 2.7.

**Warning**: Plugins which use the plugin API of Lychee must be updated to work with the new back-end.

**Notice**: It's no longer possible to edit the thumb quality in the database.

**Notice**: It's no longer possible to disable the creation of medium-sized photos when Imagick is installed on the system.

This updates includes a huge rewrite of the back-end. We are now using namespaces and the singleton pattern for Settings::get(), Database::get() and Plugins::get(). Everything is way better documented thanks to PHPDoc comments. Ugly `#` comments have been replaced with the more known `//`. Unused functions are gone and returns are more strict. We also added a handy module to output messages. Failed database updates and invalid queries will be saved to the log.

- `New` Empty titles for albums
- `New` Share albums as hidden so they are only viewable with a direct link (#27)
- `New` Log failed and successful login attempts (Thanks @qligier, #382 #246)
- `Improved` error messages and log output
- `Improved` The search shows albums above photos (#434)
- `Improved` Album id now based on the current microtime (#27)
- `Improved` Back-end modules and plugins
- `Improved` Database connect function and update mechanism
- `Improved` Default photo title now "Untitled"
- `Improved` Move to next photo after after moving a picture (#437)
- `Improved` Return to album overview when canceling album password input
- `Improved` URL import now accepts photo URLs containing "?" and ":" (Thanks @qligier, #482)
- `Improved` Replaced date by strftime to simplify date translations (Thanks @qligier, #461)
- `Fixed` Missing icons in Safari 9.1
- `Fixed` duplicate uploads (Thanks @qligier, #433)
- `Fixed` incorrect escaping when using backslashes
- `Fixed` session_start() after sending headers (#433)
- `Fixed` error when deleting last open photo in album
- `Fixed` Photo sometimes not loading when visiting directly
- `Fixed` Move album, merge album and switch album/photo menus no longer show empty titles for untitled albums/photos

### v3.0.9

Released January 10, 2016

- `Improved` Disabled dragging for thumbnails
- `Improved` Avoided unnecessary devicePixelRatio checks by using srcset for all thumbnails
- `Improved` Avoided devicePixelRatio check by using srcset for the imageview image
- `Improved` Don't show log and system information when logged out (Thanks @Bramas, #421)
- `Fixed` Swipe-gestures on mobile devices

### v3.0.8

Released December 20, 2015

- `Improved` Lychee update site now with SSL (#317)
- `Improved` Set undefined vars, remove unused vars and code that cannot be reached (Thanks @mattsches, #435)

### v3.0.7

Released November 15, 2015

- Internal changes and updated dependencies
- `New` PHP-version-check now requires PHP >= 5.5
- `New` Preloading of big photos (#185)

### v3.0.6

Released September 13, 2015

- `Improved` Share photo now shares view.php link (#392)
- `Fixed` Incorrect error messages for failed uploads (#393)
- `Fixed` XSS issues and escaping problems
- `Fixed` Broken "Download album" when album has an ampersand in the password (#356)

### v3.0.5

Released August 9, 2015

- `Fixed` view.php not displaying photos

### v3.0.4

Released July 17, 2015

- `Improved` Removed bower and updated basicModal & basicContext
- `Improved` Small interface performance improvements
- `Improved` Updated all JS-files to take advantage of ES2015
- `Improved` Better error-handling for the Dropbox-, URL- and Server-Import
- `Improved` Added skipDuplicates- and identifier-check to the diagnostics
- `Fixed` error when using "Merge All" with one selected album
- `Fixed` error when saving username and password after the initial setup
- `Fixed` Clicks not recognized when using a mouse on a touchscreen-device (#345)

### v3.0.3

Released June 28, 2015

- `New` Skip duplicates on upload (#367, [How to activate](settings.md))

### v3.0.2

Released June 13, 2015

- `Improved` Permission errors are now easier to understand (#351)
- `Improved` Escape data from database before inserting into `view.php`
- `Fixed` PHP-version-check now requires PHP >= 5.3 like written in the docs

### v3.0.1

Released May 24, 2015

- `New` Album Sorting (Thanks @ophian, #98)
- `New` Identifier to prevent login of multiple Lychee-instances (#344)
- `Improved` Albums and photos now can have a title with up to 50 chars (#332)
- `Fixed` Removing last Tag from photo not possible in Firefox (#269)

### v3.0.0

Released May 6, 2015

**Warning**: You need to enter a new username and password when upgrading from a previous version. Your installation is accessible for everyone till you enter a new login by visiting your Lychee. Both fields are now stored in a secure way. Legacy md5 code has been removed.

**Warning**: Upgrading from a previous version will set *all* public albums to private. Passwords  are now stored in a secure way. Legacy md5 code has been removed.

**Warning**: We recommend to backup your database and photos before upgrading to the newest version.

**Deprecated**: Photos uploaded with Lychee v1.1 or older aren't supported anymore. Thumbnails  fail to load on high-res screens.

- `New` Redesigned interface, icons and symbols
- `New` Rewritten Front-End
- `New` Dialog system now based on [basicModal](https://github.com/electerious/basicModal)
- `New` Context-menus now based on [basicContext](https://github.com/electerious/basicContext)
- `New` Edit the sharing options of a public album
- `New` Quickly switch between albums and photos by clicking the title in the header
- `New` Renamed API functions
- `New` Merge albums (Thanks @rhurling, #340, #341, #166)
- `New` iPhone 6 Homescreen icon
- `Improved` Performance of animations
- `Improved` Prevent download of deleted albums/photos
- `Improved` Opening a private photo when logged out now shows an error
- `Improved` Reduced attribute changes to improve performance
- `Improved` Interact with the content while the sidebar stays open
- `Improved` Username and password now stored in a safer way
- `Improved` Album passwords now stored in a safer way
- `Improved` Don't refresh albums when password-input canceled by user
- `Improved` Additional Open Graph Metadata (#299)
- `Improved` Check allow_url_fopen (#302)
- `Fixed` Prevent ctrl+a from selecting the sidebar (#230)
- `Fixed` Removed unused scrolling bars in FF (#316, #289)

And much more…

---

## Version 2

### v2.7.2

Released April 13, 2015

- `Fixed` Prevented remote code execution of photos imported using "Import from URL" (Thanks Segment S.r.l)
- `Fixed` Stopped view.php from returning data of private photos

### v2.7.1

Released January 26, 2015

- `Improved` auto-login after first installation
- `Fixed` Disabled import of the medium-folder
- `Fixed` error when using apostrophes in text #290
- `Fixed` $medium is now a tinyint like defined in the database structure
- `Fixed` incorrect height calculation for photos
- `Fixed` creation of test db #295
- `Fixed` a warning caused by set_charset #291

### v2.7

Released December 6, 2014

- `New` Intermediate sized images for small screen devices #67
- `New` Added Docker help (@renfredxh, #252)
- `New` Move-Photo context shows album previews
- `Improved` Upload shows server-errors
- `Improved` Improved thumb creation
- `Improved` Docker (@renfredxh, #252)
- `Improved` CSS has been rewritten partly
- `Improved` Front-end has been rewritten partly #245
- `Improved` Folder- and code-structure has been updated
- `Improved` Context-menu now based on [basicContext](https://github.com/electerious/basicContext) #245
- `Fixed` OpenGraph image too big for some sites #69
- `Fixed` Wrong sizes after EXIF rotation
- `Fixed` Returning to 'Albums' after searching failed
- `Fixed` Move-Photo not scrollable #215

### v2.6.3

Released October 10, 2014

- `New` Caching for albums (Thanks @r0x0r, #232)
- `New` Save scroll position of albums (Thanks @r0x0r, #232)
- `New` Added Dockerfile (@renfredxh, #236)
- `Improved` Newest album on the top (Thanks @r0x0r, #232)
- `Fixed` Login in private mode (Safari)
- `Fixed` Drag & Drop with open photo
- `Fixed` Wrong modified date of the photo files
- `Fixed` Search function always returned all photos (Thanks @powentan, #234)

### v2.6.2

Released September 12, 2014

- `New` Select all albums/photos with `cmd+a` or `ctrl+a`
- `New` Detect duplicates and only save one file (#48)
- `New` Duplicate photos (#186)
- `New` Added contributing guide
- `New` Database table prefix for multiple Lychee installations (#196)
- `Improved` Use IPTC Title when Headline not available (#216)
- `Improved` Diagnostics are showing system information
- `Improved` Harden against SQL injection attacks (#38)
- `Fixed` a problem with htmlentities and older PHP versions (#212)

### v2.6.1

Released August 22, 2014

- `New` Support for IE >= 11 (#148)
- `New` Choose if public album is downloadable or not (#191)
- `Improved` Albums gradient overlay is less harsh (#200)

### v2.6

Released August 16, 2014

- `New` Rewritten and redesigned Uploader (#101)
- `New` Custom server-import directory (#187)
- `New` Plugin documentation
- `Improved` Database and installation process (#202 #195)
- `Improved` "No public albums" now easier to read (#205)
- `Fixed` Don't show EXIF info when not available (#194)

### v2.5.6

Released July 25, 2014

- `New` Choose if album should be listed public (#177)
- `New` Gulp instead of Grunt with autoprefixer
- `Improved` Slightly better performance when opening big albums
- `Improved` Checksum with sha1 instead of md5 (#179)
- `Fixed` Missing public badge on public albums
- `Fixed` Wrong path for public photos in view.php
- `Fixed` Wrong link to thumbs when searching
- `Fixed` Wrong date in album view when takestamp was null
- `Fixed` It wasn't possible to rename albums while searching
- `Fixed` It was possible to right-click on SmartAlbums after searching

### v2.5.5

Released July 5, 2014

- `New` Smart Album "Recent"
- `New` Checksum of photo in database (#48)
- `New` Show takedate in photo-overlay (when available)
- `Improved` Permission check when running with the same UID (#174)

### v2.5

Released June 24, 2014

- `New` Swipe gestures on mobile devices
- `New` Plugin-System
- `New` Rewritten Back-End
- `New` Support for ImageMagick (thanks @bb-Ricardo)
- `New` Logging-System
- `New` Blowfish hash instead of MD5 for all new passwords (thanks @bb-Ricardo)
- `New` Compile Lychee using Grunt (with npm and bower)
- `New` Open full photo without making the photo public
- `Improved` Shortcuts
- `Improved` Album share dialog
- `Improved` Database update mechanism
- `Improved` Download photos with correct title (thanks @bb-Ricardo)
- `Improved` EXIF parsing
- `Improved` URL and Server import (thanks @djdallmann)
- `Improved` Check permissions on upload
- `Fixed` Wrong capture date in Infobox
- `Fixed` Sorting by takedate

### v2.1.1

Released March 20, 2014

- `New` Delete albums with cmd + backspace
- `New` Using iOS 7.1 minimal-ui
- `Improved` Faster loading of single photos
- `Improved` Faster and snappier animations
- `Improved` Better dialog when clearing Unsorted
- `Fixed` Warning when uploading images without EXIF-Data
- `Fixed` Close upload on error

### v2.1

Released March 4, 2014

Important: You need to reenter your database credentials and set the correct rights for `data/`, when updating from a previous version.

- `New` Multi-select (#32)
- `New` Multi-folder import from server (#47)
- `New` Tagging (#5)
- `New` Import of original image name (#39)
- `New` Makefile
- `Improved` Upload-process
- `Improved` Documentation
- `Improved` Overlay for photos
- `Fixed` Dropbox import (#84)
- `Fixed` Wrong login or password annotation (#71)
- `Fixed` Escaping issue (#89)
- `Moved` Config now located in `data/`

### v2.0.3

Released February 26, 2014

- Critical security fix
- Notifications for Chrome

### v2.0.2

Released January 30, 2014

- Clear search button (#62)
- Speed improvements (#57)
- Show tooltip when album/photo title too long (#66)
- Fixed php notices
- Avoid empty downloads in empty albums (#56)
- Correct position of upload modal on mobile devices
- Improved security

### v2.0.1

Released January 24, 2014

- Share > Direct Link
- Download individual images (Issue #43)
- ContextMenu stays within the window (Issue #41)
- Prevent default ContextMenu (Issue #45)
- Small ContextMenu improvements
- Small security improvements

### v2.0

Released January 22, 2014

- All new redefined interface
- Faster animations and transitions
- Import from Dropbox
- Import from Server
- Download public albums
- Several sorting options
- Installation assistant
- Infobox and description for albums
- Faster loading and improved performance
- Better file handling and upload
- Album covers are chosen intelligent
- Prettier URLs
- Massive changes under the hood
- IPTC support (Headline and Caption)
- EXIF Orientation support