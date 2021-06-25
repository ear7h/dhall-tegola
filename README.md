# dhall-tegola

A [dhall](https://dhall-lang.org/) schema for [tegola](https://tegola.io/).
Dhall does not currently have support for TOML (it's a WIP) so you can't
generate the configs directly, but soon!

# example

```dhall
let T = ./tegola.dhall

let testVal =
	{ tile_buffer = Some 64
	, webserver = T.Webserver::{ port = Some ":8080" }
	, cache = Some (T.Cache.File T.FileCache::
		{ max_zoom = 10
		, basepath = "./tegola-cache/"
		})
	, providers =
		[ T.Provider.PostGIS T.PostGIS::
			{ name = "ne"
			, host = "localhost"
			, database = "natural_earth"
			, user = "julio"
			, password = ""
			, layers =
				[ T.PostGISLayer::
					{ source = T.LayerSource.Table
						{ table = "ne_110m_coastline"
						, fields = None (List Text)
						}
					}
				]
			}
		]
	, maps =
		[ Map::
			{ name = "natural"
			, layers =
				[ T.MapLayer::
					{ provider_layer = "ne.ne_100m_coastline"
					}
				]
			}
		]
	} : T.Tegola
```
