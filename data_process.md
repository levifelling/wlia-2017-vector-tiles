# export PostGIS to GeoJSON
ogr2ogr -geomfield geo -t_srs EPSG:4326 -f GeoJSON data/json/hydro_poly.json PG:"host=localhost dbname=wgx_eauclairecowi" -sql "select st_transform(the_geom,4326) as geo, common_nam as name from hydro_poly where st_isvalid(the_geom)"

ogr2ogr -geomfield geo -t_srs EPSG:4326 -f GeoJSON data/json/parks.json PG:"host=localhost dbname=wgx_eauclairecowi" -sql "select st_transform(the_geom,4326) as geo, name from parks where st_isvalid(the_geom)"

ogr2ogr -geomfield geo -t_srs EPSG:4326 -f GeoJSON data/json/plss_townships.json PG:"host=localhost dbname=wgx_eauclairecowi" -sql "select st_transform(the_geom,4326) as geo, township_i from plss_townships where st_isvalid(the_geom)"

ogr2ogr -geomfield geo -t_srs EPSG:4326 -f GeoJSON data/json/civil_divisions.json PG:"host=localhost dbname=wgx_eauclairecowi" -sql "select st_transform(the_geom,4326) as geo, cd_label from civil_divisions where st_isvalid(the_geom)"

ogr2ogr -geomfield geo -t_srs EPSG:4326 -f GeoJSON data/json/master_centerlines.json PG:"host=localhost dbname=wgx_eauclairecowi" -sql "select st_transform(the_geom,4326) as geo, str_class, stlabel from master_centerlines where st_isvalid(the_geom)"

ogr2ogr -geomfield geo -t_srs EPSG:4326 -f GeoJSON data/json/parcel.json PG:"host=localhost dbname=wgx_eauclairecitywi" -sql "select st_transform(the_geom,4326) as geo, total_assessed_value,year_constructed,property_class, pin_no from wgx_parcel where st_isvalid(the_geom)"

tippecanoe -rg -f -o data/mbtiles/eauclaire.mbtiles data/json/*.json
tileserver-gl-light data/mbtiles/eauclaire.mbtiles

# clipping mbtiles
npm install -g tl
npm install -g mbtiles

wget https://openmaptiles.os.zhdk.cloud.switch.ch/v3.3/extracts/united_states_of_america.mbtiles (6.3GB)

tl copy -z 0 -Z 14 -b "-93.284912109375 42.35042512243457 -86.50634765625 46.98774725646568" mbtiles://./united_states_of_america.mbtiles mbtiles://./wisconsin.mbtiles 
(~200MB)

tl copy -z 0 -Z 14 -b "-93.80126953124999 43.213183300738876 -89.56054687499999 45.99696161820381" mbtiles://./wisconsin.mbtiles mbtiles://./osm_eauclaire.mbtiles
(87MB)


