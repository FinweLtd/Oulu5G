# Oulu5G
========

Repository for Oulu 5G Demo (360 Live Video Stream with Data Hotspots)

Abstract
--------

During the event, live 360 video stream from Nokia Ozo camera can be viewed via mobile devices / headsets. The video player allows embedding hotspots with live sensor data on top of the 360 degree video background. Companies can reserve a hotspot of their own, customize it, and publish their own IoT sensor data for everyone to see. 

This repository contains information about data formats etc. details. As an example case, we will consider publishing the readings of a simple temperature sensor from an office building that is physically visible from the location of the Ozo camera.

Data Format
-----------

A single sensor data reading needs to be packaged to a text file with UTF-8 encoding and the following JSON format:

{
  "sensors": [
    {
      "id": "fi.finwe.temperature.office",
      "title": "Office Temp",
      "value": "21.0° C",
      "timestamp": "2017-04-19T10:15:30.123Z",
      "image_uri": "http://www.finwe.fi/sensor.png",
      "coordinates": {
        "lat": "45.0",
        "lon": "112.0"
      }
    }
}

The JSON file contains an array of sensors. It is ok to have only one sensor. Each sensor will be mapped to one hotspot on top of the video layer.

Each sensor has an id string that needs to be unique. It will not be shown to users. The recommened format is Java package name style:
https://docs.oracle.com/javase/tutorial/java/package/namingpkgs.html

The title string does not need to be unique, but it should be as short as possible. Title can be shown to users.

The value string contains the sensor measurement. The maximum is 160 characters, but reading long texts via VR headset isn't very comfortable, so keep it as short as possible.

The timestamp is an optional field where you specify the exact time when the measurement was taken. Use XML time with UTC time zone (notice the Z character in the end of the string).

The primary UI resource for the hotspot is its icon image. Provide here a URI to your icon file. Notice that you can change the icon based on sensor reading if you wish, or use a single image all the time. If you decide to use multiple images, please keep it within 1-5 images that we can easily cache on the player device.

The coordinates define the hotspot's location on the spherical 360 image, i.e. the direction of your sensor from the physical location of the camera. These needs to be tuned once the exact location and orientation of the camera is known. They are given in degrees using two values: latitude and longitude.


















