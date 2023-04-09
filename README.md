# Nightscout xDrip+ with changes for Freestyle Libre 3 minute values 

This is a slightly modified XDrip+ version based on the official <a href="https://github.com/nightscoutfoundation/xdrip/">Nightscout xDrip+ master repo</a>.



## What does it do? - the problem!

With <a href="http://jkaltes.byethost16.com/Juggluco/mgdL/index.html?i=1">Juggluco</a> it is possible to use and start Freestyle Libre 3 sensors and broadcast the minute values to <a href="https://github.com/nightscout/AndroidAPS">AndroidAPS</a> and / or Xdrip+.

One of the big drawbacks of the FL3 sensor is the fact that you can not officially calibrate sensor values. To do that, you must broadcast values to Xdrip+. XDrip+ itself can forward the calibrated value to AndroidAPS for closed looping with minute values!

Some Freestyle Libre 3 sensors send their minute glucose values not every minute (60s), but send them at slightly different times. (58s, 59s, or 61s, 62s). There is a sanity check in Xdrip+ that prevents broadcasting values that are below a certain threshold - in this case 60s.

### This can lead to AndroidAPS not getting minute values from Xdrip. 

<img src="https://insulinclub.de/core/index.php?attachment/20160-05df8019-445d-438e-8236-6e9b1c7b348b-jpeg/">


## What was changed here?

The following sanity check was simply commented out. Thats it.

```
if (Math.abs(bgReading.timestamp - lastTimestamp) < MINUTE_IN_MS) {
                    val msg = String.format("Refusing to broadcast a reading with close timestamp to last broadcast:  %s (%d) vs %s (%d) ", dateTimeText(lastTimestamp), lastTimestamp, dateTimeText(bgReading.timestamp), bgReading.timestamp);
                    if (bgReading.timestamp == lastTimestamp) {
                        UserError.Log.d(TAG, msg);
                    } else {
                        UserError.Log.wtf(TAG, msg);
                    }
                    return;
                }
```

