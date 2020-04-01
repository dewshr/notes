# Notes on Using Systemd

- [Notes on Using Systemd](#notes-on-using-systemd)
  - [Running Services as User](#running-services-as-user)
  - [Creating Custom Systemd Services](#creating-custom-systemd-services)
  - [Scheduling Systemd Tasks](#scheduling-systemd-tasks)

Knowing systemd is useful for automating many tasks in Linux.

## Running Services as User

Running services as a user instead of root is beneficial in many cases.

For example, we may want to run Music Player Daemon (mpd) on startup as a user in order to use user configuration files instead of the global ones. In this case, we must first disable the default service, and re-enable it as user:

    systemctl stop mpd.service
    systemctl disable mpd.service
    systemctl --user enable mpd.service

## Creating Custom Systemd Services

User services are stored under: `~/.config/systemd/user/`. Place any custom scripts that you write in this folder.

Here is an example change-GTK-theme.service, which changes my computer's GTK theme to dark mode:

```
[Unit]
Description=Change the GTK theme to dark mode.
After=graphical.target

[Service]
Type=oneshot
ExecStart=/bin/sh -c 'gsettings set org.gnome.desktop.interface gtk-theme Adwaita-dark && gsettings set org.gnome.gedit.preferences.editor scheme builder-dark'

[Install]
WantedBy=default.target
```

## Scheduling Systemd Tasks

Systemd can be used as a more powerful alternative to cron jobs.

To do this, you need to create two files:  `my-service.service`, and a `my-service.timer`, both files must have the same root name.

Example timer file:

```
[Unit]
Description=Change the GTK theme daily at a given time.

[Timer]
OnCalendar=*-*-* 16:00:00
Persistent=true

[Install]WantedBy=timers.target
```
This timer runs the change-GTK.service shown previously every day at 16:00 hrs.

For more on timers: https://wiki.archlinux.org/index.php/Systemd/Timers