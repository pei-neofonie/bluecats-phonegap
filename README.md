# com.bluecats.beacons

This plugin provides a Phonegap or Cordova app access to Bluetooth Low Energy iBeacons and Beacons via the BlueCats (www.bluecats.com) BLE SDKs for iOS and Android.

## Installation

Cordova

    cordova plugin add https://github.com/bluecats/bluecats-phonegap.git

Phonegap

    phonegap plugin add https://github.com/bluecats/bluecats-phonegap.git

## Suported Platforms

- iOS
- Android

## Methods

- com.bluecats.beacons.startPurringWithAppToken
- com.bluecats.beacons.watchMicroLocation
- com.bluecats.beacons.watchEnterBeacon
- com.bluecats.beacons.watchExitBeacon
- com.bluecats.beacons.watchClosestBeaconChange
- com.bluecats.beacons.clearWatch
- com.bluecats.beacons.localNotificationReceived
- com.bluecats.beacons.scheduleLocalNotification
- com.bluecats.beacons.cancelAllLocalNotifications

## Objects

- SDKOptions
- BeaconWatchOptions
- MicroLocation

## Quick Starts

## Hello Beacons

###  Quick Start for com.bluecats.beacons plugin 

This plugin provides a Phonegap or Cordova app access to Bluetooth Low Energy iBeacons and Beacons via the BlueCats (www.bluecats.com) BLE SDKs for iOS and Android.

This sample creates the default 'Hello World' Phonegap project and extends with Beacon functionality.

### Phonegap

    phonegap create MyCordovaProject
    cd MyCordovaProject
    phonegap plugin add https://github.com/bluecats/bluecats-phonegap.git
    cp plugins/com.bluecats.beacons/Samples/HelloBeacons/index.js www/js/index.js

Edit www/js/index.js and update the line:

    var blueCatsAppToken = 'BLUECATS-APP-TOKEN';

replacing `BLUECATS-APP-TOKEN` with your app token generated in the BlueCats console: https://app.bluecats.com/apps

If building for iOS 

    phonegap platform add ios
    phonegap build ios
    
If building for Android

    phonegap platform add android
    phonegap build android

### Cordova

If using Cordova replace the `phonegap` in the above comands with `cordova`

## com.bluecats.beacons.startPurringWithAppToken

Initialises the BlueCats SDK and starts scanning for bluetooth devices. The app token is generated in the BlueCats web console: https://app.bluecats.com

    com.bluecats.beacons.startPurringWithAppToken(appToken, success, error, sdkOptions)

### Example

```javascript
function success() {
    alert('BlueCats SDK is purring');
    //Start watching beacons using com.bluecats.beacons.watchEnterBeacon etc.
};

function error() {
    alert('onError!');
};

var sdkOptions = {
    trackBeaconVisits:true, //Log visits to beacons to the BlueCats api
    useLocalStorage:true, //Cache beacons in local db for offline availability
    cacheAllBeaconsForApp:true, //Cache all beacons on startup
    discoverBeaconsNearby:true, //Cache beacons as detected by the device
    cacheRefreshTimeIntervalInSeconds:300 //Period to check for changes in seconds
};

com.bluecats.beacons.startPurringWithAppToken('YOUR-APP-TOKEN', success, error, sdkOptions);
```

## com.bluecats.beacons.watchMicroLocation

Provides beacons and sites in range of the device and their current proximity.

The returned watch ID references the particular watch configuration,
and can be used with `com.bluecats.beacons.clearWatch` to stop watching the
beacons.

    var watchID = com.bluecats.beacons.watchMicroLocation(success,
                                                           error,
                                                           beaconWatchOptions);

###  Example

```javascript
function success(watchData) {
  var beacons = watchData.filteredMicroLocation.beacons;
  var sites = watchData.filteredMicroLocation.sites;
    alert('Beacons: ' + beacons.length + '\n' +
          'Sites: ' + sites.length);
};

function error() {
    alert('error!');
};

var beaconWatchOptions = {
  minimumTriggerIntervalInSeconds:2, //Integer. Minimum seconds between callbacks (default 1)
  repeatCount:3, //Integer. Default repeat infinite
    filter:{
      minimumProximity:'BC_PROXIMITY_IMMEDIATE', //String. Closest proximity to include. Default BC_PROXIMITY_IMMEDIATE
      maximumProximity:'BC_PROXIMITY_NEAR', //String. Furthest proximity to include. Default BC_PROXIMITY_UNKNOWN
      minimumAccuracy:0, //Number. Minimum distance in metres (Default 0)
        maximumAccuracy:0.5 //Number. Maximum distnace in metres (Default unrestricted)
        sitesNamed:['Site1','Another Site'],//Array of string. Only include beacons in specified sites
        categoriesNamed:['Entrance','Another Category'],//Array of string. Only include beacons in specified categories

    }
};

var watchID = com.bluecats.beacons.watchMicroLocation(success,
                                                       error,
                                                       beaconWatchOptions);

```

