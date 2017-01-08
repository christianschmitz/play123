mplay
=====

Synopsis
--------
Wrapper around the **mpg123** mp3-playing utility, which makes it easier to couple it to custom key-bindings. Handles file loading, pause/play, seeking and volume. The filesystem should then act as the music library manager. Has a simple queu internally allowing quick repeat of previously played songs.

Usage
-----
**mplay** [FILE | '<' | '>' | '[' | ']' | '-' | '+' | -h | r | p | n]
- '<' : jump -1min
- '>' : jump +1min
- '[' : jump -5min
- ']' : jump +5min
- '-' : vol  -10%
- '+' : vol  +10%
- -h  : display help message
- 'r' : (re)play current queu file
- 'p' : play prev queu file
- 'n' : play next queu file
- called without commands **mplay** pauses/plays the current file

Details
-------
**mpg123** is run in remote mode in the background. **mplay** sends commands to **mpg123** via a \*nix fifo pipe. If no instance of **mpg123** is running then one is started, with its stdin redirected from the fifo pipe. Beware that multiple instances of **mpg123** might conflict with eachother. The hidden folder ~/.mplay contains the volume state, the fifo pipe and the queu file. This hidden folder is created if it doesn't exist.

Every file explicitly played by calling **mplay FILE** is appended to the queu, and then the mark is moved to that file.

Integration with ratpoison
--------------------------
The included 'ratpoisonrc' file includes some example bindings to **mplay**. Using **mplay** like this turns it into a very simple music player that runs completely in the background.

Dependencies
------------
- **mplay** is a **bash**-script
- **mpg123** v1.16.0, but it might work with other versions
- a \*nix OS obviously (and I assume the way I set up the pipe is universal to \*nix-based systems)

How to create playlists
-----------------------
Use mp3wrap 
m3u files are not properly supported
