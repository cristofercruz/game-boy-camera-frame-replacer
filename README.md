# Game Boy Camera frame replacer

Python script to replace the built-in frames in the Game Boy Camera rom with your own!

## Requisites
This script requires the following Python libraries:
```
python -m pip install --upgrade Pillow
python -m pip install --upgrade img2gb
```
## Usage
```
usage: gbc-fr.py [-h] --mode {copy,inject}
                  --frame-type {standard,wild}
                  --source-rom src.gb --source-frame [1-18]
                  --source-image frame.png
                  --target-rom trgt.gb --target-frame [1-18]

arguments:
  --mode, -m {copy,inject}

  --frame-type, -ft {standard,wild}

  --source-rom, -sr src.gb
    source rom to get data from in copy mode

  --source-frame, -sf [1-18]
    frame number from source rom, standard:[1-18] wild:[1-8]

  --source-image, -si frame.png
    source image for inject mode (.png, .bmp or already formatted tile data .bin)

  --target-rom, -tr trgt.gb
    target rom file to be modified with changes

  --target-frame, -tf [1-18]
    frame number from target rom, standard:[1-18] wild:[1-8]
```

Two modes are available copy or inject.

Copy allows taking frame data from one rom file and pasting into another. You specify the source rom and source frame as well as target rom and target frame.

**Example**: Copy wild frame 2 from JP Pocket Camera rom onto the international rom, replacing wild frame 4.  
<pre>
python gbc-fr.py <b>--mode</b> copy <b>--frame-type</b> wild <b>--source-rom</b> pocketcam-jp.gb <b>--source-frame</b> 2 <b>--target-rom</b> gameboycam-intl.gb <b>--target-frame</b> 4
</pre>

Inject allows using a completely new image to replace an existing frame. You can specify the source image as a .png, .bmp and it will be converted to tile data or you can provide already formatted tile data as .bin. You will also specify the target rom and target frame.

**Example**: Load tile data from supplied image and inject into the International rom, replacing frame 7.  
<pre>
python gbc-fr.py <b>--mode</b> inject <b>--frame-type</b> standard <b>--source-image</b> cameraclub.png <b>--target-rom</b> gameboycam-intl.gb <b>--target-frame</b> 7
</pre>

## Designing your frame image
Game Boy Camera standard frames can only use up to 96 unique tiles but a full standard frame is made up of 136 tiles so you will need to re-use or pattern some tiles. When designing your frame, you can show a grid to be aware of how many unique tiles you've used up. The script will ignore unique tiles after hitting the 96 tile limit and will re-use the last tile for any remaining slots. The example below uses just 46 unique tiles and a re-used black tile for the rest of the frame.

*Standard frame dimensions 160px × 144px*  
<a href="#design-grid"><img id="design-grid" src="docs/frame-unique-tiles.png" alt="Designing with grid"/></a>  

Wild frames don't share the same limit and can use all unique tiles across the entire image.

*Wild frame dimensions 160px × 224px*  
<a href="#wild-frame"><img id="wild-frame" src="docs/wild-frame.png" alt="Designing wild frame"/></a>  

***Design Templates***:

[Standard frame PSD Template](samples/standard-frame-template.psd?raw=1)  
[Wild frame PSD template](samples/wild-frame-template.psd?raw=1)

## Saving your frame image
Save your image reduced down to 4 colors, this will give you a good result when converting to Game Boy tile format. **Ensure your final image has good contrast or the converted result will appear washed out and may use less than 4 shades.**  
<a href="#gameboy-palette"><img id="gameboy-palette" src="docs/4-shades.png" alt="4 shades color map"/></a>  
<a href="#reduced-colors"><img id="reduced-colors" src="docs/reduced-colors.png" alt="Save for Web dialog with reduced colors"/></a> 

## Converting your frame image
This script uses img2gb library to convert .png and .bmp source images to tile data but if you prefer to do the conversion yourself for more control over the result, you can do so using a utility like [Pic2Tiles](http://www.budmelvin.com/dev/index.html)  
<a href="#pic2tiles"><img id="pic2tiles" src="docs/pic2tiles.png" alt="Pic2Tiles application window"/></a> 

## Purpose
Your modified ROM can be loaded into emulators with printer emulation like mGBA (some emulators may show a warning about incorrect checksum). Eventually, your custom rom could be loaded onto a [Game Boy Camera Flash Cartridge](https://github.com/HDR/Gameboy-Camera-Flashcart) once it becomes available.

## Notes
This script will also patch your rom to expose all 8 available wild frame slots. The Japan region Pocket Camera normally only exposes 6 wild frames and the international region Game Boy Camera only exposes 7.

## Supported ROMs
This script supports the 4 standard release rom variants (Game Boy Camera, Zelda Game Boy Camera, Pocket Camera, CoroCoro Pocket Camera) as a target or source file. A recent leak has brought to light a new unreleased Hello Kitty Pocket Camera rom variant which stores frame data unlike the others and as such is unsupported as a target rom but is supported as a source rom using the copy function so you can copy any of its 25 standard and 6 wild frames onto one of the release rom variants.

## Credits
Thanks to @jkbenaim for their [gbcamextract](https://github.com/jkbenaim/gbcamextract) program which helped to figure out the frame data and tile map addresses.  
Thanks to @flozz for their [img2gb](https://github.com/flozz/img2gb) library which provides tile conversion for this script.