## com.bluecats.beacons.watchEnterBeacon

Provides beacons and sites as they come into range as specified by beaconWatchOptions filters.

The returned watch ID references the particular watch configuration,
and can be used with `com.bluecats.beacons.clearWatch` to stop watching the
beacons.

    var watchID = com.bluecats.beacons.watchEnterBeacon(success,
                                                           error,
                                                           beaconWatchOptions);

###  Example

```javascript
function success(watchData) {
  var beacons = watchData.filteredMicroLocation.beacons;
  var sites = watchData.filteredMicroLocation.sites;
    alert('Entered Beacons: ' + beacons.length + '\n' +
          'Sites: ' + sites.length);
};

function error() {
    alert('error!');
};

var beaconWatchOptions = {
  minimumTriggerIntervalInSeconds:2, //Integer. Minimum seconds between callbacks (default 1)
  repeatCount:3, //Integer. Default repeat infinite
  secondsBeforeExitBeacon:5, //Integer. Seconds after beacon leaves range before it can re-enter (Default 5)
    filter:{
      minimumProximity:'BC_PROXIMITY_IMMEDIATE', //String. Closest proximity to include. Default BC_PROXIMITY_IMMEDIATE
      maximumProximity:'BC_PROXIMITY_NEAR', //String. Furthest proximity to include. Default BC_PROXIMITY_UNKNOWN
      minimumAccuracy:0, //Number. Minimum distance in metres (Default 0)
        maximumAccuracy:0.5 //Number. Maximum distnace in metres (Default unrestricted)
        sitesNamed:['Site1','Another Site'],//Array of string. Only include beacons in specified sites
        categoriesNamed:['Entrance','Another Category'],//Array of string. Only include beacons in specified categories
    }
};

var watchID = com.bluecats.beacons.watchEnterBeacon(success,
                                                       error,
                                                       beaconWatchOptions);
```

## com.bluecats.beacons.watchExitBeacon

Provides beacons and sites as they leave range as specified by beaconWatchOptions filters.

The returned watch ID references the particular watch configuration,
and can be used with `com.bluecats.beacons.clearWatch` to stop watching the
beacons.

    var watchID = com.bluecats.beacons.watchExitBeacon(success,
                                                           error,
                                                           beaconWatchOptions);

###  Example

```javascript
function success(watchData) {
  var beacons = watchData.filteredMicroLocation.beacons;
  var sites = watchData.filteredMicroLocation.sites;
    alert('Exited Beacons: ' + beacons.length + '\n' +
          'Sites: ' + sites.length);
};

function error() {
    alert('error!');
};

var beaconWatchOptions = {
  minimumTriggerIntervalInSeconds:2, //Integer. Minimum seconds between callbacks (default 1)
  repeatCount:3, //Integer. Default repeat infinite
  secondsBeforeExitBeacon:5, //Integer. Seconds after beacon leaves range before counts as 'exited' (Default 5)
    filter:{
      minimumProximity:'BC_PROXIMITY_IMMEDIATE', //String. Closest proximity to include. Default BC_PROXIMITY_IMMEDIATE
      maximumProximity:'BC_PROXIMITY_NEAR', //String. Furthest proximity to include. Default BC_PROXIMITY_UNKNOWN
      minimumAccuracy:0, //Number. Minimum distance in metres (Default 0)
        maximumAccuracy:0.5 //Number. Maximum distnace in metres (Default unrestricted)
        sitesNamed:['Site1','Another Site'],//Array of string. Only include beacons in specified sites
        categoriesNamed:['Entrance','Another Category'],//Array of string. Only include beacons in specified categories
    }
};

var watchID = com.bluecats.beacons.watchExitBeacon(success,
                                                       error,
                                                       beaconWatchOptions);
```


## com.bluecats.beacons.watchClosestBeaconChange

Provides the closest beacon and its site if the current closest beacon changes as specified by beaconWatchOptions filters.

The returned watch ID references the particular watch configuration,
and can be used with `com.bluecats.beacons.clearWatch` to stop watching the
beacons.

    var watchID = com.bluecats.beacons.watchClosestBeaconChange(success,
                                                           error,
                                                           beaconWatchOptions);

###  Example

