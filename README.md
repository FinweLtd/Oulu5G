# Oulu5G

Repository for Oulu 5G Demo (360 Live Video Stream with IoT Data Hotspots)

Abstract
--------

During the event, live 360 video stream from a Nokia Ozo camera can be viewed via several mobile devices / headsets. The video player allows embedding hotspots with live sensor data on top of the 360 degree video background. Companies can reserve a hotspot of their own, customize it, and publish their own IoT sensor data for everyone to see.

This repository contains information about data formats etc. details. As an example case, we will consider publishing the readings of a simple temperature sensor from an office building that is physically visible from the location of the Ozo camera.

Data Format
-----------

A single sensor data reading needs to be packaged to a text file with UTF-8 encoding and the following JSON format:

```json
{
  "sensors": [
    {
      "id": "fi.finwe.temperature.office",
      "title": "Office Temp",
      "value": "21.0° C",
      "timestamp": "2017-04-19T10:15:30.123Z",
      "image_uri": "https://github.com/FinweLtd/Oulu5G/blob/master/temp_20.png",
      "coordinates": {
        "lat": "45.0",
        "lon": "112.0"
      }
    }
}
```

The JSON file contains an array of sensors (it is OK to have only one sensor). Each sensor will be mapped to one hotspot on top of the video layer. You must use only only sensor/hotspot unless you have reserved more than one.

Each sensor has an id string that needs to be unique. It will not be shown to users. The recommened format is the Java package name style:
https://docs.oracle.com/javase/tutorial/java/package/namingpkgs.html

The title string does not need to be unique, but it should be as short as possible. The title can be shown to users.

The value string contains the sensor measurement. The maximum is 160 characters, but reading long texts via a VR headset isn't very comfortable, so keep it as short as possible.

The timestamp is an optional field where you specify the exact time when the measurement was taken. Use XML time with UTC time zone (notice the Z character in the end of the string):
http://books.xmlschemata.org/relaxng/ch19-77049.html

The primary UI resource for the hotspot is its icon image. Provide here a URI to your own icon file. Notice that you can change the icon based on sensor reading if you wish, or use the same image all the time. If you decide to use multiple images, please keep it within 1-5 image files that we can easily cache on the player devices.

The coordinates define the hotspot's location on the spherical 360 video background, i.e. the direction of your sensor from the physical location of the camera. These needs to be tuned once the exact location and orientation of the camera and the sensor is known. They are given in degrees using two values: latitude and longitude.

Hotspot Icon
------------

The icon image of the hotspot should be clean, distinguishable logo that presents your company / sensor type / data value. We accept only 256x256 pixel 32-bit PNG images with alpha channel (part of the image can be transparent).

Upload the image file(s) on a web server and reference to the image you wish to use in your sensor's JSON field 'image_uri'. The player will download the image and render it to the direction specified by lat, lon coordinates.

Example images can be found here:

https://github.com/FinweLtd/Oulu5G/blob/master/temp_20.png
https://github.com/FinweLtd/Oulu5G/blob/master/temp_40.png
https://github.com/FinweLtd/Oulu5G/blob/master/temp_60.png
https://github.com/FinweLtd/Oulu5G/blob/master/temp_80.png
https://github.com/FinweLtd/Oulu5G/blob/master/temp_100.png

Publishing Sensor Readings
--------------------------

After reading a new measurement from your sensor, and packaging it to a JSON file, you will need to publish it via HTTP POST to our server. The server runs on Amazon AWS and combines input from all sensors to a single JSON file that will be fetched by the player devices a few times per second.

The URL of the backend server will be published soon with instructions for posting your own data.
