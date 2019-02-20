/*!
@page UnityAnimatedEmoji_page Creating Animated Emoji with Nuitrack

In this tutorial you'll learn to create animated emoji using the face info from Nuitrack and blendshapes. You'll be able to track several skeletons (from 1 to 6) and see the fox animated emoji instead of the user's face. Fox face mimics user's facial expressions. When a user turns his/her head, the fox head is also rotated. You can notice that fox fur is slightly moving when the fox turns its face - an incredible thing to watch. The fox face is zoomed in and out, i.e. the closer the user is, the bigger the fox head is, and vice versa.  

To create this project, you'll need just a couple of things:
<ul>
<li> [Nuitrack Runtime and Nuitrack SDK](https://nuitrack.com/#rec54893368)
<li> Unity (2017.4 or higher)
<li> Any compatible sensor (see the complete list at [Nuitrack website](https://nuitrack.com/#rec24776680)) 
</ul>

You can find the finished project in <b>Nuitrack SDK: Unity 3D → NuitrackSDK.unitypackage → Tutorials → Animated Emoji </b>

@image html images/Uanimoji_1.gif  
@image latex images/Uanimoji_1.gif  

@section animoji_settings Setting Up the Scene

<ol>
<li> Download and import <b>NuitrackSDK.unitypackage</b> from <b>Nuitrack SDK</b> to your project (except for the folder <b>NuitrackSDK/Tutorials/Animated Emoji/Final Assets</b> because it includes ready-made scripts).
<li> Drag-and-drop the <b>NuitrackScripts</b> prefab to the scene. Tick the required Nuitrack modules: <b>Color Module On</b> (to output an RGB image from a sensor), <b>Skeleton Tracker Module On</b> (for skeleton tracking).

@image html images/Uanimoji_2.png  
@image latex images/Uanimoji_2.png  

<li> Create a new <b>Canvas: Create → UI → Canvas</b>, name it <b>Face Canvas</b>. Set its Sort Order to 2. This canvas is used to display the fox face (over the canvases for outputting RGB and displaying skeletons). 

@image html images/Uanimoji_3.png  
@image latex images/Uanimoji_3.png  

@note
In this project, we display the fox face in 2D as the image in this format will be displayed correctly for all supported sensors. If we wanted to create the fox face in 3D, we would need to know which sensor is used and what is its resolution. Nuitrack doesn't provide this information at the moment. 

<li> Create a child object to the <b>Canvas: Create → UI → Raw Image</b>, name it <b>Fox Face</b>. Go to the settings of this object and set <b>Width = 1, Height = 1</b> in the <b>RectTransform</b> section. Set the appropriate <b>Scale</b>, for example: <b>X: 100, Y: 100, Z: 1</b>. This is the image size in pixels, which will soon become the fox face. It's important to keep the 1:1 aspect ratio, otherwise the fox face will be stretched vertically or horizontally. The image size shouldn't be too small, otherwise you won't even see the fox face when you run the project at this stage. 

@image html images/Uanimoji_4.png  
@image latex images/Uanimoji_4.png  

<li> Drag-and-drop the <b>Fox Face Model</b> prefab to the scene (it's located in <b>NuitrackSDK/Tutorials/Animated Emoji/Prefabs</b>).
<li> Create a new texture in the folder <b>Animated Emoji (Create → Render Texture)</b>, name it <b>Face</b>. This is the texture for rendering the image from the sensor. 
<li> Go to the settings of the <b>Fox Face</b> object and drag-and-drop the <b>Face</b> texture to the <b>Texture</b> field. 

@image html images/Uanimoji_5.png  
@image latex images/Uanimoji_5.png  

<li> Go to the settings of the <b>Camera</b> object (which can be found under the ready-made <b>Fox Face Model</b> prefab) and drag-and-drop the <b>Face</b> texture to the <b>Target Texture</b> field. 

@image html images/Uanimoji_6.png  
@image latex images/Uanimoji_6.png  

</ol>

Great job! Now we have a 2D fox face that we can further use to mimic our facial expressions. 

@image html images/Uanimoji_7.jpg  
@image latex images/Uanimoji_7.jpg  

@section animoji_switch Switch the User Face with the Fox Face

<ol>
<li> Save <b>Fox Face</b> as a prefab. Then, delete the <b>Fox Face</b> and <b>Fox Face Model</b> prefabs from the scene.
<li> Create a new script <i>FaceAnimController</i> to manage the fox faces. Add the <i>UnityEngine.UI</i> namespace. Add the necessary fields (their names speak for themselves). The <i>slerpRotation</i> is used to smooth the head rotation (otherwise, the head would jitter when turned). The <i>faceRaw</i> field is used to display the fox face.

@code
using UnityEngine;
using UnityEngine.UI;
 
public class FaceAnimController : MonoBehaviour
{
	[SerializeField] Transform headModel;
	[SerializeField] Transform headRoot;
 
	[SerializeField] RawImage rawImage;
	[SerializeField] Camera faceCamera;
 
	[SerializeField] SkinnedMeshRenderer faceMeshRenderer;
	[SerializeField] RenderTexture renderTextureSample;
	[SerializeField] float smoothHeadRotation = 5;

	RenderTexture renderTexture;
 
	RawImage faceRaw;
}
@endcode 

@note
Learn more about [Render Texture](https://docs.unity3d.com/Manual/class-RenderTexture.html).

<li> In the <i>Init</i> method, pass the <i>Canvas</i> to make <i>rawImage</i> the child object and create <i>renderTexture</i> based on the <i>RenderTexture</i>, which we've created in the first section. Pass <i>renderTexture</i> to <i>Camera</i> and to the texture in the <i>rawImage</i> object. Set their position (far away from each other) and stretch the image (our fox face) in screen height. Don't forget to keep the aspect ratio. Spawn the <i>rawImage</i> object.

@code
public class FaceAnimController : MonoBehaviour
{
...
	public void Init(Canvas canvas)
	{
		faceRaw = Instantiate(rawImage, canvas.transform).GetComponent<RawImage>(); // Spawn RawImage
		faceRaw.transform.localScale = Vector2.one * Screen.height;
 
		renderTexture = new RenderTexture(renderTextureSample);
		faceCamera.targetTexture = renderTexture;
		faceRaw.texture = renderTexture;
		faceRaw.gameObject.SetActive(false);
	}
}
@endcode

<li> Create the public method <i>UpdateFace</i> and specify the required parameters: the info about a user's face from JSON and head joint. Get the position of the head joint in projective coordinates received from Nuitrack  and set the head to these coordinates (multiply X and Y by the width and height of the screen). Change the <i>headRoot</i> position along the Z axis using the info from Nuitrack. This would stand for the face zoom, which depends on the distance between a user and a sensor (the closer the user to the sensor is, the bigger the fox face is).

@code
public class FaceAnimController : MonoBehaviour
{
...
	public void UpdateFace(Instances instance, nuitrack.Joint headJoint)
	{
		Vector3 headProjPosition = headJoint.Proj.ToVector3();
		faceRaw.transform.position = new Vector2(headProjPosition.x * Screen.width, Screen.height - headProjPosition.y * Screen.height);
 
		headRoot.localPosition = new Vector3(0, 0, -headJoint.Real.Z * 0.001f);
	}
}
@endcode 

@note
Keep in mind that 1 Unity unit is about 1 m, so we need to adjust the obtained data. To do that, multiply the received values by 0.001 (convert m to mm).

<li> Create the <i>OnEnable</i> and <i>OnDisable</i> methods: when a face is enabled, rawImage is also enabled to render this face, and vice versa.

@code
public class FaceAnimController : MonoBehaviour
{
...
	void OnDisable()
	{
		if(faceRaw != null)
			faceRaw.gameObject.SetActive(false);
	}
 
	void OnEnable()
	{
		if (faceRaw != null)
			faceRaw.gameObject.SetActive(true);
	}
}
@endcode 

<li> Drag-and-drop the <b>Fox Face Model</b> prefab to the scene. Then, drag-and-drop the <i>FaceAnimController</i> to this prefab. Fill in the fields as shown in the image below. After that, click <b>Apply</b> and delete <b>Fox Face Model</b> from the scene. 

@image html images/Uanimoji_8.png  
@image latex images/Uanimoji_8.png  

<li> Create a new script <i>FaceAnimManager</i>. In this script, we'll describe the display of the fox face instead of the user's face. Add the <i>nuitrack</i> namespace. Add the <i>canvas</i> field for <b>Canvas</b> and the <i>facePrefab</i> field for the fox face. Set <i>faceCount</i> from 0 to 6 (faces are "linked" to skeletons, and the maximum number of tracked skeletons is 6). Add <i>faceInfo</i> (parsed JSON from Nuitrack, from which we get all the info about faces) and list of <i>faceAnimControllers</i> (list of faces).

@code
using UnityEngine;
using System.Collections.Generic;
using nuitrack;
 
public class FaceAnimManager : MonoBehaviour
{
	[SerializeField] Canvas canvas;

	[SerializeField] FaceAnimController facePrefab;

	[Range(0, 6)]
	[SerializeField] int faceCount = 6; // Max number of skeletons tracked by Nuitrack

	FaceInfo faceInfo;
	List<FaceAnimController> faceAnimControllers = new List<FaceAnimController>();
}
@endcode 

<li> In <i>Start</i>, spawn as many faces as you set in <i>faceCount</i>. Add the fox face to the scene, get <i>faceAnimController</i> from it, and call the <i>Init</i> method. Place fox faces far away from each other so that the <b>Camera</b> won't catch several fox faces at the same time, otherwise, there'll be several fox faces displayed instead of one user's face - this may look funny but it's incorrect. Add <i>faceAnimController</i> to the list of <i>faceAnimControllers</i>. Specify the necessary number of tracked skeletons (keep in mind that faces are linked to skeletons). Subscribe to <i>OnSkeletonUpdateEvent</i>.

@code
public class FaceAnimManager : MonoBehaviour
{
...
	void Start()
	{
		for (int i = 0; i < faceCount; i++)
		{
			GameObject newFace = Instantiate(facePrefab.gameObject, new UnityEngine.Vector3(i*headsDistance,0,0), Quaternion.identity);
			newFace.SetActive(false);
			FaceAnimController faceAnimController = newFace.GetComponent<FaceAnimController>();
			faceAnimController.Init(canvas);
			faceAnimControllers.Add(faceAnimController);
		}
 
		NuitrackManager.SkeletonTracker.SetNumActiveUsers(faceCount);
		NuitrackManager.SkeletonTracker.OnSkeletonUpdateEvent += OnSkeletonUpdate;
	}
}
@endcode

<li> Create the <i>OnSkeletonUpdateEvent</i> method: create the <i>string json</i> variable and pass the info from JSON to it. Pass the parsed info from JSON to <i>faceInfo</i>: replace quotation marks with square brackets to prevent a conversion error in case an array is empty (no info about faces received). If there is no info about faces, the rest of the method is not executed.

@code
public class FaceAnimManager : MonoBehaviour
{
...
	void OnSkeletonUpdate(SkeletonData skeletonData)
	{
		string json = Nuitrack.GetInstancesJson();
		faceInfo = JsonUtility.FromJson<FaceInfo>(json.Replace("\"\"", "[]"));
 
		if (faceInfo.Instances.Length == 0)
			return;
	}
}
@endcode

<li> If a face is received, loop over <i>faceAnimControllers</i>. Activate as many faces as many skeletons were found, the rest of the faces are deactivated. By default, 6 faces are activated at startup. Create the <i>skeleton</i> variable to store the skeleton corresponding to the face (face ID and skeleton ID are the same). If a skeleton is found, get <i>headJoint</i> from it and activate the head joint if its confidence is greater than 0.5. Call the <i>UpdateFace</i> method of <i>faceAnimController</i>, pass <i>Instance</i> (all the face parameters) from JSON and <i>headJoint</i>. If a skeleton isn't found, the face is deactivated.

@code
public class FaceAnimManager : MonoBehaviour
{
...
	void OnSkeletonUpdate(SkeletonData skeletonData)
	...
		for (int i = 0; i < faceAnimControllers.Count; i++)
		{
			if (i < skeletonData.Skeletons.Length)
			{
				Skeleton skeleton = skeletonData.GetSkeletonByID(faceInfo.Instances[i].id);
				if(skeleton != null)
				{
					nuitrack.Joint headJoint = skeleton.GetJoint(JointType.Head);
 
					faceAnimControllers[i].gameObject.SetActive(headJoint.Confidence > 0.5f);
					faceAnimControllers[i].UpdateFace(faceInfo.Instances[i], headJoint);
				}
			}
			else
			{
				faceAnimControllers[i].gameObject.SetActive(false);
			}
		}
}
@endcode

<li> Drag-and-drop the <i>FaceAnimManager</i> script to <b>Face Canvas</b>.
<li> Drag-and-drop <b>Face Canvas</b> to the <b>Canvas</b> field. Drag-and-drop the <b>Fox Face Model</b> prefab to the <b>Face Prefab</b> field.

@image html images/Uanimoji_9.png  
@image latex images/Uanimoji_9.png  

<li> Drag-and-drop the <b>Color Frame Canvas</b> prefab from <b>NuitrackSDK.unitypackage</b> to the scene. 
<li> Run the project. You should see the fox faces instead of the users' faces. This looks quite nice, however, they're all the same at this stage! Let's move on and add emotions to our foxes, which will give them some personality. 
</ol>

@image html images/Uanimoji_10.gif  
@image latex images/Uanimoji_10.gif  

@section animoji_emotions Make the Fox Emotional! 

<ol>
<li> First of all, turn on <b>depth-to-color registration</b> because a depth map doesn't accurately match an RGB image and we have to align them. To turn on depth-to-color registration, you have to open <i>nuitrack.config</i> and set <i>DepthProvider.Depth2ColorRegistration</i> to <i>true</i>.
<li> By default, face tracking in Nuitrack is turned off. To turn on this function, open the <i>nuitrack.config</i> file and set <i>Faces.ToUse</i> to <i>true</i>. 
<li> Add the IDs of blendshapes to <i>FaceAnimController</i>. You can see the full list of blendshapes in the <b>Fox_RigFace</b> object settings. With the help of these blendshapes, you can animate different parts of the fox face. In this project, the fox can open its mouth, blink, smile, and raise eyebrows. When the fox face is turned, its ears and fur on the cheeks are slightly moving. As you can see, we use only 7 blendshapes in this project, though there are much more of them in the list of blendshapes on <b>Fox_RigFace</b>. The point is that we can receive the limited number of anthropometric points at the moment (see [Nuitrack Instance-based API [Beta]](http://download.3divi.com/Nuitrack/doc/Instance_based_API.html) for details), therefore, there is not enough information for other blendshapes (such as cheeks). Add the fields <i>baseRotation</i> (initial rotation of <i>headRoot</i>), <i>blendshapeWeights</i> (from 0 to 100%), and <i>newRotation</i> (current head rotation).

@code
public class FaceAnimController : MonoBehaviour
{
...
	//Face Animation
	[Header("BlendShapesIds")]
	[SerializeField] int jawOpen = 6;
	[SerializeField] int eyeBlinkLeft = 0;
	[SerializeField] int eyeBlinkRight = 2;
	[SerializeField] int mouthLeft = 10;
	[SerializeField] int mouthRight = 11;
	[SerializeField] int browUpLeft = 17;
	[SerializeField] int browUpRight = 18;

	Quaternion baseRotation; 
	BlendshapeWeights blendshapeWeights = new BlendshapeWeights();
	Quaternion newRotation;
...
}
@endcode 

@note
Animation of fox ears and fur is described in the <i>LerpRotation</i> script. 

<li> In the <i>Init</i> method, save <i>baseRotation</i> (initial head rotation, which we use to calculate the current head rotation).

@code
public class FaceAnimController : MonoBehaviour
{
...
	public void Init(Canvas canvas)
	{
		baseRotation = headRoot.rotation; 
	}
...
}
@endcode

<li> In <i>UpdateFace</i>, add the local variable <i>face</i> and save the face from JSON. Pass <i>baseRotation</i> to the <i>newRotation</i> variable.

@code
public class FaceAnimController : MonoBehaviour
{
...
	public void UpdateFace(Instances instance, nuitrack.Joint headJoint)
	{
	...
		Face face = instance.face;
 
		newRotation = baseRotation;
	}
}
@endcode

<li> If anthropometric points were detected on a user's face (for this, it should be clearly visible and not overlapped by other objects), set the weights of blendshapes. They're all set in the same way with the help of the <i>SetBlendShapeWeight</i> method: pass the index and weight of a blendshape (from 0 to 100) to this method - the weight is taken from the <i>blendshapeWeights</i> class, then call the relevant method, for example, <i>GetJawOpen</i> to open the fox mouth, pass the <i>face</i> to this method, and so on. As a result, we get a weight value for each blendshape. Calculate the head rotation: pass the product of <i>baseRotation</i> (initial rotation) and head rotation from JSON to the <i>newRotation</i> variable.

@code
public class FaceAnimController : MonoBehaviour
{
...
	public void UpdateFace(Instances instance, nuitrack.Joint headJoint)
	{
	...
		if (instance.face.landmark == null)
			return;
 
		// Mouth
		faceMeshRenderer.SetBlendShapeWeight(jawOpen, blendshapeWeights.GetJawOpen(face));
 
		// Eyes
		faceMeshRenderer.SetBlendShapeWeight(eyeBlinkLeft, blendshapeWeights.GetEyeBlinkLeft(face));
		faceMeshRenderer.SetBlendShapeWeight(eyeBlinkRight, blendshapeWeights.GetEyeBlinkRight(face));
 
		// Smile
		faceMeshRenderer.SetBlendShapeWeight(mouthLeft, blendshapeWeights.GetSmile(face));
		faceMeshRenderer.SetBlendShapeWeight(mouthRight, blendshapeWeights.GetSmile(face));
 
		// Eyebrows
		faceMeshRenderer.SetBlendShapeWeight(browUpLeft, blendshapeWeights.GetBrowUpLeft(face));
		faceMeshRenderer.SetBlendShapeWeight(browUpRight, blendshapeWeights.GetBrowUpRight(face));
 
		// Head rotation
		newRotation = baseRotation * Quaternion.Euler(face.angles.yaw, -face.angles.pitch, face.angles.roll);
	}
}
@endcode

<li> In <i>Update</i>, smoothly rotate the head. 

@code
public class FaceAnimController : MonoBehaviour
{
...
	void Update()
	{
		headRoot.rotation = Quaternion.Slerp(headRoot.rotation, newRotation, slerpRotation * Time.deltaTime);
	}
}
@endcode

@note
Learn more about [Quaternion.Slerp](https://docs.unity3d.com/ScriptReference/Quaternion.Slerp.html).

<li> Run the project. Now our fox face looks quite lively - we can turn the head, see the emotions and moving ears and fur. You can use this sample to develop more sophisticated projects with face tracking, skeleton tracking, and animated emojis using Nuitrack. Have fun!
</ol>

@image html images/Uanimoji_1.gif  
@image latex images/Uanimoji_1.gif  

*/
