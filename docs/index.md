# Upcal - Myanmar Uposatha calendar and dawn time widget

+ 🎉 Upcal new version: 2 (2021-01-20) (code re-organized - fresh installation needed for all)

## 1. Introduction

+ It shows calculated **Myanmar uposatha dates** for the current device month & dawn time for the current date in the new widget on your iPhone, iPad home screen.

![Upcal_Uposatha_Calendar_IOS](img_Upcal_Uposatha_Calendar_IOS.jpeg)

+ List a whole year Uposatha dates of any  year when user running **Upcal_Widget** in app mode.

![Whole Year Uposatha Calendar](img_Upcal_Whole_Year_Uposatha_Calendar_IOS.jpeg)

+ Can use GPS to update location or manually type in location data in **Upcal_Setting**.

## How To Install

1. Install [**Scriptable**](https://apps.apple.com/vn/app/scriptable/id1405459188) app by **Simon Støvring** from **App Store**.

   *(Note: iOS/iPadOS widget is only available for iOS/iPadOS version >=14)*

2. **Select** and **Copy** the below code:


```javascript

const gitURL = "https://vpnry.github.io/upcal/Upcal_Module.js";
const widgetName = "Upcal_Widget.js";
const moduleName = "Upcal_Module.js";
const jsonParsePoint = "u@pc@l";

let fm = FileManager.local();
const is_iCloud = fm.isFileStoredIniCloud(module.filename);
fm = is_iCloud ? FileManager.iCloud() : fm;

let filename = fm.fileName(module.filename, true);
if (filename != widgetName) {
  let newPath = fm.joinPath(fm.documentsDirectory(), widgetName);
  let str = fm.readString(module.filename);
  fm.writeString(newPath, str.trim());
  fm.remove(module.filename);
}
let modulePath = fm.joinPath(fm.documentsDirectory(), moduleName);
if (!fm.fileExists(modulePath)) {
  console.log(moduleName + " is not exist. Trying to download it from git.")
  let str = "";
  try {
    let r = new Request(gitURL);
    str = await r.loadString();
  } catch (e) {
    console.log("Error when trying to download module file:\n" +
      gitURL + "\n" + e);
    return;
  }
  if (str.indexOf(jsonParsePoint) > 0) {
    fm.writeString(modulePath, str);
  } else {
    console.log("Incompatible "+ moduleName +" file. Check!");
    return;
  }
}
// Must await & invoke app.init(fm) first
let app = await importModule(modulePath);
await app.init(fm);
if (config.runsInWidget) {
  let widget = await app.createUpCalWidget();
  Script.setWidget(widget);
} else { console.log(app.settings);
  await app.selectTask(); }

Script.complete();


```


1. [Click this URLScheme](scriptable:///add) to open Scriptable app and **Paste** the above code. _**Pls do not**_ click on the Play ▶️ icon to run it yet.  (So that the script will be renamed properly in step 3 below).
2. Tap **Done** at the top left corner of Scriptable app to save it.
3. In the Scriptable main screen, tap on the script file you have just created to run it.
4. It will rename the file to **Upcal_Widget** and download **Upcal_Module** from git server.
5. Follow its instructions to do location setup for dawn time calculation [or skip it].
6. Close Scriptable app, then add a Scriptable widget (and select the script file **Upcal_Widget**).

If you do not know how to add a Scriptable widget to home screen, try reading [4. How to add a widget to home screen on iOS/iPadOS (ver >=14)](#4-how-to-add-a-widget-to-home-screen-on-iosipados-ver-14) or do a search on the Internet.

## 2. Quick intro to Scriptable app

+ If you haven't used Scriptable app before, here is a quick guide:

> + To **create** a new file: Open Scriptable app, tap on the Plus ➕ icon (top right corner).

> + To **edit** an exist script file: tap on the three dot (top right) of the file.

> + To **run** a script file: in Scriptable app main screen, tap on the middle of the file, or on script file icon(top left). Or tap on the **Play** ▶️ icon (bottom right corner) if you are editing it.
> + To **delete** a script file: tap & hold, select **Delete** in the pop up menu.

## 3. Update Latitude and Longitude
You can use GPS to update location data automatically or type in manually:

### Fill in location manually

+ You can fill in location latitude and longitude manually. In Scriptable app main screen, open the script file **Upcal_Setting** and update the default values.

### Use GPS to auto fill in location
+ In Scriptable app main screen, run **Upcal_Widget**, it will show a few menus which include “📡 **Use GPS Update Location**”, use this option to use GPS to detect your location **<u>if you have no concerns about location privacy.</u>**

**Notes**:

+ Remember to turn on  Location service (GPS) in your device *Setting > Privacy*, and give location access to Scriptable app before running the update location function. You may just allow it to access location service for one time; 
+ After Scriptable app successfully updated your location, you can  revoke Scriptable app location access permission and turn off your Location service completely to save battery.

If you get errors related to location, see [**Troubleshooting**](#5-troubleshooting) session below. 

![Auto update location](img_checkaccuracy.jpeg)


If it detects your location successfully! Congrats!

## 4. How to add a widget to home screen on iOS/iPadOS (ver >=14)

For this thing, you can search more on the Internet. One way to do is, from iPad/iPhone home screen:

+ Swipe from left to right until you arrive the widget page (= you can not swipe any more).
+ Tap and hold on an empty place until the icons/screen are shaking.
+ Click on the plus ➕ icon (top left corner) to add new widget.
+ On the search field area, type Scriptable to search, and tap on it in the search result.
+ Swipe from right to left or left to right to find & choose the **biggest** size widget. Tap Add widget.
+ A new Scripable widget will appear. Tap on it to configure it with these choices:

```text

Script field: select **Upcal_Widget**
When Interacting: choose **Run Script**

```
The option When Interacting: **Run Script** will allow you to click on the widget to run Upcal in app directly.

##  5. Troubleshooting

### 5.1 Scriptable cannot detect your location

If this error occurs when Scriptable app is trying to get location data, something like:

```bash
 'kCLErrorDomain error 0.' occurs:
```

**To fix this error and other possible errors related to fetching GPS data, you may try:**

> + Close Scriptable app first
> + Make sure you have turned on location service in your device *Setting > Privacy* and allow location access for both Maps app (by Apple - or other your-trusted map apps should work too) and Scriptable app.
> + Opening the **Maps** app (by Apple) (or  your other trusted map apps) and ask Maps to detect  your location, wait until Maps successfully locate  📍  your location (blue circle dot).
> + Then quickly open Scriptable app, in its main screen,  run `Upcal_Widget` and choose  “📡 **Use GPS Update Location**” to detect location again.

These issues may likely happen if you did not turn on Location service on your iPhone / iPad for a long time.

### 5.2 I have updated my location why the widget is still showing the same values?

+ Make sure you have saved your updates or let the  update process finish.
+ To force the widget to reload the script, simply tap on it (to run it, give the fact that you chose When Interacting: **Run Script** as instructed above).
+ Swipe screen from left to right or right to left to refresh the Widget?

### 5.3 The formulas for dawn time calculation

+ Currently, Upcal has 2 formulas for dawn time calculation. 
+ Formula 2 `Dawn = Sunrise - (Civil dawn - Nautical dawn)` is enabled by default.

+ To use Formula 1: `Dawn time = Sunrise time - u_DawnMinus (u_DawnMinus is a fixed minute)`.  In Scriptable app main screen, open the script file **Upcal_Setting** and change the value of  `”useDynamicDawn_Formula2”` to `false`.

+ Note: `u_DawnMinus` = 40 is a observed minute number in our place, it may not be correct for all places. You can change this value or simply use the Formula 2 instead.

### 5.4 Show|hide Nautical & Civil dawn info on the widget

+  By default Upcal hides the Nautical & Civil dawn values which helps the widget concise.
+  To show or hide them, in Scriptable app main screen, open the script file **Upcal_Setting** and change the value of `”displayNauticalCivilDawn”` to `true`. Or to `false` to hide them.

## 6. Related reading
There is an interesting research book Dawn (aruṇa) and Dawnrise (aruṇuggamana) in the Pāḷi Text:

```text

When Is Dawn (aruṇa)? When Is Dawnrise (aruṇuggamana)?
Research on Dawn (aruṇa) and Dawnrise (aruṇuggamana) in the Pāḷi Text  with Photographs and Scientific Explanations 
by Bhikkhu Ñāṇadassana

```


## 8. Notes:

+ When you move to a new city/new place, remember to update the lat & longitude values (manually or automatically). Otherwise, the dawn time calculation will not be correct for your new location. 
+ The author(s) and contributor(s) of this project take no responsibilities or warranties for anything issues related to the use of this app, includes, but not limited to, your location privacy or the accuracy of calculated data...
+ Keep in mind that the calculated data are for references only, it may vary (especially the dawn time) and incorrect , use with wisdom and peaceful mind!
+ If you do not trust Scriptable app, or Apple Maps or this [open source Upcal](https://github.com/vpnry/upcal) simply don't use them.


## 9. Open Source Libraries Acknowledgements 

   + Myanmar uposatha dates: powered by the algorithm of Modern Myanmar Calendrical Calculations - [MMCal](https://github.com/yan9a/mmcal) (Yan Naing Aye)  (MIT License)
  + Dawn time, sunrise...:  powered by [SunCalc](https://github.com/mourner/suncalc) (Vladimir Agafonkin) (MIT License)

  + Scriptable Widget and table UI is based on this  [Coronavirus Scriptable Widget](https://gist.github.com/planecore/e7b4c1e5db2dd28b1a023860e831355e) (03 Oct 2020)

## 10. Feedback

+ If you have any suggestions or found bugs, may give feedback here on [Google Form](https://forms.gle/1FzEVDYajhNXSkad7).

+ May you all be well and happy! 




