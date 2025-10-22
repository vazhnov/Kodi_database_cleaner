Video.database.cleaner documentation
====================================

.. contents::

About
-----

Kodi's video database accumulates un-needed and un-wanted file links over time.

This add-on was written to help users to clean all the rubbish out and make the
built in 'clean library' more efficient as well as much faster.

Settings
--------

General settings
++++++++++++++++

A) Keep any pvr information

When enabled the add-on will not delete any references to files recorded with a
live-tv backend.

B) Keep bookmarked files

When enabled, any files that have bookmarks will not be deleted, even if they are
in a path that would have been deleted otherwise.

C) Automatically trigger clean library

When enabled, the add-on will call Kodi's built in 'clean library' routine after it
has finished it's own cleaning.  This cleans the other tables in the database that the
add-on doesn't touch.  It is recommended that this setting is left enabled at all times.

-Run clean library 2-6 times
* Kodi will sometimes require multiple passes to clean all the paths that it can potentially clean
(This is because of "cascading" effects of removing invalid files, invalid paths and invalid
parent paths).
When enabled, this option will run Clean Library a number of additional times, from 2-6, until there are
the same number of paths and files left at the end.

D) Back-up local database before cleaning

When enabled, the database will be backed up to a sub-directory in Kodi's Database directory.
The back-up is time and date stamped in case a user wishes/needs to revert to an older database.
Optionally, it is also possible to specify a name for the back-up. NOTE - It is not possible
for the add-on to back up a MySQL shared database. Users with MySQL databases should use their
tool of choice to manually back-up their database. phpMyAdmin is one such tool used by the authors.

E) Show summary window

When enabled, a pop-up window will list various settings and paths, depending upon the options
chosen in the add-on settings. It is possible to abort the add-on run at this point which will
leave the database un-touched, or to select the 'Do It !!' button which will run the cleaning
routine with the chosen settings. If this setting is disabled, the add-on will run without user
intervention although any logs will still be created.

Debug and log settings
++++++++++++++++++++++

A) Log paths to database-cleaner.log

If enabled the add-on will create a logfile in Kodi's 'temp' directory that records all the
paths that the add-on will delete. If 'show summary window' is enabled it is possible to stop
the add-on by pressing the 'abort' button. The logfile will still be created so that users can
see what will be deleted before actually doing so. When the add-on cleans the video database, any
existing log will be backed-up to ``database-cleaner.old.log``.

B) Log creation style

Two options are available - create new log each run or add to log each run. If the first option
is chosen, any existing log will be deleted and replaced with a fresh log. If the second option
is chosen, the logfile will be added to on each run. Logfiles are still backed-up on cleaning,
regardless of this setting.

C) Enable debugging to Kodi debug log

In normal use, the add-on writes very little to the Kodi logfile. In the event of an error or an
issue with using this add-on, users should first enable the system-wide Kodi debug log and enable
this setting to produce a debug log that the authors can use to determine the cause of the error or issue.

Sources
+++++++

A) Use ``sources.xml`` to determine files to keep

When a user scans a video source into Kodi, the path is stored in a ``sources.xml`` file. Enabling
this setting tells the add-on that any files that have been added into the video library by
this method should not be deleted. NOTE - Kodi should delete any files when a user removes a source
but if this has not happened, the add-on will remove them automatically.

B) Use ``sources.xml`` on this machine

Generally, this setting should be enabled. Users of MySQL databases do not need to have a ``sources.xml`` on each
machine that their database is shared with, because the paths are contained within the shared database.
Such users can disable this setting and can then set the path to a ``sources.xml`` on a different networked
machine. This means MySQL users can run the add-on on any of their machines whilst only having a
``sources.xml`` on one machine.

Advanced
++++++++

A) Force database name

If enabled, makes the add-on connect to the specified database rather than the automatically detected one.
Useful for testing purposes to see what effect the add-on will have on a database.  NOTE - This setting
does not alter the database that Kodi's built in 'clean library' cleans.  This setting should be
disabled unless the user understands the implications of enabling it.

B) Remove specific path from database

If enabled the user must enter a path to be deleted from the database.  NOTE - A wildcard will be
appended to the end of the supplied path.  The add-on will remove the path from the files table
in the database. This setting will revert to disabled once the add-on has removed the path from
the database.  It is however possible, by using the summary window, to generate a logfile showing
the details of paths to be removed by aborting the add-on at this point and viewing the log file
that it has created.  The setting will not revert until either the summary window is disabled OR
the 'do it !!' button is pressed.

C) Replace path in database

If enabled, the user must specify an existing path and a new path. Any paths containing the
specified existing path will be renamed to the new path. This is useful if you move a
directory on disk but do not want to remove and re-scan the source in Kodi. The add-on
generates the list of paths by appending a wildcard to the supplied 'old path'. This means that
a supplied path of ``nfs://OLD_SERVER`` will include ``nfs://OLD_SERVER/Movies`` and any other
directories in the tree. If the new path were ``nfs://NEW_SERVER`` then every path starting with
``nfs://OLD_SERVER`` will become ``nfs://NEW_SERVER`` e.g. with the ``Movies`` directory above, the path
would become ``nfs://NEW_SERVER/Movies``. Renaming a path automatically turns off once the path
has been renamed. Again, it is possible by aborting the add-on in the summary window to generate a
logfile showing the original path and the new path to aid users in determining if everything is correct.
Clicking on the 'Do it !' button or turning off the summary window will rename the path(s) and turn this setting off.

