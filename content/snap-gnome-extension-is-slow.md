+++
title = "Snapcraft Gnome extension is slow to install"
date = 2025-05-08
+++

Just a very quick one to note that if you add the gnome extension to your `snapcraft.yml`, e.g.:
```yml
apps:
  lychee-slicer:
    extensions: [gnome]
    command: opt/LycheeSlicer/lycheeslicer
    plugs:
```

The first time you run `snapcraft` it will actually be very slow and won't give you any feedback other than a timer running behind the `Installing build-snaps` line. Looking at the log of the snap it indicates it is waiting on the terminal, but it is actually installing things, such as the mesa and gnome dependencies. It took about 10 minutes to run for me, after I finally decided it was actually doing something and waited.

