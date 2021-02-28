# drop-cache-if-idle
The tool which doesn't interrupt your processes, drops the cache if and only if the machine is idle.

### Why?
In WSL2 (Windows Subsystem for Linux, version 2), there's a [bug](https://github.com/microsoft/WSL/issues/4166). Due to that issue, WSL2 doesn't return the cache, instead, the amount of cache grows until the WSL2 instance's assigned RAM is full. Well, you can simply drop the cache. But, there would be a problem for the speed of your process. Instead, the script makes sure that the WSL2 instance is idle, then drops the cache. With that approach, we could eliminate the speed issue, for some of the cases.

### Installation
1. Install Python 3.6+ (with `pip`), with `psutil` installed. Make sure that `psutil` is not installed as user, for example don't do `pip3 install psutil`. Instead, install it with `sudo` or if your distro have the related package something like `python3-psutil`, go with it.
2. Download the repo, you can find `drop_cache_if_idle` script there.
3. Copy `drop_cache_if_idle` file on one of your `$PATH`, for example `/usr/bin/`.
4. On your WSL bash execute `sudo crontab -e -u root` and add the following line: `*/3 * * * * drop_cache_if_idle`. The "*/3" means that it will be executed every 3 minutes. You can change it if you wish.
5. On your `~/.bashrc` add the following line: `[ -z "$(ps -ef | grep cron | grep -v grep)" ] && sudo /etc/init.d/cron start &> /dev/null`
6. On your WSL bash execute $ sudo visudo and add the following line: `%sudo ALL=NOPASSWD: /etc/init.d/cron start`
7. Then, run `wsl --shutdown` on cmd.exe. This will shut all of your instances of WSL2 down.

### References
Thanks to the comment [comment on the issue of WSL repository](https://github.com/microsoft/WSL/issues/4166#issuecomment-618159162) and repo [AdnanHodzic/auto-cpufreq](https://github.com/AdnanHodzic/auto-cpufreq) for providing the related information used in this repo. 

### Notes
Also, you can use [arkane-systems/wsl-drop-cache]( https://github.com/arkane-systems/wsl-drop-cache). This is a systemd-based implementation of that package, using the same underlying Python script, but modified to run from a systemd timer and to be installed using an apt package. 
