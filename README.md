# Map content for the Uni HD App

## Requirements

- gdal

## Generating Tiles

1. Create VRT file from map image with at least 4 sets of coordinates (x,y) in pixels from the top left and (lat, long) in degrees:

	`$ gdal_translate -of VRT -a_srs EPSG:4326 [-gcp x y long lat]* [source_file] [destination_file]`

2. Generate tiles:

	`python gdal2tiles.py -p mercator [vrt_file]`
	
### Coordinates

- INF: `-gcp 510 543 8.658369 49.422397 -gcp 2920 489 8.674420 49.422641 -gcp 3237 2805 8.676662 49.412631 -gcp 1482 2266 8.664986 49.414950`	

## Deployment


## Displaying on iOS

	NSString *tileDirectory = [[[NSBundle mainBundle] resourcePath] stringByAppendingPathComponent:@"inf"];
    NSURL *tileDirectoryURL = [NSURL fileURLWithPath:tileDirectory isDirectory:YES];
    NSString *tileTemplate = [NSString stringWithFormat:@"%@{z}/{x}/{y}.png", tileDirectoryURL];
    
    MKTileOverlay *infOverlay = [[MKTileOverlay alloc] initWithURLTemplate:tileTemplate];
    infOverlay.geometryFlipped = YES;
    infOverlay.minimumZ = 14;
    infOverlay.maximumZ = 17;
    infOverlay.canReplaceMapContent = NO;
    [self.mapView addOverlay:infOverlay level:MKOverlayLevelAboveLabels];

