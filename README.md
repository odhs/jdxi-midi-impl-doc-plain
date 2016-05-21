      
# MIDI Implementation

<pre>
Model:   JD-Xi
Date:    May 1, 2015
Version: 1.00
</pre>

## 1. Data Reception (Sound Source Section)

### Channel Voice Messages

#### Note off

<pre>
Status  2nd byte 3rd byte
------  -------- --------
8nH     kkH      vvH
9nH     kkH      00H

n  = MIDI channel number:  0H  - FH  (ch.1 - 16)
kk = note number:          00H - 7FH (0 - 127)
vv = note off velocity:    00H - 7FH (0 - 127)
</pre>

### Note on

<pre>
Status  2nd byte 3rd byte
------  -------- --------
9nH     kkH      vvH

n = MIDI channel number:  0H  - FH  (ch.1 - 16)
kk = note number:         00H - 7FH (0 - 127)
vv = note on velocity:    01H - 7FH (1 - 127)
</pre>

### Polyphonic Key Pressure

<pre>
Status  2nd byte 3rd byte
------  -------- --------
AnH     kkH      vvH

n = MIDI channel number:      0H - FH (ch.1 - 16)
kk = note number:             00H - 7FH (0 - 127)
vv = Polyphonic Key Pressure: 00H - 7FH (0 - 127)

(*) Not received when the Receive Polyphonic Key Pressure parameter (SysEx) is OFF.
</pre>

### Control Change

#### Bank Select (Controller number 0, 32)

<pre>
Status  2nd byte 3rd byte
------  -------- --------
BnH     00H      mmH
BnH     20H      llH

n = MIDI channel number:  0H - FH (ch.1 - 16)
mm, ll = Bank number:     00 00H - 7F 7FH (bank.1 - bank.16384)

(*) Not received when the Receive Bank Select parameter (SysEx) is OFF.
</pre>

The Programs corresponding to each Bank Select are as follows.

<pre>
BANK SELECT      | PROGRAM   | GROUP                      | NUMBER
MSB  | LSB       | NUMBER    |                            |
-----+-----------+-----------+----------------------------+-----------
085  | 000       | 001 - 064 | User Bank Program (E)      | E01 - E64
085  | 000       | 065 - 128 | User Bank Program (F)      | F01 - F64
085  | 001       | 001 - 064 | User Bank Program (G)      | G01 - G64
085  | 001       | 065 - 128 | User Bank Program (H)      | H01 - H64
-----+-----------+-----------+----------------------------+-----------
085  | 064       | 001 - 064 | Preset Bank Program (A)    | A01 - A64
085  | 064       | 065 - 128 | Preset Bank Program (B)    | B01 - B64
085  | 065       | 001 - 064 | Preset Bank Program (C)    | C01 - C64
085  | 065       | 065 - 128 | Preset Bank Program (D)    | D01 - D64
-----+-----------+-----------+----------------------------+-----------
085  | 096       | 001 - 064 | Extra Bank Program (S)     | S01 - S64
     | :         | :         | :                          | :
085  | 103       | 001 - 064 | Extra Bank Program (Z)     | Z01 - Z64
</pre>

======================================================================

## 5. Parameter Address Map

* Transmission of “#” marked address is divided to some packets. For
example, ABH in hexadecimal notation will be divided to 0AH and 0BH,
and is sent/received in this order.

* “<*>” marked address or parameters are ignored when the JD-Xi
received them.

JD-Xi (ModelID = 00H 00H 00H 0EH)

<pre>
+------------------------------------------------------------------------------+
| Start       |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 01 00 00 00 | Setup                                                          |
|-------------+----------------------------------------------------------------|
| 02 00 00 00 | System                                                         |
|-------------+----------------------------------------------------------------|
| 18 00 00 00 | Temporary Program                                              |
| 19 00 00 00 | Temporary Tone (Digital Synth Part 1)                          |
| 19 20 00 00 | Temporary Tone (Digital Synth Part 2)                          |
| 19 40 00 00 | Temporary Tone (Analog Synth Part)                             |
| 19 60 00 00 | Temporary Tone (Drums Part)                                    |
+------------------------------------------------------------------------------+

* System

+------------------------------------------------------------------------------+
| Offset   |                                                                   |
| Address  | Description                                                       |
|----------+-------------------------------------------------------------------|
| 00 00 00 | System Common                                                     |
| 00 03 00 | System Controller                                                 |
+------------------------------------------------------------------------------+

* Temporary Tone

+------------------------------------------------------------------------------+
| Offset   |                                                                   |
| Address  | Description                                                       |
|----------+-------------------------------------------------------------------|
| 01 00 00 | Temporary SuperNATURAL Synth Tone                                 |
| 02 00 00 | Temporary Analog Synth Tone                                       |
| 10 00 00 | Temporary Drum Kit                                                |
+------------------------------------------------------------------------------+

* Program

+------------------------------------------------------------------------------+
| Offset   |                                                                   |
| Address  | Description                                                       |
|-------------+----------------------------------------------------------------|
| 00 00 00 | Program Common                                                    |
| 00 01 00 | Program Vocal Effect                                              |
| 00 02 00 | Program Effect 1                                                  |
| 00 04 00 | Program Effect 2                                                  |
| 00 06 00 | Program Delay                                                     |
| 00 08 00 | Program Reverb                                                    |
| 00 20 00 | Program Part (Digital Synth Part 1)                               |
| 00 21 00 | Program Part (Digital Synth Part 2)                               |
| 00 22 00 | Program Part (Analog Synth Part)                                  |
| 00 23 00 | Program Part (Drums Part)                                         |
| 00 30 00 | Program Zone (Digital Synth Part 1)                               |
| 00 31 00 | Program Zone (Digital Synth Part 2)                               |
| 00 32 00 | Program Zone (Analog Synth Part)                                  |
| 00 33 00 | Program Zone (Drums Part)                                         |
| 00 40 00 | Program Controller                                                |
+------------------------------------------------------------------------------+

* SuperNATURAL Synth Tone

+------------------------------------------------------------------------------+
| Offset   |                                                                   |
| Address  | Description                                                       |
|-------------+----------------------------------------------------------------|
| 00 00 00 | SuperNATURAL Synth Tone Common                                    |
| 00 20 00 | SuperNATURAL Synth Tone Partial (1)                               |
| 00 21 00 | SuperNATURAL Synth Tone Partial (2)                               |
| 00 22 00 | SuperNATURAL Synth Tone Partial (3)                               |
| 00 50 00 | SuperNATURAL Synth Tone Modify                                    |
+------------------------------------------------------------------------------+

* Analog Synth Tone

+------------------------------------------------------------------------------+
| Offset   |                                                                   |
| Address  | Description                                                       |
|-------------+----------------------------------------------------------------|
| 00 00 00 | Analog Synth Tone                                                 |
+------------------------------------------------------------------------------+

* Drum Kit

