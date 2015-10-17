The following guide applies to the Raspberry Pi 2 running Raspbian.
Running a program at system startup may require different steps on different Linux distributions.

I prefer running my auto-starting scripts in `tmux` as I can then watch their terminal output even if I connect later over `ssh`.

First install `tmux`:

    sudo apt-get install tmux

...then edit `/etc/rc.local`. Add this line before `exit 0`:

    sudo -H -u pi tmux new -d -s openwebrx-session 'bash -c "cd /home/pi/openwebrx; ./openwebrx.py; bash"'

(Don't forget to substitute the correct path to OpenWebRX!)

Next time you restart your Pi, OpenWebRX should be running automatically.

You will also be able to inspect the terminal output by typing:

    tmux a

`tmux` is quite a useful tool. The programs you start within a `tmux` session will persist even if you close your `ssh` session. Some basic keybindings:

    Ctrl+b, then d        :  Detaches the tmux session. You can return to it by `tmux a`.
    Ctrl+b, then % or "   :  Splits your screen horizontally or vertically.
                             Press Ctrl+d to close the newly created pane.
                             Press Ctrl+
    Ctrl+b, then c        :  Creates a new window for you.
    Ctrl+b, then 1-2-3... :  Lets you navigate through your windows.




