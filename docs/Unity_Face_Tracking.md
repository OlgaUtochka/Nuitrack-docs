/*!
@page UnityFaceTracking_page Face Tracking with Nuitrack
  @tableofcontents

In this useful tutorial you'll learn how to track and get information about faces with <b>Nuitrack</b>. This feature is available in Nuitrack since version 0.23.3 (upgrade your Nuitrack if you haven't done this yet!). In this project, an RGB image from the sensor is displayed in the background and emojis are shown instead of faces. The way an emoji looks (its gender, age type, emotion) is determined by the parameters received from Nuitrack. Also you can set the desired number of tracked skeletons in this project (up to 6), because more people is more fun! 2D skeleton is displayed on the user's body.<br>
<br>
To create this wonderful project, only a couple of things are required: 
<ul>
<li> Nuitrack;
<li> Any supported sensor (see the full list [at our website](https://nuitrack.com/)).
</ul>
<br>
You can find the finished project in <b>Nuitrack SDK: Unity 3D → NuitrackSDK.unitypackage → Tutorials → FaceTracker</b>.

@image html images/Uface_1.gif  
@image latex images/Uface_1.gif 

@note
Nuitrack displays the info about a face <b>only after the user's skeleton is detected</b>. So, if only your face is visible, Nuitrack won't display the information. If you're only interested in face tracking (without any info about skeletons), you can take a look at [3DiVi Face SDK](https://facesdk.3divi.com/), which is a software for face tracking, detection, and matching. Unlike Nuitrack SDK, <b>3DiVi Face SDK</b> focuses on processing the data about faces and provides more advanced features in this field.

@section ft_setting Setting Up the Scene

<ol>
<li> Create a new Unity project. 
<li> Import <b>NuitrackSDK.unitypackage</b> from <b>Nuitrack SDK/Unity3D</b> to the project (except for the folder <b>Tutorials/FaceTracker/FinalAssets</b>, because this folder contains the scripts and other files that we're going to create in this tutorial): <b>Assets → Import Package → Custom Package...</b>
<li> Drag-and-drop the <b>NuitrackScripts</b> prefab for skeleton tracking to the scene. In the <b>NuitrackManager</b> section, tick the required modules: <b>Color Module On</b> (to output an RGB image from a sensor), <b>Skeleton Module On</b> (for skeleton tracking).

@image html images/Uface_2.png  
@image latex images/Uface_2.png 

<li> Drag-and-drop the <b>Color Frame Canvas</b> prefab to the scene.

@note
<b>Color Frame Canvas</b> displays the RGB image from a sensor on the standard interface component <b>Image</b>. The <i>DrawColorFrame</i> script creates a Texture2d in the format of RGB24 using the <i>ColorFrame</i> data. Each time a <i>ColorFrame</i> is received, the texture is updated. 

<li> Drag-and-drop the <b>Skeletons Canvas</b> prefab to the scene. In the <b>Canvas</b> section, set <b>Sort Order = 1</b>, so that skeletons are always displayed over <b>Color Frame Canvas</b>. <b>Sort Order</b> determines the rendering order: <b>Sort Order</b> in <b>Color Frame Canvas</b> is set to the default value of 0, that's why it is used as a background.

@image html images/Uface_3.png  
@image latex images/Uface_3.png 

@note
If your skeleton looks shifted, you have to turn on <b>depth-to-color registration</b> using the  <i>nuitrack.config</i> file: find the section <i>“DepthProvider”</i> and set <i>“Depth2ColorRegistration”</i> to <i>true</i>.

@note
In this project, we display a 2D skeleton on the user's body because it's a simpler and more versatile implementation. To display a 3D skeleton, you have to take into account the aspect, position, and FOV of your sensor in the Unity editor.

<li> Run the project. You should see an RGB image from the sensor on the scene and tracked skeletons displayed on users' bodies. Time to move on to the most interesting part of this tutorial - face tracking! 

@image html images/Uface_4.gif  
@image latex images/Uface_4.gif 

</ol>

@section ft_creating_faces Creating the Faces

<ol>
<li> Create a new script and name it <i>FaceInfo</i>. This script will contain the parameters and values from the JSON response received from Nuitrack (see a sample JSON response [in our documentation](http://download.3divi.com/Nuitrack/doc/Instance_based_API.html)). 

@note
By default, face tracking in Nuitrack is turned off. To turn on this function, open the  <i>nuitrack.config</i> file and set <i>“Faces.ToUse”</i> and <i>“DepthProvider.Depth2ColorRegistration”</i> to <i>true</i>. 

@code
using UnityEngine;
 
[System.Serializable]
public class FaceInfo 
{
    public string Timestamp;
    public Instances[] Instances;
}
 
[System.Serializable]
public class Instances
{
    public int id;
    public string @class;
    public Face face;
}
 
[System.Serializable]
public class Face
{
    public Rectangle rectangle;
    public Vector2[] landmark;
    public Vector2 left_eye;
    public Vector2 right_eye;
    public Angles angles;
    public Emotions emotions;
    public Age age;
    public string gender;
}
 
[System.Serializable]
public class Rectangle
{
    public float left;
    public float top;
    public float width;
    public float height;
}
 
[System.Serializable]
public class Angles
{
    public float yaw;
    public float pitch;
    public float roll;
}
 
[System.Serializable]
public class Emotions
{
    public float neutral;
    public float angry;
    public float surprise;
    public float happy;
}
 
[System.Serializable]
public class Age
{
    public string type;
    public float years;
}
@endcode

@note
A keyword cannot be used as an identifier (name of variable, class, interface etc). However, they can be used with the prefix '@'. For example, <i>class</i> is a reserved keyword so it cannot be used as an identifier, but '@'class can be used.

<li> Create a new script and name it <i>FaceManager</i>. Create enumerators for genders, age types, and emotions and list all possible values. 

@code
using System.Collections.Generic;
using UnityEngine;
 
public enum Gender
{
    any,
    male,
    female
}
 
public enum AgeType
{
    any,
    kid,
    young,
    adult,
    senior
}
 
public enum Emotions
{
    any,
    happy,
    surprise,
    neutral,
    angry
}
@endcode

<li> Add the <i>canvas</i> field for the <b>Canvas</b> displaying the emojis, <i>faceController</i> field for an emoji prefab, <i>skeletonController</i> field, list of <i>FaceControllers</i>, and an array with the data about faces. 

@code
...
public class FaceManager : MonoBehaviour 
{
    [SerializeField] Canvas canvas;
    [SerializeField] GameObject faceController;
    [SerializeField] SkeletonController skeletonController;
    List<FaceController> faceControllers = new List<FaceController>();
    Instances[] faces;
 
    FaceInfo faceInfo;    
}
@endcode

@note
Here are sample emojis for 2 genders, 4 age types, and 4 emotions (just to let you know how emojis of different ages and genders look like in this project):
<ul>
<li> happy little girl
<li> surprised young man
<li> neutral adult woman
<li> angry old man
</ul>

@image html images/Uface_5.png  
@image latex images/Uface_5.png 

<li> In <i>Start</i>, add faces to the scene and include them in the list of <i>FaceControllers</i>. 

@code
public class FaceManager : MonoBehaviour 
{
...
     void Start()
     {
        for (int i = 0; i < skeletonController.skeletonCount; i++)
        {
                 faceControllers.Add(Instantiate(faceController, canvas.transform).GetComponent<FaceController>());
        }
     }
}
@endcode

<li> In <i>Update</i>, get a JSON response and include the data from JSON to the <i>faceInfo</i> class, which stores the received face info. Add the faces from <i>faceInfo</i> (<i>faceInfo.Instances</i>) to the <i>faces</i> array. Loop over the <i>FaceControllers</i>: if the face info is available, the emoji is displayed, otherwise, it's hidden. Create the <i>id</i> variable and <i>currentFace</i> variable that stores the info about a current face. Pass the face info to <i>FaceController</i> and display the face. Get the <i>id</i> parameter from the face info and pass it to the <i>id</i> variable. Each of the detected users has his/her own id. User id and skeleton id in Nuitrack are always the same. Create a user's skeleton. If skeleton data from Nuitrack is received, we try to get a skeleton with the same id. If a skeleton is found, create the <i>joint</i> variable and call it <i>head</i>, get the <i>joint head</i> from the skeleton and place it in the coordinates received from the skeleton. An emoji will be displayed in place of the joint head.

@code
public class FaceManager : MonoBehaviour {
...
void Update () 
{
	// Pass the data from JSON to faceInfo
	string json = nuitrack.Nuitrack.GetInstancesJson();
	// Replace quotation marks with square brackets to prevent a conversion error 
	// in case an array is empty (no info about faces received)
	faceInfo = JsonUtility.FromJson<FaceInfo>(json.Replace("\"\"", "[]"));
 
	// Get all faces (faceInfo.Instances) from FaceInfo
	faces = faceInfo.Instances;
	for (int i = 0; i < faceControllers.Count; i++)
	{
		if(faces != null && i < faces.Length - 1)
		{
			int id = 0;
			Face currentFace = faces[i].face;
 
			// Pass face to FaceController
			faceControllers[i].SetFace(currentFace);
			faceControllers[i].gameObject.SetActive(true);
 
			// Face ids and skeleton ids are the same
			id = faces[i].id;
 
			nuitrack.Skeleton skeleton = null;
			if (NuitrackManager.SkeletonData != null)
			    skeleton = NuitrackManager.SkeletonData.GetSkeletonByID(id);
 
			if(skeleton != null)
			{
				nuitrack.Joint head = skeleton.GetJoint(nuitrack.JointType.Head);
				faceControllers[i].transform.position = new Vector2(head.Proj.X * Screen.width, Screen.height - head.Proj.Y * Screen.height);
				//stretch the face to fit the rectangle
				if(currentFace.rectangle != null) faceControllers[i].transform.localScale = new Vector2(currentFace.rectangle.width * Screen.width, currentFace.rectangle.height * Screen.height);
			}
		}
		else
		{
			faceControllers[i].gameObject.SetActive(false);
		}
	}
}
@endcode

@note
Learn more about [JsonUtility](https://docs.unity3d.com/ScriptReference/JsonUtility.html) (utility functions for working with JSON data).

<li> Create a new script and name it <i>FaceController</i>. Create public fields for gender, emotions, and age, and a text field for age. Age types and emotions are stored in the dictionaries <i>Dictionary<string, AgeType> age</i> and <i>Dictionary<EmotionType, float> emotionDict</i>. You can get "age type" by age name (string), and get "emotion value" (float) by "emotion type". 

@code
﻿using UnityEngine;
using System.Collections.Generic;
using System.Linq;

public class FaceController : MonoBehaviour 
{
	public Gender genderType;
	public EmotionType emotions;
	public AgeType ageType;

	Dictionary<string, AgeType> age = new Dictionary<string, AgeType>()
	{
		{ "kid", AgeType.kid },
		{ "young", AgeType.young }
		{ "adult", AgeType.adult },
		{ "senior", AgeType.senior },
	};

	Dictionary<EmotionType, float> emotionDict = new Dictionary<EmotionType, float>()
	{
		{ EmotionType.happy, 0 },
		{ EmotionType.surprise, 0 },
		{ EmotionType.neutral, 0 },
		{ EmotionType.angry, 0 },
	};
}
@endcode

<li> The <i>SetFace</i> method takes the <i>Face</i> class from the <i>FaceInfo</i> script that stores all the info about the face. In this method, all characteristics of a particular face are assigned. Assign gender (either male or female). Get the age type by its name. Emotion is defined as follows: the values stored in the dictionare are looped over, and an emotion with the highest value is selected. 

@code
public class FaceController : MonoBehaviour 
{
...
	public void SetFace(Face newFace)
	{
		//Gender
		if (newFace.gender == "female")
			genderType = Gender.female;
		else
			genderType = Gender.male;

		//Age
		if(newFace.age != null)
			age.TryGetValue(newFace.age.type, out ageType);

		//Emotion
		if (newFace.emotions != null)
		{
			emotionDict[EmotionType.happy] = newFace.emotions.happy;
			emotionDict[EmotionType.surprise] = newFace.emotions.surprise;
			emotionDict[EmotionType.neutral] = newFace.emotions.neutral;
			emotionDict[EmotionType.angry] = newFace.emotions.angry;

			KeyValuePair<EmotionType, float> prevailingEmotion = emotionDict.First();
			foreach (KeyValuePair<EmotionType, float> emotion in emotionDict)
			if (emotion.Value > prevailingEmotion.Value) prevailingEmotion = emotion;

			emotions = prevailingEmotion.Key;
		}
	}
}
@endcode

<li> In Unity, create an <b>Empty Object</b> and name it <b>FaceManager</b>. Drag-and-drop the <i>FaceManager</i> script to this object. Create a new Canvas, name it Face Canvas, and set the Sort Order to 2. Drag-and-drop <b>Face Canvas</b> to the <b>Canvas</b> field of this object. Drag-and-drop <b>Head</b> (<b>FaceTracker/Prefabs</b>) to the <b>Face Controller</b> field and <b>Skeletons Canvas</b> (from the Scene) to the <b>Skeleton Controller</b> field of this object. 

@image html images/Uface_6.png  
@image latex images/Uface_6.png 

<li> Drag-and-drop the <b>Head</b> prefab to the scene. Drag-and-drop the <i>FaceController</i> script to this prefab and click <b>Apply</b>. Delete the head from the scene. 
<li> Run the project. You should see emojis instead of users' faces but they look exactly the same at the moment (a young happy man is a default emoji) because the received face parameters are not applied to the emojis yet. At this point, emojis  move according to users' movements.
</ol>

@image html images/Uface_7.gif  
@image latex images/Uface_7.gif 

@section ft_application Applying the Received Data about Faces

<ol>
<li> Create a new script and name it <i>FaceSwitcher</i>. It will switch the face parameters in Unity according to the info from JSON. Add the necessary fields (<i>gender, ageType, emotions</i>) and two objects: <i>enabledObject</i>, which is enabled, if all conditions are <i>true</i>, and <i>disabled</i>, if at least one is <i>false</i>, and <i>disabledObject</i>, which operates vice versa. Add the <i>FaceController</i> field and the boolean variable <i>display</i> to display/hide a face. 

@code
using UnityEngine;
 
public class FaceSwitcher : MonoBehaviour 
{
    [SerializeField] Gender gender;
    [SerializeField] AgeType ageType;
    [SerializeField] Emotions emotions;
    [SerializeField] GameObject enabledObject;
    [SerializeField] GameObject disabledObject;
 
    FaceController faceController;
    bool display = false;
}
@endcode

<li> In <i>Start</i>, find the <i>FaceController</i> component in a parent object and pass it to the <i>faceController</i> variable. 

@code
public class FaceSwitcher : MonoBehaviour 
{
...
     void Start () 
     {
        faceController = GetComponentInParent<FaceController>();
     }
}
@endcode

<li> In the <i>SwitchObjects</i> method, enable/disable the corresponding object to display/hide the parameters if they're <i>true/false</i>. 

@code
public class FaceSwitcher : MonoBehaviour 
{
...
     void SwitchObjects()
     {
        if (enabledObject != null)
            enabledObject.SetActive(display);
 
        if (disabledObject != null)
            disabledObject.SetActive(!display);
     }
}
@endcode

<li> In <i>Update</i>, the value of <i>display</i> is changed according to the specified conditions. If all conditions are true (gender, age type, and emotion correspond to the ones specified in Unity), then the <i>enabledObject</i> is displayed. If at least one condition is false, the <i>display</i> becomes <i>false</i> and <i>enabledObject</i> is not displayed. 

@code
public class FaceSwitcher : MonoBehaviour 
{
...
     void Update()
     {
        display = (gender == Gender.any ||gender == faceController.genderType) &&
                  (ageType == AgeType.any || ageType == faceController.ageType) &&
                  (emotions == EmotionType.any ||emotions==faceController.emotions);
 
        SwitchObjects();
     }
}
@endcode

<li> Drag-and-drop the <b>Head</b> prefab to the scene again. Select <b>Head → Face</b>, add the <b>FaceSwitcher</b> component, and select <b>Gender: Male, Age Type: Any, Emotions: Any</b> (they'll be assigned hierarchically). Assign the <b>enabledObject: Male, disabledObject: Female</b>.

@image html images/Uface_8.png  
@image latex images/Uface_8.png 

<li> Assign emotions and age types in the relevant fields as shown on the screenshots (you have to add the <b>FaceSwitcher</b> component several times).

@image html images/Uface_9.png  
@image latex images/Uface_9.png 

<li> Run the project. You should see the tracked skeletons and faces of several users with emojis instead of faces. Different emojis are displayed depending on gender, age type and emotion of a user.
</ol>

@image html images/Uface_1.gif  
@image latex images/Uface_1.gif 

Congratulations, you've just learnt how to use face tracking in your project with Nuitrack! You can use the information in gaming or other fields. By the way, check out other parameters in the [JSON response from Nuitrack](http://download.3divi.com/Nuitrack/doc/Instance_based_API.html), there are far more interesting things besides the ones covered in this tutorial. Have fun!
