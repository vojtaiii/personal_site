---
title: "Convert a PNG image to XML vector for Android use"

date: 2021-04-27T21:00:00-04:00
  - tools
tags:
  - android
---

XML vector images are for their little weight and simplicity commonly used in Android (and many other systems). You can find them, for example, as app icons sitting on the very top of the phone screen.

![alt text][icon_prev]

For some reason, I find the process of creating a simple xml vector from a raster image, a procedure undertaken by almost every app developer for Android, not so straightforward as it might seem.
One might say, use the [vector asset tool](https://developer.android.com/studio/write/vector-asset-studio), which is part of Android studio, and it will do the job for you. 
However, the tool had never correctly worked for me. The PSD (photoshop document) or SVG files I have solemnly chosen for the conversion rendered blank preview screen and some error logs. 

Scratching the surface of an Android developer site about conditions that apply for PSD or SVG files for a proper conversion I gave up. Instead of trying to find how to make vector asset tool working
I googled the heck out of stack overflow and finally arrived at a functional solution. Months later, when encountering the same requisite for another app, I, in horror, realised the procedure went to oblivion in my mind. Luckily being able to trace back the solution, I am posting it here for the sake of my future work and maybe yours.

## Steps to reproduce

1. Have your PNG file - size should not exceed 200x200px due to longer rendering, and the background should be one color. The color of the image itself does not matter so much. My example:

    ![alt text][sumys_png]

    **Note:** If you created the image using graphical software, make sure that you *flatten* it before the export. This way, no layer complexity and other features remain. 
    E. g. in photoshop, go to `layer -> flatten image`. 

2. Now convert the given flat PNG to SVG. There is a variety of choice for this but inspired by [this thread](https://stackoverflow.com/questions/52670937/how-do-i-convert-pngs-directly-to-android-vector-drawables)
    I recommend the [Autotracer tool](https://www.autotracer.org/), an online image vectorizer.

3. From the SVG its now time to create XML. This could be possibly done in Android studio vector asset. Nevertheless, I sometimes struggled. A simple and useful online implementation can be found here: [http://inloop.github.  io/svg2android/](http://inloop.github.io/svg2android/). You can even see the generated xml code after the conversion. 

4. And this might be the result. But, the problem you'll probably encounter is that the background is still there, as shown in the picture below. Hence, the shown icon would be just a white rectangle. How to get rid of the    background? 

    ![alt text][sumys_background]

    Well, peek into the xml code, and most probably at the beginning is the background path. If it's white, it might be something like:

    "`xml
    <path
        android:fillColor="#ffffff"
        android:strokeWidth="1"
        android:pathData="M0 0L0 302L270 302L270 0L0 0z" />
    ```

    Now, delete this part, and you'll get the desired result.

![alt text][sumys_nobackground]

![alt text][sumys_final]

[icon_prev]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/png_xml/icon_prev.JPG?raw=true
[sumys_png]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/png_xml/sumys_notification.png?raw=true
[sumys_background]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/png_xml/icon_background.JPG?raw=true
[sumys_nobackground]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/png_xml/icon_nobackground.JPG?raw=true
[sumys_final]: https://github.com/vojtaiii/personal_site/blob/gh-pages/assets/images/png_xml/icnon_final.JPG?raw=true