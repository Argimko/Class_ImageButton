# Class_ImageButton #

Image Buttons for AHK GUIs.

To run the sample script, you have to copy PIC1.jpg and PIC2.jpg included in the \Resources folder into the script folder.

## Features ##
The class creates an image list and assigns it to the pushbutton control. Button image lists may contain one or six images. If they contain only one image, the image will be used to draw all button states (i.e. the button won't be animated). Otherwise the images will be used to draw the appropriate six [button states](http://msdn.microsoft.com/en-us/windows/bb775975):  

    PBS_NORMAL    = 1
    PBS_HOT       = 2
    PBS_PRESSED   = 3
    PBS_DISABLED  = 4
    PBS_DEFAULTED = 5
    PBS_STYLUSHOT = 6 ; used only on tablet computers
    
## How to use ##

1. Create a push button (e.g. `Gui, Add, Button, vMyButton HwndHBTN, Caption`) using the `Hwnd` option to get its HWND.  

2. Call `ImageButton.Create()` passing two parameters: 
     
        HWND        -  Button's HWND.  
        Options*    -  variadic array containing up to 6 option arrays (see below).

	The index of each option object determines the corresponding button state on which the bitmap will be shown (see above).

	If you don't want the button to be 'animated' on themed GUIs, just pass one option object with index 1.  

	Each option array may contain the following values:  

        Index Value
		1     Mode        mandatory:
						  0  -  unicolored or bitmap
						  1  -  vertical bicolored
						  2  -  horizontal bicolored
						  3  -  vertical gradient
						  4  -  horizontal gradient
						  5  -  vertical gradient using StartColor at both borders and
								TargetColor at the center
						  6  -  horizontal gradient using StartColor at both borders and 
								TargetColor at the center
						  7  -  'raised' style (just try out!)
		2     StartColor  mandatory for Option[1], higher indices will inherit the value of
						  Option[1], if omitted:
						  -  ARGB integer value (0xAARRGGBB) or HTML color name ("Red").
						  -  Path of an image file or HBITMAP handle for Mode 0
		3     TargetColor mandatory for Option[1] if Mode > 0, ignored if Mode = 0. Higher
						  indcices will inherit the color of Option[1], if omitted:
						  -  ARGB integer value (0xAARRGGBB) or HTML color name ("Red").
		4     TextColor   optional, if omitted, the default text color will be used for Option[1],
						  higher indices will inherit the color of Option[1]:
						  -  ARGB integer value (0xAARRGGBB) or HTML color name ("Red").
						  -  Pass value "none" to force Class_ImageButton to Not redraw
						     button caption with GDI+. Instead of it the button text will be stay
						     drawn by the system using black color. It is useful when you need
						     a black button caption and if you do not like how GDI+ library
						     applies antialiasing to your button text.
						  -  Default: 0x000000 (black)
		5     Rounded     optional:
						  -  Radius of the rounded corners in pixel; the letters 'H' and 'W' may be
						     specified also to use the half of the button's height or width respectively.
							 Default: 0 - not rounded
		6     GuiColor    optional, needed for rounded buttons if you've changed the GUI background color:
						  -  RGB integer value (0xRRGGBB) or HTML color name ("Red").
							 Default: AHK default GUI background color
        7     BorderColor optional, ignored for modes 0 (bitmap) and 7, color of the border:
                          -  RGB integer value (0xRRGGBB) or HTML color name ("Red").
        8     BorderWidth optional, ignored for modes 0 (bitmap) and 7, width of the border in pixels:
                          -  Default: 1
	
	If the the button has a caption it will be drawn above the bitmap.

3. That's all!

## Extended features
You can add an `icon` object to each option. Example code:
```AutoHotkey
Gui, Add, Button, Left vBT4 w95 h50 hwndHBT5, Save
	Opt1 := { 1:0, 2:0xECEFF3, 7:0xBCC4D0, icon:{file:"..\Resources\save.png", padding: 8} }
	Opt2 := { 1:0, 2:0xCAD3DE, 7:0xA5AFBF, icon:{file:"..\Resources\save.png", padding: 8} }
	Opt3 := { 1:0, 2:0xC1CAD7, 7:0x99A5B7, icon:{file:"..\Resources\save.png", padding: 8} }
	ImageButton.Create(HBT5, Opt1, Opt2, Opt3)
```

Also you can load a bmp-file from itself executable, when it was compilled with Ahk2Exe:
```AutoHotkey
Gui, Add, Checkbox, hwndHCB, Install database
	Opt1 := { 1:0, icon:{hmodule: 0, file: "checkbox_unchecked.bmp", padding: 4} }
	Opt2 := { 1:0, icon:{hmodule: 0, file: "checkbox_unchecked.bmp", padding: 4} }
	Opt3 := { 1:0, icon:{hmodule: 0, file: "checkbox_checked.bmp"  , padding: 4} }
	ImageButton.Create(HCB, Opt1, Opt2, Opt3)
```
In this case `file` is the resource name.

The `icon` object has the following properties:
* `file` - The "icon" image file
* `hmodule` - Handle to the module with a bitmap resource. Pass 0 to load bitmap from calling process.
* `padding` - (optional) The image's left and right padding. Default value is 5.
* `x` - (optional) image's x position. If omited, it uses the padding value.
* `y` - (optional) image's y position.
* `w` - (optional) image width
* `h` - (optional) image height

Screenshot of **Resources\Sample2.ahk**:

![Screenshot of Sample2.ahk](https://github.com/tmplinshi/Class_ImageButton/blob/master/Resources/Sample2.png)
