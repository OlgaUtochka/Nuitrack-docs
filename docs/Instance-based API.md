# Nuitrack Instance-based API [Beta]

Current implementation of the Instance-based API allows you to get information about faces of users with detected skeletons.

Currently supported platforms:
* Ubuntu amd64
* Windows x86/x86_64

### Note
Face modules are by default disabled. To enable face modules, open *nuitrack.config* file and set *Faces.ToUse* and *DepthProvider.Depth2ColorRegistration* to `true`.
You should also set the value of *AstraProPerseeDepthProvider.Windows.CameraID* for Orbbec Astra PRO camera on Windows.

## JSON Getting Information about Instances

Nuitrack allows to get information about people standing in front of the camera in JSON format. To get the information, call the @ref tdv::nuitrack::Nuitrack::getInstancesJson "Nuitrack::getInstancesJson" (C++) or @ref nuitrack.Nuitrack.GetInstancesJson "Nuitrack.GetInstancesJson" (CSharp) function. The result will be returned in the form of a JSON string.<br>
The JSON string includes the following properties:

* <b>Timestamp</b> - frame timestamp in microseconds;
* <b>Instances</b> - array with instances tracked by Nuitrack:
     * <b>id</b> - identifier of an instance, corresponds to user segment id;
     * <b>class</b> - class of an instance;
     * <b>face</b> - characteristics of a detected person’s face:
          * <b>rectangle</b> - normalized screen coordinates of a face rectangle in the image:
              * <b>left</b> - X coordinate of the upper-left corner of the rectangle;
              * <b>top</b> - Y coordinate of the upper-left corner of the rectangle;
              * <b>width</b> - rectangle width;
              * <b>height</b> - rectangle height;
          * <b>landmark</b> - facial landmarks. The <i>singlelbf</i> set of anthropometric points is used (31 points). Normalized coordinates of each point from the set are returned. ![Singlelbf set of points](@ref images/facerec/singlelbf.png)
          * <b>left_eye</b> - normalized coordinates of the center of a person’s left eye;
          * <b>right_eye</b> - normalized coordinates of the center of a person’s right eye;
          * <b>angles</b> - face orientation angles in degrees:
              * <b>yaw</b> - yaw angle;
              * <b>pitch</b> - pitch angle;
              * <b>roll</b> - roll angle;
          * <b>emotions</b> - emotion scores for a person’s face. Confidence degree for each emotion is returned as a positive real number in the range of [0; 1]. <i>Values</i>: `neutral | angry | happy | surprise`
          * <b>age</b> - estimated age of a person:
              * <b>type</b> - age group of a person depending on his/her age. <i>Values</i>: `kid | young | adult | senior`
              * <b>years</b> - estimated age of a person, returned as a real positive number.
          * <b>gender</b> - estimated gender of a person. <i>Values</i>: `male | female`

Here is an example of output JSON data:
@verbinclude Instance_based_API.json
*/
