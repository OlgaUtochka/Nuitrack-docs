# Nuitrack Instance-based API [Beta]

Current implementation of the Instance-based API allows you to get information about faces of users with detected skeletons.

Currently supported platforms:
* Ubuntu amd64
* Windows x86/x86_64

### Note
Face modules are by default disabled. To enable face modules, open *nuitrack.config* file and set ```Faces.ToUse``` and ```DepthProvider.Depth2ColorRegistration``` to `true`.
You should also set the value of ```AstraProPerseeDepthProvider.Windows.CameraID``` for Orbbec Astra PRO camera on Windows.

## JSON Getting Information about Instances

Nuitrack allows to get information about people standing in front of the camera in JSON format. To get the information, call the ```tdv::nuitrack::Nuitrack::getInstancesJson "Nuitrack::getInstancesJson"``` (C++) or ```nuitrack.Nuitrack.GetInstancesJson "Nuitrack.GetInstancesJson"``` (C#) function. The result will be returned in the form of a JSON string.
The JSON string includes the following properties:

* **Timestamp** - frame timestamp in microseconds;
* **Instances** - array with instances tracked by Nuitrack:
     * **id** - identifier of an instance, corresponds to user segment id;
     * **class** - class of an instance;
     * **face** - characteristics of a detected person’s face:
          * **rectangle** - normalized screen coordinates of a face rectangle in the image:
              * **left** - X coordinate of the upper-left corner of the rectangle;
              * **top** - Y coordinate of the upper-left corner of the rectangle;
              * **width** - rectangle width;
              * **height** - rectangle height;
          * **landmark** - facial landmarks. The *singlelbf* set of anthropometric points is used (31 points). Normalized coordinates of each point from the set are returned. ![Singlelbf set of points](@ref images/facerec/singlelbf.png)
          * **left_eye** - normalized coordinates of the center of a person’s left eye;
          * **right_eye** - normalized coordinates of the center of a person’s right eye;
          * **angles** - face orientation angles in degrees:
              * **yaw** - yaw angle;
              * **pitch** - pitch angle;
              * **roll** - roll angle;
          * **emotions** - emotion scores for a person’s face. Confidence degree for each emotion is returned as a positive real number in the range of [0; 1]. *Values*: `neutral | angry | happy | surprise`
          * **age** - estimated age of a person:
              * **type** - age group of a person depending on his/her age. <i>Values</i>: `kid | young | adult | senior`
              * **years** - estimated age of a person, returned as a real positive number.
          * **gender** - estimated gender of a person. <i>Values</i>: `male | female`

Here is an example of output JSON data:
@verbinclude Instance_based_API.json
*/
