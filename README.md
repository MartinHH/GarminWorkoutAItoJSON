# 🏋️ Garmin Workout Generator

This tool lets you **generate Garmin-compatible workouts** (in JSON format) for **running 🏃**, **cycling 🚴**, and **swimming 🏊**, by simply pasting a structured text line. It's designed for easy batch creation and export of complex, multi-step workouts — including loops, targets, and equipment.

---

## 🚀 How It Works

1. Each workout is written as a **single line of text** using a specific syntax.
2. You paste your workouts into the text area.
3. The tool generates the proper Garmin workout files (`.json`) for download.
4. Use one of these extensions to import the workout files :

- https://chromewebstore.google.com/detail/odgdfpclpfmmemajpmgfipfdfmjgihac
- https://chromewebstore.google.com/detail/kdpolhnlnkengkmfncjdbfdehglepmff
- https://chromewebstore.google.com/detail/eifpkcoofcgpichllembbenjkfcgnfde


---

## ✍️ Workout Syntax

Each workout line follows this structure:

```
sport,name,description[,poolLength];step1;step2;repeat(n)[step3;step4];step5
```

- `sport`: `running`, `cycling`, or `swimming`
- `name`: name of the workout
- `description`: free text
- `poolLength` (optional): only for swimming (default = 25 meters)

### ✅ Supported Step Types:
- `warmup(condition,type,target)`
- `interval(condition,type,target)`
- `recovery(condition,type,target)`
- `cooldown(condition,type,target)`
- `main(condition,type,target)` -->intervals for swimming 
- `rest(condition,type,target)`
- `repeat(n)[ ... ]`

---

## ⏱️ Supported Conditions:
Each step must start with a **condition**:
- `time` → in seconds (e.g. `time,600`)
- `distance` → in meters (e.g. `distance,1000`)
- `lap` → user presses lap button
- `fixed` → for swimming rest intervals by "send-off time" (e.g.`fixed,80`)

---

## 🎯 Targets by Sport

### 🏃 Running — Supported Targets:
- `heart rate,zone` → e.g. `heart rate,3`
- `heart rate,min,max` → e.g. `heart rate,130,150`
- `pace,min,max` → in `min/km` (e.g. `pace,5.5,6.5`)
- `cadence,min,max` → e.g. `cadence,150,180`

### 🚴 Cycling — Supported Targets:
- `power,zone` → from 1 to 7 (e.g. `power,4`)
- `power,min,max` → e.g. `power,200,250`
- `heart rate,zone` → from 1 to 5
- `heart rate,min,max` → e.g. `heart rate,120,140`
- `cadence,min,max` → e.g. `cadence,85,100`
- `speed,min,max` → in km/h (e.g. `speed,20,28`)

### 🏊 Swimming — Supported Options:
Each `main()` step can include:

```
main(distance,length,targetZone,equipmentType,strokeType,drillType)
```

- `distance,length`: meters
- `targetZone`: `5` for swim instruction
- `equipmentType`:  
  - `fins`
  - `kickboard`
  - `paddles`
  - `pull_buoy`
- `strokeType`:  
  - `free`, `breaststroke`, `fly`, `backstroke`  
- `drillType` (optional): `kick`, `pull`, `drill`

Example:

```
main(distance,100,5,2,free,pull)
```

---

## 🔁 Loops with `repeat()`

You can wrap any sequence of steps in `repeat(n)[ ... ]`.

Example:

```
repeat(4)[interval(time,60,power,250,300);recovery(time,30)]
```

---

## 📦 Example Workouts


🏊 SWIMMING WORKOUT 
-------------------------------------------------------
1. Endurance Swim
-------------------------------------------------------
swimming,EnduranceBase,Long steady swim,25;warmup(time,300);main(distance,1000,5,0,free);cooldown(time,180)

Translation:
Swim workout named "EnduranceBase".
- Pool length: 25m
- Warm up for 5 minutes
- Swim 1000m freestyle
- Cool down for 3 minutes

-------------------------------------------------------
2. Drills with Equipment
-------------------------------------------------------
swimming,DrillsSet,Technique with equipment,25;warmup(time,180);main(distance,200,5,2,free);main(distance,200,5,4,free);cooldown(time,120)

Translation:
Workout "DrillsSet" to improve technique using equipment.
- 25m pool
- Warm up 3 minutes
- Swim 200m freestyle with kickboard
- Swim 200m freestyle with pull buoy
- Cool down 2 minutes

-------------------------------------------------------
3. Stroke Variety
-------------------------------------------------------
swimming,AllStrokes,Try all strokes,25;main(distance,100,5,0,breaststroke);main(distance,100,5,0,fly);main(distance,100,5,0,backstroke);main(distance,100,5,0,free)

Translation:
"AllStrokes" workout to practice all 4 main strokes.
- Swim 100m breaststroke
- Swim 100m butterfly
- Swim 100m backstroke
- Swim 100m freestyle

-------------------------------------------------------
4. Sprint Repeat Set
-------------------------------------------------------
swimming,SprintSet,Short sprints with rest,25;warmup(time,200);repeat(4)[main(distance,50,5,0,free);rest(time,20)];cooldown(time,150)

