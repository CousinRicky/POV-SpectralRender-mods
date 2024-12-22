# CousinRicky’s Modifications to SpectralRender

Version RC3-0.22-3, 2024-Dec-22

Ive of LILYsoft has developed [SpectralRender](https://www.lilysoft.org/CGI/SR/Spectral%20Render.htm), a spectral rendering engine for [POV-Ray](https://www.povray.org/).

I have made three sets of modifications to one of the files, namely `SpectralComposer.pov`. Since the project has recently been “un-licensed,” I am now publishing my modifications.

1. I first converted it to a proper include file. In its standard form, `SpectralComposer.pov` must be edited every time you create a new scene, or even render the same scene with different parameters; and the final output image must be renamed. With my changes, you can `#include` the composer file in your scene description file, and `#declare` the scene name and other parameters in your scene description file, in an INI file, or on the command line. There will be no need to edit the composer file or rename the output image.

2. I then implemented gamut mapping to deal with colors that are too rich for the color systems supported by SpectralRender.

3. White points are accepted as low as 2000 K. Values for `Whitepoint` less than 4000 will use the black body white; 4000 and above will use the daylight white.

File **`SpectralComposer.inc`** has these three changes. Three gamut mapping methods are available; the method is selected by setting the variable `GamutMap` prior to including `SpectralComposer.inc`:

* `GamutMap` = 1: Out-of-range RGB channel values are clipped. This is the default setting, as well as the behavior of the original `SpectralComposer.pov`.
* `GamutMap` = 2: Out-of-gamut colors are projected toward the white point; this is the method used by the file `SpectralComposer-gm2.inc` in previous versions of this project.
* `GamutMap` = 3 is reserved for possible future use.
* `GamutMap` = 4: Out-of-gamut colors are desaturated while preserving luminance (gray level).

File **`SpectralComposer-gm2.inc`** is retained for legacy reasons.

Only these two files are included in this repository. For the full project and documentation, please visit the [SpectralRender download page](https://www.lilysoft.org/CGI/SR/Spectral%20Render.htm).

## Usage

One can organize a scene description file in this manner:

    #version 3.7;
    #ifndef (Preview) #declare Preview = false; #end
    #if (clock_on | Preview)
      global_settings { assumed_gamma 1 }
      #include "spectral.inc"
      // Define the scene here.
    #else
      #declare FName = "MySceneFileName"
      #declare GamutMap = 4;
      // Set other parameters here (optional).
      #include "SpectralComposer.inc"
    #end

With this arrangement, if animation is not used _and_ `Preview` is set to `false`, then the spectral composer will be invoked.

Note that gamut mapping methods 2 and 4 are considerably slower than method 1, so you may want to use the latter for drafts, or if there are no bright colors in your scene.

## Change Log

### Version RC3-0.22-3

* The gamut mapping is faster.
* Luminance-based gamut mapping is added.
* Gamut mapping is merged into file `SpectralComposer.inc`; file `SpectralComposer-gm2.inc` is no longer needed.

### Version RC3-0.22-2

* White points are accepted as low as 2000 K.

### Version RC3-0.22

* `SpectralComposer.pov` is converted to an include file, and parameters can be declared beforehand without editing the file.
* A second include file is created with gamut mapping.
