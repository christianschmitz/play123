mplay
=====

Synopsis
--------
Wrapper around *mpg123*, running it in the background. Commands are then sent to *mpg123* via a \*Nix-*pipe*. If no instance of *mpg123* is running then one is started. Multiple instances of *mpg123* might conflict with eachother

Usage
-----
**mplay** [FILE | '<' | '>' | '[' | ']' | '-' | '+' | -h]
With:
- '<' : jump -1min
- '>' : jump +1min
- '[' : jump -5min
- ']' : jump +5min
- '-' : vol  -10%
- '+' : vol  +10%
- -h  : display help message
- called without commands **mplay** pauses/plays the current file

Integration with ratpoison
--------------------------
The included 'ratpoisonrc' file includes some bindings to **mplay**. This keeps this very simple music player completely in the background.

Dependencies
------------
- **mplay** is a *bash*-script
- *mpg123 v1.16.0*, but it might work with other versions
- a \*Nix OS obviously (and I assume the way I set up the pipe is universal to \*Nix-based systems)