```javascript
function success(watchData) {
  var closestBeacon = watchData.filteredMicroLocation.beacons[0];
  var site = watchData.filteredMicroLocation.sites[0];
    alert('Closest Beacon is: ' + closestBeacon.name + '\n' +
          'In Site: ' + site.name);
};

function error() {
    alert('error!');
};

var beaconWatchOptions = {
  minimumTriggerIntervalInSeconds:2, //Integer. Minimum seconds between callbacks (default 1)
  repeatCount:3, //Integer. Default repeat infinite
  secondsBeforeExitBeacon:5, //Integer. Seconds after beacon leaves range before counts as 'exited' (Default 5)
    filter:{
      minimumProximity:'BC_PROXIMITY_IMMEDIATE', //String. Closest proximity to include. Default BC_PROXIMITY_IMMEDIATE
      maximumProximity:'BC_PROXIMITY_NEAR', //String. Furthest proximity to include. Default BC_PROXIMITY_UNKNOWN
      minimumAccuracy:0, //Number. Minimum distance in metres (Default 0)
        maximumAccuracy:0.5 //Number. Maximum distnace in metres (Default unrestricted)
        sitesNamed:['Site1','Another Site'],//Array of string. Only include beacons in specified sites
        categoriesNamed:['Entrance','Another Category'],//Array of string. Only include beacons in specified categories
    }
};

var watchID = com.bluecats.beacons.watchClosestBeaconChange(success,
                                                       error,
                                                       beaconWatchOptions);
```


## com.bluecats.beacons.clearWatch

Stop watching beacons referenced by the `watchID` parameter.

    com.bluecats.beacons.clearWatch(watchID);

- __watchID__: The ID returned by `com.bluecats.beacons.watchMicroLocation` or `com.bluecats.beacons.watchEnterBeacon` or `com.bluecats.beacons.watchExitBeacon` or `com.bluecats.beacons.watchClosestBeaconChange`.

###  Example

```javascript
var watchID = com.bluecats.beacons.watchEnterBeacon(onSuccess, onError, options);

// ... later on ...

com.bluecats.beacons.clearWatch(watchID);
```

## com.bluecats.beacons.localNotificationReceived

Notifies when the app is launched by tapping open any local notification (not only notifications triggered by beacons)

The returned watch ID references the particular watch configuration,
and can be used with `com.bluecats.beacons.clearWatch` to stop watching the
beacons.

    com.bluecats.beacons.localNotificationReceived(success, error);

###  Example

```javascript
function success(notificationData) {
    alert('Notification received' + JSON.stringify(notificationData));
};

function error() {
    alert('error!');
};

com.bluecats.beacons.localNotificationReceived(success, error);
```

## com.bluecats.beacons.scheduleLocalNotification

Schedules a local notification to be triggered by detecting a beacon matching the specified local notification options

    com.bluecats.beacons.scheduleLocalNotification(success, error);

###  Example

```javascript
function success() {
    alert('Scheduled notification'));
};

function error() {
    alert('error!');
};

var categoryToTriggerByName = {name:'Entrance'};
var categoryToTriggerById = {id:'BLUECATS-CATEGORY-ID'};
var customNotificationData = {someKey:'key1', anotherKey:'other data'};
var localNotification = {
  fireInCategories:[categoryToTriggerByName,categoryToTriggerById],
  fireAfterDelayInSeconds:60,//Delay the earliest time this notification can trigger. Once the delay has passed, the other criteria will need to be met before triggering.
  alertAction:'View this',
  alertBody:'Welcome back. Here is some great info for you.',
  userInfo:customNotificationData //This object will be provided as notificationData to com.bluecats.beacons.localNotificationReceived callback
};

com.bluecats.beacons.scheduleLocalNotification(localNotification, success, error);
```


## com.bluecats.beacons.cancelAllLocalNotifications

Cancels all local notifications scheduled by com.bluecats.beacons.scheduleLocalNotification that have not yet been triggered/presented.

    com.bluecats.beacons.cancelAllLocalNotifications(success, error);

###  Example

```javascript
function success() {
    alert('Cancelled all beacon triggered local notifications'));
};

function error() {
    alert('error!');
};

com.bluecats.beacons.cancelAllLocalNotifications(success, error);
```

## BeaconWatchData and MicroLocation

Each of the beacon watch success callbacks `com.bluecats.beacons.watchMicroLocation` or `com.bluecats.beacons.watchEnterBeacon` or `com.bluecats.beacons.watchExitBeacon` or `com.bluecats.beacons.watchClosestBeaconChange` returns a 'beaconWatchData' object. This contains details of the beacons that met the specified filters and configuration of the watch instance.

### Example BeaconWatchData

```javascript
{
  triggeredCount:3,
  filteredMicroLocation:{}//See 'MicroLocation' below
}
```

### Example MicroLocation

