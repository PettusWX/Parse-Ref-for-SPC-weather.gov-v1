JSON FILES EXPLAINED
============================

This folder has sample JSON files from the National Weather Service and Storm Prediction Center.
They show what the real data looks like when you pull it from their websites.


CONVECTIVE OUTLOOKS
-------------------
Files: convective_outlook_day1_categorical.json
       convective_outlook_day1_tornado.json
       convective_outlook_day2.json

These come from the Storm Prediction Center (SPC). They show areas where severe weather might happen.

The categorical ones show risk levels:
  DN 2 = TSTM (thunderstorms expected, nothing severe)
  DN 3 = MRGL (marginal risk, maybe 1 or 2 severe storms)
  DN 4 = SLGT (slight risk, scattered severe storms)
  DN 5 = ENH (enhanced risk, more storms, some strong)
  DN 6 = MDT (moderate risk, lots of severe storms likely)
  DN 8 = HIGH (high risk, this is rare and really bad)

The tornado one shows percent chance of a tornado within 25 miles of any point:
  0.02 = 2% chance
  0.05 = 5% chance
  0.10 = 10% chance
  SIGN = significant, means strong tornadoes (EF2+) possible

Key fields:
  VALID = when the outlook starts
  EXPIRE = when it ends
  ISSUE = when they made it
  LABEL = short name
  LABEL2 = long name
  stroke/fill = colors for the map


MESOSCALE DISCUSSIONS
---------------------
Files: mesoscale_discussion_1.json
       mesoscale_discussion_2.json
       mesoscale_discussion_3.json

These are updates from SPC forecasters about whats happening right now. They write these when storms are forming or getting stronger.

Key fields:
  NUM = the discussion number (goes up all year)
  CONCERNING = what its about
  WATCH_PROB = chance they will issue a watch (0 to 100)
  WATCH_TYPE = TORNADO or SEVERE if a watch is coming
  DISCUSSION = what the forecaster wrote
  ATTN_WFOS = which local weather offices should pay attention
  LAT_LON = coordinates in a weird format SPC uses


SEVERE WEATHER WATCHES
----------------------
Files: severe_weather_watch_1.json
       severe_weather_watch_2.json
       severe_weather_watch_3.json

SPC issues these when conditions are right for severe storms. They cover big areas and last several hours.

Key fields:
  NUM = watch number
  TYPE = SVR means severe thunderstorm watch
  PDS = true means "particularly dangerous situation" (really bad)
  MAX_HAIL = biggest hail expected in inches
  MAX_WIND = strongest wind expected in mph
  STATES = which states are in the watch
  WATCH_COUNTIES = how many counties
  TORNADO_PROB = LOW, MODERATE, or HIGH
  HAIL_PROB = same
  WIND_PROB = same
  DISCUSSION = what they expect to happen


TORNADO WATCHES
---------------
Files: tornado_watch_1.json
       tornado_watch_2.json
       tornado_watch_3.json

Same as severe watches but tornadoes are expected.

Extra fields:
  TYPE = TOR means tornado watch
  EF2_PROB = chance of strong tornadoes (EF2 or higher)
  NOCTURNAL = true if its a nighttime event (more dangerous because you cant see them)


SEVERE THUNDERSTORM WARNINGS
----------------------------
Files: severe_warning_box_1.json
       severe_warning_box_2.json
       severe_warning_box_3.json

These come from local weather offices (not SPC). They mean a severe storm is happening RIGHT NOW.

Key fields:
  event = "Severe Thunderstorm Warning"
  areaDesc = counties in the warning
  geocode.SAME = county codes for weather radios
  geocode.UGC = zone codes
  sent/effective/onset = when it started
  expires/ends = when it ends
  severity = Severe or Extreme
  certainty = Observed (they see it) or Likely
  senderName = which weather office made it
  headline = the main message
  description = all the details about the storm

In the parameters section:
  VTEC = special code that tracks the warning
  maxWindGust = wind speed
  maxHailSize = hail size in inches
  thunderstormDamageThreat = EXPECTED, CONSIDERABLE, or DESTRUCTIVE
  tornadoDetection = POSSIBLE if it might spin up


TORNADO WARNINGS
----------------
Files: tornado_warning_box_1.json
       tornado_warning_box_2.json
       tornado_warning_box_3.json

These mean a tornado is happening or about to happen. Take shelter.

Key fields (same as severe warnings plus):
  event = "Tornado Warning"
  tornadoDetection = RADAR INDICATED or OBSERVED
  tornadoDamageThreat = CONSIDERABLE or CATASTROPHIC
  motionVector = direction and speed (like "225DEG 35KT" means from southwest at 35 knots)
  tornadoEmergency = TRUE means confirmed violent tornado in a populated area


