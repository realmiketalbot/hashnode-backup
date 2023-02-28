# How I discovered cloud GIS

In early 2019, I started down the arduous but (in retrospect) life-changing path of improving my geospatial data management practices. I had recently been pondering making the switch from ArcGIS Desktop to QGIS rather than bothering with ArcGIS Pro - which is a whole other story - and randomly stumbled across the [Switch from Shapefile](http://switchfromshapefile.org/) page. It advised trying the [OGC GeoPackage](http://www.geopackage.org/) data format and did a pretty convincing job. At the time, I was working with a GIS data manager who was getting ready to retire and was set in their ways, so after some pushback on this idea I decided it would be better to do a bit of my own testing and research. I worked at a small company and, while it has since grown, I didn't have access to either a GIS or an IT team - these were single-person departments back then without a lot of extra bandwidth.

I had attempted to switch to QGIS several times previously: a couple of years prior at the same job while it was QGIS 2.0, and a few years before that while it was QGIS 1.0. By now, QGIS 3.4 was out and I was finally starting to like what I saw. Armed with this new vendetta against Shapefiles, and seeing that ESRI didn't appear to be interested in allowing me to work with GeoPackages unless I bought the Data Interoperability Extension (thanks but no thanks), I made the leap of faith and, soon enough, I was hooked.

It didn't take long for me to get the hang of things since QGIS 3 had a much more intuitive GUI than its predecessors. At some point, I noticed the empty "PostGIS" menu in the Browser panel and had an epiphany: wasn't Mikhail (the Russian PhD student I had shared an office with during my MS, and possibly one of the most clever individuals with whom I've ever worked) always talking [about PostGIS](https://postgis.net/)? What *was* PostGIS anyway?

As it turned out, PostGIS was a spatial extension for [PostgreSQL](https://www.postgresql.org/) - an open-source relational database management system (RDBMS). Talk about a rabbit hole, not to mention a mouthful. PostGIS "adds support for geographic objects allowing location queries to be run in SQL" - which basically means you can read from and write to a point, line, or polygon layer represented as a table in a database, just as you would with a Shapefile. However, it also means you can write SQL queries and perform *server-side* manipulation of spatial data. What's that? Did I hear someone say "[thin client](https://en.wikipedia.org/wiki/Thin_client) computing"? Neat stuff to say the least. I convinced our GIS data manager to attend an "Intro to PostGIS" workshop with me taught by [U-Spatial](https://rc.umn.edu/uspatial)'s Pete Wiringa and, while it didn't catch on for them, once again I was hooked.

Fast forward six months or so: I was deploying PostgreSQL using Google Cloud and testing it with a small group of half-willing QGIS converts (god bless them all for sticking with me). And it was working - albeit not as smoothly as it was a year later when I migrated to [CrunchyBridge](https://www.crunchydata.com/products/crunchy-bridge) and leveraged accelerated networking in Azure... but working nonetheless. We were reading and writing points, lines, and polygons like nobody's business. We were using GIS while working remotely *without a VPN connection* (this was late 2019 now and would come in p-r-e-t-t-y handy during the imminent pandemic, by the way). We were *concurrently editing* spatial datasets. It was glorious. But there was this question in the back of my mind... one that was haunting me like a bad dream: *What about rasters?*

For a while, I tried - believe me, I really did - to use rasters in PostGIS. I just couldn't make it work - at least not with QGIS. I'm still holding out hope that some knight in shining, open-source armor will come along and show me the ropes (could it be you?) but, aside from the fact that I didn't like how the overviews cluttered up the schemas, I just couldn't get them to render correctly.

At some point during the countless hours I spent Googling my issues, I came across yet another open-source project that sent me in yet another new and wonderful direction: [Cloud Optimized GeoTIFFs](https://www.cogeo.org/), or COGs. In case you haven't heard of them, a COG is:

> ...a regular GeoTIFF file, aimed at being hosted on a HTTP file server, with an internal organization that enables more efficient workflows on the cloud. It does this by leveraging the ability of clients issuing ​HTTP GET range requests to ask for just the parts of a file they need.

What this amounts to is a GeoTIFF that has internal overviews and tiles, the presence of which allows you to load just the portion of a COG within your current map window and render it only at the resolution required for your current zoom level. Imagine adding a 10 GB lidar-derived DEM to your map and having it render more or less instantly every time you pan or zoom. Now imagine it's in the cloud. Is your mind blown? Maybe not but, for what it's worth, mine was. It was only a matter of time before I was storing COGs in Azure Blob Storage and making our statewide lidar datasets more accessible than we had ever dreamed.

Within the span of two years, I had discovered QGIS, PostGIS, and COGs, and had become familiar with both Google Cloud Platform and Microsoft Azure (sorry not sorry, Bezos). Our GIS data manager also retired during this time and in their place was a young and (mostly) willing participant in my experiments. The rest, as they say, is history - albeit one that is still being written by me and those I've managed to hitch to the cloud GIS wagon. In the years since, I've seen the shift in our GIS users' attitudes toward this framework from annoyance to skepticism to acceptance and finally to enthusiasm, and I feel I have accomplished something. I had lived a sheltered little GIS existence and, working at a small company, the concept of cloud computing had snuck up behind me and just walked right on past without so much as a wink or a nod. To me, "The Cloud" had been Dropbox and Google Drive, but no more. Now I'm walking tall with my head in the cloud.