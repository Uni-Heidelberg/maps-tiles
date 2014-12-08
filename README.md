# Map content for the Uni HD App

## Requirements

- [gdal](http://www.gdal.org), also available to install via `brew install gdal`

## Generating Tiles

1. Create VRT file from map image with at least 4 sets of coordinates (x,y) in pixels from the top left and (lat, long) in degrees:

	`$ gdal_translate -of VRT -a_srs EPSG:4326 [-gcp x y long lat]* [source_file] [destination_file]`

2. Generate tiles:

	`$ python gdal2tiles.py -p mercator [vrt_file] [-z 15-20]`
	
### Coordinates

- INF: `-gcp 510 543 8.658369 49.422397 -gcp 2920 489 8.674420 49.422641 -gcp 3237 2805 8.676662 49.412631 -gcp 1482 2266 8.664986 49.414950`

## Deployment

- Upload to server directory `/static/map-tiles/{campusRegion.identifier}/`

## Displaying on iOS

- Add a `MKTileOverlay` with appropriate URL template to the map view:

	```
    MKTileOverlay *overlay = [[MKTileOverlay alloc] initWithURLTemplate:[NSString stringWithFormat:@"%@/{z}/{x}/{y}.png", serverURL]];
    overlay.geometryFlipped = YES;
    overlay.minimumZ = 14;
    overlay.maximumZ = 17;
    overlay.canReplaceMapContent = NO;
    [mapView addOverlay:overlay level:MKOverlayLevelAboveLabels];
	```
	
- In `mapView:rendererForOverlay:` return an `MKTileOverlayRenderer` initialized with the tile overlay


Altstadt

gdal_translate -of VRT -a_srs EPSG:4326 -gcp 214 700 8.693435 49.413864 -gcp 3056 2218 8.706047 49.409340 -gcp 5728 547 8.706047 49.409340 -gcp 3816 209 8.709498 49.415122 resources/alt.png alt_tile

python gdal2tiles.py -p mercator alt_tile -z 15-20

Bergheim:

gdal_translate -of VRT -a_srs EPSG:4326 -gcp 537 1353 8.682831 49.408787 -gcp 3384 2434 8.687846 49.408416 -gcp 6474 1567 8.692385 49.410241 -gcp 3391 710 8.687073 49.410302 resources/berg.png berg_tile

python gdal2tiles.py -p mercator berg_tyle -z 15-20

