# Using SAGA GIS with QGIS

There is a question I've seen a lot over the past couple of years on StackExchange, GitHub, and elsewhere. It comes in many forms and variations, but the crux of it is this:

> Why can't I get SAGA to work with QGIS?

Well, dear reader, I'm here to tell you that there's an easy solution.

## What is SAGA?

If you want the whole story, I'm going send you [over to Wikipedia](https://en.wikipedia.org/wiki/SAGA_GIS) for that one. The short story is this: [SAGA](https://saga-gis.sourceforge.io/en/index.html) - short for *System for Automated Geoscientific Analysis* - is a set of FOSS GIS tools that have been around for quite a while. There are some pretty great tools included - both for vector and raster processing - including a suite of Hydrology tools that I've used quite frequently over the years.

## What is QGIS?

Again, let's just [head to Wikipedia](https://en.wikipedia.org/wiki/QGIS) first - don't worry, you can come right back. As I mentioned in [another post](https://miketalbot.io/how-i-discovered-cloud-gis), I was initiated in GIS through ESRI software and it took me a few tries to finally make the switch to QGIS, but once I did there was really no looking back. I'll write more about that later. The short story, again, is this: QGIS is a free and open-source desktop GIS software that can very likely do anything you need it to do and do it well. Pertinent to our discussion here, through community contributions to its extensive [plugin repository](https://plugins.qgis.org/plugins/) it can also act as a GUI for other FOSS GIS toolsets, including SAGA, [GRASS](https://grass.osgeo.org/), [WhiteboxTools](https://www.whiteboxgeo.com/), and [TauDEM](https://hydrology.usu.edu/taudem/taudem5/), among others.

## Installing SAGA

Installing SAGA is pretty straightforward. Head to the [downloads page](https://sourceforge.net/projects/saga-gis/files/) and choose the installer for your version and OS of choice. For the purposes of this exercise you're going to want to **download and install version 7.2.0**. If you're running Windows, I've made it easy for you since you can install it via [my Chocolatey package](https://community.chocolatey.org/packages/saga-gis):

```powershell
choco install saga-gis --version=7.2.0
```

For the record, I use Ubuntu most of the time, but I've spent enough time doing IT work to know my audience. If you're running any flavor of Linux, I'll just assume you're clever enough to figure this out. And if you're using a Mac, may god have mercy on your soul. Just kidding 😉 Seriously though, I'm simply not cool enough to be able to help you. Don't hold it against me.

## Setting up the SAGA QGIS Plugin

For quite a while, SAGA came installed with QGIS along with a companion plugin that was enabled by default called "SAGA GIS provider". Starting around QGIS 3.16, the left hand and the right hand apparently weren't talking, and the version of SAGA that came installed with QGIS changed (SAGA v7.8.3) while the plugin remained stuck supporting an older version (SAGA v2.3.2). This has apparently created headaches for a lot of folks - myself included - since QGIS 3.16 became the long-term release. I recently learned that this [will sort of be fixed](https://github.com/qgis/QGIS/issues/51041) in QGIS 3.28.1, but only in that the old SAGA plugin will no longer be enabled by default.

Then, last year, I finally stumbled upon [a YouTube video](https://www.youtube.com/watch?v=VKdaripCups) explaining that a new plugin, "Processing SAGA NextGen Provider", is available that supports a newer (but sadly no, not the latest) version of SAGA: v7.2.0. Even though it isn't being actively supported and developed (it was intended to be a community effort), I'm still thankful to the folks down under at [North Road Consulting](https://north-road.com/) for getting us this far. For what it's worth, in my experience it seems to work pretty well as is.

Assuming you've already installed the correct version of SAGA, simply head to the Plugins menu in QGIS, search for SAGA, and install and enable the *Processing SAGA NextGen Provider* plugin. If you're using a version of QGIS below 3.28.1, this would also be a good time to disable (i.e., uncheck) the old *SAGA GIS provider* plugin to avoid any confusion going forward.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677712848912/31c610b1-93a5-4160-9d4f-ae598ef8c3e6.png align="center")

From here, there's just one more step. Head to Options in the QGIS Settings menu and under `Processing -> Providers -> SAGANG`, and point the SAGA folder to the path where your installation is located. On my Windows machine, it was at `C:/Program Files (x86)/SAGA-GIS`:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1677713140123/1361b628-efb3-4bfe-b66a-7e43c426f173.png align="center")

And that's it! Enjoy using the SAGA tools - I hope without too much frustration.