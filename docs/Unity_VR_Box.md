# Creating a VR Boxing Game

So, you've decided to create a VR game with skeleton tracking but don't know what to start with? Well, you're at the right page because in this tutorial you'll learn how to create a VR game using **Nuitrack**. To plunge into the subject and to understand how to animate your avatar's skeleton in a VR game, we recommend you to take a look at our tutorial [Animating the Avatar using Skeleton](http://download.3divi.com/Nuitrack/doc/UnityAnimation_page.html). 

In this tutorial, we'll create a simple VR game, in which your avatar is a boxer and you have to punch a dummy as hard as you can. The maximum punch speed will be measured and displayed at the top of the screen. To make this game, you'll need the <b>Nuitrack SDK</b>, a sensor (from the list of supported sensors, see the [Nuitrack website](https://nuitrack.com/)) and a VR headset (any Mobile VR headset or Gear VR headset).

You can find the finished project in <b>Nuitrack SDK</b>: <b>Unity 3D → NuitrackSDK.unitypackage → Tutorials → Box</b> 

@image html images/Ubox_9.gif 
@image latex images/Ubox_9.gif 

@section box_environment Creating VR Environment and animating the Avatar 

<ol>
<li> Create a new Unity project. 
<li> Import <b>NuitrackSDK.unitypackage</b> from the <b>Nuitrack SDK: Assets → Import Package → Custom Package</b>. Tick the following folders that we'll need for work:
<ul>
<li> <b>Nuitrack</b> (includes all Nuitrack modules and is responsible for skeleton detection, tracking, etc. Also, this folder includes a Head prefab that is used in VR games, and a file with settings for Nuitrack automatic calibration); 
<li> <b>Platform Changer</b> (changes the Unity settings (<b>PlayerSettings</b>) according to the selected platform); 
<li> <b>Plugins</b> (includes <b>Android Manifest</b> that specifies permissions and other settings of your app);
<li> <b>Resources</b> (stores the <b>Platform Changer</b> settings);
<li> <b>Tutorials → Box</b> (we need this folder for our project because it includes the Dummy prefab. Also, you can find here a finished VR Boxing Game for reference);
<li> <b>Tutorials → Avatar Animation</b> (includes the Unitychan prefab that we'll use to display our Boxer (very interesting choice, huh?);
<li> <b>VicoVRCalibrationRGB</b> (includes all the necessary components for calibration in the T-Pose. Calibration is required for any VR game).
</ul>

@note
<b>Nuitrack</b> provides improved automatic calibration for Android and Gear VR. Any gyroscope has one unpleasant feature: when you turn the head during a game, its position may become slightly distorted and rotation is not aligned back, which may affect your gaming experience. Fortunately, <b>Nuitrack automatic calibration</b> helps to eliminate this nuisance. By default, automatic calibration is enabled, however, if you'd like to disable it for some reason, you can do it using the <b>VicoVR app: Settings → Developer Options → Autocalibration (Off)</b>.

<li> Create a new Scene: <b>File → New Scene</b>. Drag-and-drop the following prefabs to the Scene:
<ul>
<li> <b>NuitrackScripts</b> (includes the <b>Nuitrack Manager</b> components (initiates Nuitrack  and its modules), <b>Current User Tracker</b> (responsible for tracking of a current user), <b>T-Pose Calibration</b> (determines the calibration settings), <b>Calibration Info</b> (stores the calibration settings);
<li> <b>RiggedAvatar2</b> (includes all the components for direct skeleton mapping. In this game, we'll animate the avatar using direct mapping to make its moves more accurate and smooth. Learn more about the difference between direct and indirect mapping in our tutorial [Animating the Avatar using Skeleton](http://download.3divi.com/Nuitrack/doc/UnityAnimation_page.html);
<li> <b>HeadParent</b> (head of our avatar; contains the camera and scripts that define the rotation of the head). 
</ul>
<li> Hide the head of the Unitychan prefab so that you won't see it from the inside during our VR game: <b>Character1_Neck → Transform → Scale (0; 0; 0)</b>

@image html images/Ubox_1.png Hiding the Head
@image latex images/Ubox_1.png Hiding the Head

<li> In the <b>NuitrackAvatar</b> settings, drag-and-drop the <b>HeadParent</b> to <b>Element 16</b>,  so that our avatar will be able to move her head.

@image html images/Ubox_2.png Setting Up the Head
@image latex images/Ubox_2.png Setting Up the Head

@note
The default platform in a project is <b>Android</b>. Before building the project, you can select the appropriate platform: <b>iOS, Android (default) or GearVR</b>. To do that, select <b>Windows → Platform Changer → TargetPlatform → Change Platform</b>.

<li> Build and run the project. Calibration should run and reach 100%. After that, check that your avatar is displayed correctly and her movements correspond to yours. 
</ol>

@image html images/Ubox_3.gif Calibration in the T-Pose
@image latex images/Ubox_3.gif Calibration in the T-Pose

@section box_dummy Setting Up the punching Dummy

<ol>
<li> Now, when we've created our avatar, it's time to introduce our victim - a punching dummy (or, alternatively, you can use the avatar of a person who pisses you off - just kidding).  Drag-and-drop the <b>PunchingDummy</b> prefab to the Scene. 
<li> Create a new script <i>PunchSpeedMeter.cs</i>. In this script, we'll determine when the dummy appears and also define all the settings associated with the speed of our punch. In the script, create fields for the dummy and transformTarget (which is a spine joint; we use this very joint because this is the first moving joint in the hierarchy and it defines the moves of other body joints). 

@code
[SerializeField] GameObject dummy;
[SerializeField] Transform transformTarget;
@endcode

<li> Create the <i>Awake</i> method that will be executed at startup: deactivate the dummy so that it isn't displayed during calibration.  

@code
private void Awake()
{
	dummy.SetActive(false);
}
@endcode

<li> Create the <i>OnEnable</i> method. Subscribe to the <i>onSuccess</i> event from the <i>TPoseCalibration</i> script. It's called if calibration is successful. 

@code
private void OnEnable()
{    
	TPoseCalibration.Instance.onSuccess += OnSuccessCalibration;
}
@endcode

<li> Create the <i>OnSuccessCalibration</i> method and specify the required argument <i>(Quaternion rotation)</i>. In this method, we define the initial position of the dummy in front of our avatar. The dummy is activated after successful calibration and stands at a distance of 1m from the user and 1m below the target joint set in the editor (you can specify any distance that you want). 

@code
void OnSuccessCalibration(Quaternion rotation)
{
	dummy.SetActive(true);
	transform.position = transformTarget.position + new Vector3(0, -1, 1);
}
@endcode

<li> Don't forget to unsubscribe from the <i>OnSuccessCalibration</i> event.

@code
private void OnDisable()
{
	TPoseCalibration.Instance.onSuccess -= OnSuccessCalibration;
}
@endcode 

<li> Drag-and-drop the script to the dummy. In Unity, drag-and-drop the <b>Dummy</b> to <b>Dummy</b> and <b>Character1_Spine</b> to <b>Transform Target</b> (the dummy will be set in relation to this joint). 

@image html images/Ubox_4.png Setting Up the Punching Dummy
@image latex images/Ubox_4.png Setting Up the Punching Dummy

<li> Run the project and check that the dummy is displayed correctly. At this stage, you won't be able to punch him because we haven't set up the hands of our character yet, so, the hand will just move through the dummy.
</ol>

@image html images/Ubox_5.gif You should see the dummy after calibration
@image latex images/Ubox_5.gif You should see the dummy after calibration

@section box_speed Punching the Dummy and displaying the Punch Speed

<ol>
<li> Have you ever played on the 'Punching Bag' game machine that measures the force of your punch? In this project, you'll have the opportunity to break your record several times: the maximum speed of your punch will be displayed above the dummy and will be updated every time the record is broken. In the <i>PunchSpeedMeter.cs</i> script, add the <i>UI</i> namespace, which we need to display text with information about the speed of our punch. Also, create the <i>Text</i> field.

@code
using UnityEngine.UI;

public class PunchSpeedMeter : MonoBehaviour {
	...
	[SerializeField] Text speedMeterText;
	...
}
@endcode 

<li> In the <i>PunchSpeedMeter</i> class, add the <i>maximumPunchSpeed</i> variable to store the information about the maximum speed of a punch. 

@code
float maximumPunchSpeed = 0;
@endcode

<li> Create the <i>CalculateMaxPunchSpeed</i> method (which takes the punch speed). If the new speed is greater than the current speed, the counter will be updated, otherwise the result will not change. 

@code
public void CalculateMaxHitSpeed(float speed)
{
	if (maximumPunchSpeed < speed)
	    maximumPunchSpeed = speed;
	speedMeterText.text = maximumPunchSpeed + " m/s ";
}
@endcode

<li> Create a new script <i>PunchSpeedSender.cs</i>. We need a handler for punches, since we can punch the dummy in several body parts (<b>TorsoBone</b> and <b>NeckBone</b>). When you punch the dummy in one of these parts, the <b>PunchSpeedSender</b> sends the info about that event to the <b>PunchSpeedMeter</b>. In this script, make a reference to the <b>PunchSpeedMeter</b> and create the  <i>OnCollisionEnter</i> method. When two objects collide, we'll get the relative punch speed. 

@code
public class HitSpeedSender : MonoBehaviour {
 
	[SerializeField] HitSpeedMeter hitSpeedMeter;
 
	private void OnCollisionEnter(Collision collision)
	{
		hitSpeedMeter.CalculateMaxHitSpeed(collision.relativeVelocity.magnitude);
	}
}
@endcode

@note
The <i>relativeVelocity</i> property allows to determine the correct collision speed of two objects (in our case, these are the Unitychan's fist and the dummy). In case we only determined the speed of the fist, we'd get an incorrect value.  To learn more about <i>relativeVelocity</i>, click [here](https://docs.unity3d.com/ScriptReference/Collision-relativeVelocity.html). 

<li> In Unity, drag-and-drop this script to the <b>TorsoBone</b> and <b>NeckBone</b>. In the <b>Punch Speed Meter</b> field, specify <b>PunchSpeedMeter</b>. 

@image html images/Ubox_6.png Punch Speed Sender Settings
@image latex images/Ubox_6.png Punch Speed Sender Settings

<li> In Unity, create two objects for the 'boxing gloves' of our avatar: <b>Create → Empty Object → Left Glove, Right Glove</b>. Actually, our avatar will punch the dummy with her bare hands, but we need to attach imaginary 'boxing gloves' to her hands in order to calculate the punch speed. Let's add the necessary components: <b>Add Component → Capsule Collider, RigidBody</b> to handle the collisions correctly. Select the direction for glove movement along the arm: <b>Capsule Collider → Direction → X-Axis</b>. Set the position and size of the 'gloves': <b>Height → 0.2</b> (optimum), <b>Radius → 0.05</b>. If you want to display the boxing gloves, then create the appropriate prefab and apply the above settings to it. 

@image html images/Ubox_7.png Setting Up the Gloves
@image latex images/Ubox_7.png Setting Up the Gloves

<li> Create the script <i>RigidbodyFollower.cs</i> and describe the behavior of our 'gloves'. Create the <i>target</i> field  so that our 'gloves' will follow these target objects (in our case, these are the avatar's hands). Also, create the <i>rigidbody</i> field. 

@code
public class RigidbodyFollower : MonoBehaviour
{
	[SerializeField] Transform target;
 
	Rigidbody rigidbody;
}
@endcode

<li> At the start of the game, a reference to the Rigidbody that's attached to the capsule will be received. 

@code
void Start()
{
	rigidbody = GetComponent<Rigidbody>();
}
@endcode

<li> Create the <i>FixedUpdate</i> method. In this method, we'll describe the behavior of the capsule: it will move (position, rotation) according to the target transform. In this project we use <i>MovePosition</i> instead of <i>transform.position</i> because the gloves of our avatar should move using the RigidBody Physics. A simple change of position may be processed incorrectly because every time the transform would 'teleport' ignoring the physics.  

@code
void FixedUpdate()
{
	rigidbody.MovePosition(target.position);
	rigidbody.MoveRotation(target.rotation);
}
@endcode

@note
We use <i>FixedUpdate</i> instead of <i>Update</i> in case we are dealing with <i>Rigidbody</i>.

<li> Drag-and-drop the script to the right and left glove and specify the <b>Target (Character1_LeftHand, Character1_RightHand)</b>.

@image html images/Ubox_8.png Setting Up the Rigidbody Follower
@image latex images/Ubox_8.png Setting Up the Rigidbody Follower

<li> Run the project and punch the dummy. Check the displayed speed of your punch: it should be displayed above the dummy. Try to break your record several times! 

@image html images/Ubox_9.gif Beat him up!
@image latex images/Ubox_9.gif Beat him up!

Congratulations! You've just created a VR Boxing Game with skeleton tracking! 
</ol>
*/