+------------------------------------------------------------------------------+
| Offset   |                                                                   |
| Address  | Description                                                       |
|----------+-------------------------------------------------------------------|
| 00 00 00 | Drum Kit Common                                                   |
| 00 2E 00 | Drum Kit Partial (Key # 36)                                       |
| 00 30 00 | Drum Kit Partial (Key # 37)                                       |
| :        |                                                                   |
| 00 76 00 | Drum Kit Partial (Key # 72)                                       |
+------------------------------------------------------------------------------+

* Setup

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0000 000a | (reserve) <*>                                      |
| 00 01       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 03       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 04       | 0aaa aaaa | Program BS MSB (CC# 0)                   (0 - 127) |
| 00 05       | 0aaa aaaa | Program BS LSB (CC# 32)                  (0 - 127) |
| 00 06       | 0aaa aaaa | Program PC (PC)                          (0 - 127) |
|-------------+-----------+----------------------------------------------------|
| 00 07       | 0aaa aaaa | (reserve) <*>                                      |
| 00 08       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 3A       | 00aa aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 3B | Total Size                                                     |
+------------------------------------------------------------------------------+

* System Common

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
|# 00 00      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | Master Tune                            (24 - 2024) |
|             |           |                              -100.0 - 100.0 [cent] |
| 00 04       | 00aa aaaa | Master Key Shift                         (40 - 88) |
|             |           |                                          -24 - +24 |
| 00 05       | 0aaa aaaa | Master Level                             (0 - 127) |
| 00 06       | 0000 000a | (reserve) <*>                                      |
| 00 07       | 0000 000a | (reserve) <*>                                      |
| 00 08       | 0000 000a | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 09       | 000a aaaa | (reserve) <*>                                      |
| 00 0A       | 000a aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 10       | 000a aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 11       | 000a aaaa | Program Control Channel                   (0 - 16) |
|             |           |                                        1 - 16, OFF |
|-------------+-----------+----------------------------------------------------|
| 00 12       | 0aaa aaaa | (reserve) <*>                                      |
| 00 13       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 28       | 0000 000a | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 29       | 0000 000a | Receive Program Change                     (0 - 1) |
|             |           |                                            OFF, ON |
| 00 2A       | 0000 000a | Receive Bank Select                        (0 - 1) |
|             |           |                                            OFF, ON |
|-------------+----------------------------------------------------------------|
| 00 00 00 2B | Total Size                                                     |
+------------------------------------------------------------------------------+

* System Controller

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0000 000a | Transmit Program Change                    (0 - 1) |
|             |           |                                            OFF, ON |
| 00 01       | 0000 000a | Transmit Bank Select                       (0 - 1) |
|             |           |                                            OFF, ON |
| 00 02       | 0aaa aaaa | Keyboard Velocity                        (0 - 127) |
|             |           |                                      REAL, 1 - 127 |
| 00 03       | 0000 00aa | Keyboard Velocity Curve                    (1 - 3) |
|             |           |                               LIGHT, MEDIUM, HEAVY |
| 00 04       | 000a aaaa | Keyboard Velocity Curve Offset           (54 - 73) |
|             |           |                                           -10 - +9 |
|-------------+-----------+----------------------------------------------------|
| 00 05       | 0000 0aaa | (reserve) <*>                                      |
| 00 06       | 0000 000a | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 10       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 11 | Total Size                                                     |
+------------------------------------------------------------------------------+

* Program Common

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | Program Name 1                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 01       | 0aaa aaaa | Program Name 2                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 02       | 0aaa aaaa | Program Name 3                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 03       | 0aaa aaaa | Program Name 4                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 04       | 0aaa aaaa | Program Name 5                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 05       | 0aaa aaaa | Program Name 6                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 06       | 0aaa aaaa | Program Name 7                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 07       | 0aaa aaaa | Program Name 8                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 08       | 0aaa aaaa | Program Name 9                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 09       | 0aaa aaaa | Program Name 10                         (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0A       | 0aaa aaaa | Program Name 11                         (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0B       | 0aaa aaaa | Program Name 12                         (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
|-------------+-----------+----------------------------------------------------|
| 00 0C       | 0aaa aaaa | (reserve) <*>                                      |
| 00 0D       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 0F       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
|  00 10      | 0aaa aaaa | Program Level                            (0 - 127) |
|# 00 11      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | Program Tempo                        (500 - 30000) |
|             |           |                                      5.00 - 300.00 |
| 00 15       | 0000 aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 16       | 0000 aaaa | Vocal Effect                               (0 - 2) |
|             |           |                           OFF, VOCODER, AUTO-PITCH |
|-------------+-----------+----------------------------------------------------|
| 00 17       | 0000 000a | (reserve) <*>                                      |
| 00 18       | 0000 000a | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 1A       | 0000 000a | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 1B       | 0000 00aa | (reserve) <*>                                      |
| 00 1C       | 0aaa aaaa | Vocal Effect Number                       (0 - 20) |
|             |           |                                             1 - 21 |
| 00 1D       | 0000 aaaa | Vocal Effect Part                          (0 - 1) |
|             |           |                                              1 - 2 |
|-------------+-----------+----------------------------------------------------|
| 00 1E       | 0000 000a | Auto Note Switch                           (0 - 1) |
|             |           |                                            OFF, ON |
|-------------+----------------------------------------------------------------|
| 00 00 00 1F | Total Size                                                     |
+------------------------------------------------------------------------------+

* Program Vocal Effect

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | Level                                    (0 - 127) |
| 00 01       | 0aaa aaaa | Pan                                      (0 - 127) |
|             |           |                                          L64 - 63R |
| 00 02       | 0aaa aaaa | Delay Send Level                         (0 - 127) |
| 00 03       | 0aaa aaaa | Reverb Send Level                        (0 - 127) |
| 00 04       | 0000 0aaa | Output Assign                              (0 - 4) |
|             |           |                          EFX1, EFX2, DLY, REV, DIR |
| 00 05       | 0000 000a | Auto Pitch Switch                          (0 - 1) |
|             |           |                                            OFF, ON |
| 00 06       | 0000 0aaa | Auto Pitch Type                            (0 - 3) |
|             |           |                   SOFT, HARD, ELECTRIC1, ELECTRIC2 |
| 00 07       | 0000 000a | Auto Pitch Scale                           (0 - 1) |
|             |           |                                CHROMATIC, Maj(Min) |
| 00 08       | 000a aaaa | Auto Pitch Key                            (0 - 23) |
|             |           |                         C, Db, D, Eb, E, F, F#, G, |
|             |           |                    Ab, A, Bb, B, Cm, C#m, Dm, D#m, |
|             |           |                  Em, Fm, F#m, Gm, G#m, Am, Bbm, Bm |
| 00 09       | 0000 aaaa | Auto Pitch Note                           (0 - 11) |
|             |           |                                C, C#, D, D#, E, F, |
|             |           |                                F#, G, G#, A, A#, B |
| 00 0A       | 000a aaaa | Auto Pitch Gender                         (0 - 20) |
|             |           |                                          -10 - +10 |
| 00 0B       | 0000 00aa | Auto Pitch Octave                          (0 - 2) |
|             |           |                                            -1 - +1 |
| 00 0C       | 0aaa aaaa | Auto Pitch Balance                       (0 - 100) |
|             |           |                                  D100:0W - D0:100W |
|-------------+-----------+----------------------------------------------------|
| 00 0D       | 0000 000a | Vocoder Switch                             (0 - 1) |
|             |           |                                            OFF, ON |
| 00 0E       | 0000 00aa | Vocoder Envelope                           (0 - 2) |
|             |           |                                  SHARP, SOFT, LONG |
| 00 0F       | 0aaa aaaa |                                          (0 - 127) |
| 00 10       | 0aaa aaaa | Vocoder Mic Sens                         (0 - 127) |
| 00 11       | 0aaa aaaa | Vocoder Synth Level                      (0 - 127) |
| 00 12       | 0aaa aaaa | Vocoder Mic Mix Level                    (0 - 127) |
| 00 13       | 0000 aaaa | Vocoder Mic HPF                           (0 - 13) |
|             |           |                                            BYPASS, |
|             |           |                      1000, 1250, 1600, 2000, 2500, |
|             |           |                      3150, 4000, 5000, 6300, 8000, |
|             |           |                           10000, 12500, 16000 [Hz] |
| 00 14       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 15       | 0000 000a | (reserve) <*>                                      |
| 00 16       | 0aaa aaaa | (reserve) <*>                                      |
| 00 17       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 18 | Total Size                                                     |
+------------------------------------------------------------------------------+

* Program Effect 1

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | EFX1 Type                                  (0 - 4) |
| 00 01       | 0aaa aaaa | EFX1 Level                               (0 - 127) |
| 00 02       | 0aaa aaaa | EFX1 Delay Send Level                    (0 - 127) |
| 00 03       | 0aaa aaaa | EFX1 Reverb Send Level                   (0 - 127) |
| 00 04       | 0000 00aa | EFX1 Output Assign                         (0 - 1) |
|             |           |                                          DIR, EFX2 |
|-------------+-----------+----------------------------------------------------|
| 00 05       | 0aaa aaaa | (reserve) <*>                                      |
| 00 06       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 10       | 000a aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
|# 00 11      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | EFX1 Parameter 1                   (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
|# 00 15      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | EFX1 Parameter 2                   (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
| :           |           |                                                    |
|# 01 0D      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | EFX1 Parameter 32                  (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
|-------------+----------------------------------------------------------------|
| 00 00 01 11 | Total Size                                                     |
+------------------------------------------------------------------------------+

* Program Effect 2

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | EFX2 Type                               (0, 5 - 8) |
| 00 01       | 0aaa aaaa | EFX2 Level                               (0 - 127) |
| 00 02       | 0aaa aaaa | EFX2 Delay Send Level                    (0 - 127) |
| 00 03       | 0aaa aaaa | EFX2 Reverb Send Level                   (0 - 127) |
| 00 04       | 0000 00aa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 05       | 0aaa aaaa | (reserve) <*>                                      |
| 00 06       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 10       | 000a aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
|# 00 11      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | EFX2 Parameter 1                   (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
|# 00 15      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | EFX2 Parameter 2                   (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
| :           |           |                                                    |
|# 01 0D      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | EFX2 Parameter 32                  (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
|-------------+----------------------------------------------------------------|
| 00 00 01 11 | Total Size                                                     |
+------------------------------------------------------------------------------+

* Program Delay

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0000 aaaa | (reserve) <*>                                      |
| 00 01       | 0aaa aaaa | Delay Level                              (0 - 127) |
| 00 02       | 0000 00aa | (reserve) <*>                                      |
| 00 03       | 0aaa aaaa | Delay Reverb Send Level                  (0 - 127) |
|-------------+-----------+----------------------------------------------------|
|# 00 04      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | Delay Parameter 1                  (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
|# 00 08      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | Delay Parameter 2                  (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
| :           |           |                                                    |
|# 00 60      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | Delay Parameter 24                 (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
|-------------+----------------------------------------------------------------|
| 00 00 00 64 | Total Size                                                     |
+------------------------------------------------------------------------------+

* Program Reverb

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0000 aaaa | (reserve) <*>                                      |
| 00 01       | 0aaa aaaa | Reverb Level                             (0 - 127) |
| 00 02       | 0000 00aa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
|# 00 03      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | Reverb Parameter 1                 (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
|# 00 07      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | Reverb Parameter 2                 (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
| :           |           |                                                    |
|# 00 5F      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | Reverb Parameter 24                (12768 - 52768) |
|             |           |                                    -20000 - +20000 |
|-------------+----------------------------------------------------------------|
| 00 00 00 63 | Total Size                                                     |
+------------------------------------------------------------------------------+

* Program Part

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0000 aaaa | Receive Channel                           (0 - 15) |
|             |           |                                             1 - 16 |
| 00 01       | 0000 000a | Part Switch                                (0 - 1) |
|             |           |                                            OFF, ON |
| 00 02       | 0000 000a | (reserve)                                      (1) |
| 00 03       | 0000 000a | (reserve)                                      (1) |
| 00 04       | 0000 000a | (reserve)                                      (1) |
| 00 05       | 0000 000a | (reserve)                                      (1) |
|-------------+-----------+----------------------------------------------------|
| 00 06       | 0aaa aaaa | Tone Bank Select MSB (CC# 0)             (0 - 127) |
| 00 07       | 0aaa aaaa | Tone Bank Select LSB (CC# 32)            (0 - 127) |
| 00 08       | 0aaa aaaa | Tone Program Number (PC)                 (0 - 127) |
|-------------+-----------+----------------------------------------------------|
| 00 09       | 0aaa aaaa | Part Level (CC# 7)                       (0 - 127) |
| 00 0A       | 0aaa aaaa | Part Pan (CC# 10)                        (0 - 127) |
|             |           |                                          L64 - 63R |
| 00 0B       | 0aaa aaaa | Part Coarse Tune (RPN# 2)               (16 - 112) |
|             |           |                                          -48 - +48 |
| 00 0C       | 0aaa aaaa | Part Fine Tune (RPN# 1)                 (14 - 114) |
|             |           |                                          -50 - +50 |
| 00 0D       | 0000 00aa | Part Mono/Poly           (MONO ON/POLY ON) (0 - 2) |
|             |           |                                   MONO, POLY, TONE |
| 00 0E       | 0000 00aa | Part Legato Switch                (CC# 68) (0 - 2) |
|             |           |                                      OFF, ON, TONE |
| 00 0F       | 000a aaaa | Part Pitch Bend Range (RPN# 0)            (0 - 25) |
|             |           |                                       0 - 24, TONE |
| 00 10       | 0000 00aa | Part Portamento Switch (CC# 65)            (0 - 2) |
|             |           |                                      OFF, ON, TONE |
|# 00 11      | 0000 aaaa |                                                    |
|             | 0000 bbbb | Part Portamento Time                     (0 - 128) |
|             |           |                                      0 - 127, TONE |
| 00 13       | 0aaa aaaa | Part Cutoff Offset (CC# 74)              (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 14       | 0aaa aaaa | Part Resonance Offset (CC# 71)           (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 15       | 0aaa aaaa | Part Attack Time Offset (CC# 73)         (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 16       | 0aaa aaaa | Part Decay Time Offset (CC# 75)          (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 17       | 0aaa aaaa | Part Release Time Offset (CC# 72)        (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 18       | 0aaa aaaa | Part Vibrato Rate (CC# 76)               (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 19       | 0aaa aaaa | Part Vibrato Depth (CC# 77)              (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 1A       | 0aaa aaaa | Part Vibrato Delay (CC# 78)              (0 - 127) |
|             |           |                                          -64 - +63 |
|-------------+-----------+----------------------------------------------------|
| 00 1B       | 0000 0aaa | Part Octave Shift                        (61 - 67) |
|             |           |                                            -3 - +3 |
| 00 1C       | 0aaa aaaa | Part Velocity Sens Offset                (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 1D       | 0000 0000 | (reserve) <*>                                      |
| 00 1E       | 0aaa aaaa | (reserve) <*>                                      |
| 00 1F       | 0000 0000 | (reserve) <*>                                      |
| 00 20       | 0000 0000 | (reserve) <*>                                      |
| 00 21       | 0aaa aaaa | Velocity Range Lower                     (1 - 127) |
|             |           |                                          1 - UPPER |
| 00 22       | 0aaa aaaa | Velocity Range Upper                     (0 - 127) |
|             |           |                                        LOWER - 127 |
| 00 23       | 0aaa aaaa | Velocity Fade Width Lower                (0 - 127) |
| 00 24       | 0aaa aaaa | Velocity Fade Width Upper                (0 - 127) |
| 00 25       | 0000 000a | Mute Switch                                (0 - 1) |
|             |           |                                          OFF, MUTE |
|-------------+-----------+----------------------------------------------------|
| 00 26       | 0aaa aaaa | (reserve) <*>                                      |
| 00 27       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 29       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 2A       | 0aaa aaaa | (reserve) <*>                                      |
| 00 2B       | 0aaa aaaa | Part Delay Send Level (CC# 94)           (0 - 127) |
| 00 2C       | 0aaa aaaa | Part Reverb Send Level (CC# 91)          (0 - 127) |
| 00 2D       | 0000 0aaa | Part Output Assign                         (0 - 4) |
|             |           |                          EFX1, EFX2, DLY, REV, DIR |
| 00 2E       | 0000 000a | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 2F       | 0aaa aaaa | Part Scale Tune Type                       (0 - 8) |
|             |           |                 CUSTOM, EQUAL, JUST-MAJ, JUST-MIN, |
|             |           |                    PYTHAGORE, KIRNBERGE, MEANTONE, |
|             |           |                                  WERCKMEIS, ARABIC |
| 00 30       | 0aaa aaaa | Part Scale Tune Key                       (0 - 11) |
|             |           |                     C, C#, D, D#, E, F, F#, G, G#, |
|             |           |                                           A, A#, B |
| 00 31       | 0aaa aaaa | Part Scale Tune for C                    (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 32       | 0aaa aaaa | Part Scale Tune for C#                   (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 33       | 0aaa aaaa | Part Scale Tune for D                    (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 34       | 0aaa aaaa | Part Scale Tune for D#                   (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 35       | 0aaa aaaa | Part Scale Tune for E                    (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 36       | 0aaa aaaa | Part Scale Tune for F                    (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 37       | 0aaa aaaa | Part Scale Tune for F#                   (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 38       | 0aaa aaaa | Part Scale Tune for G                    (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 39       | 0aaa aaaa | Part Scale Tune for G#                   (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 3A       | 0aaa aaaa | Part Scale Tune for A                    (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 3B       | 0aaa aaaa | Part Scale Tune for A#                   (0 - 127) |
|             |           |                                          -64 - +63 |
| 00 3C       | 0aaa aaaa | Part Scale Tune for B                    (0 - 127) |
|             |           |                                          -64 - +63 |
|-------------+-----------+----------------------------------------------------|
| 00 3D       | 0000 000a | Receive Program Change                     (0 - 1) |
|             |           |                                            OFF, ON |
| 00 3E       | 0000 000a | Receive Bank Select                        (0 - 1) |
|             |           |                                            OFF, ON |
| 00 3F       | 0000 000a | Receive Pitch Bend                         (0 - 1) |
|             |           |                                            OFF, ON |
| 00 40       | 0000 000a | Receive Polyphonic Key Pressure            (0 - 1) |
|             |           |                                            OFF, ON |
| 00 41       | 0000 000a | Receive Channel Pressure                   (0 - 1) |
|             |           |                                            OFF, ON |
| 00 42       | 0000 000a | Receive Modulation                         (0 - 1) |
|             |           |                                            OFF, ON |
| 00 43       | 0000 000a | Receive Volume                             (0 - 1) |
|             |           |                                            OFF, ON |
| 00 44       | 0000 000a | Receive Pan                                (0 - 1) |
|             |           |                                            OFF, ON |
| 00 45       | 0000 000a | Receive Expression                         (0 - 1) |
|             |           |                                            OFF, ON |
| 00 46       | 0000 000a | Receive Hold-1                             (0 - 1) |
|             |           |                                            OFF, ON |
|-------------+-----------+----------------------------------------------------|
| 00 47       | 0000 0aaa | (reserve)                                      (0) |
|-------------+-----------+----------------------------------------------------|
| 00 48       | 0aaa aaaa | (reserve) <*>                                      |
| 00 49       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 4B       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 4C | Total Size                                                     |
+------------------------------------------------------------------------------+

* Program Zone

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | (reserve) <*>                                      |
| 00 01       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 02       | 0000 000a | (reserve) <*>                                      |
| 00 03       | 0000 000a | Arpeggio Switch                            (0 - 1) |
|             |           |                                            OFF, ON |
| 00 04       | 0000 000a | (reserve) <*>                                      |
| 00 05       | 0000 000a | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 0D       | 0000 000a | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
|# 00 0E      | 0000 aaaa |                                                    |
|             | 0000 bbbb | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 18       | 0aaa aaaa | (reserve) <*>                                      |
| 00 19       | 0000 0aaa | Zone Octave Shift                        (61 - 67) |
|             |           |                                            -3 - +3 |
| 00 1A       | 0000 aaaa | (reserve) <*>                                      |
| 00 1B       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 22       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 23 | Total Size                                                     |
+------------------------------------------------------------------------------+

* Program Controller

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0000 000a | (reserve) <*>                                      |
| 00 01       | 0aaa aaaa | Arpeggio Grid                              (0 - 8) |
|             |           |                           04_, 08_, 08L, 08H, 08t, |
|             |           |                                 16_, 16L, 16H, 16t |
| 00 02       | 0aaa aaaa | Arpeggio Duration                          (0 - 9) |
|             |           |                        30, 40, 50, 60, 70, 80, 90, |
|             |           |                                      100, 120, FUL |
| 00 03       | 0000 000a | Arpeggio Switch                            (0 - 1) |
|             |           |                                            OFF, ON |
| 00 04       | 0aaa aaaa | (reserve) <*>                                      |
| 00 05       | 0aaa aaaa | Arpeggio Style                           (0 - 127) |
|             |           |                                            1 - 128 |
| 00 06       | 0aaa aaaa | Arpeggio Motif                            (0 - 11) |
|             |           |                      UP/L, UP/H, UP/_, dn/L, dn/H, |
|             |           |                      dn/_, Ud/L, Ud/H, Ud/_, rn/L, |
|             |           |                                       rn/_, PHRASE |
| 00 07       | 0000 0aaa | Arpeggio Octave Range                    (61 - 67) |
|             |           |                                            -3 - +3 |
| 00 08       | 0000 000a | (reserve) <*>                                      |
| 00 09       | 0aaa aaaa | Arpeggio Accent Rate                     (0 - 100) |
| 00 0A       | 0aaa aaaa | Arpeggio Velocity                        (0 - 127) |
|             |           |                                      REAL, 1 - 127 |
| 00 0B       | 0000 aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 0C | Total Size                                                     |
+------------------------------------------------------------------------------+

* SuperNATURAL Synth Tone Common

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | Tone Name 1                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 01       | 0aaa aaaa | Tone Name 2                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 02       | 0aaa aaaa | Tone Name 3                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 03       | 0aaa aaaa | Tone Name 4                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 04       | 0aaa aaaa | Tone Name 5                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 05       | 0aaa aaaa | Tone Name 6                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 06       | 0aaa aaaa | Tone Name 7                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 07       | 0aaa aaaa | Tone Name 8                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 08       | 0aaa aaaa | Tone Name 9                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 09       | 0aaa aaaa | Tone Name 10                            (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0A       | 0aaa aaaa | Tone Name 11                            (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0B       | 0aaa aaaa | Tone Name 12                            (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
|-------------+-----------+----------------------------------------------------|
| 00 0C       | 0aaa aaaa | Tone Level (0 - 127)                               |
|-------------+-----------+----------------------------------------------------|
|# 00 0D      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc | (reserve) <*>                                      |
| 00 10       | 0000 000a | (reserve) <*>                                      |
| 00 11       | 0000 000a | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 12       | 0000 000a | Portamento Switch                          (0 - 1) |
|             |           |                                            OFF, ON |
| 00 13       | 0aaa aaaa | Portamento Time (CC# 5)                  (0 - 127) |
| 00 14       | 0000 00aa | Mono Switch                                (0 - 1) |
|             |           |                                            OFF, ON |
| 00 15       | 0000 0aaa | Octave Shift                             (61 - 67) |
|             |           |                                            -3 - +3 |
| 00 16       | 000a aaaa | Pitch Bend Range Up                       (0 - 24) |
| 00 17       | 000a aaaa | Pitch Bend Range Down                     (0 - 24) |
| 00 18       | 0000 0aaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 19       | 0000 000a | Partial1 Switch                            (0 - 1) |
|             |           |                                            OFF, ON |
| 00 1A       | 0000 000a | Partial1 Select                            (0 - 1) |
|             |           |                                            OFF, ON |
| 00 1B       | 0000 000a | Partial2 Switch                            (0 - 1) |
|             |           |                                            OFF, ON |
| 00 1C       | 0000 000a | Partial2 Select                            (0 - 1) |
|             |           |                                            OFF, ON |
| 00 1D       | 0000 000a | Partial3 Switch                            (0 - 1) |
|             |           |                                            OFF, ON |
| 00 1E       | 0000 000a | Partial3 Select                            (0 - 1) |
|             |           |                                            OFF, ON |
|-------------+-----------+----------------------------------------------------|
| 00 1F       | 0000 00aa | RING Switch                                (0 - 2) |
|             |           |                                       OFF, ---, ON |
|-------------+-----------+----------------------------------------------------|
| 00 20       | 0000 000a | (reserve) <*>                                      |
| 00 21       | 0000 00aa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 2D       | 0000 000a | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 2E       | 0000 000a | Unison Switch                              (0 - 1) |
|             |           |                                            OFF, ON |
| 00 2F       | 0000 000a | (reserve) <*>                                      |
| 00 30       | 0000 000a | (reserve) <*>                                      |
| 00 31       | 0000 000a | Portamento Mode                            (0 - 1) |
|             |           |                                     NORMAL, LEGATO |
| 00 32       | 0000 000a | Legato Switch                              (0 - 1) |
|             |           |                                            OFF, ON |
| 00 33       | 0000 000a | (reserve) <*>                                      |
| 00 34       | 0aaa aaaa | Analog Feel                              (0 - 127) |
| 00 35       | 0aaa aaaa | Wave Shape                               (0 - 127) |
| 00 36       | 0aaa aaaa | Tone Category                            (0 - 127) |
|# 00 37      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | (reserve) <*>                                      |
| 00 3B       | 0000 0aaa | (reserve) <*>                                      |
| 00 3C       | 0000 00aa | Unison Size                                (0 - 3) |
|             |           |                                         2, 4, 6, 8 |
| 00 3D       | 0aaa aaaa | (reserve) <*>                                      |
| 00 3E       | 0aaa aaaa | (reserve) <*>                                      |
| 00 3F       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 40 | Total Size                                                     |
+------------------------------------------------------------------------------+

* SuperNATURAL Synth Tone Modify

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 01       | 0aaa aaaa | Attack Time Interval Sens                (0 - 127) |
| 00 02       | 0aaa aaaa | Release Time Interval Sens               (0 - 127) |
| 00 03       | 0aaa aaaa | Portamento Time Interval Sens            (0 - 127) |
| 00 04       | 0000 00aa | Envelope Loop Mode                         (0 - 2) |
|             |           |                          OFF, FREE-RUN, TEMPO-SYNC |
| 00 05       | 000a aaaa | Envelope Loop Sync Note                   (0 - 19) |
|             |           |                 16, 12, 8, 4, 2, 1, 3/4, 2/3, 1/2, |
|             |           |               3/8, 1/3, 1/4, 3/16, 1/6, 1/8, 3/32, |
|             |           |                             1/12, 1/16, 1/24, 1/32 |
| 00 06       | 0000 000a | Chromatic Portamento                       (0 - 1) |
|             |           |                                            OFF, ON |
| 00 07       | 0aaa aaaa | (reserve) <*>                                      |
| 00 08       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 24       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 25 | Total Size                                                     |
+------------------------------------------------------------------------------+

* SuperNATURAL Synth Tone Partial

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0000 0aaa | OSC Wave                                   (0 - 7) |
|             |           |                       SAW, SQR, PW-SQR, TRI, SINE, |
|             |           |                              NOISE, SUPER-SAW, PCM |
| 00 01       | 00aa aaaa | OSC Wave Variation                         (0 - 2) |
|             |           |                                            A, B, C |
| 00 02       | 0000 00aa | (reserve) <*>                                      |
| 00 03       | 00aa aaaa | OSC Pitch                                (40 - 88) |
|             |           |                                          -24 - +24 |
| 00 04       | 0aaa aaaa | OSC Detune                              (14 - 114) |
|             |           |                                          -50 - +50 |
| 00 05       | 0aaa aaaa | OSC Pulse Width Mod Depth                (0 - 127) |
| 00 06       | 0aaa aaaa | OSC Pulse Width                          (0 - 127) |
| 00 07       | 0aaa aaaa | OSC Pitch Env Attack Time                (0 - 127) |
| 00 08       | 0aaa aaaa | OSC Pitch Env Decay                      (0 - 127) |
| 00 09       | 0aaa aaaa | OSC Pitch Env Depth                      (1 - 127) |
|             |           |                                          -63 - +63 |
|-------------+-----------+----------------------------------------------------|
| 00 0A       | 0000 0aaa | FILTER Mode                                (0 - 7) |
|             |           |                        BYPASS, LPF, HPF, BPF, PKG, |
|             |           |                                   LPF2, LPF3, LPF4 |
| 00 0B       | 0000 000a | FILTER Slope                               (0 - 1) |
|             |           |                                      -12, -24 [dB] |
| 00 0C       | 0aaa aaaa | FILTER Cutoff                            (0 - 127) |
| 00 0D       | 00aa aaaa | FILTER Cutoff Keyfollow                  (54 - 74) |
|             |           |                                        -100 - +100 |
| 00 0E       | 0aaa aaaa | FILTER Env Velocity Sens                 (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 0F       | 0aaa aaaa | FILTER Resonance                         (0 - 127) |
| 00 10       | 0aaa aaaa | FILTER Env Attack Time                   (0 - 127) |
| 00 11       | 0aaa aaaa | FILTER Env Decay Time                    (0 - 127) |
| 00 12       | 0aaa aaaa | FILTER Env Sustain Level                 (0 - 127) |
| 00 13       | 0aaa aaaa | FILTER Env Release Time                  (0 - 127) |
| 00 14       | 0aaa aaaa | FILTER Env Depth                         (1 - 127) |
|             |           |                                          -63 - +63 |
|-------------+-----------+----------------------------------------------------|
| 00 15       | 0aaa aaaa | AMP Level                                (0 - 127) |
| 00 16       | 0aaa aaaa | AMP Level Velocity Sens                  (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 17       | 0aaa aaaa | AMP Env Attack Time                      (0 - 127) |
| 00 18       | 0aaa aaaa | AMP Env Decay Time                       (0 - 127) |
| 00 19       | 0aaa aaaa | AMP Env Sustain Level                    (0 - 127) |
| 00 1A       | 0aaa aaaa | AMP Env Release Time                     (0 - 127) |
| 00 1B       | 0aaa aaaa | AMP Pan                                  (0 - 127) |
|             |           |                                          L64 - 63R |
|-------------+-----------+----------------------------------------------------|
| 00 1C       | 0000 0aaa | LFO Shape                                  (0 - 5) |
|             |           |                       TRI, SIN, SAW, SQR, S&H, RND |
| 00 1D       | 0aaa aaaa | LFO Rate                                 (0 - 127) |
| 00 1E       | 0000 000a | LFO Tempo Sync Switch                      (0 - 1) |
|             |           |                                            OFF, ON |
| 00 1F       | 000a aaaa | LFO Tempo Sync Note                       (0 - 19) |
|             |           |                 16, 12, 8, 4, 2, 1, 3/4, 2/3, 1/2, |
|             |           |               3/8, 1/3, 1/4, 3/16, 1/6, 1/8, 3/32, |
|             |           |                             1/12, 1/16, 1/24, 1/32 |
| 00 20       | 0aaa aaaa | LFO Fade Time                            (0 - 127) |
| 00 21       | 0000 000a | LFO Key Trigger                            (0 - 1) |
|             |           |                                            OFF, ON |
| 00 22       | 0aaa aaaa | LFO Pitch Depth                          (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 23       | 0aaa aaaa | LFO Filter Depth                         (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 24       | 0aaa aaaa | LFO Amp Depth                            (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 25       | 0aaa aaaa | LFO Pan Depth                            (1 - 127) |
|             |           |                                          -63 - +63 |
|-------------+-----------+----------------------------------------------------|
| 00 26       | 0000 0aaa | Modulation LFO Shape                       (0 - 5) |
|             |           |                       TRI, SIN, SAW, SQR, S&H, RND |
| 00 27       | 0aaa aaaa | Modulation LFO Rate                      (0 - 127) |
| 00 28       | 0000 000a | Modulation LFO Tempo Sync Switch           (0 - 1) |
|             |           |                                            OFF, ON |
| 00 29       | 000a aaaa | Modulation LFO Tempo Sync Note            (0 - 19) |
|             |           |                 16, 12, 8, 4, 2, 1, 3/4, 2/3, 1/2, |
|             |           |               3/8, 1/3, 1/4, 3/16, 1/6, 1/8, 3/32, |
|             |           |                             1/12, 1/16, 1/24, 1/32 |
| 00 2A       | 0aaa aaaa | OSC Pulse Width Shift                    (0 - 127) |
| 00 2B       | 0000 000a | (reserve) <*>                                      |
| 00 2C       | 0aaa aaaa | Modulation LFO Pitch Depth               (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 2D       | 0aaa aaaa | Modulation LFO Filter Depth              (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 2E       | 0aaa aaaa | Modulation LFO Amp Depth                 (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 2F       | 0aaa aaaa | Modulation LFO Pan Depth                 (1 - 127) |
|             |           |                                          -63 - +63 |
|-------------+-----------+----------------------------------------------------|
| 00 30       | 0aaa aaaa | Cutoff Aftertouch Sens                   (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 31       | 0aaa aaaa | Level Aftertouch Sens                    (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 32       | 0aaa aaaa | (reserve) <*>                                      |
| 00 33       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 34       | 0000 00aa | Wave Gain                                  (0 - 3) |
|             |           |                                -6, 0, +6, +12 [dB] |
|# 00 35      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | Wave Number                            (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
| 00 39       | 0aaa aaaa | HPF Cutoff                               (0 - 127) |
| 00 3A       | 0aaa aaaa | Super Saw Detune                         (0 - 127) |
| 00 3B       | 0aaa aaaa | Modulation LFO Rate Control              (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 3C       | 000a aaaa | AMP Level Keyfollow                      (54 - 74) |
|             |           |                                        -100 - +100 |
|-------------+----------------------------------------------------------------|
| 00 00 00 3D | Total Size                                                     |
+------------------------------------------------------------------------------+

* Analog Synth Tone

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | Tone Name 1                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 01       | 0aaa aaaa | Tone Name 2                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 02       | 0aaa aaaa | Tone Name 3                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 03       | 0aaa aaaa | Tone Name 4                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 04       | 0aaa aaaa | Tone Name 5                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 05       | 0aaa aaaa | Tone Name 6                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 06       | 0aaa aaaa | Tone Name 7                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 07       | 0aaa aaaa | Tone Name 8                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 08       | 0aaa aaaa | Tone Name 9                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 09       | 0aaa aaaa | Tone Name 10                            (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0A       | 0aaa aaaa | Tone Name 11                            (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0B       | 0aaa aaaa | Tone Name 12                            (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
|-------------+-----------+----------------------------------------------------|
| 00 0C       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 0D       | 0000 0aaa | LFO Shape                                  (0 - 5) |
|             |           |                       TRI, SIN, SAW, SQR, S&H, RND |
| 00 0E       | 0aaa aaaa | LFO Rate                                 (0 - 127) |
| 00 0F       | 0aaa aaaa | LFO Fade Time                            (0 - 127) |
| 00 10       | 0000 000a | LFO Tempo Sync Switch                      (0 - 1) |
|             |           |                                            OFF, ON |
| 00 11       | 000a aaaa | LFO Tempo Sync Note                       (0 - 19) |
|             |           |                 16, 12, 8, 4, 2, 1, 3/4, 2/3, 1/2, |
|             |           |               3/8, 1/3, 1/4, 3/16, 1/6, 1/8, 3/32, |
|             |           |                             1/12, 1/16, 1/24, 1/32 |
| 00 12       | 0aaa aaaa | LFO Pitch Depth                          (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 13       | 0aaa aaaa | LFO Filter Depth                         (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 14       | 0aaa aaaa | LFO Amp Depth                            (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 15       | 0000 000a | LFO Key Trigger                            (0 - 1) |
|             |           |                                            OFF, ON |
|-------------+-----------+----------------------------------------------------|
| 00 16       | 0000 0aaa | OSC Waveform                               (0 - 2) |
|             |           |                                   SAW, TRI, PW-SQR |
| 00 17       | 00aa aaaa | OSC Pitch Coarse                         (40 - 88) |
|             |           |                                          -24 - +24 |
| 00 18       | 0aaa aaaa | OSC Pitch Fine                          (14 - 114) |
|             |           |                                          -50 - +50 |
| 00 19       | 0aaa aaaa | OSC Pulse Width                          (0 - 127) |
| 00 1A       | 0aaa aaaa | OSC Pulse Width Mod Depth                (0 - 127) |
| 00 1B       | 0aaa aaaa | OSC Pitch Env Velocity Sens              (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 1C       | 0aaa aaaa | OSC Pitch Env Attack Time                (0 - 127) |
| 00 1D       | 0aaa aaaa | OSC Pitch Env Decay                      (0 - 127) |
| 00 1E       | 0aaa aaaa | OSC Pitch Env Depth                      (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 1F       | 0000 00aa | Sub Oscillator Type                        (0 - 2) |
|             |           |                                  OFF, OCT-1, OCT-2 |
|-------------+-----------+----------------------------------------------------|
| 00 20       | 0000 0aaa | Filter Switch                              (0 - 1) |
|             |           |                                        BYPASS, LPF |
| 00 21       | 0aaa aaaa | Filter Cutoff                            (0 - 127) |
| 00 22       | 00aa aaaa | Filter Cutoff Keyfollow                  (54 - 74) |
|             |           |                                        -100 - +100 |
| 00 23       | 0aaa aaaa | Filter Resonance                         (0 - 127) |
| 00 24       | 0aaa aaaa | Filter Env Velocity Sens                 (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 25       | 0aaa aaaa | Filter Env Attack Time                   (0 - 127) |
| 00 26       | 0aaa aaaa | Filter Env Decay Time                    (0 - 127) |
| 00 27       | 0aaa aaaa | Filter Env Sustain Level                 (0 - 127) |
| 00 28       | 0aaa aaaa | Filter Env Release Time                  (0 - 127) |
| 00 29       | 0aaa aaaa | Filter Env Depth                         (1 - 127) |
|             |           |                                          -63 - +63 |
|-------------+-----------+----------------------------------------------------|
| 00 2A       | 0aaa aaaa | AMP Level                                (0 - 127) |
| 00 2B       | 000a aaaa | AMP Level Keyfollow                      (54 - 74) |
|             |           |                                        -100 - +100 |
| 00 2C       | 0aaa aaaa | AMP Level Velocity Sens                  (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 2D       | 0aaa aaaa | AMP Env Attack Time                      (0 - 127) |
| 00 2E       | 0aaa aaaa | AMP Env Decay Time                       (0 - 127) |
| 00 2F       | 0aaa aaaa | AMP Env Sustain Level                    (0 - 127) |
| 00 30       | 0aaa aaaa | AMP Env Release Time                     (0 - 127) |
|-------------+-----------+----------------------------------------------------|
| 00 31       | 0000 000a | Portamento Switch                          (0 - 1) |
|             |           |                                            OFF, ON |
| 00 32       | 0aaa aaaa | Portamento Time (CC# 5)                  (0 - 127) |
| 00 33       | 0000 000a | Legato Switch                              (0 - 1) |
|             |           |                                            OFF, ON |
| 00 34       | 0000 0aaa | Octave Shift                             (61 - 67) |
|             |           |                                            -3 - +3 |
| 00 35       | 000a aaaa | Pitch Bend Range Up                       (0 - 24) |
| 00 36       | 000a aaaa | Pitch Bend Range Down                     (0 - 24) |
| 00 37       | 0000 0aaa | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 38       | 0aaa aaaa | LFO Pitch Modulation Control             (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 39       | 0aaa aaaa | LFO Filter Modulation Control            (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 3A       | 0aaa aaaa | LFO Amp Modulation Control               (1 - 127) |
|             |           |                                          -63 - +63 |
| 00 3B       | 0aaa aaaa | LFO Rate Modulation Control              (1 - 127) |
|             |           |                                          -63 - +63 |
|-------------+-----------+----------------------------------------------------|
| 00 3C       | 0aaa aaaa | (reserve) <*>                                      |
| 00 3D       | 0aaa aaaa | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 3F       | 0aaa aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 40 | Total Size                                                     |
+------------------------------------------------------------------------------+

* Drum Kit Common

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | Kit Name 1                              (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 01       | 0aaa aaaa | Kit Name 2                              (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 02       | 0aaa aaaa | Kit Name 3                              (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 03       | 0aaa aaaa | Kit Name 4                              (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 04       | 0aaa aaaa | Kit Name 5                              (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 05       | 0aaa aaaa | Kit Name 6                              (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 06       | 0aaa aaaa | Kit Name 7                              (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 07       | 0aaa aaaa | Kit Name 8                              (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 08       | 0aaa aaaa | Kit Name 9                              (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 09       | 0aaa aaaa | Kit Name 10                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0A       | 0aaa aaaa | Kit Name 11                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0B       | 0aaa aaaa | Kit Name 12                             (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
|-------------+-----------+----------------------------------------------------|
| 00 0C       | 0aaa aaaa | Kit Level                                (0 - 127) |
| 00 0D       | 0000 000a | (reserve) <*>                                      |
|# 00 0E      | 0000 aaaa |                                                    |
|             | 0000 bbbb | (reserve) <*>                                      |
| :           |           |                                                    |
| 00 11       | 0000 aaaa | (reserve) <*>                                      |
|-------------+----------------------------------------------------------------|
| 00 00 00 12 | Total Size                                                     |
+------------------------------------------------------------------------------+

* Drum Kit Partial

+------------------------------------------------------------------------------+
| Offset      |                                                                |
| Address     | Description                                                    |
|-------------+----------------------------------------------------------------|
| 00 00       | 0aaa aaaa | Partial Name 1                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 01       | 0aaa aaaa | Partial Name 2                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 02       | 0aaa aaaa | Partial Name 3                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 03       | 0aaa aaaa | Partial Name 4                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 04       | 0aaa aaaa | Partial Name 5                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 05       | 0aaa aaaa | Partial Name 6                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 06       | 0aaa aaaa | Partial Name 7                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 07       | 0aaa aaaa | Partial Name 8                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 08       | 0aaa aaaa | Partial Name 9                          (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 09       | 0aaa aaaa | Partial Name 10                         (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0A       | 0aaa aaaa | Partial Name 11                         (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
| 00 0B       | 0aaa aaaa | Partial Name 12                         (32 - 127) |
|             |           |                                   32 - 127 [ASCII] |
|-------------+-----------+----------------------------------------------------|
| 00 0C       | 0000 000a | Assign Type                                (0 - 1) |
|             |           |                                      MULTI, SINGLE |
| 00 0D       | 000a aaaa | Mute Group                                (0 - 31) |
|             |           |                                        OFF, 1 - 31 |
|-------------+-----------+----------------------------------------------------|
| 00 0E       | 0aaa aaaa | Partial Level                            (0 - 127) |
| 00 0F       | 0aaa aaaa | Partial Coarse Tune                      (0 - 127) |
|             |           |                                           C-1 - G9 |
| 00 10       | 0aaa aaaa | Partial Fine Tune                       (14 - 114) |
|             |           |                                          -50 - +50 |
| 00 11       | 000a aaaa | Partial Random Pitch Depth                (0 - 30) |
|             |           |                      0, 1, 2, 3, 4, 5, 6, 7, 8, 9, |
|             |           |                    10, 20, 30, 40, 50, 60, 70, 80, |
|             |           |                       90, 100, 200, 300, 400, 500, |
|             |           |                    600, 700, 800, 900, 1000, 1100, |
|             |           |                                               1200 |
| 00 12       | 0aaa aaaa | Partial Pan                              (0 - 127) |
|             |           |                                          L64 - 63R |
| 00 13       | 00aa aaaa | Partial Random Pan Depth                  (0 - 63) |
| 00 14       | 0aaa aaaa | Partial Alternate Pan Depth              (1 - 127) |
|             |           |                                          L63 - 63R |
| 00 15       | 0000 000a | Partial Env Mode                           (0 - 1) |
|             |           |                                    NO-SUS, SUSTAIN |
|-------------+-----------+----------------------------------------------------|
| 00 16       | 0aaa aaaa | Partial Output Level                     (0 - 127) |
| 00 17       | 0aaa aaaa | (reserve) <*>                                      |
| 00 18       | 0aaa aaaa | (reserve) <*>                                      |
| 00 19       | 0aaa aaaa | Partial Chorus Send Level                (0 - 127) |
| 00 1A       | 0aaa aaaa | Partial Reverb Send Level                (0 - 127) |
| 00 1B       | 0000 aaaa | Partial Output Assign                      (0 - 4) |
|             |           |                          EFX1, EFX2, DLY, REV, DIR |
|-------------+-----------+----------------------------------------------------|
| 00 1C       | 00aa aaaa | Partial Pitch Bend Range                  (0 - 48) |
| 00 1D       | 0000 000a | Partial Receive Expression                 (0 - 1) |
|             |           |                                            OFF, ON |
| 00 1E       | 0000 000a | Partial Receive Hold-1                     (0 - 1) |
|             |           |                                            OFF, ON |
| 00 1F       | 0000 000a | (reserve) <*>                                      |
|-------------+-----------+----------------------------------------------------|
| 00 20       | 0000 00aa | WMT Velocity Control                       (0 - 2) |
|             |           |                                    OFF, ON, RANDOM |
|-------------+-----------+----------------------------------------------------|
| 00 21       | 0000 000a | WMT1 Wave Switch                           (0 - 1) |
|             |           |                                            OFF, ON |
| 00 22       | 0000 00aa | WMT1 Wave Group Type                           (0) |
|# 00 23      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT1 Wave Group ID                     (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
|# 00 27      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT1 Wave Number L (Mono)              (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
|# 00 2B      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT1 Wave Number R                     (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
| 00 2F       | 0000 00aa | WMT1 Wave Gain                             (0 - 3) |
|             |           |                                -6, 0, +6, +12 [dB] |
| 00 30       | 0000 000a | WMT1 Wave FXM Switch                       (0 - 1) |
|             |           |                                            OFF, ON |
| 00 31       | 0000 00aa | WMT1 Wave FXM Color                        (0 - 3) |
|             |           |                                              1 - 4 |
| 00 32       | 000a aaaa | WMT1 Wave FXM Depth                       (0 - 16) |
| 00 33       | 0000 000a | WMT1 Wave Tempo Sync                       (0 - 1) |
|             |           |                                            OFF, ON |
| 00 34       | 0aaa aaaa | WMT1 Wave Coarse Tune                   (16 - 112) |
|             |           |                                          -48 - +48 |
| 00 35       | 0aaa aaaa | WMT1 Wave Fine Tune                     (14 - 114) |
|             |           |                                          -50 - +50 |
| 00 36       | 0aaa aaaa | WMT1 Wave Pan                            (0 - 127) |
|             |           |                                          L64 - 63R |
| 00 37       | 0000 000a | WMT1 Wave Random Pan Switch                (0 - 1) |
|             |           |                                            OFF, ON |
| 00 38       | 0000 00aa | WMT1 Wave Alternate Pan Switch             (0 - 2) |
|             |           |                                   OFF, ON, REVERSE |
| 00 39       | 0aaa aaaa | WMT1 Wave Level                          (0 - 127) |
| 00 3A       | 0aaa aaaa | WMT1 Velocity Range Lower                (1 - 127) |
|             |           |                                          1 - UPPER |
| 00 3B       | 0aaa aaaa | WMT1 Velocity Range Upper                (1 - 127) |
|             |           |                                        LOWER - 127 |
| 00 3C       | 0aaa aaaa | WMT1 Velocity Fade Width Lower           (0 - 127) |
| 00 3D       | 0aaa aaaa | WMT1 Velocity Fade Width Upper           (0 - 127) |
|-------------+-----------+----------------------------------------------------|
| 00 3E       | 0000 000a | WMT2 Wave Switch                           (0 - 1) |
|             |           |                                            OFF, ON |
| 00 3F       | 0000 00aa | WMT2 Wave Group Type                           (0) |
|# 00 40      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT2 Wave Group ID                     (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
|# 00 44      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT2 Wave Number L (Mono)              (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
|# 00 48      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT2 Wave Number R                     (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
| 00 4C       | 0000 00aa | WMT2 Wave Gain                             (0 - 3) |
|             |           |                                -6, 0, +6, +12 [dB] |
| 00 4D       | 0000 000a | WMT2 Wave FXM Switch                       (0 - 1) |
|             |           |                                            OFF, ON |
| 00 4E       | 0000 00aa | WMT2 Wave FXM Color                        (0 - 3) |
|             |           |                                              1 - 4 |
| 00 4F       | 000a aaaa | WMT2 Wave FXM Depth                       (0 - 16) |
| 00 50       | 0000 000a | WMT2 Wave Tempo Sync                       (0 - 1) |
|             |           |                                            OFF, ON |
| 00 51       | 0aaa aaaa | WMT2 Wave Coarse Tune                   (16 - 112) |
|             |           |                                          -48 - +48 |
| 00 52       | 0aaa aaaa | WMT2 Wave Fine Tune                     (14 - 114) |
|             |           |                                          -50 - +50 |
| 00 53       | 0aaa aaaa | WMT2 Wave Pan                            (0 - 127) |
|             |           |                                          L64 - 63R |
| 00 54       | 0000 000a | WMT2 Wave Random Pan Switch                (0 - 1) |
|             |           |                                            OFF, ON |
| 00 55       | 0000 00aa | WMT2 Wave Alternate Pan Switch             (0 - 2) |
|             |           |                                   OFF, ON, REVERSE |
| 00 56       | 0aaa aaaa | WMT2 Wave Level                          (0 - 127) |
| 00 57       | 0aaa aaaa | WMT2 Velocity Range Lower                (1 - 127) |
|             |           |                                          1 - UPPER |
| 00 58       | 0aaa aaaa | WMT2 Velocity Range Upper                (1 - 127) |
|             |           |                                        LOWER - 127 |
| 00 59       | 0aaa aaaa | WMT2 Velocity Fade Width Lower           (0 - 127) |
| 00 5A       | 0aaa aaaa | WMT2 Velocity Fade Width Upper           (0 - 127) |
|-------------+-----------+----------------------------------------------------|
| 00 5B       | 0000 000a | WMT3 Wave Switch                           (0 - 1) |
|             |           |                                            OFF, ON |
| 00 5C       | 0000 00aa | WMT3 Wave Group Type                           (0) |
|# 00 5D      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT3 Wave Group ID                     (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
|# 00 61      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT3 Wave Number L (Mono)              (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
|# 00 65      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT3 Wave Number R                     (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
| 00 69       | 0000 00aa | WMT3 Wave Gain                             (0 - 3) |
|             |           |                                -6, 0, +6, +12 [dB] |
| 00 6A       | 0000 000a | WMT3 Wave FXM Switch                       (0 - 1) |
|             |           |                                            OFF, ON |
| 00 6B       | 0000 00aa | WMT3 Wave FXM Color                        (0 - 3) |
|             |           |                                              1 - 4 |
| 00 6C       | 000a aaaa | WMT3 Wave FXM Depth                       (0 - 16) |
| 00 6D       | 0000 000a | WMT3 Wave Tempo Sync                       (0 - 1) |
|             |           |                                            OFF, ON |
| 00 6E       | 0aaa aaaa | WMT3 Wave Coarse Tune                   (16 - 112) |
|             |           |                                          -48 - +48 |
| 00 6F       | 0aaa aaaa | WMT3 Wave Fine Tune                     (14 - 114) |
|             |           |                                          -50 - +50 |
| 00 70       | 0aaa aaaa | WMT3 Wave Pan                            (0 - 127) |
|             |           |                                          L64 - 63R |
| 00 71       | 0000 000a | WMT3 Wave Random Pan Switch                (0 - 1) |
|             |           |                                            OFF, ON |
| 00 72       | 0000 00aa | WMT3 Wave Alternate Pan Switch             (0 - 2) |
|             |           |                                   OFF, ON, REVERSE |
| 00 73       | 0aaa aaaa | WMT3 Wave Level                          (0 - 127) |
| 00 74       | 0aaa aaaa | WMT3 Velocity Range Lower                (1 - 127) |
|             |           |                                          1 - UPPER |
| 00 75       | 0aaa aaaa | WMT3 Velocity Range Upper                (1 - 127) |
|             |           |                                        LOWER - 127 |
| 00 76       | 0aaa aaaa | WMT3 Velocity Fade Width Lower           (0 - 127) |
| 00 77       | 0aaa aaaa | WMT3 Velocity Fade Width Upper           (0 - 127) |
|-------------+-----------+----------------------------------------------------|
| 00 78       | 0000 000a | WMT4 Wave Switch                           (0 - 1) |
|             |           |                                            OFF, ON |
| 00 79       | 0000 00aa | WMT4 Wave Group Type                           (0) |
|# 00 7A      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT4 Wave Group ID                     (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
|# 00 7E      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT4 Wave Number L (Mono)              (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
|# 01 02      | 0000 aaaa |                                                    |
|             | 0000 bbbb |                                                    |
|             | 0000 cccc |                                                    |
|             | 0000 dddd | WMT4 Wave Number R                     (0 - 16384) |
|             |           |                                     OFF, 1 - 16384 |
| 01 06       | 0000 00aa | WMT4 Wave Gain                             (0 - 3) |
|             |           |                                -6, 0, +6, +12 [dB] |
| 01 07       | 0000 000a | WMT4 Wave FXM Switch                       (0 - 1) |
|             |           |                                            OFF, ON |
| 01 08       | 0000 00aa | WMT4 Wave FXM Color                        (0 - 3) |
|             |           |                                              1 - 4 |
| 01 09       | 000a aaaa | WMT4 Wave FXM Depth                       (0 - 16) |
| 01 0A       | 0000 000a | WMT4 Wave Tempo Sync                       (0 - 1) |
|             |           |                                            OFF, ON |
| 01 0B       | 0aaa aaaa | WMT4 Wave Coarse Tune                   (16 - 112) |
|             |           |                                          -48 - +48 |
| 01 0C       | 0aaa aaaa | WMT4 Wave Fine Tune                     (14 - 114) |
|             |           |                                          -50 - +50 |
| 01 0D       | 0aaa aaaa | WMT4 Wave Pan                            (0 - 127) |
|             |           |                                          L64 - 63R |
| 01 0E       | 0000 000a | WMT4 Wave Random Pan Switch                (0 - 1) |
|             |           |                                            OFF, ON |
| 01 0F       | 0000 00aa | WMT4 Wave Alternate Pan Switch             (0 - 2) |
|             |           |                                   OFF, ON, REVERSE |
| 01 10       | 0aaa aaaa | WMT4 Wave Level                          (0 - 127) |
| 01 11       | 0aaa aaaa | WMT4 Velocity Range Lower                (1 - 127) |
|             |           |                                          1 - UPPER |
| 01 12       | 0aaa aaaa | WMT4 Velocity Range Upper                (1 - 127) |
|             |           |                                        LOWER - 127 |
| 01 13       | 0aaa aaaa | WMT4 Velocity Fade Width Lower           (0 - 127) |
| 01 14       | 0aaa aaaa | WMT4 Velocity Fade Width Upper           (0 - 127) |
|-------------+-----------+----------------------------------------------------|
| 01 15       | 000a aaaa | Pitch Env Depth                          (52 - 76) |
|             |           |                                          -12 - +12 |
| 01 16       | 0aaa aaaa | Pitch Env Velocity Sens                  (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 17       | 0aaa aaaa | Pitch Env Time 1 Velocity Sens           (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 18       | 0aaa aaaa | Pitch Env Time 4 Velocity Sens           (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 19       | 0aaa aaaa | Pitch Env Time 1                         (0 - 127) |
| 01 1A       | 0aaa aaaa | Pitch Env Time 2                         (0 - 127) |
| 01 1B       | 0aaa aaaa | Pitch Env Time 3                         (0 - 127) |
| 01 1C       | 0aaa aaaa | Pitch Env Time 4                         (0 - 127) |
| 01 1D       | 0aaa aaaa | Pitch Env Level 0                        (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 1E       | 0aaa aaaa | Pitch Env Level 1                        (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 1F       | 0aaa aaaa | Pitch Env Level 2                        (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 20       | 0aaa aaaa | Pitch Env Level 3                        (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 21       | 0aaa aaaa | Pitch Env Level 4                        (1 - 127) |
|             |           |                                          -63 - +63 |
|-------------+-----------+----------------------------------------------------|
| 01 22       | 0000 0aaa | TVF Filter Type                            (0 - 6) |
|             |           |                     OFF, LPF, BPF, HPF, PKG, LPF2, |
|             |           |                                               LPF3 |
| 01 23       | 0aaa aaaa | TVF Cutoff Frequency                     (0 - 127) |
| 01 24       | 0000 0aaa | TVF Cutoff Velocity Curve                  (0 - 7) |
|             |           |                                       FIXED, 1 - 7 |
| 01 25       | 0aaa aaaa | TVF Cutoff Velocity Sens                 (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 26       | 0aaa aaaa | TVF Resonance                            (0 - 127) |
| 01 27       | 0aaa aaaa | TVF Resonance Velocity Sens              (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 28       | 0aaa aaaa | TVF Env Depth                            (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 29       | 0000 0aaa | TVF Env Velocity Curve Type                (0 - 7) |
|             |           |                                       FIXED, 1 - 7 |
| 01 2A       | 0aaa aaaa | TVF Env Velocity Sens                    (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 2B       | 0aaa aaaa | TVF Env Time 1 Velocity Sens             (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 2C       | 0aaa aaaa | TVF Env Time 4 Velocity Sens             (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 2D       | 0aaa aaaa | TVF Env Time 1                           (0 - 127) |
| 01 2E       | 0aaa aaaa | TVF Env Time 2                           (0 - 127) |
| 01 2F       | 0aaa aaaa | TVF Env Time 3                           (0 - 127) |
| 01 30       | 0aaa aaaa | TVF Env Time 4                           (0 - 127) |
| 01 31       | 0aaa aaaa | TVF Env Level 0                          (0 - 127) |
| 01 32       | 0aaa aaaa | TVF Env Level 1                          (0 - 127) |
| 01 33       | 0aaa aaaa | TVF Env Level 2                          (0 - 127) |
| 01 34       | 0aaa aaaa | TVF Env Level 3                          (0 - 127) |
| 01 35       | 0aaa aaaa | TVF Env Level 4                          (0 - 127) |
|-------------+-----------+----------------------------------------------------|
| 01 36       | 0000 0aaa | TVA Level Velocity Curve                   (0 - 7) |
|             |           |                                       FIXED, 1 - 7 |
| 01 37       | 0aaa aaaa | TVA Level Velocity Sens                  (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 38       | 0aaa aaaa | TVA Env Time 1 Velocity Sens             (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 39       | 0aaa aaaa | TVA Env Time 4 Velocity Sens             (1 - 127) |
|             |           |                                          -63 - +63 |
| 01 3A       | 0aaa aaaa | TVA Env Time 1                           (0 - 127) |
| 01 3B       | 0aaa aaaa | TVA Env Time 2                           (0 - 127) |
| 01 3C       | 0aaa aaaa | TVA Env Time 3                           (0 - 127) |
| 01 3D       | 0aaa aaaa | TVA Env Time 4                           (0 - 127) |
| 01 3E       | 0aaa aaaa | TVA Env Level 1                          (0 - 127) |
| 01 3F       | 0aaa aaaa | TVA Env Level 2                          (0 - 127) |
| 01 40       | 0aaa aaaa | TVA Env Level 3                          (0 - 127) |
|-------------+-----------+----------------------------------------------------|
| 01 41       | 0000 000a | One Shot Mode                              (0 - 1) |
|             |           |                                            OFF, ON |
| 01 42       | 0aaa aaaa | Relative Level                           (0 - 127) |
|             |           |                                          -64 - +63 |
|-------------+----------------------------------------------------------------|
| 00 00 01 43 | Total Size                                                     |
+------------------------------------------------------------------------------+
</pre>
