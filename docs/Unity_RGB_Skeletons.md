# Displaying Skeletons on an RGB Image 

In this tutorial, you'll learn how to display a color image from your sensor in a Unity project and display the skeletons of users received from Nuitrack.

You can find the finished project in **Nuitrack SDK: Unity 3D → NuitrackSDK.unitypackage → Tutorials → RGB and Skeletons** 

To create this project, you'll need just a couple of things:
* Nuitrack Runtime and Nuitrack SDK
* Any supported sensor (see the complete list at [Nuitrack website](https://nuitrack.com/))
* Unity 2017.4 or higher 

<p align="center">
<img width="500" src="https://github.com/OlgaUtochka/Nuitrack-docs/blob/master/images/Urgb_1.gif">
</p>

## Setting Up the Scene and Getting RGB Output from a Sensor 

<ol>
<li> Create a new project.
<li> Download <b>Nuitrack SDK</b> and import <b>NuitrackSDK.unitypackage</b> to your project (<b>Assets → Import Package → Custom Package...</b>) except <b>Tutorials/RGBandSkeletons/FinalAssets</b> and <b>Tutorials/FaceTracker/Final Assets</b>. 
<li> Drag-and-drop the <b>NuitrackScripts</b> prefab to the scene (this prefab allows you to use Nuitrack modules in your project). 
<li> In the <b>Nuitrack Manager</b> section of the <b>NuitrackScripts</b> prefab, tick the necessary modules: <b>Color Module On</b> (to display the RGB image), <b>Skeleton Tracker Module On</b> (for skeleton tracking, as you can guess).

@image html images/Urgb_2.png  
@image latex images/Urgb_2.png 

<li> Create a new script and name it <i>DrawColorFrame</i>. In this script, determine the output of an RGB image to the scene. 
<li> Create a public field <i>RawImage background</i> to display the image. In <i>Start</i>, subscribe to update each color frame. 

@code
using UnityEngine;
using UnityEngine.UI;
 
public class DrawColorFrame : MonoBehaviour
{
	[SerializeField] RawImage background;
 
	void Start()
	{
		NuitrackManager.onColorUpdate += DrawColor;
	}
}
@endcode

@note
Why do we use <i>RawImage</i> instead of <i>Image</i> in this project? <i>Image</i> is used for displaying Sprites only. <i>RawImage</i> is used for displaying any type of texture. Sprites are easier to work with, but <i>Sprite.Create</i> is an expensive operation. It takes a comparatively long time and uses a lot of memory. By using a <i>RawImage</i> you can skip the step of creating a sprite. <i>RawImage</i> accepts the texture, which we've created with <i>ToTexture2D()</i> from the data received from Nuitrack (frame).

<li> In the <i>DrawColor</i> method, get the texture of <i>ColorFrame</i> and pass it to the <i>background</i> texture.

@code
public class DrawColorFrame : MonoBehaviour
{
…
	void DrawColor(nuitrack.ColorFrame frame)
	{ 
		background.texture = frame.ToTexture2D();
	}
}
@endcode

<li> Create a new <b>Canvas</b> on the scene (<b>Create → UI → Canvas</b>). After this, create a child object to the <b>Canvas</b>: <b>Create → UI → Raw Image</b>. The received texture (the image from a sensor) is stretched across the <b>Raw Image</b>.  
<li> In the <b>Raw Image</b> settings, select <b>Anchor Presets</b>, press <b>Alt</b> and stretch the object across the width and height of the <b>Canvas</b>.

@image html images/Urgb_3.png  
@image latex images/Urgb_3.png 

<li> Rotate <b>RawImage</b> by 180 degrees along X (otherwise, the output image will be inverted).

@image html images/Urgb_4.png  
@image latex images/Urgb_4.png 

<li> Rename <b>Canvas</b> to <b>ColorFrameCanvas</b> and add the <i>DrawColorFrame</i> script to it. 
<li> Drag-and-drop the <b>Raw Image</b> to the <b>Background</b> field of the script. 

@image html images/Urgb_5.png  
@image latex images/Urgb_5.png 

<li> Run the project. You should see a color image from the sensor displayed on the screen. 
</ol>

@image html images/Urgb_6.gif  
@image latex images/Urgb_6.gif 

@section rgb_skeleton Displaying the Skeleton

<ol>
<li> All right, color image from the sensor is fun, but we're interested in the skeletons of users (otherwise, what do we need Nuitrack for?) Let's start tracking and displaying skeletons. First of all, turn on <b>depth-to-color registration</b> because a depth map doesn't accurately match an RGB image and we have to align them. To turn on depth-to-color registration, you have to open <i>nuitrack.config</i> and set <i>DepthProvider.Depth2ColorRegistration</i> to true. 
<li> Create two prefabs to display a user's skeleton (a ball for a joint and a line for a connection). As an option, you can use the prefabs from <b>NuitrackSDK.unitypackage</b> (<b>Assets/NuitrackSDK/Tutorials/RGBandSkeletons/Prefabs (JointUI and ConnectionUI)</b>). 
<li> Create a new script and name it <i>SimpleSkeletonAvatar</i>. In this script, determine tracking and displaying of a single skeleton. 
<li> Create a new bool variable <i>autoProcessing</i>. If we need to process only one skeleton, we set the value of this variable to <i>true</i> and Nuitrack passes all data about this skeleton. At this stage, this is quite sufficient because we want to display only one skeleton. However, when it comes to displaying several skeletons, we'll need additional components, because multiple skeletons cannot be processed automatically. 
<li> Create two fields for the joint and connection prefabs: <i>jointPrefab</i> and <i>connectionPrefab</i>.

@code
using System.Collections.Generic;
using UnityEngine;
 
public class SimpleSkeletonAvatar : MonoBehaviour
{
	public bool autoProcessing = true;
	[SerializeField] GameObject jointPrefab = null, connectionPrefab = null;
}
@endcode

<li> Create the <i>jointsInfo</i> array with the list of all joints that are needed to display the skeleton. All in all, there are 19 joints tracked by Nuitrack. 

@code
public class SimpleSkeletonAvatar : MonoBehaviour
{
…
	nuitrack.JointType[] jointsInfo = new nuitrack.JointType[]
	{
		nuitrack.JointType.Head,
		nuitrack.JointType.Neck,
		nuitrack.JointType.LeftCollar,
		nuitrack.JointType.Torso,
		nuitrack.JointType.Waist,
		nuitrack.JointType.LeftShoulder,
		nuitrack.JointType.RightShoulder,
		nuitrack.JointType.LeftElbow,
		nuitrack.JointType.RightElbow,
		nuitrack.JointType.LeftWrist,
		nuitrack.JointType.RightWrist,
		nuitrack.JointType.LeftHand,
		nuitrack.JointType.RightHand,
		nuitrack.JointType.LeftHip,
		nuitrack.JointType.RightHip,
		nuitrack.JointType.LeftKnee,
		nuitrack.JointType.RightKnee,
		nuitrack.JointType.LeftAnkle,
		nuitrack.JointType.RightAnkle
	};
}
@endcode

<li> Create a 2D array containing all the connections. Specify the initial and end joints (a connection is created between these two joints).  

@code
public class SimpleSkeletonAvatar : MonoBehaviour
{
…
	nuitrack.JointType[,] connectionsInfo = new nuitrack.JointType[,]
	{ 
		{nuitrack.JointType.Neck,			nuitrack.JointType.Head},
		{nuitrack.JointType.LeftCollar,		nuitrack.JointType.Neck},
		{nuitrack.JointType.LeftCollar, 	nuitrack.JointType.LeftShoulder},
		{nuitrack.JointType.LeftCollar, 	nuitrack.JointType.RightShoulder},
		{nuitrack.JointType.LeftCollar, 	nuitrack.JointType.Torso},
		{nuitrack.JointType.Waist,			nuitrack.JointType.Torso},
		{nuitrack.JointType.Waist,			nuitrack.JointType.LeftHip},
		{nuitrack.JointType.Waist,			nuitrack.JointType.RightHip},
		{nuitrack.JointType.LeftShoulder, 	nuitrack.JointType.LeftElbow},
		{nuitrack.JointType.LeftElbow, 		nuitrack.JointType.LeftWrist},
		{nuitrack.JointType.LeftWrist, 		nuitrack.JointType.LeftHand},
		{nuitrack.JointType.RightShoulder, 	nuitrack.JointType.RightElbow},
		{nuitrack.JointType.RightElbow, 	nuitrack.JointType.RightWrist},
		{nuitrack.JointType.RightWrist, 	nuitrack.JointType.RightHand},
		{nuitrack.JointType.LeftHip, 		nuitrack.JointType.LeftKnee},
		{nuitrack.JointType.LeftKnee, 		nuitrack.JointType.LeftAnkle},
		{nuitrack.JointType.RightHip, 		nuitrack.JointType.RightKnee},
		{nuitrack.JointType.RightKnee, 		nuitrack.JointType.RightAnkle}
	};
}
@endcode

<li> Create an array containing ready-made connections and a dictionary with ready-made joints (the key is a joint type and the value is a spawned object). 

@code

public class SimpleSkeletonAvatar : MonoBehaviour
{
…
	GameObject[] connections;
	Dictionary<nuitrack.JointType, GameObject> joints;
}
@endcode

<li> In <i>Start</i>, call the <i>CreateSkeletonParts</i> method. 

@code
public class SimpleSkeletonAvatar : MonoBehaviour
{
…
	void Start()
	{
		CreateSkeletonParts();
	}
}
@endcode

<li> In the <i>CreateSkeletonParts</i> method, create all joints and connections. Joints are spawned, turned off and added to the list of joints. Do the same thing with all connections. Joints and connections are "turned on" as soon as a skeleton is detected. 

@code
public class SimpleSkeletonAvatar : MonoBehaviour
{
…    
	void CreateSkeletonParts()
	{
		joints = new Dictionary<nuitrack.JointType, GameObject>();
 
		for (int i = 0; i < jointsInfo.Length; i++)
		{
			if (jointPrefab != null)
			{
				GameObject joint = Instantiate(jointPrefab, transform, true);
				joint.SetActive(false);
				joints.Add(jointsInfo[i], joint);
			}
		}
 
		connections = new GameObject[connectionsInfo.GetLength(0)];
 
		for (int i = 0; i < connections.Length; i++)
		{
			if (connectionPrefab != null)
			{
				GameObject connection = Instantiate(connectionPrefab, transform, true);
				connection.SetActive(false);
				connections[i] = connection;
			}
		}
	}
}
@endcode

<li> Create a new method <i>ProcessSkeleton</i> for processing the skeleton received from Nuitrack. Check for null values (if a skeleton isn't detected, the other part of the method won't be executed). 

@code
public class SimpleSkeletonAvatar : MonoBehaviour
{
…
	public void ProcessSkeleton(nuitrack.Skeleton skeleton)
	{
		if (skeleton == null)
			return;
	}
}
@endcode

<li> Loop over the joints. If a specific joint is detected (confidence > 0.5), it's displayed and its position is set in 2D (projective coordinates received from Nuitrack are used). Otherwise, the joint is not displayed. 

@code
public class SimpleSkeletonAvatar : MonoBehaviour
{
…
	public void ProcessSkeleton(nuitrack.Skeleton skeleton)
	{
		for (int i = 0; i < jointsInfo.Length; i++)
		{
			nuitrack.Joint j = skeleton.GetJoint(jointsInfo[i]);
			if (j.Confidence > 0.5f)
			{
				joints[jointsInfo[i]].SetActive(true);
				// Bring proj coordinates from Nuitrack into accordance with screen coordinates
				joints[jointsInfo[i]].transform.position = new Vector2(j.Proj.X * Screen.width, Screen.height - j.Proj.Y * Screen.height);
			}
			else
			{
				joints[jointsInfo[i]].SetActive(false);
			}
		}
	}
}
@endcode

@note
Currently, there are only two values of confidence: 0 (Nuitrack thinks that this isn't a joint) and 0.75 (a joint).

<li> Loop over the connections. If both joint models required to create the connection are displayed (the initial and end joint), activate the connection. Set the connection position based on the positions of corresponding joint models. To rotate the connection, use the difference between the positions of the initial and end joints. Calculate the connection size: 
<ul>
<li> find the distance between the initial and end joints;
<li> set the size: Vector3 (distance, 1, 1)
</ul>

If any joint required to create the connection is not displayed, the connection is not displayed, too. 

@code
public class SimpleSkeletonAvatar : MonoBehaviour
{
…
	public void ProcessSkeleton(nuitrack.Skeleton skeleton)
	{
		for (int i = 0; i < connectionsInfo.GetLength(0); i++)
		{
			GameObject startJoint = joints[connectionsInfo[i, 0]];
			GameObject endJoint = joints[connectionsInfo[i, 1]];

			if (startJoint.activeSelf && endJoint.activeSelf)
			{
				connections[i].SetActive(true);

				connections[i].transform.position = startJoint.transform.position;
				connections[i].transform.right = endJoint.transform.position - startJoint.transform.position;
				float distance = Vector3.Distance(endJoint.transform.position, 	startJoint.transform.position);
				connections[i].transform.localScale = new Vector3(distance, 1f, 1f);
			}
			else
			{
				connections[i].SetActive(false);
			}
		}
	}
}
@endcode

<li> In <i>Update</i>, if <i>autoProcessing</i> is set to <i>true</i>, call the <i>ProcessSkeleton</i> and pass the parameters <i>CurrentUserTracker.CurrentSkeleton</i>. 

@code
public class SimpleSkeletonAvatar : MonoBehaviour
{
…    
	void Update()
	{
		if (autoProcessing)
			ProcessSkeleton(CurrentUserTracker.CurrentSkeleton);
	}
}
@endcode

<li> Create a new <b>Canvas</b> named <b>SkeletonsCanvas</b>. Set its <b>Sort Order</b> to <b>1</b>, so that skeletons are displayed over the <b>ColorFrameCanvas</b>. 

@image html images/Urgb_7.png  
@image latex images/Urgb_7.png 

<li> Create a child object to the <b>SkeletonsCanvas</b> (<b>Create Empty</b>) and name it <b>Simple Skeleton Avatar</b>. We'll use this object to display the skeleton. Drag-and-drop the <i>SimpleSkeletonAvatar</i> script to this object. 
<li> Drag-and-drop the <b>jointUI</b> prefab (<b>Tutorials/RGBandSkeletons/Prefabs</b>) to the <b>Joint Prefab</b> field.  Drag-and-drop the <b>ConnectionUI</b> prefab (from the same folder) to the <b>Connection Prefab</b> field. 

@image html images/Urgb_8.png  
@image latex images/Urgb_8.png 

<li> Run the project. At this point, you should see a 2D skeleton displayed over the RGB image from the sensor. However, if there are several users, their skeletons won't be displayed. Let's move on to the next point so as not to upset them. 
</ol>

@image html images/Urgb_9.gif  
@image latex images/Urgb_9.gif 

@section rgb_skeletons Displaying Multiple Skeletons 

<ol>
<li> Create a new script and name it <i>SkeletonController</i>. In this script, we'll define how to track and display multiple skeletons. 
<li> Add the <i>nuitrack</i> namespace. Create the <i>SkeletonCount</i> public variable and set the range from 0 to 6 (the number of tracked skeletons). Create the <i>SimpleSkeletonAvatar</i> field (for the <b>SkeletonAvatar</b> model that we've created). Create a list of <i>SkeletonAvatars</i> (displayed skeletons).

@code
using nuitrack;
using UnityEngine;
using System.Collections.Generic;
 
public class SkeletonController : MonoBehaviour
{
	[Range(0, 6)]
	public int skeletonCount = 6;         
	[SerializeField] SimpleSkeletonAvatar skeletonAvatar;
 
	List<SimpleSkeletonAvatar> avatars = new List<SimpleSkeletonAvatar>();
}
@endcode

@note
We set the range from 0 to 6 because Nuitrack tracks up to 6 skeletons. 

<li> In <i>Start</i>, spawn skeletons on the scene, get the <i>SimpleSkeletonAvatar</i> component from each skeleton, and set <i>autoProcessing</i> to <i>false</i> (because we want to process several skeletons). Add a skeleton to the list of avatars <i>avatars.Add(simpleSkeleton)</i>. Pass the desired number of tracked skeletons to Nuitrack. You can set the desired number of skeletons in the Unity editor. 

@code
public class SkeletonController : MonoBehaviour
{    
...
	void Start()
	{
		for (int i = 0; i < skeletonCount; i++)
		{
			GameObject newAvatar = Instantiate(skeletonAvatar.gameObject, transform, true);
			SimpleSkeletonAvatar simpleSkeleton = 	newAvatar.GetComponent<SimpleSkeletonAvatar>();
			simpleSkeleton.autoProcessing = false;
			avatars.Add(simpleSkeleton);
		}
		NuitrackManager.SkeletonTracker.SetNumActiveUsers(skeletonCount);
	}
}
@endcode

<li> Create the <i>OnSkeletonUpdate</i> method, which accepts all the info about the received skeletons (<i>skeletonData</i>). Loop over all the spawned skeletons depending on the number of tracked skeletons specified in Unity. If there is a skeleton received from Nuitrack for a skeleton avatar, it's processed and displayed, otherwise, it's hidden. 

@code
public class SkeletonController : MonoBehaviour
{    
…
	void OnSkeletonUpdate(SkeletonData skeletonData)
	{
		for (int i = 0; i < avatars.Count; i++)
		{
			if (i < skeletonData.Skeletons.Length)
			{
				avatars[i].gameObject.SetActive(true);
				avatars[i].ProcessSkeleton(skeletonData.Skeletons[i]);
			}
			else
			{
				avatars[i].gameObject.SetActive(false);
			}
		}
	}
}
@endcode

<li> In the <i>OnEnable</i> method, subscribe to <i>OnSkeletonUpdateEvent</i>, which is called each time the skeleton is updated (each frame). Update the skeleton info. 

@code
public class SkeletonController : MonoBehaviour
{    
	…    
	void OnEnable()
	{
		NuitrackManager.SkeletonTracker.OnSkeletonUpdateEvent += OnSkeletonUpdate;
	}
	…
}
@endcode

<li> Save the <b>SimpleSkeletonAvatar</b> prefab and delete it from the scene. 
<li> Add the <i>SkeletonController</i> script to <b>SkeletonsCanvas</b>. 
<li> Set the desired number of tracked skeletons with a slider. 

@image html images/Urgb_10.png  
@image latex images/Urgb_10.png 

<li> Drag-and-drop the <b>Simple Skeleton Avatar</b> prefab to the <b>Skeleton Avatar</b> field of the <i>SkeletonController</i> script.

@image html images/Urgb_11.jpg  
@image latex images/Urgb_11.jpg 

<li> Run the project. Now the skeletons of several users are tracked and displayed on the RGB image. Congratulations! 
</ol>

@image html images/Urgb_1.gif  
@image latex images/Urgb_1.gif 
*/
