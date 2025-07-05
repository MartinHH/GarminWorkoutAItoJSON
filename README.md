# GarminAiWorkout
Use ChatGPT to create sport workout plan and import JSON File to Garmin Connect as Workout


Prompt to give to ChatGPT
I want you to create a running workout for me using the following compact format:

Workout_Title,Description;step1;step2;...;repeat(n)[stepA;stepB;...];etc

- The title of the session followed by the description (separated by a comma).
- Each workout step is separated by a semicolon `;`.
- Possible steps are:
    - warmup(lap,zone): warm-up, ends when pressing the lap button
    - warmup(time,seconds,zone): warm-up for a specific duration (in seconds)
    - interval(time,seconds,zone) or interval(distance,meters,zone): work interval
    - recovery(time,seconds,zone) or recovery(lap,zone): recovery
    - cooldown(...): cool-down
- The `zone` parameter is a number (1 to 5) for heart rate zone; use 0 or leave empty if no target.
- To repeat a block of steps, use: repeat(n)[stepA;stepB] to repeat the inner steps n times.

Just return the line to copy/paste, NO extra explanation or formatting—only the workout in the requested format.

Expected example:

10 x 400 Threshold Repeat,Threshold interval session;warmup(time,600,1);repeat(10)[interval(distance,400,4);recovery(time,60,1)];cooldown(time,600,1)

Once the JSON file download, you can upload it on garmin connect with this tools :

https://chromewebstore.google.com/detail/odgdfpclpfmmemajpmgfipfdfmjgihac
https://chromewebstore.google.com/detail/kdpolhnlnkengkmfncjdbfdehglepmff
https://chromewebstore.google.com/detail/eifpkcoofcgpichllembbenjkfcgnfde