NOTE - Paths should NOT end with a terminating slash (either ``\`` or ``/`` depending on your OS) as this will lead to
double slashes being inserted into the database.

texturecache.py
+++++++++++++++

Inclusion of ``texturecache.py`` (by Milhouse):

Additional settings in the Video Database Cleaner addon to allow user to run ``texturecache.py``
after the Video Database Cleaner has completed cleaning the MyMovies.db.

Additional settings allow the user to:

a. Enable or Disable the running of ``texturecache.py`` at the end of the Video Database cleanup
b. Configure any of the settings available in the ``texturecache.cfg`` file.
If ``texturecache.py`` is Enabled, run it, multiple times, each time with one of the following flags:

[c, C] Automatically cache missing artwork (c), or with option C force the re-caching of existing artwork by removing first then downloading. Can use multiple threads (default is 2)
[lc] Same as c and nc, but only considers those media (movies, tvshows/episodes) added since the modification timestamp of the file identified by the property lastrunfile
[p, P] Prune texture cache by identifying (p) or removing (P) accumulated cruft such as image previews, previously deleted movies/tv shows/music whose artwork remains in the texture cache even after cleaning the database. Essentially, remove any cached file that is no longer associated with an entry in the media library and is therefore just wasting disk space
[Xd] Remove those rows from the texture cache database that do not have corresponding files in the Thumbnails folder (ie. remove same rows identified by X)
[r, R] Reverse query cache, identifying "orphaned" files that are no longer referenced in the texture cache database. Use R option to auto-delete these files.
[qa] Perform QA check on media library recently added items, identifying missing properties (eg. plot, mpaa certificate, artwork etc.). Default QA period is previous 30 days. Add property qa.file = yes to verify file exists during QA. Add additional QA fields using qa.art.*, qa.blank.* and qa.zero.* properties.
[qax] Like the qa option, but performs a remove and then rescan of any media found to fail the QA tests
[duplicates] List movies that appear more than once in the media library with the same imdb number

Pipe ``texturecache.py`` output to the ``database.cleaner`` logfile will do just that. WARNING: This will create a large file!


Deep Clean
++++++++++

WARNING - CAN BE VERY SLOW! (it will probe every physical file/directory as well as all that are listed in the database).

This option manually scans the locations listed in ``sources.xml``, and builds a list of current, legitimate directories and files.
It doesn't know or care if directories are removable media/network storage/mount points and ignores the 'keep bookmarks' function and doesn't work on the 'remove specific path' operations (only on the general cleaning operations).

Once it has the list of existing, legitimate media files, it will compare this to the database and remove any entries that do not correspond.

* This is helpful when there have been a lot of manually deleted files/folders that the built in Kodi Video cleaner dos not clean up.
It will help to remove many of the "directory does not exist" errors in the ``kodi.log`` file

Settings:

- Files and Paths only in one directory - this can help to focus the Deep Clean on a specific directory, instead of all media.


USAGE
=====

To quickly remove any streaming links from your database, turn OFF 'Use sources.xml to determine files to keep'
in the add-on's 'sources' settings.  This will remove only streaming links from your database when the add-on is
run.  No other cleaning will be done however.

To run the add-on silently as a scheduled task, turn OFF 'Show summary window' in general settings.

To remove old links, streaming info and general rubbish accumulated in the database, ensure that
'use sources.xml to determine files to keep' is turned ON.  If you have a PVR backend, turn on
'keep any pvr information' otherwise all information relating to your recordings will be deleted.

ADVANCED USAGE
==============

The add-on can exclude user defined paths (including plugins) from the clean.  This is done by
creating a file called ``excludes.xml`` in the add-ons ``addon_data`` directory.  This is located
in Kodi's ``userdata`` directory. Inside the addon_data directory, users should find a directory
called ``script.database.cleaner``.  NOTE - if you have not changed any settings, this directory
may not exist by default.  Changing one setting should ensure this directory exists. This file
however does not exist by default, it must be created by the user.

The format of the ``excludes.xml`` file is simple. Example:

 .. code-block:: xml
  <excludes>
    <exclude>plugin://plugin.video.youtube</exclude>
    <exclude>plugin://plugin.video.itunes_trailers</exclude>
  </excludes>

The ``excludes.xml`` file must start with the ``<excludes>`` tag and end with ``</excludes>``.
Any amount of ``<exclude></exclude>`` tags can be included.
A wildcard is appended to the contents of any <exclude> tag, so:

- ``<exclude>plugin://</exclude>`` would exclude ALL plugins
- ``<exclude>plugin://plugin.video</exclude>`` excludes all video plugins
- etc.

Users can be as precise or as vague as they require. Fox example:

- ``<exclude>plugin://plugin.video.itunes_trailers</exclude>`` keeps all the links that
the ``itunes_trailers`` plugin has played. Useful if you want to maintain watched status
for your trailers.
- ``<exclude>plugin://plugin.video.itunes_trailers/trailer/play/http%3A%2F%2Fmovietrailers.apple.com%2Fmovies%2Fdisney%2Fzootopia%2Fzootopia-tlr1_h720p.mov</exclude>``
keeps just that one trailer and deletes all the others.

NOTE - It is possible to specify any  valid path or part of a path in the ``<exclude>`` tag
This allows very fine control over what is removed or kept.

