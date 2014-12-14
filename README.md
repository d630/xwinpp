### xwinpp v0.1.2.5 [GNU GPLv3]

`xwinpp`(1) is a bash shell script, that takes a list of X window ids (of your EWMH/NetWM compatible X windo manager) and performs some actions on it. Mainly, `xwinpp`(1) was written to process a preparing output with variables on that list. A typical output looks like this:

```
_xwinpp_desk_curr=0
_xwinpp_desk_select=(0)
_xwinpp_win_active=0x380000d
_xwinpp_win_active_geo_x_y=(0,1)
_xwinpp_win_active_geo_w_h=(1400,1049)
_xwinpp_win_active_frame_extents=0,0,0,0
_xwinpp_win_active_tags=null
_xwinpp_win_xid=(0x1a00003 0x2e0000d 0x360000d 0x380000d 0x3a0000d 0x1e00002 0x1606c0d)
_xwinpp_win_number=7
_xwinpp_win_geo_x_y=(0,1 700,210 700,419 0,1 700,628 0,1 0,1)
_xwinpp_win_geo_w_h=(1400,1049 698,198 698,198 1400,1049 698,198 1400,1049 1400,1049)
_xwinpp_win_frame_extents=(0,0,0,0 0,0,0,0 0,0,0,0 0,0,0,0 0,0,0,0 0,0,0,0 0,0,0,0)
_xwinpp_win_tags=(null null null null null null null)
```

#### Install

* Get the newest version of `xwinpp`(1) with `$ git clone https://github.com/D630/xwinpp.git` or
  download its last release on https://github.com/D630/xwinpp/tags
* Make `xwinpp` executable and copy it elsewhere into `<PATH>`

Explicitly required:
- `GNU bash`(1)
- `xprop`(1)
- `xwininfo`(1)

#### Options

```
-B, --hidden
      Select windows, which are hidden. A window is hidden, if
      _NET_WM_STATE_HIDDEN is set.

-b, --visible
      Select  windows, which are visible. A window is visible,
      if _NET_WM_STATE_HIDDEN is not set.

-D, --desk= DESK...
      Select windows, which are on DESK. By  default,  windows
      on current desktop are used.

-I, --input-file= FILE or hyphen (-)
      Obtain  X window ids from FILE. If FILE is a hyphen, get
      output by reading from standard input.

-P, --win-active-pos= POS
      By default, xwinpp(1) processes X window ids in order of
      input.  To  specify the position of active window in the
      output use this option together with -p.

-t, --tags= TTAG...
      Select windows, which are tagged with TTAG.

-V, --win-active= XID
      By default, xwinpp(1) uses  _NET_ACTIVE_WINDOW  to  find
      the  active  window  out. To specify another window, you
      can use this option.
```

#### Subcommands

```
-h, --help
      Display a short help.

-p, --print
      Print a preparing output with variables. You can specify
      the output with above options.

-S, --set STAG
      Set or unset window property _XWINPP_TAGS.

-v, --version
      Print current version of xwinpp(1).
```

#### Arguments

```
<DESK>      'any', 'curr' or relative to the current
            desktop 'next' or 'prev'. Specify a desktop
            number (starts at 0) with the prefix 'i:'; a
            desktop name is prefixed with 's:'.
            Examples: 'i:0'; 's:web'; ''s:some stuff''.
            To select more than one desk, seperate with
            a comma like 'i:0,i:1,s:web'.
<FILE>      File may be a regular file or a named pipe.
<POS>       Position of the active window in the output
            specified by an integer. You may also write
            'start' or 'end'. Position starts with '0'.
<STAG>      You have to set all tags in _XWINPP_TAGS
            property at the same time. To set a string
            with more than one tag, seperate it with a
            comma and prefix it with 'tag:'. Example:
            'tag:gui,viewer,pdf'. To remove use only
            'tag:'.
<TTAG>      Tag, which was specified as _XWINPP_TAGS
            property in an X server. To select more than
            one tag, seperate it with a comma.
            Example: 'gui,pdf'.
<XID>       Numeric window identity.
```

#### Examples

To print a preparing output:

```
wmctrl -l | xwinpp -I - -p
wmctrl -lx | grep xterm | xwinpp -I - -p
xwinpp -I - -p < <(wmctrl -l)
xwinpp -I ./tmp/xids -p
xwinpp -I <(wmctrl -l) -p
wmctrl -l | XWINPP_WIN_ACTIVE=0x3000022 xwinpp -I - -p
wmctrl -l | xwinpp -I - -V 0x3000022 -p
```

To print a preparing output, but filter before:

```
wmctrl -l | xwinpp print -I - -b
wmctrl -l | xwinpp print -I - -D i:1
wmctrl -l | xwinpp print -I - -b -D curr,next
wmctrl -l | xwinpp print -I - -B -D s:web
wmctrl -l | xwinpp print -I - -t gui,pdf
```

To print a preparing output and reorder active win:

```
wmctrl -l | xwinpp --print -I - --win-active-pos=end
wmctrl -l | xwinpp -p -I - -P 1
```

To tag or untag windows with _XWINPP_TAGS property:

```
wmctrl -l | xwinpp -I - -S tag:office,audio,video
wmctrl -l | xwinpp -I - -D curr -S tag:gui,viewer,pdf
wmctrl -l | xwinpp -I - -B -S tag:
```

#### Environment

`XWINPP_WIN_ACTIVE`
You may use this variable instead option `-V`.

`XWINPP_INPUT_FILE`
Use this instead of option `-I`.

#### Notes

- You can write all long commands and options without masking `--`. So, instead of `--help` you may use help
- You do not have to use `wmctrl`(1) for fetching X window ids. For example, you can also use `xdotool`(1), `xlsclients`(1), `xwit`(1), `xwininfo`(1), `xprop`(1) or `lsw`(1).  But  ensure  to  get an input, which is separated by newline; X window ids must be in the first field and  separated from  second  field with space character (delimiter=`[:space:]`). See also: https://github.com/D630/gist-bin-pub/tree/master/get-xids and http://www.ict.griffith.edu.au/anthony/info/X/WindowID.hints

#### To do

See [file TODO](../master/doc/TODO.md)

#### Bugs & Requests

Report it on https://github.com/D630/xwinpp/issues

#### See also

https://github.com/D630/xwinreg
