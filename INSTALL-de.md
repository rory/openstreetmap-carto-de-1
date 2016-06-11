# OpenStreetMap Carto DE setup instructions

The database scheme we use in german manik style does not use use separate
database columns for diffenent tags. Instead only one hstore column is used
to provide all important tags (see hstore-only.style osm2pgsql style).

For this reason the osm2pgsql commandline required will look slightly different:

```
osm2pgsql -S hstore-only.style --hstore --hstore-match-only -d osm planet-latest.osm.pbf
```

The actual rendering is done using database views providing virtual columns
instead of using the raw tables generated by osm2pgsql. These views have to
be created after the initial osm2pgsql data import using the provides SQL
files as follows:

```
psql -d osm -f osm_tag2num.sql
psql -d osm -f views_osmde/view-line.sql
psql -d osm -f views_osmde/view-point.sql
psql -d osm -f views_osmde/view-polygon.sql
psql -d osm -f views_osmde/view-roads.sql
```

It is also possible to use this database scheme for the generic
openstreetmap-carto style by replacing the hardcoded database table names in
project.yaml by the view names used in german style.

The style is currently developed using Debian GNU/Linux 8.x with all the
required software (postgresql-9.4, mapnik2.2 node-carto 0.9.5-2, ...)
provided by the distribution itself. Using Ubuntu >=14.4 should also work.