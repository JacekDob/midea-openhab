# midea-openhab

Rough hack to connect a Midea (or compatible) aircon with Openhab.

WIP.

All credit to:
- https://github.com/yitsushi/midea-air-condition for reverse engineering the protocol
- and https://github.com/NeoAcheron/midea-ac-py for porting it to python
- and https://github.com/bricky/midea-openhab for integration with Openhab

# Configuration

## Settings

- Copy file
```
cp settings.sample.py settings.py
```

- Set parameters
```
APPKEY = '3742e9e5842d4ad59c2db887e12449f9'  # from android application or any other working
EMAIL = ''                                   # your registered email
PASSWORD = ''                                # email to account
AIRCONS = ('Name1', 'Name2')                 # tuple of aircons you want to sync as configured names in App/Cloud
OH_URL = 'http://192.168.1.2:8080'           # url of your openhab server

MIDEA_POLL_FREQ_SECS = 900
OPENHAB_POLL_FREQ_SECS = 15
```

## Openhab

For `Name1` (as set above) as below, for `Name2` and other just rename `Name1` with `Name2` accordingly.

### Items

```
Switch  ac_Name1_online                     "Online [%s]"                   <switch>                (gPersChng)
Switch  ac_Name1_power_state                "Power state [%s]"              <switch>                (gPersChng)
Number  ac_Name1_operational_mode           "Mode []"                       <pump>                  (gPersChng)
Switch  ac_Name1_active                     "Active [%s]"                   <switch>                (gPersChng)
Number  ac_Name1_target_temperature         "Target temperature [%d°C]"     <temperature_cold>      (gPersChng)
Number  ac_Name1_indoor_temperature         "Indoor temperature [%d°C]"     <temperature>           (gPersChng)
Number  ac_Name1_outdoor_temperature        "Outdoor temperature [%d°C]"    <temperature>           (gPersChng)
Number  ac_Name1_fan_speed                  "Fan speed []"                  <fan>                   (gPersChng)
Number  ac_Name1_swing_mode                 "Swing mode []"                 <flow>                  (gPersChng)
Switch  ac_Name1_eco_mode                   "Eco mode [%s]"                 <switch>                (gPersChng)
Switch  ac_Name1_turbo_mode                 "Turbo mode [%s]"               <switch>                (gPersChng)
```

### Sitemap

```
Text            item=ac_Name1_online
Switch          item=ac_Name1_power_state                   mappings=[OFF="OFF",ON="ON"]
Switch          item=ac_Name1_operational_mode              mappings=[1.0="Auto",2.0="Cool",3.0="Dry",4.0="Heat",5.0="Fan"]
Switch          item=ac_Name1_active
Setpoint        item=ac_Name1_target_temperature            minValue=17 maxValue=30
Text            item=ac_Name1_indoor_temperature
Text            item=ac_Name1_outdoor_temperature
Switch          item=ac_Name1_fan_speed                     mappings=[20.0="Silent",40.0="Low",60.0="Medium",80.0="High",102.0="Auto"]
Switch          item=ac_Name1_swing_mode                    mappings=[0.0="Off",12.0="Vertical",3.0="Horizontal",15.0="Both"]
Switch          item=ac_Name1_eco_mode
Switch          item=ac_Name1_turbo_mode
```

# Running

```
python3 main.py
```
