# ios2droid

A simple Perl script to port iOS images to the appropriate Android density buckets.
I use this in my work with [Xamarin](https://www.xamarin.com/), but it could certainly be used in other contexts.

Porting iOS images to Android can be time-consuming.
This script --
inspired in part by [ios2android](https://github.com/Ninjanetic/ios2android) by [Jason Adams](https://github.com/Ninjanetic) --
aims to automate the process as much as possible.


## Prerequisites

[ImageMagick](http://www.imagemagick.org/script/binary-releases.php)

On Mac OS X, the easiest way to install ImageMagick is with [Homebrew](http://brew.sh/):

    brew install imagemagick

(Sorry, I haven't tried getting this to work on Windows.)


## Usage

Put the `ios2droid` script somewhere in your path.

Go to a terminal prompt and `cd` to the top-level directory of your project.
If you're using Xamarin this should be where your `.sln` file is.
There should be an `iOS` and a `Droid` directory underneath.

From the terminal prompt, run using either one of these command lines:

    ios2droid test
    ios2droid

Using the `test` argument will show what it *plans* to do -- nothing will be modified.
Without `test` it will do the actual copying.


## Sample output

    ./iOS/Resources/bg.jpg                  -->  ./Droid/Resources/drawable/bg.jpg

    ./iOS/Resources/btn_circle_info.png     -->  ./Droid/Resources/drawable/btn_circle_info.png
    ./iOS/Resources/btn_circle_info@2x.png  -->  ./Droid/Resources/drawable-hdpi/btn_circle_info.png (resize 75%)
    ./iOS/Resources/btn_circle_info@2x.png  -->  ./Droid/Resources/drawable-xhdpi/btn_circle_info.png
    ./iOS/Resources/btn_circle_info@3x.png  -->  ./Droid/Resources/drawable-xxhdpi/btn_circle_info.png

    ./iOS/Resources/btn_locked.png          -->  ./Droid/Resources/drawable/btn_locked.png
    ./iOS/Resources/btn_locked@2x.png       -->  ./Droid/Resources/drawable-hdpi/btn_locked.png (resize 75%)
    ./iOS/Resources/btn_locked@2x.png       -->  ./Droid/Resources/drawable-xhdpi/btn_locked.png
    ./iOS/Resources/btn_locked@3x.png       -->  ./Droid/Resources/drawable-xxhdpi/btn_locked.png

    ./iOS/Resources/btn_minus.png           -->  ./Droid/Resources/drawable/btn_minus.png
    ./iOS/Resources/btn_minus@2x.png        -->  ./Droid/Resources/drawable-hdpi/btn_minus.png (resize 75%)
    ./iOS/Resources/btn_minus@2x.png        -->  ./Droid/Resources/drawable-xhdpi/btn_minus.png
    ./iOS/Resources/btn_minus@3x.png        -->  ./Droid/Resources/drawable-xxhdpi/btn_minus.png

    ./iOS/Resources/btn_plus.png            -->  ./Droid/Resources/drawable/btn_plus.png
    ./iOS/Resources/btn_plus@3x.png         -->  ./Droid/Resources/drawable-hdpi/btn_plus.png (resize 50%)
    ./iOS/Resources/btn_plus@3x.png         -->  ./Droid/Resources/drawable-xhdpi/btn_plus.png (resize 67%)
    ./iOS/Resources/btn_plus@3x.png         -->  ./Droid/Resources/drawable-xxhdpi/btn_plus.png

    ./iOS/Resources/btn_unlocked.png        -->  ./Droid/Resources/drawable/btn_unlocked.png
    ./iOS/Resources/btn_unlocked@2x.png     -->  ./Droid/Resources/drawable-hdpi/btn_unlocked.png (resize 75%)
    ./iOS/Resources/btn_unlocked@2x.png     -->  ./Droid/Resources/drawable-xhdpi/btn_unlocked.png
    ./iOS/Resources/btn_unlocked@3x.png     -->  ./Droid/Resources/drawable-xxhdpi/btn_unlocked.png


## Details

The script looks for two sub-directories (regardless of case):

* "ios"
* "android" or "droid"

Beneath each sub-directory there must be a "Resources" directory (case must match).

It gathers a list of all jpg, jpeg, and png files in the iOS/Resources directory
that do not contain "@" or begin with "icon-".

For each of these image files it finds, it tries to put a new image into *each* of the following Android directories:

**drawable** (1x, or ~160 dpi)<br/>
Copies the image here.<br/>

**drawable-hdpi** (1.5x, or ~240 dpi)<br/>
If there's a @2x version of the image, the script resizes it to 75%, removing the @2x.<br/>
If there's a @3x version of the image, the script resizes it to 50%, removing the @3x.<br/>

**drawable-xhdpi** (2x, or ~320 dpi)<br/>
If there's a @2x version of the image, the script copies it, removing the @2x.<br/>
If there's a @3x version of the image, the script resizes it to 67%, removing the @3x.<br/>

**drawable-xxhdpi** (3x, or ~480 dpi)<br/>
If there's a @3x version of the image, the script copies it, removing the @3x.<br/>
If there's a @2x version of the image, the script resizes it to 150%, removing the @2x.<br/>

All images in the iOS directory are left untouched.

The script does not deal with `@4x` images on the iOS side, nor any of the other possible `drawable-????` directories on the Android side.


## License

The MIT License (MIT)

Copyright (c) 2016 Michael Lorenz

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