```javascript
{
  "beacons": [
      {
        "id": "c0563500-dca2-69a2-9541-126eb7d81234",
        "teamID": "7e680e00-7ba2-8ab2-3943-f06fccd91234",
        "proximity": "BC_PROXIMITY_FAR",
        "bluetoothAddress": "000780681234",
        "siteID": "ef615500-7ba2-2e88-1f46-d12e5fb11324",
        "version": 20,
        "proximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
        "siteName": "BlueCats HQ",
        "accuracy": 6.1271604369518,
        "rssi": -96,
        "categories": [
          {
            "id": "cf0c5100-faa2-e1a0-ce4b-a5372eb11234",
            "name": "Soft Drinks"
          },
          {
            "id": "2a096301-81a2-7db7-2847-fd74630f1234",
            "name": "Food"
          }
        ],
        "name": "BlueCats iBeacon",
        "minor": 1010,
        "measuredPowerAt1Meter": -94,
        "major": 3
      },
      {
        "id": "0ef43100-dca2-d0a6-2041-e35e0dee1234",
        "teamID": "7e680e00-7ba2-8ab2-3943-f06fccd91234",
        "proximity": "BC_PROXIMITY_NEAR",
        "bluetoothAddress": "000780681234",
        "siteID": "8e4d1400-9aa2-668b-c44f-d14b1a0f1234",
        "version": 19,
        "proximityUUID": "61687109-905F-4436-91F8-E602F514C96D",
        "siteName": "Another Site",
        "accuracy": 0.37260415444465,
        "rssi": -73,
        "categories": [
          {
            "id": "8db32e00-01a3-f0ac-db4d-dbc9f1271234",
            "name": "Entrance"
          }
        ],
        "name": "BlueCats iBeacon",
        "minor": 1001,
        "measuredPowerAt1Meter": -82,
        "major": 3
      }
    ]
  },
  "sites": [
    {
      "name": "BlueCats HQ",
      "id": "ef615500-7ba2-2e88-1f46-d12e5fb11234",
      "teamID": "7e680e00-7ba2-8ab2-3943-f06fccd91234",
      "beaconCount": 3
    },
    {
      "name": "Another Site",
      "id": "8e4d1400-9aa2-668b-c44f-d14b1a0f1234",
      "teamID": "7e680e00-7ba2-8ab2-3943-f06fccd91234",
      "beaconCount": 2
    }
  ]
}
```

### Beacon
A beacon represents a single Bluetooth beacon device. It contains both static information about the beacon as well as the current state of the range beacon such as proximity and accuracy.

- __id__: string 
Bluecats unique identifier for this beacon

- __teamID__: string 
BlueCats unique identifier for the team this beacon belongs to

- __proximity__: string
Simplified classification of the distance to the beacon. Availalble values are: BC_PROXIMITY_IMMEDIATE (~< 0.5m) BC_PROXIMITY_NEAR (~< 3m) BC_PROXIMITY_FAR (~>3m) BC_PROXIMITY_UNKNOWN (unable to determine distance). These distances are approximations only and will vary by device and beacon configuration.

- __bluetoothAddress__: string
Unique identifier for the bluetooth device

- __siteID__: string
BlueCats unique identifier for the site this beacon belongs to

- __version__: integer
Updates made to a beacon that need to be flashed to the physical beacon increments the version number

- __siteName__: string
The name of the site the beacon belongs to

- __accuracy__: double
Approximite distance from the beacon in metres. 

- __rssi__: integer
Strength of the signal from the beacon. Can be in the range 0 to -100. The closer to zero the strong the signal

- __categories__: array see 'Category'
Categories 'tag' beacons with extra information. For example a beacon can be tagged with the category 'Entrance' to signify it is the entrance to a building or 'Refreshments' to signify it is a refreshment area etc.

- __name__: string
A friendly name for the beacon

- __proximityUUID__: string
The first component of the iBeacon identity

- __major__: integer
The second component of an iBeacon identity

- __minor__: integer
The third component of an iBeacon identity

- __measuredPowerAt1Meter__: integer
This is the expected rssi to be measured when a device is 1 meter from the beacon. This could be used for custom distance calculations.


### Site
A site is used by the BlueCats platform to group beacons into a physical location such as a single building or address.

- __name__: string
Friendly name for the site

- __id__:id string
Bluecats unique identifier for the site

- __teamID__: string
BlueCats unique identifier for the team this site belongs to

- __beaconCount__: integer
Total number of beacons in this site (whether or not they are in range)

### Category
A category 'tags' a beacon with extra information. For example a beacon can be tagged with the category 'Entrance' to signify it is the entrance to a building or 'Refreshments' to signify it is a refreshment area etc.

- __id__:id string
BlueCats unique identifier for the category

- __name__: string
Friendly name for the category

