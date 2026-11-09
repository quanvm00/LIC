# Maps

- **Widget ID:** `com.mendix.widget.native.maps.Maps`
- **Type:** PLUGGABLEWIDGET
- **Version:** 5.0.0

## MDL Example

```sql
PLUGGABLEWIDGET 'com.mendix.widget.native.maps.Maps' widget1 {
  marker item1   -- one entry of `markers`
  dynamicmarker item2   -- one entry of `dynamicMarkers`
}
```

## Properties

| Property | Type | Required | Default | Values / notes | Group | Description |
|----------|------|----------|---------|----------------|-------|-------------|
| `markers` | object |  |  | list; 10 sub-properties below | General::Markers | A list of static locations on the map. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `locationType` | enumeration | Yes | address | `address` \| `latlng` |  | Location |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `address` | textTemplate |  |  |  |  | Address containing (a subset of) street, number, zipcode, city and country. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `latitude` | expression |  |  |  |  | Decimal number from -90.0 to 90.0. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `longitude` | expression |  |  |  |  | Decimal number from -180.0 to 180.0. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `title` | textTemplate |  |  |  |  | Title displayed when clicking the marker. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `description` | textTemplate |  |  |  |  | Description displayed when clicking the marker. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `onClick` | action |  |  |  |  | On click |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `icon` | icon |  |  |  |  | Icon |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `iconSize` | integer | Yes | 32 |  |  | Size |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `iconColor` | string |  |  |  |  | Overrides the default color provided through design properties. Not supported for custom images. |
| `dynamicMarkers` | object |  |  | list; 11 sub-properties below | General::Markers | A list of markers showing dynamic locations on the map. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `markersDS` | datasource |  |  | list |  | Data source |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `locationDynamicType` | enumeration | Yes | address | `address` \| `latlng` |  | Location |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `address` | attribute |  |  |  |  | Address containing (a subset of) street, number, zipcode, city and country. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `latitude` | attribute |  |  |  |  | Decimal number from -90.0 to 90.0. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `longitude` | attribute |  |  |  |  | Decimal number from -180.0 to 180.0. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `title` | attribute |  |  |  |  | Title displayed when clicking the marker. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `description` | attribute |  |  |  |  | Description displayed when clicking the marker. |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `onClick` | action |  |  |  |  | On click |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `icon` | icon |  |  |  |  | Icon |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `iconSize` | integer | Yes | 32 |  |  | Size |
| &nbsp;&nbsp;&nbsp;&nbsp;↳ `iconColor` | string |  |  |  |  | Overrides the default color provided through design properties. Not supported for custom images. |
| `fitToMarkers` | boolean | Yes | true |  | View::Automatic | The map view is centered and zoomed so that all the markers are visible. When switched off or when no marker is available the map centers on the default location. |
| `centerAddress` | expression |  |  |  | View::Default center | When latitude and longitude are provided, the address is ignored. |
| `centerLatitude` | expression |  |  |  | View::Default center | Latitude |
| `centerLongitude` | expression |  |  |  | View::Default center | Longitude |
| `defaultZoomLevel` | enumeration | Yes | city | `world` \| `continent` \| `country` \| `city` \| `town` \| `streets` \| `building` | View::Zoom | Zoom level for single marker, no marker or when fit to markers is disabled. |
| `minZoomLevel` | enumeration | Yes | world | `world` \| `continent` \| `country` \| `city` \| `town` \| `streets` \| `building` | View::Zoom | Minimum |
| `maxZoomLevel` | enumeration | Yes | building | `world` \| `continent` \| `country` \| `city` \| `town` \| `streets` \| `building` | View::Zoom | Maximum |
| `mapType` | enumeration | Yes | standard | `standard` \| `satellite` | Map | Type |
| `provider` | enumeration | Yes | default | `default` \| `google` | Map | The default map provider for iOS is Apple Maps, and for Android it is Google Maps. |
| `interactive` | boolean | Yes | false |  | Map | Enables interactive zoom and pitch and click events. Not recommended when you have multiple maps in the same page for better performance. |
| `showsUserLocation` | boolean | Yes | false |  | Map | Show the current location of the user, requires permission to access the user's location. |

## Object Lists (repeating child entries)

### `marker` → property `markers`

Item properties:

| Property | Operation |
|----------|-----------|
| `locationType` | primitive |
| `address` | texttemplate |
| `latitude` | expression |
| `longitude` | expression |
| `title` | texttemplate |
| `description` | texttemplate |
| `onClick` | action |
| `iconSize` | primitive |
| `iconColor` | primitive |

### `dynamicmarker` → property `dynamicMarkers`

Item properties:

| Property | Operation |
|----------|-----------|
| `markersDS` | datasource |
| `locationDynamicType` | primitive |
| `address` | attribute |
| `latitude` | attribute |
| `longitude` | attribute |
| `title` | attribute |
| `description` | attribute |
| `onClick` | action |
| `iconSize` | primitive |
| `iconColor` | primitive |

---

Regenerated by `mxcli widget docs` and by `refresh catalog`. For the same data live from the `.mpk` — including anything added by a widget upgrade since this file was written — run `mxcli widget describe nativemaps -p <app.mpr>`.