Translation:
Workout "SprintSet" for speed development.
- Warm up 3:20 minutes
- Repeat 4 times:
  - 50m freestyle sprint
  - 20 seconds rest
- Cool down 2.5 minutes

-------------------------------------------------------
5. Mixed Drill Set
-------------------------------------------------------
swimming,MixedDrills,Drill practice,25;warmup(time,150);main(distance,100,5,0,free,kick);main(distance,100,5,0,free,pull);main(distance,100,5,0,free,drill);cooldown(time,120)

Translation:
Workout "MixedDrills" to focus on different drills.
- Warm up 2.5 minutes
- 100m kicking drill (freestyle)
- 100m pulling drill (freestyle)
- 100m general drill (freestyle)
- Cool down 2 minutes


=======================================================
🏃 RUNNING WORKOUT EXAMPLES
=======================================================

-------------------------------------------------------
1. Easy Run with Heart Rate
-------------------------------------------------------
running,EasyHRRun,Base endurance heart rate,;warmup(time,300,1);main(time,1800,heart rate,2);cooldown(time,300,1)

Translation:
"EasyHRRun" - Run to develop aerobic base using heart rate.
- Warm up 5 min
- 30 min run in heart rate zone 2
- Cool down 5 min

-------------------------------------------------------
2. Custom Pace Run
-------------------------------------------------------
running,CustomPaceRun,Maintain specific pace,;warmup(time,300);main(time,2400,pace,5.2,5.5);cooldown(time,180)

Translation:
"CustomPaceRun" - Run with specific min/max pace.
- Warm up 5 min
- 40 min run between 5:12/km and 5:30/km pace
- Cool down 3 min

-------------------------------------------------------
3. Intervals Run with Repeats
-------------------------------------------------------
running,IntervalSet,VO2max intervals,;warmup(time,360);repeat(5)[main(time,300,pace,4.2,4.5);rest(time,120)];cooldown(time,240)

Translation:
"IntervalSet" - Intervals to work VO2max.
- Warm up 6 min
- Repeat 5x:
  - 5 min @ 4:12–4:30 pace/km
  - 2 min rest
- Cool down 4 min

-------------------------------------------------------
4. Run with Custom Heart Rate Range
-------------------------------------------------------
running,CustomHRRun,Target HR zone,;main(time,1800,heart rate,145,155)

Translation:
"CustomHRRun" - Run using BPM range (not zones).
- 30 min run keeping heart rate between 145–155 bpm

-------------------------------------------------------
5. Cadence Focus Run
-------------------------------------------------------
running,CadenceRun,Cadence training,;warmup(time,240);main(time,900,cadence,170,180);cooldown(time,180)

Translation:
"CadenceRun" - Focus on turnover speed.
- Warm up 4 min
- Run 15 min with cadence between 170–180 spm
- Cool down 3 min

=======================================================
🚴 CYCLING WORKOUT EXAMPLES
=======================================================

-------------------------------------------------------
1. FTP Steady Ride
-------------------------------------------------------
cycling,SteadyPowerRide,FTP control,;warmup(time,360);main(time,2400,power,220,230);cooldown(time,240)

Translation:
"SteadyPowerRide" - Ride at steady power zone.
- Warm up 6 min
- 40 min ride between 220–230 watts
- Cool down 4 min

-------------------------------------------------------
2. Power Zone Training
-------------------------------------------------------
cycling,PowerZoneSet,Structured zone ride,;warmup(time,300);main(time,900,power,2);main(time,600,power,3);main(time,300,power,4);cooldown(time,180)

Translation:
"PowerZoneSet" - Cycling through multiple power zones.
- Warm up 5 min
- 15 min @ power zone 2
- 10 min @ power zone 3
- 5 min @ power zone 4
- Cool down 3 min

-------------------------------------------------------
3. Sprint Intervals
-------------------------------------------------------
cycling,SprintIntervals,High-intensity set,;warmup(time,300);repeat(6)[main(time,30,power,350,450);rest(time,60)];cooldown(time,240)

Translation:
"SprintIntervals" - Sprint bursts with recovery.
- Warm up 5 min
- Repeat 6x:
  - 30 sec @ 350–450 watts
  - 1 min rest
- Cool down 4 min

-------------------------------------------------------
4. Cadence Drill Ride
-------------------------------------------------------
cycling,CadenceDrill,Cadence control,;warmup(time,300);main(time,1200,cadence,85,95);cooldown(time,180)

Translation:
"CadenceDrill" - Work on cadence range.
- Warm up 5 min
- 20 min ride between 85–95 rpm
- Cool down 3 min

-------------------------------------------------------
5. Power Zone (Zone 7)
-------------------------------------------------------
cycling,MaxPowerZone,Neuromuscular efforts,;warmup(time,300);main(time,60,power,7);cooldown(time,180)

Translation:
"MaxPowerZone" - Short high power effort in zone 7.
- Warm up 5 min
- 1 min at power zone 7
- Cool down 3 min
