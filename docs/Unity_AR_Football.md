# Creating an AR Football Game using Nuitrack and ARCore

In this tutorial you'll learn how to create an interesting multiplayer game using **ARCore*** and **Nuitrack** - we are excited to present you **AR Football**! At least two players are needed for this game (the more the better). One player - "a striker" - points his Android device's camera at any surface and after that a grid appears on suitable surfaces that you can use to place an ARCore object, which is the goal with a goalkeeper. The striker's goal is to throw the ball and beat the goalkeeper. In turn, the other player - "the goalkeeper" - should catch the ball. The goalkeeper sees the goal on the TV screen or monitor. Several players can play as strikers. Nuitrack tracks the movement of players, the data is synchronized and sent via Wi-Fi network. 

You can find the finished project in **Nuitrack SDK**: **Unity 3D → NuitrackSDK.unitypackage → Tutorials → AR Football**

<p align="center">
<img width="500" src="https://github.com/OlgaUtochka/Nuitrack-docs/blob/master/images/UARCore_1.gif">
</p>

To create this game, you'll need a couple of things: 

Hardware: 
* **TVico** (with the pre-installed Nuitrack.apk) or a desktop with a connected sensor from the [list of supported devices](https://nuitrack.com/) 
<li> [<b>ARCore compatible device</b>] (https://developers.google.com/ar/discover/supported-devices)
</ul>

Software:
<ul>
<li> <b>ARCore Unity SDK</b> (we’ve built this project with v1.2.1)
<li> <b>Nuitrack SDK</b> (we’ve built this project with v1.3.3)
<li> <b>Unity</b> (we’ve built this project with v2017.4.0f1)
</ul>

@section arcore_settings Setting Up the Project  

<ol>
<li> To create an ARCore project, you need to download [ARCore SDK] (https://developers.google.com/ar/develop/downloads).
<li> Create a new Unity project. 
<li> Select the <b>Android</b> platform in <b>File → Build Settings</b>. 

@image html images/UARCore_2.png
@image latex images/UARCore_2.png

<li> In <b>Player Settings</b>, fill in the <b>Company Name</b> and <b>Product Name</b>. 

@image html images/UARCore_3.png
@image latex images/UARCore_3.png

<li> Go to <b>XR Settings</b> and enable <b>ARCore support</b>. 

@image html images/UARCore_4.png
@image latex images/UARCore_4.png

<li> In <b>Other Settings</b>, select <b>Minimum API Level 7.0</b> (which is required by ARCore), disable <b>Multithreaded Rendering</b> (which is an ARCore requirement as well) and fill in the <b>Package Name</b> in the <b>Identification</b> section.

@image html images/UARCore_5.png
@image latex images/UARCore_5.png

<li> Import <b>Arcore SDK</b> and <b>NuitrackSDk.unitypackage</b> from Nuitrack SDK to your project: <b>Assets → Import Package → Custom Package</b>. 
</ol>

@section arcore_integration Integrating ARCore to the Project

<ol>
<li> This project is based on the <b>HelloAR</b> project, which is a simple example of using ARCore by Google. Create a new scene and name it, for example, <b>Striker</b>. Delete <b>Main Camera</b> and <b>Directional Light</b> from the scene as we’ll need other objects in this project. Copy all the objects from the HelloAR scene to this scene (<b>Google ARCore → Examples → HelloAR → Scenes → HelloAR</b>):
<ul>
<li> <b>ARCore Device</b> - an object that contains the camera. The camera moves according to the movement of an Android device; the image received from the  camera of the Android device becomes the background of the project. 
<li> <b>Canvas</b> - this object is used to display the "Searching for planes" message.
<li> <b>Example Controller</b> - an object that is responsible for user interaction with gaming ARCore planes.
<li> <b>Plane Generator</b> - well, you guessed it, this object creates planes.
<li> <b>Point Cloud</b> - used to visualize a point cloud for creating the planes. 
</ul>

@image html images/UARCore_6.png
@image latex images/UARCore_6.png

<li> Drag-and-drop the <b>Environment</b> object from Nuitrack SDK to the scene. This object is very important because it represents a goal with a goalkeeper. 
<li> Create a new C# Script <i>Environment.cs</i>, in which we'll describe the behavior of this object. This script will contain a reference to the <b>Target</b> object (target for the ball) that's already attached to our prefab. Also, the size of the Environment object will be set at start.

@code
using UnityEngine;
using UnityEngine.Networking;

public class Environment : MonoBehaviour {

	public Transform aim;
	[SerializeField] Vector3 clientSize;

	void Start()
	{
		transform.localScale = clientSize;
	}
}
@endcode
<li> Drag-and-drop the <b>Target</b> object to the <b>aim</b> field. Set an appropriate size of the <b>Environment</b> object in the <b>Size</b> field, for example, (0.1,0.1,0.1).

@image html images/UARCore_7.png
@image latex images/UARCore_7.png

<li> Rename <b>Example Controller</b> to <b>Football Controller</b>, just for the sake of convenience. Delete the <i>HelloARController</i> script. Instead, let's create our own script <i>FootballARController.cs</i>: <b>Add Component → C# Script → FootballARController</b>. In this script, we’ll describe interaction of a user with AR.
<li> Add the <i>GoogleARCore</i> and <i>UnityEngine.Networking</i> namespaces to the script.
<li> Add some necessary fields:

@code
// After ARCore finds the anchor points in the real world, the camera starts to move around the scene.
public Camera mainCamera;

// A model to place when a raycast from a user touch hits a plane.
GameObject environment;

// A gameobject parenting UI for displaying the "searching for planes" snackbar.
// Message "Searching for surfaces". 
public GameObject searchingForPlaneUI;

// The rotation in degrees need to apply to model when model is placed.
private const float modelRotation = 180.0f; // Rotate the Environment to make it face the camera.

// A list to hold all planes ARCore is tracking in the current frame. 
// This object is used across the application to avoid per-frame allocations.
private List<DetectedPlane> allPlanes = new List<DetectedPlane>();

[SerializeField] Transform aRCoreDevice; // Must be the parent of the camera. 
@endcode

<li> Get found surfaces in <i>Update</i>. Create a variable to show / hide a surface. If at least one surface in tracked, the message "Searching for Surface" is hidden.

@code
public void Update()
{
	// Hide snackbar when currently tracking at least one plane.
	// Get the list of found surfaces.
	Session.GetTrackables<DetectedPlane>(allPlanes);
	bool showSearchingUI = true;
	for (int i = 0; i < allPlanes.Count; i++)
	{
		// If at least one surface is tracked, the searching stops.
		if (allPlanes[i].TrackingState == TrackingState.Tracking)
		{
			showSearchingUI = false;
			break;
		}
	}
 
	// Hide or show the message "Searching for Surfaces..."
	searchingForPlaneUI.SetActive(showSearchingUI);
}
@endcode

<li> In <i>Update</i>, check whether the "striker" user touches the screen or not.

@code
// If the player has not touched the screen, we are done with this update.
Touch touch;
if (Input.touchCount < 1 || (touch = Input.GetTouch(0)).phase != TouchPhase.Began)
{
	return;
}

@endcode

<li> In <i>Update</i>, create a variable to store the information about the ray (after the user touched the screen). It stores the coordinates of the place, from where the ray is cast. Add the filter that specifies the surfaces for collision with the ray. 

@code
// Raycast against the location the player touched to search for planes.
TrackableHit hit;
TrackableHitFlags raycastFilter = TrackableHitFlags.PlaneWithinPolygon |
TrackableHitFlags.FeaturePointWithSurfaceNormal;
@endcode

<li> In <i>Update</i>, process the raycasting: we make sure that the surface is tracked and then cast the ray from the appropriate side (because it’s against the football rules to throw the ball in the back of the goalkeeper). Then we process the throwing of a ball. The goal should be placed on the way of the ball; if not, we place the goal with the goalkeeper (<b>Environment</b>) and turn it accordingly. In this project, we use two rays - one is an ARCore ray (detects the surfaces), and the other is a Unity ray (which detects the required Unity object, the goal). 

@code
environment = FindObjectOfType<Environment>();

// Casting the ray to the surface found by ARCore.
if (Frame.Raycast(touch.position.x, touch.position.y, raycastFilter, out hit))
{
	// Use hit pose and camera pose to check if hittest is from the
	// back of the plane, if it is, no need to create the anchor.
	if ((hit.Trackable is DetectedPlane) &&
		Vector3.Dot(mainCamera.transform.position - hit.Pose.position,
			hit.Pose.rotation * Vector3.up) < 0)
	{
		Debug.Log("Hit at back of the current DetectedPlane");
	}
	else
	{
		// If the ball touched the correct (not reversed) side of the surface, check, whether there is the goal along the way  If the surface is "empty", place the goal. If there is the goal, then kick the ball.
		if (KickBall() == false && environment)
		{
			environment.transform.position = hit.Pose.position;
			environment.transform.rotation = hit.Pose.rotation;
			environment.transform.Rotate(0, modelRotation, 0, Space.Self);
		}
	}
}
else
{
	// If there are no surfaces but the goal on the way of the ARCore ray, then kick the ball.
	KickBall();
}
@endcode

<li> Check whether there is the goal on the way of the ball: if yes, kick the ball. Set up our ray, which is cast to the place defined by the user's touch. Put the target point if the ball touched anything. In the script, make the camera child to the <b>Environment</b> in order to find the local position (coordinates) of the camera in relation to the <b>Environment</b>. We need to do all that stuff to find the point from which the ball should be thrown.  Create a ball on the scene, set its position, rotation and the child object (<b>Environment</b>) (the same settings as the camera). Set the target point. Return the camera to the original position above the <b>ARCoreDevice</b>. The return method returns true only if the ball hit some object (and there was the goal), otherwise false. 

@code
// If all conditions are satisfied, kick the ball and return true, otherwise, false.
bool KickBall()
{
	Ray ray = mainCamera.ScreenPointToRay(Input.mousePosition);
	RaycastHit hitRay;

	// Cast a standard Unity ray.
	if (Physics.Raycast(ray, out hitRay, 100)  && environment)
	{
		environment.aim.position = hitRay.point;

		mainCamera.transform.parent = envirnoment.transform; // Temporarily make the camera child to the Environment.
		mainCamera.transform.parent = aRCoreDevice.transform; // Return the camera.
		return true;
	}
	return false;
}
@endcode

<li> In Unity, set the fields in <b>FootballController</b> as shown in the picture below: 

@image html images/UARCore_8.png
@image latex images/UARCore_8.png

<li> Connect your Android device to the PC and run the project. When a user points the camera of the Android device at real surfaces (for example, a table), he will see a grid. As the user touches the grid, an ARCore goal with a goalkeeper are placed. By default, a user sees one goal with a goalkeeper, which is created automatically after the start of the project. 

@image html images/UARCore_9.gif
@image latex images/UARCore_9.gif

@note
It's not necessary to build the project to test some ARCore functions - they're available from the Unity editor. You just need to connect your Android device via USB and run the project. If you don't need this function, you can easily disable it: untick <b>Edit/Project Settings/Arcore/Instant Preview enabled</b> 
</ol>

@section arcore_multiplayer Creating a Multiplayer

Since several players can participate in our game (one goalkeeper and several strikers), we have to create a server and client for network play.  The goalkeeper will be a server and strikers will connect to it as clients. All players should be in one Wi-Fi network. To connect to the server, a client will only need to press the <b>Connect</b> button. 

<ol>
<li> We'll use <i>Network Manager.cs</i>, which is a standard Unity script for networking. Add a new object to the <b>Striker</b> scene: <b>Empty Object → Network Manager</b>, and add the <b>Network Manager(Script)</b> component. 
<li> Save the <b>Environment</b> prefab and delete it from the scene.
<li> Add the <b>Environment</b> prefab to <b>Network Manager</b> to make our system know that this object should be spawned: <b>Network Manager (Script) → Registered Spawnable Prefabs</b>.

@image html images/UARCore_10.png
@image latex images/UARCore_10.png

<li> The <i>Network Discovery</i> script defines the searching of servers in the local network. This is a standard Unity script as well. We'll modify this script to make our game a little bit simpler for a user. By default, a user has to select the required server from the list of found servers. We'll make this project a bit more user-friendly: the connection to the server will be automatic. Create a new script <i>QuickConnectNetworkDiscovery.cs</i>. The <i>QuickConnectNetworkDiscovery</i> class must inherit from the standard <i>NetworkDiscovery</i> class.

@code
using UnityEngine.Networking;

public class QuickConnectNetworkDiscovery : NetworkDiscovery {

	public override void OnReceivedBroadcast(string fromAddress, string data)
	{
		base.OnReceivedBroadcast(fromAddress, data);
 
		if(NetworkManager.singleton.IsClientConnected()) 
			return;
 
		NetworkManager.singleton.networkAddress = fromAddress; // Found server IP is added to Network Manager.
		NetworkManager.singleton.StartClient(); // Connection.
	}
}
@endcode

<li> Drag-and-drop the <i>QuickConnectNetworkDiscovery</i> script to <b>NetworkManager</b>.
<li> In Unity, find <b>Network Manager</b>, select <b>QuickConnectNetworkDiscovery</b>, tick <b>Use Network Manager</b> and untick <b>Show GUI</b> to hide a debug menu.

@image html images/UARCore_11.png
@image latex images/UARCore_11.png

<li> Create a new script <i>NetworkController.cs</i>. In this script, we'll create a client and a server, and describe the actions of the server when the client is connected. 
<li> Add the namespaces <i>UnityEngine.UI</i> and <i>UnityEngine.Networking</i>. 
<li> Add the fields for scores, client / server, text fields. Hide the scores field to prevent setting the scores from the Unity editor. 

@code
[HideInInspector]public int score;
public bool isClient; // Select the client or server.

[Header("Server")]
[SerializeField] Text scoreText; // Scores text.
[SerializeField] Text connectionsText; // Text showing the number of connected clients.
[SerializeField]  GameObject environmentPrefab;

[Header("Client")]
[SerializeField] Text connectText; // Text showing the connection state.
@endcode

<li> If the <b>GoalKeeper</b> scene is run, the server is started. 
<li> In the <i>StartClient</i> method, the client is initialized and started. Similar, in the <i>StartServer</i> method the server, as well as the host, are initialized and started. The goalkeeper (as server) will see the scores, and the strikers (the client) will see the number of connected players. The <i>StartClient</i> method is bound to the <b>Connect</b> button. 

@code
private void Start()
{
	// If it's not a client, create a server.
	if (isClient == false)
	{
		StartServer();
	}
}

public void StartClient()
{
	FindObjectOfType<NetworkDiscovery>().Initialize();
	FindObjectOfType<NetworkDiscovery>().StartAsClient();
}

void StartServer()
{
	FindObjectOfType<NetworkDiscovery>().Initialize();
	FindObjectOfType<NetworkDiscovery>().StartAsServer();
	NetworkManager.singleton.StartHost();

	GameObject environment = (GameObject)Instantiate(environmentPrefab);
	NetworkServer.Spawn(environment);
}
@endcode

@note
You can learn more about clients and servers in Unity [here] (https://docs.unity3d.com/Manual/UNetDiscovery.html).

<li> In <i>Update</i>, update the text with the scores and number of connected players. Also, we'll add the button showing the text with the connection state (<b>Connect</b> or <b>Connected</b>).

@code
private void Update()
{
	if (isClient == false)
	{
		scoreText.text = “Scores: ” + score.ToString(); // Update the scores counter.
		connectionsText.text = "connected: " + NetworkManager.singleton.numPlayers; // 	Number of connected clients.
	}
	else
	{
		if(NetworkManager.singleton.IsClientConnected())
			connectText.text = "Connected";
		else
			connectText.text = "Connect";
	}
}
@endcode

<li> Drag-and-drop the <i>NetworkController.cs</i> script to the <b>Network Manager</b>. Tick <b>Is Client</b> (as this script describes the client's behavior). 

@image html images/UARCore_12.png
@image latex images/UARCore_12.png

<li> Create a <b>Canvas</b> that will be used to display the <b>Connect</b> button: <b>Create → UI → Canvas</b>. Create a button on it: <b>UI → Button</b> (the <b>Connect</b> button). 
<li> Select the button and add the object from the scene: <b>Button → OnClick + NetworkManager</b>, then select <b>Function → NetworkController → StartClient()</b> (when we press the button, the <i>StartClient</i> method is called). 

@image html images/UARCore_13.png
@image latex images/UARCore_13.png

<li> Drag-and-drop the text from the <b>Button</b> to the <b>Connect Text</b> field in <b>Network Controller</b>. 

@image html images/UARCore_14.png
@image latex images/UARCore_14.png
 
<li> Create a new scene and name it, for example, <b>GoalKeeper</b>. 
<li> Set the camera position to (0, 1, -5) so that the goal with the goalkeeper are displayed correctly.

@image html images/UARCore_15.png
@image latex images/UARCore_15.png

<li> Add the <b>NuitrackScripts</b> prefab from <b>NuitrackSDK.unitypackage</b> to the scene. Tick <b>Skeleton Module On</b> for skeleton tracking.

@image html images/UARCore_16.png
@image latex images/UARCore_16.png

<li> Create a <b>Canvas</b> on this scene and create two text fields - <b>ScoreText</b> and <b>ConnectedText</b> - for displaying the scores and connection text. Place the text fields on the <b>Canvas</b> the way you want.
</ol>

@section arcore_ball Creating a Ball

<ol>
<li> Time to create a ball on the <b>Striker</b> scene! We have to create two objects for the ball: the first object <b>Ball</b> always stays in one place and becomes child to the <b>Environment</b> when a user kicks the ball; the second object <b>Ball Model</b> is child to the <b>Ball</b> and moves when a user kicks the ball. Only the <b>Ball Model</b> object is synchronized. Create <b>Empty → Ball</b> and then add a standard script <b>Network Transform</b>, which synchronizes the movement and rotation of GameObjects across the network. 
<li> In <b>Network Transform</b>, set <b>Network Send Rate - 0</b> (as we don't synchronize the parent object), the other settings remain the same.

@image html images/UARCore_17.png
@image latex images/UARCore_17.png
 
<li> Add one more component to the <b>Ball</b> - <b>Network Transform Child</b> - and set its <b>Network Send Rate</b> to <b>20</b> (20 packages / 1 sec, this will make our ball move smoothly).

@image html images/UARCore_18.png
@image latex images/UARCore_18.png

<li> Create a child sphere on the <b>Ball</b>: <b>Create 3D → Object → Sphere</b> and call it, for example, <b>Ball Model</b>. Set up the <b>Ball Model</b>: Scale (0.3, 0.3, 0.3), Position (0, 0, 0), Rotation (0, 0, 0). 

@image html images/UARCore_19.png
@image latex images/UARCore_19.png

<li> Add the <b>RigidBody</b> component and untick <b>Use Gravity</b>.

@image html images/UARCore_20.png
@image latex images/UARCore_20.png
 
<li> In the <b>Ball</b> object, select <b>Network Transform Child → Target: Ball Model</b> (so that the position of the child object is synchronized between the server and client).

@image html images/UARCore_21.png
@image latex images/UARCore_21.png
 
<li> Create a script <i>BallController.cs</i>, in which we'll describe the behaviour of our ball. Add the field <i>startPosition</i> for the initial position of our ball and <i>networkController</i> that we'll use to differentiate the client from server, set its local coordinates and position. 

@code
[SerializeField]
GameObject ball;
Vector3 startPosition;
Vector3 endPosition;
float ballSpeed = 3;
Rigidbody rb;
bool inGame = true;
NetworkController networkController;

void Start()
{
	// Get a reference to the RigidBody component.
	rb = GetComponentInChildren<Rigidbody>();
	// Destroy the ball in 7 seconds after Start.
	Destroy(gameObject, 7.0f);
	transform.parent = FindObjectOfType<Environment>().transform;

	transform.localScale = Vector3.one;
	transform.localPosition = Vector3.zero;
	transform.localEulerAngles = Vector3.zero;

	ball.transform.localPosition = startPosition;

	networkController = FindObjectOfType<NetworkController>();
}
@endcode
<li> In <i>Update</i>, define the movement of our ball if it's in play. When the ball is in game, it moves to a specified point. The movement of the ball according to the script and Unity physics is processed on the server. The clients only receives the position of the ball.

@code
void Update () {
	if (inGame && networkController.isClient == false) 
	{
		ball.transform.localPosition = Vector3.MoveTowards(ball.transform.localPosition, endPosition, ballSpeed * Time.deltaTime); // Current position, end position, speed.
		ball.transform.Rotate(Vector3.one * ballSpeed); // Rotating the ball when it moves (just for fun).
	}
}
@endcode

@note
You can learn more about Vector3.MoveTowards in Unity [here] (https://docs.unity3d.com/ScriptReference/Vector3.MoveTowards.html).

<li> In the <i>Setup</i> method, set the start and end positions of the ball.

@code
public void Setup(Vector3 startPos, Vector3 endPos)
{
	endPosition = endPos;
	startPosition = startPos;
}
@endcode

<li> When the ball touches other objects, the physics is enabled. If the ball touches the hand, one point is added. 

@code
public void OnCollide(Collision collision)
{
	// If the ball touched something, the inGame variable changes its status to false so that collisions are no longer processed.
	if (inGame && networkController.isClient == false) 
	{
		Debug.Log("Ball collide");
		// If the ball touched the hand, add one point.
		if (collision.transform.tag == "Hand")
			FindObjectOfType<NetworkController>().score++;

		// Enable gravity to make our ball fall.
		rb.useGravity = true;
		inGame = false;
	}
}
@endcode

<li> The <b>Ball Model</b> object, which is a child to the <b>Ball</b>, includes colliders, unlike its parent. Let's create a new script <i>CollideChecker.cs</i>, in which we'll describe the behavior of our ball when it collides with other objects.

@code
using UnityEngine;
 
public class CollideChecker : MonoBehaviour 
{
	private void OnCollisionEnter(Collision collision)
	{
		GetComponentInParent<BallController>().OnCollide(collision); // If the ball touched any object, send this information to BallController, which is attached to a parenting object.
	}
}
@endcode

<li> Drag-and-drop the script to the <b>Ball Model</b>.
<li> Drag-and-drop the <i>BallController.cs</i> script to the <b>Ball</b> and put the <b>Ball Model</b> to the <b>Ball</b> field.

@image html images/UARCore_22.png
@image latex images/UARCore_22.png

<li> Save the <b>Ball</b> as a prefab and delete it from the scene. After that define that the <b>Ball</b> will be automatically spawned on the server and on the client:  <b>Network Manager - Network Manager (Script) → Spawn Info → Registered Spawnable Prefabs → Ball - Ball</b>.
</ol>

@image html images/UARCore_23.png
@image latex images/UARCore_23.png

@section arcore_striker Creating a Striker

<ol>
<li> In Unity, let's create one more important player for our game - a striker: <b>Empty Object → Player</b>. 
<li> Create a new script called <i>PlayerController.cs</i>, in which we'll describe the actions of the striker. Add the <i>Networking</i> namespace to the script. Inherit the class from <i>NetworkBehaviour</i> so that it can send and receive the messages from the server. Create a field for the <i>ballPrefab</i>. Create a new method <i>Kick</i>, in which we'll set the start and end positions of the ball. This method is called on the server. To call the method on the server, add the <i>[Command]</i> attribute and the <i>Cmd</i> method prefix. 

@code
using UnityEngine;
using UnityEngine.Networking;

public class PlayerController : NetworkBehaviour {

	[SerializeField] GameObject ballPrefab;

	[Command] 
	void CmdKick(Vector3 startPos, Vector3 endPos)
	{
		GameObject ball = (GameObject)Instantiate(ballPrefab);
		ball.GetComponent<BallController>().Setup(startPos, endPos);
		NetworkServer.Spawn(ball);
	}

	public void Kick(Vector3 startPos, Vector3 endPos)
	{
		CmdKick(startPos, endPos);
	}
}
@endcode

<li> Drag-and-drop <i>PlayerController.cs</i> to the <b>Player Controller</b> object and drag-and-drop the <b>Ball</b> to the <b>Ball Prefab</b> field. Save the <b>Player</b> object as a prefab and delete it from the scene. 
<li> Add the sending of a message on the ball kick to the server. 

@code
bool KickBall()
{
...
	if (Physics.Raycast(ray, out hitRay, 100) && environment)
	{
...

		FindObjectOfType<PlayerController>().Kick(mainCamera.transform.localPosition, environment.aim.transform.localPosition);
...
		return true;
	}
}
@endcode

<li> Then select <b>NetworkManager → Network Manager (Script) → Spawn Info</b> and drag-and-drop <b>Player</b> to <b>Player Prefab</b> (please note that the <b>Player</b> prefab should have the <b>Network Identity</b> component). 

@image html images/UARCore_24.png
@image latex images/UARCore_24.png

<li> Copy the <b>NetworkManager</b> object to the <b>GoalKeeper</b> scene. 
<li> On the <b>GoalKeeper</b> scene, untick <b>AutoCreatePlayer</b> in <b>Network Manager</b> so that a new player is not created in this scene - we need only one goalkeeper.

@image html images/UARCore_25.png
@image latex images/UARCore_25.png
 
<li> Untick <b>Is Client</b> on the server. 

@image html images/UARCore_26.png
@image latex images/UARCore_26.png

<li> In <b>Network Manager</b>, select <b>Server</b> and set the fields for the scores and connection texts: <b>ScoreText - ScoreText (Text)</b>, <b>Connection Text - Connection Text (Text)</b>. 

@image html images/UARCore_27.png
@image latex images/UARCore_27.png

<li> Put the <b>Environment</b> prefab to the <b>Environment Prefab</b> field.

@image html images/UARCore_28.png
@image latex images/UARCore_28.png

<li> To keep the size of the <b>Environment</b> on the server, add the following code to the <i>Start</i> method of the <i>Environment.cs</i> script:

@code
void Start()
{
	if(FindObjectOfType<NetworkIdentity>().isServer == false)
		transform.localScale = clientSize;
}
@endcode

<li> In <b>Build Settings</b>, untick <b>GoalKeeper</b> (as we don’t need it on the Android device)

@image html images/UARCore_29.png
@image latex images/UARCore_29.png

<li> Go to <b>Player Settings → XR Settings</b> and untick <b>ARCore</b> (as it's not needed on TVico).

@image html images/UARCore_30.png
@image latex images/UARCore_30.png

<li> Select the <b>Android version</b> as on TVico: <b>Other Settings → Android</b> (our version on TVico is 5.1.1).

@image html images/UARCore_31.png
@image latex images/UARCore_31.png

<li> Connect your TVico to your PC via USB, click <b>Build and Run</b> or just connect a compatible sensor to the desktop. 
<li> Build the project in two stages: 
<ol>
<li> First of all, build the <b>Striker</b> scene with ARCore on the Android device.
<li> Then, build the <b>GoalKeeper</b> scene without ARCore on the TVico or PC with a sensor.
</ol>
</ol>

Our project is almost ready: a "striker" user places the goal and goalkeeper on the grid and throws the ball. In his turn, the "goalkeeper" user tries to catch the ball using the TV screen / desktop. 

@image html images/UARCore_32.gif
@image latex images/UARCore_32.gif

However, at this stage the "striker" can notice that the goalkeeper's avatar does not move but somehow catches the ball. To eliminate this issue, we have to synchronize the server and client. 

@section arcore_sync Synchronizing the Server and Client

<ol>
<li> All right, it's time to write some more code to synchronize the movement of avatars. Create a new script, let's call it <i>AvatarSync.cs</i>. 

@note
As an alternative, you can synchronize the server and client without writing the script: just use 11 components (10 bones and the Avatar) of the Network Transform Child. We've synchronized the Ball using this way.

<li> Add the <i>Networking</i> namespace and inherit the <i>AvatarSync</i> class from <i>NetworkBehaviour</i>. Add an array with bones and avatar transform that will be synchronized. When the rotation of bones and position of the avatar are changed, the server sends messages. 

@code
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
 
public class AvatarSync : NetworkBehaviour 
{
	[SerializeField] Transform[] syncBones;  
	[SerializeField] Transform avatar;  
}

[ClientRpc] // Server sends a message to all clients.
public void RpcOnBonesTransformUpdate(BonesInfoMessage boneMsg)
{
	for (int i = 0; i < boneMsg.bonesRot.Length; i++)
	{
		syncBones[i].localRotation = boneMsg.bonesRot[i];
}
 
	avatar.localPosition = boneMsg.avatarPos;
}

public class BonesInfoMessage : MessageBase
{
	public Quaternion[] bonesRot;  // Rotations of bones.
	public Vector3 avatarPos;  // Avatar position.
}
@endcode

<li> Check whether it is the server or not in the <i>FixedUpdate</i> method.

@code
private void FixedUpdate()
{
	if (isServer)
	{
		BoneUpdate(syncBones);
	}
}
@endcode

<li> Update the information about the bones and avatar and send a message. 

@code
public void BoneUpdate(Transform[] bones)
{
	List<Quaternion> rotations = new List<Quaternion>();
 
	for (int i = 0; i < bones.Length; i++)
		rotations.Add(bones[i].localRotation);
 
	BonesInfoMessage msg = new BonesInfoMessage
	{
		bonesRot = rotations.ToArray(), // Rotations of bones.
		avatarPos = avatar.position,
	};
 
	RpcOnBonesTransformUpdate(msg); // Sending the message. 
}
@endcode

<li> Drag-and-drop the script to the <b>Environment</b> prefab.
<li> Put the <b>RiggedAvatar</b> object to the <b>Avatar</b> field. Fill in the <b>Sync Bones</b> array with the bones: you can find the bones on the <b>Rigged Avatar</b> object in the <b>Rigged Avatar</b> array: <b>Rigged Model → Model Joints</b> (all in all, you have to select 10 bones).

@image html images/UARCore_33.png
@image latex images/UARCore_33.png
 
<li> Run the project. Now everything is ready for play: the server and client are synchronized, avatars are moving and you're ready to have fun with your friends!  
</ol>

@image html images/UARCore_34.gif
@image latex images/UARCore_34.gif
*/
