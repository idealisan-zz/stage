# CentOS 8 如何更正时区

最近发现程序输出的时间不对，尝试使用date查看时间，发现也不对，按照网上的tzselect修改时区，还是不对。

发现有效的方法是使用timedatectl修改。

使用

```bash
$ sudo timezonectl --help
```

查看帮助，可以看到set-timezone TIMEZONE可以设置时区，使用list-timezones可以查看时区。

```
timedatectl [OPTIONS...] COMMAND ...

Query or change system time and date settings.

  -h --help                Show this help message
     --version             Show package version
     --no-pager            Do not pipe output into a pager
     --no-ask-password     Do not prompt for password
  -H --host=[USER@]HOST    Operate on remote host
  -M --machine=CONTAINER   Operate on local container
     --adjust-system-clock Adjust system clock when changing local RTC mode
     --monitor             Monitor status of systemd-timesyncd
  -p --property=NAME       Show only properties by this name
  -a --all                 Show all properties, including empty ones
     --value               When showing properties, only print the value

Commands:
  status                   Show current time settings
  show                     Show properties of systemd-timedated
  set-time TIME            Set system time
  set-timezone ZONE        Set system time zone
  list-timezones           Show known time zones
  set-local-rtc BOOL       Control whether RTC is in local time
  set-ntp BOOL             Enable or disable network time synchronization

systemd-timesyncd Commands:
  timesync-status          Show status of systemd-timesyncd
  show-timesync            Show properties of systemd-timesyncd

```

使用

```bash
$ sudo timedatectl list-timezones |grep Asia
```

可以看到亚洲的时区，其中我需要的是上海时区，记作Asia/Shanghai，于是使用

```bash
$ sudo timedatectl set-timezone Asia/Shanghai
```

将时区设置为上海时区，再次查看date，会发现已经修改正确，使用的是CST时间而不是EST时间。

为了把时间保存到主板上，使用

```bash
$ sudo hwclock -w
```

命令把时间写入到硬件时间。