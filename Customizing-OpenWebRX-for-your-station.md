You can do several things to apply a unique look and feel to your receiver.

## Top photo
The original idea behind the top photo is to show a panorama from the receiver site.

You can replace the image in `htdocs/gfx/openwebrx-top-photo.jpg` with your own 2048×615 JPEG file.

The original one looks like this: 

![](https://raw.githubusercontent.com/simonyiszk/openwebrx/master/htdocs/gfx/openwebrx-top-photo.jpg)

Originally I imagined that a HD webcam could be placed at the receiver site, and the user would see how the landscape looks at the given moment. If someone wants to implement that, just has to replace the URL to the image in `htdocs/index.wrx`.

## Avatar
The avatar is shown both at the top bar and at [sdr.hu](http://sdr.hu) next to the name of the receiver (if it is listed).

You should replace it with your own logo of 256×256 sized PNG.

This is how the stock avatar looks like:

![](https://raw.githubusercontent.com/simonyiszk/openwebrx/master/htdocs/gfx/openwebrx-avatar.png)

## "Web GUI configuration" settings in config_webrx.py
They are quite self explanatory. Do not forget to remove the first line of the value of *photo_desc*:
`You can add your own background photo and receiver information.<br />`