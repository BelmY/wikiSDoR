## using `rc.local`

The following guide applies to the Raspberry Pi 2 running Raspbian. Running a program at system startup may require different steps on different Linux distributions.

I prefer running my auto-starting scripts in tmux as I can then watch their terminal output even if I connect later over ssh.

First install `tmux`:

    sudo apt-get install tmux

...then edit /etc/rc.local. Add this line before exit 0:

    sudo -H -u pi tmux new -d -s openwebrx-session 'bash -c "cd /home/pi/openwebrx; ./openwebrx.py; bash"'

(Don't forget to substitute the correct path to OpenWebRX!)

Next time you restart your Pi, OpenWebRX should be running automatically.

You will also be able to inspect the terminal output by typing:

    tmux a

`tmux` is quite a useful tool. The programs you start within a `tmux` session will persist even if you close your ssh session. Some basic keyboard shortcuts:

```
Ctrl+b, then d                    :  Detaches the tmux session. You can return to it by `tmux a`.
Ctrl+b, then % or "               :  Splits your screen horizontally or vertically.
                                     Press Ctrl+d to close the newly created pane.
Ctrl+b, then left/right/up/down   :  Navigate between split panes. 
Ctrl+b, then c                    :  Creates a new window for you.
Ctrl+b, then 1-2-3...             :  Lets you navigate through your windows.
```

## using `init.d`

The following guide applies to CentOS/Debian/Ubuntu (32/64 bit)

On the basis of the original (`sudo -H -u pi tmux new -d -s openwebrx-session 'bash -c "cd /home/pi/openwebrx; ./openwebrx.py; bash"'`), I changed to use `init.d` to control the process of OpenWebRX, which makes it easier to control the start and stop of OpenWebRX.


First install `tmux` (For CentOS , use `yum`):
```
sudo apt install tmux
```

Copy the following line, and save them to `/etc/init.d/openwebrx`:

```
#!/bin/bash

### BEGIN INIT INFO
# System Required:   CentOS/Debian/Ubuntu (32bit/64bit)
# Description:       Manager for OpenWebRX, Written by Yukiho Kikuchi
# Author:            Yukiho Kikuchi
# Provides:          OpenWebRX
# Required-Start:    $network $local_fs $remote_fs
# Required-Stop:     $network $local_fs $remote_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Description:       Open source, multi-user SDR receiver software with a web interface
### END INIT INFO

NAME="OpenWebRX"
NAME_BIN="openwebrx"
Info_font_prefix="\033[32m" && Error_font_prefix="\033[31m" && Info_background_prefix="\033[42;37m" && Error_background_prefix="\033[41;37m" && Font_suffix="\033[0m"
RETVAL=0

check_running(){
	PID=`ps -ef |grep "${NAME_BIN}" |grep -v "grep" |grep -v "init.d" |grep -v "service" |awk '{print $2}'`
	if [[ ! -z ${PID} ]]; then
		return 0
	else
		return 1
	fi
}
do_start(){
	check_running
	if [[ $? -eq 0 ]]; then
		echo -e "${Info_font_prefix}[Info]${Font_suffix} $NAME Has been running..." && exit 0
	else
		ulimit -n 51200
		sleep 5s
		tmux new -d -s openwebrx-session 'bash -c "cd /home/openwebrx; ./openwebrx.py; bash"'
		sleep 2s
		check_running
		if [[ $? -eq 0 ]]; then
			echo -e "${Info_font_prefix}[Info]${Font_suffix} $NAME Start successed!"
		else
			echo -e "${Error_font_prefix}[Error]${Font_suffix} $NAME Failed to start!"
		fi
	fi
}
do_stop(){
	check_running
	if [[ $? -eq 0 ]]; then
		kill -9 ${PID}
		RETVAL=$?
		if [[ $RETVAL -eq 0 ]]; then
			echo -e "${Info_font_prefix}[Info]${Font_suffix} $NAME Stop successed!"
		else
			echo -e "${Error_font_prefix}[Error]${Font_suffix}$NAME Failed to stop!"
		fi
	else
		echo -e "${Info_font_prefix}[Info]${Font_suffix} $NAME isn't running!"
		RETVAL=1
	fi
}
do_status(){
	check_running
	if [[ $? -eq 0 ]]; then
		echo -e "${Info_font_prefix}[Info]${Font_suffix} $NAME has been running..."
		echo -e "${Info_font_prefix}[Info]${Font_suffix} Listed PID:\n${PID}"
	else
		echo -e "${Info_font_prefix}[Info]${Font_suffix} $NAME isn't running!"
		RETVAL=1
	fi
}
do_restart(){
	do_stop
	do_start
}
case "$1" in
	start|stop|restart|status)
	do_$1
	;;
	*)
	echo "Usage: $0 { start | stop | restart | status }"
	RETVAL=1
	;;
esac
exit $RETVAL
```

( Don't forget to substitute the correct path to OpenWebRX while copying )

( Be careful, the line break should be UNIX LF )

```
sudo chmod +x /etc/init.d/openwebrx
```

Then edit `/etc/rc.local`. Add this line before `exit 0`:

```
sudo /etc/init.d/openwebrx start
```

Next time you restart your Pi/PC, OpenWebRX should be running automatically.

To check if OpenWebRX is running, use `sudo /etc/init.d/openwebrx status`, to restart, use `sudo /etc/init.d/openwebrx restart`, and use `sudo /etc/init.d/openwebrx stop` to stop.

You will also be able to inspect the terminal output by typing:

    tmux a

`tmux` is quite a useful tool. The programs you start within a `tmux` session will persist even if you close your `ssh` session. Some basic keyboard shortcuts:

    Ctrl+b, then d                    :  Detaches the tmux session. You can return to it by `tmux a`.
    Ctrl+b, then % or "               :  Splits your screen horizontally or vertically.
                                         Press Ctrl+d to close the newly created pane.
    Ctrl+b, then left/right/up/down   :  Navigate between split panes. 
    Ctrl+b, then c                    :  Creates a new window for you.
    Ctrl+b, then 1-2-3...             :  Lets you navigate through your windows.




