# CousinRicky’s Modifications to SpectralRender

Ive of LILYsoft has developed [SpectralRender](https://www.lilysoft.org/CGI/SR/Spectral%20Render.htm), a spectral rendering engine for [POV-Ray](https://www.povray.org/).

I have made two sets of modifications to one of the files, namely `SpectralComposer.pov`. Since the project has recently been “un-licensed,” I am now publishing my modifications.

1. I first converted it to a proper include file. In its standard form, `SpectralComposer.pov` must be edited every time you create a new scene, or even render the same scene with different parameters; and the final output image must be renamed. With my changes, you can `#include` the composer file in your scene description file, and `#declare` the scene name and other parameters in your scene description file, in an INI file, or on the command line. There will be no need to edit the composer file or rename the output image.

2. I then implemented gamut mapping to deal with colors that are too rich for the color systems supported by SpectralRender.

File **`SpectralComposer.inc`** has only the parameter upgrade.

File **`SpectralComposer-gm2.inc`** has both the parameter upgrade and the gamut mapping.

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
      // Set other parameters here (optional).
      #include "SpectralComposer-gm2.inc"
    #end

With this arrangement, if animation is not used _and_ `Preview` is set to `false`, then the spectral composer will be invoked.

Note that `SpectralComposer-gm2.inc` is considerably slower than `SpectralComposer.inc`, so you may want to use the latter for drafts.
