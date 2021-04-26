

## Enabling nearby searches

If you do want to fetch places from a custom location or refresh them when the user moves the map, you must enable /nearbysearch queries in this LIB.

To do that, enable this flag in your project:
```xml  
 <bool name="enable_nearby_search">true</bool>
```

By doing so, This lib behaviour will be slightly changed:
- All places will be fetched by /nearbysearch queries.
- You get a button to refresh the places for the current location.
- You can set the initial map position to get the places from via `pingBuilder.setLatLng(LatLng)`


Add Jitpack in your root build.gradle at the end of repositories:

```gradle
    allprojects {
        repositories {
            ...
            maven { url 'https://jitpack.io' }
        }
    }
```

Step 2. Add the dependency

```gradle
    dependencies {
            // Places library
            implementation 'com.google.android.libraries.places:places:2.0.0'
            // PING Place Picker
            implementation 'com.github.Mayank8290:MapLocationPicker:1.1'
    }
```

## Setup

 1. Add Google Play Services to your project - [How to](https://developers.google.com/android/guides/setup)
 2. Sign up for API keys - [How to](https://developers.google.com/places/android-sdk/signup)
 3. Add the Android API key to your **AndroidManifest** file as in the [sample project](https://github.com/rtchagas/pingplacepicker/blob/master/sample/src/main/AndroidManifest.xml#L15).
 4. Optional but strongly recommended to enable R8 in you *[gradle.properties](https://github.com/rtchagas/pingplacepicker/blob/master/gradle.properties#L12)* file


### - Kotlin
```kotlin
    private fun showPlacePicker() {  
        val builder = PingPlacePicker.IntentBuilder()
	builder.setAndroidApiKey("YOUR_ANDROID_API_KEY")  
        	.setMapsApiKey("YOUR_MAPS_API_KEY")
	
	// If you want to set a initial location rather then the current device location.
	// NOTE: enable_nearby_search MUST be true.
        // builder.setLatLng(LatLng(37.4219999, -122.0862462))
	
        try {
            val placeIntent = pingBuilder.build(this)
            startActivityForResult(placeIntent, REQUEST_PLACE_PICKER)
        }
        catch (ex: Exception) {  
            toast("Google Play Services is not Available")  
        }
    }
    
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {  
	super.onActivityResult(requestCode, resultCode, data)  
	if ((requestCode == REQUEST_PLACE_PICKER) && (resultCode == Activity.RESULT_OK)) {  
	    val place: Place? = PingPlacePicker.getPlace(data!!)  
	    toast("You selected: ${place?.name}")  
	}  
    }
```

### - Java
```java
    private void showPlacePicker() {
	PingPlacePicker.IntentBuilder builder = new PingPlacePicker.IntentBuilder();
	builder.setAndroidApiKey("YOUR_ANDROID_API_KEY")
	       .setMapsApiKey("YOUR_MAPS_API_KEY");
	
	// If you want to set a initial location rather then the current device location.
	// NOTE: enable_nearby_search MUST be true.
        // builder.setLatLng(new LatLng(37.4219999, -122.0862462))
	
	try {
	    Intent placeIntent = builder.build(getActivity());  
	    startActivityForResult(placeIntent, REQUEST_PLACE_PICKER);  
	}  
	catch (Exception ex) {  
	    // Google Play services is not available... 
	}
    }
    
    @Override  
    public void onActivityResult(int requestCode, int resultCode, Intent data) {  
        if ((requestCode == REQUEST_PLACE_PICKER) && (resultCode == RESULT_OK)) {  
            Place place = PingPlacePicker.getPlace(data);  
	    if (place != null) {  
                Toast.makeText(this, "You selected the place: " + place.getName(), Toast.LENGTH_SHORT).show();
            }  
        }
    }
```

## API Keys

This Lib needs two API keys in order to work.

It was decided to split the API keys to clearly distinguish what you're going to be charged for. Also, the Places Web API and the Geocoding API don't allow an Android API key to be used. To not expose an unrestricted key for all APIs, the Maps API key is now required.

| Key | Restriction | Purpose
|--|--|--|
| Android key | [Android Applications](https://developers.google.com/places/android-sdk/signup#restrict-key) | Used as the Places API key. Main purpose is to retrieve the current places and place details.
| Maps key | [APIs: Geocoding, Maps Static and Places API only](https://cloud.google.com/docs/authentication/api-keys#api_key_restrictions) | Used to fetch static maps, nearby places through Places Web API and perform reverse geocoding on the current user position. That is, discover the address that the user is current pointing to. Your key should look [like this](https://raw.githubusercontent.com/rtchagas/pingplacepicker/master/images/maps_api_key.png).

**TIP:** It is strongly recommended to **not expose** your Maps API key in your resource files. Anyone could decompile your apk and have access to that key. To avoid this, the key should be at least obfuscated.
A nice approach is to save the key in the cloud through "Firebase remote config" and fetch it at runtime.


```

## Theming

PING is fully customizable and you just need to override some colors to make it seamlessly connected to your app.

Since release [2.0.0](https://github.com/rtchagas/pingplacepicker/releases/tag/2.0.0) PING supports dark/night mode by default.<br/>
Please make sure your app provide the correct resources to switch to night mode.

You can always refer to [Material Design documentation](https://material.io/develop/android/theming/dark) to know more about dark theme and how to implement it.

To customize PING you need to override these colors:

For day/light theme:

- `res/values/colors.xml`

```xml

    <!-- Toolbar color, places icons, text on top of primary surfaces -->
    <color name="colorPrimary">@color/material_teal500</color>
    <color name="colorPrimaryDark">@color/material_teal800</color>
    <color name="colorOnPrimary">@color/material_white</color>

    <!-- Accent color in buttons and actions -->
    <color name="colorSecondary">@color/material_deeporange500</color>
    <color name="colorSecondaryDark">@color/material_deeporange800</color>
    <color name="colorOnSecondary">@color/material_white</color>

    <!-- Main activity background -->
    <color name="colorBackground">@color/material_grey200</color>
    <color name="colorOnBackground">@color/material_black</color>

    <!-- Cards and elevated views background -->
    <color name="colorSurface">@color/material_white</color>
    <color name="colorOnSurface">@color/material_black</color>

    <!-- Text colors -->
    <color name="textColorPrimary">@color/material_on_surface_emphasis_high_type</color>
    <color name="textColorSecondary">@color/material_on_surface_emphasis_medium</color>

    <color name="colorMarker">@color/material_deeporange400</color>
    <color name="colorMarkerInnerIcon">@color/material_white</color>

```

For night/dark theme:

- `res/values-night/colors.xml`

```xml

    <color name="colorPrimary">@color/material_teal300</color>
    <!-- Let the primary dark color as the surface color to not colorfy the status bar -->
    <color name="colorPrimaryDark">@color/colorSurface</color>
    <color name="colorOnPrimary">@color/material_black</color>

    <color name="colorSecondary">@color/material_deeporange200</color>
    <color name="colorSecondaryDark">@color/material_deeporange300</color>
    <color name="colorOnSecondary">@color/material_black</color>

    <color name="colorBackground">@color/colorSurface</color>
    <color name="colorOnBackground">@color/colorOnSurface</color>

    <color name="colorSurface">#202125</color>
    <color name="colorOnSurface">@color/material_white</color>

    <color name="textColorPrimary">@color/material_on_surface_emphasis_high_type</color>
    <color name="textColorSecondary">@color/material_on_surface_emphasis_medium</color>

    <color name="colorMarker">@color/material_deeporange200</color>
    <color name="colorMarkerInnerIcon">@color/colorSurface</color>

```