HOW THE IDS WORK
----------------
The long ID strings like this:
  urn:oid:2.49.0.1.840.0.8a3f2c1b9e4d7a6f5c8b2e1d0a9f7c4b3e6d8a2f.001.1

Break down like this:
  2.49.0.1.840.0 = standard NWS prefix (always the same)
  8a3f2c1b... = unique code for this specific warning
  001 = sequence number
  1 = version number

The URL version points to the weather.gov API where you could look it up if it was real.


VTEC CODES
----------
The VTEC string like this:
  /O.NEW.KOUN.TO.W.0058.250520T1955Z-250520T2030Z/

Breaks down like:
  O = operational (real warning, not a test)
  NEW = new warning (could be CON for continue, CAN for cancel, etc)
  KOUN = the weather office (K + 3 letter code)
  TO = tornado (SV for severe thunderstorm)
  W = warning
  0058 = 58th tornado warning this office issued this year
  250520T1955Z = start time (2025 May 20 at 1955 UTC)
  250520T2030Z = end time


WHERE THIS DATA COMES FROM
--------------------------
These are sample files showing what the real data looks like. Here are the actual
live endpoints to get current data:


CONVECTIVE OUTLOOK ENDPOINTS (GeoJSON)
--------------------------------------
Day 1 Categorical:
  https://www.spc.noaa.gov/products/outlook/day1otlk_cat.lyr.geojson

Day 1 Tornado Probability:
  https://www.spc.noaa.gov/products/outlook/day1otlk_torn.lyr.geojson

Day 1 Wind Probability:
  https://www.spc.noaa.gov/products/outlook/day1otlk_wind.lyr.geojson

Day 1 Hail Probability:
  https://www.spc.noaa.gov/products/outlook/day1otlk_hail.lyr.geojson

Day 2 Categorical:
  https://www.spc.noaa.gov/products/outlook/day2otlk_cat.lyr.geojson

Day 2 Tornado Probability:
  https://www.spc.noaa.gov/products/outlook/day2otlk_torn.lyr.geojson

Day 3 Categorical:
  https://www.spc.noaa.gov/products/outlook/day3otlk_cat.lyr.geojson

Days 4-8 Probability:
  https://www.spc.noaa.gov/products/exper/day4-8/day48prob.lyr.geojson


MESOSCALE DISCUSSION ENDPOINTS
------------------------------
All Active MDs (GeoJSON):
  https://www.spc.noaa.gov/products/md/md.lyr.geojson

Individual MD (replace XXXX with number):
  https://www.spc.noaa.gov/products/md/mdXXXX.html (web page)

RSS Feed for new MDs:
  https://www.spc.noaa.gov/products/spcmdrss.xml


WATCH ENDPOINTS
---------------
All Active Watches (GeoJSON):
  https://www.spc.noaa.gov/products/watch/ww.lyr.geojson

Active Watch Summary:
  https://www.spc.noaa.gov/products/watch/

Individual Watch (replace XXXX with number):
  https://www.spc.noaa.gov/products/watch/wwXXXX.html

Watch Outline (KML - for mapping):
  https://www.spc.noaa.gov/products/watch/wwXXXX_outline.kml

Watch Status Reports:
  https://www.spc.noaa.gov/products/watch/wstatus.html


NWS ALERTS API (WARNINGS)
-------------------------
All Active Alerts:
  https://api.weather.gov/alerts/active

Active Tornado Warnings:
  https://api.weather.gov/alerts/active?event=Tornado%20Warning

Active Severe Thunderstorm Warnings:
  https://api.weather.gov/alerts/active?event=Severe%20Thunderstorm%20Warning

Alerts by State (replace XX with state code like TX, OK, KS):
  https://api.weather.gov/alerts/active?area=XX

Alerts by Zone:
  https://api.weather.gov/alerts/active?zone=OKC035

Alerts by Point (lat,lon):
  https://api.weather.gov/alerts/active?point=35.2,-97.4

Single Alert by ID:
  https://api.weather.gov/alerts/{id}


NOTES ON FETCHING DATA
----------------------
- SPC GeoJSON endpoints return standard GeoJSON FeatureCollections
- NWS API returns GeoJSON with extra metadata in the "properties" object
- NWS API requires a User-Agent header (use your app name and contact email)
- All times are in UTC (Zulu time) unless otherwise noted
- SPC updates outlooks at specific times: 0600Z, 1300Z, 1630Z, 2000Z, 0100Z
- Watches/warnings update in real-time as conditions change
