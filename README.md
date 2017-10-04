# play123

## Synopsis

play123 is a simple mp3 playlist manager. It uses the mpg123 console player as a backend. play123 uses a client-server approach, this makes it is easy to run over ssh.

## History 

I wrote this because I couldnt get mpg321 to do exactly what I wanted (I listen to long podcasts and require seeking/skipping). I first wrote a wrapper for mpg321. I then changed the backend to mpg123 because it has better performance and I realised I didnt need any of the extra mpg321 functionality any more. More recently I rewrote the play123 script to support sending mpg123 commands over ssh, and also to auto-continue when a file is finished playing.

## Usage

```
play123 [-r <user@host:port> ] [-p <port>] [-h] [<play-command> | <file(s)>]
```

Play commands:
* `seek [+-]<seconds>`: jump to `<seconds>` frame in current file, or jump by increment in current file
* `vol [+-]<%-pt>`: set volume, or change volume by increment
* `next`: play next file in queu
* `prev`: play previous file in queu
* `repeat`: play marked file in queu

If any of the arguments are valid files, these are appended to the queue. The first of these files is then set to play.

Options (only relevant when launching for first time):
* `-r <user@host:port>`: connect to a play123 server running on `host`, requires ssh and netcat
* `-p <port>`: start play123 as a server listening on `port`, requires netcat

Without arguments play123 pauses/plays current file.

Only one command is parsed per call. Appending file(s) is treated as one command. If multiple commands are specified only the last is parsed.

Queu location:
```
~/.play123/queu
```

If current file is finished playing the next file is played. If the last file is finished playing the first file is played.

## Details

When play123 is first started 4 processes are forked:
* a process to keep a fifo pipe open as input to mpg123
* mpg123 itself
* a process to read the output of mpg123
* a play123 daemon process that acts as server to client requests

Fifo pipes used:
* `~/.play123/driverpipe`: input pipe for mpg123
* `~/.play123/daemonpipe`: input pipe for daemon process

The process that reads the mpg123 output, as well as any play123 clients, send commands into the `daemonpipe`.

To kill all play123 related processes you can use the killall command (part of psmisc package on debian):
```
killall play123 mpg123
```

## Keybinding example: ratpoison

In ~/.ratpoisonrc:
```
bind F10 exec play123
bind comma exec play123 seek -60
bind period exec play123 seek +60
bind bracketleft exec play123 seek -300
bind bracketright exec play123 seek +300
bind F8 exec play123 vol +10
bind F7 exec play123 vol -10
bind r exec play123 repeat
bind F11 exec play123 next
bind F9 exec play123 prev
```

## Dependencies

* a linux system
* bash
* mpg123 (tested with versions 1.16.0 and 1.23.8)
* netcat and ssh when using network functionality

## Todo

* better support for m3u files
* debugging and testing of network functionality, including automatic file transfer
* smart shuffle that takes into account how many times files have been played before, and weights files based on 'novelty' 

## Copyright

Christian Schmitz

## Contact

christian.schmitz@telenet.be
