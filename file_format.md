# Sparkmaker File Format (.wow)

The sparkmaker uses G-files with embedded binary bitmaps for the layers.

## Header
```
G21;   				set unit to mm
G91;				incremental positioning
M17;				enable stepper motor
M106 S0;			LED intesity to 0 --> off
G28 Z0;				homing of Z axis
;W:480;				| resolution of the display
;H:854;				| in the follwing the axes of the layer are called W and H
```

## Layers
```
;L:1;				layer number (begins with 1)
M106 S0;			LED off
G1 Z10 F50;			lift Z axis by 10mm("Lift Distance") with speed 50mm/min("Lift Speed")
G1 Z-9.975 F150;	drop Z axis by 10mm-layer height ("Layer Thickness") with speed 150mm/min (decline speed)
{{
	BINARYLAYER
}}
M106 S255;			LED on with intensity 255 ("Exposure Strength Grade")
G4 S25;				wait for 25s ("Exposure Time Per Layer"/"Bottom Layer Exposure Time")
```

## Binary Layer Format
Inbetween the brackets the layer is written as binary bytes. Each pixel is either 0 or 1 and 8 pixel in the W dimension are written as one byte. After 480 pixels (60 bytes) with W = 0 to 479 are written for a line with H=h, the next line with H=h+1 is wirtten the same way.

```
------------------ | -------------- |       | ----------------- | ------------- |
W0H0 W1H0 ... W7H0 | W8H0 ... W15H0 |  ...  | W471H0 ... W479H0 | W0H1 ... W7H1 | ...
------------------ | -------------- |	    | ----------------- | ------------- |
     Byte 0              Byte 1                     Byte 59           Byte 60
``` 

## Footer
```
M106 S0;			LED PWM to 0 --> off
G1 Z20.0;			lift Z axis by 20mm
G4 S300;			wait for 300s
M18;				disable stepper motor
```