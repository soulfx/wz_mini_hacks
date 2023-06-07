waitMotion related notes
========================

The following is documented behavior of waitMotion as observed on the following hardware
* v3

Motion Events
-------------

When running waitMotion it will output one of the following

* `detect x_start x_end y_start y_end - -` When motion is detected it will output the coordinates
  of the upper left and lower left corner of the motion box. For example: `detect 0 126 64 200 - -`
    * `x_start` and `x_end` values are in the range of 0 - 639
    * `y_start` and `y_end` values are in the range of 0 - 359
* `clear` Is output when motion in previously detected areas have stopped
* `timeout` Whenver no motion has been detected within the time period set when running waitMotion
 
A crash and stack trace can be output sometimes when first starting waitMotion up prior to
any motion being detected.  Once motion is detected, the app will function as expected. Crashes can also
occur if multiple waitMotions are being run at the same time.  One app will recieve the motion events, while
the other crashes.

Motion Field
------------

The detect results indicate the location of the motion within the motion field.  The upper left corner is (0,0)
and the lower right corner is (639,359).

```
 (0,0)                            (x,y)
   ┌──────────────────────────┐
   │                          │
  (y)        16 x 9           │
   │                          │
   └────────────(x)───────────┘
                           (639,359)
```

Motion Configuration
--------------------

Areas where motion should be detected and the sensetivity of the detection can be configurated either via the wyze app
or by setting values in the `.user_config` file.  The events output will honor the configured motion sensetivity
and detection zone.

The following is a description of the settings within the `.user_config` file that can control the motion config
by defining the motion sensetivity and the box of the motion detection zone.

| [SETTING] | Description |
|-----------|-------------|
| MMALevel  | Sets the motion sensetivity.  Range is from 2 (Low) to 254 (High).  For example `MMALevel=127` would be 50% motion sensetivity. |
| MAT       | Toggles the detection zone box. 1 for on, 0 for off. |
| AASX      | Sets the starting X position for the detection zone box. Range is 0 to 100 |
| AALX      | Sets the length of the X portion of the detection zone box. Range is 99 - AASX |
| AASY      | Sets the starting Y position for the detection zone box. Range is 0 to 100 |
| AALY      | Sets the length of the Y portion of the detection zone box. Range is 99 - AALY |

```
 (0,0)                           (x,y)
   ┌──────────────────────────┐
   │                          │
  (y)         16 x 9          │
   │                          │
   └────────────(x)───────────┘
                           (100,100)
```

The detection zone as defined by the AA** settings is a single box located within the motion field.
Although the app appears to allow for selection of multiple zones and non box like areas of detection, it really is just helping
to define the bounds of the single detection box.






