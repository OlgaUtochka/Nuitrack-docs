/*!
@page UnityGallery_page Creating an Interactive Multi-Touch Gallery
  @tableofcontents

In this tutorial, you'll learn how to create a virtual gallery that can be controlled using gestures. The gallery will have two modes: preview (several images displayed on one page) and view (when you click on an image, it opens in full screen). The gallery can be controlled with one or two hands (multi-touch) and gestures "click", "swipe up", "swipe left", "swipe right". For this project, you'll need Nuitrack and a sensor (from the list of supported sensors, see the [Nuitrack website] (https://nuitrack.com/)).

You can find the finished project in <b>Nuitrack SDK</b>: <b>Unity 3D → NuitrackSDK.unitypackage → Tutorials → HandTracker</b> 

@image html images/Ugallery_15.gif 
@image latex images/Ugallery_15.gif 

@section gallery_scene Setting Up the Scene

<ol>
<li> Drag-and-drop the <b>Nuitrack Scripts</b> prefab from the <b>Nuitrack SDK</b> and tick the required modules in the <b>Nuitrack Manager</b> section: <b>Skeleton Tracker Module</b> (tracking of a user), <b>Hands Tracker Module</b> (detecting user's hands), <b>Gestures Recognizer Module</b> (gesture recognition).

@image html images/Ugallery_1.png Required Nuitrack modules for this project
@image latex images/Ugallery_1.png Required Nuitrack modules for this project

<li> Create a new script <i>Pointer.cs</i>. It will store the settings of a pointer that is used to control the gallery. In the <i>Start</i> method, subscribe to the <i>onHandsTrackerUpdate</i> event to receive data on the state of user's hands. 

@code
private void Start()
{  	 
	NuitrackManager.onHandsTrackerUpdate += NuitrackManager_onHandsTrackerUpdate;
}
@endcode

<li> Unsubscribe from this event in the <i>OnDestroy</i> method in order to prevent issues with null references when switching to another scene. 

@code
private void OnDestroy()
{
	NuitrackManager.onHandsTrackerUpdate -= NuitrackManager_onHandsTrackerUpdate;
}
@endcode

<li> In the <i>NuitrackManager_onHandsTrackerUpdate</i> method, check whether the left or the right hand is used for control. Add the processing of data on user's hands to move the corresponding pointer. Define that the "click" event occurs when a user clenches his hand into a fist. If a hand is inactive, it is hidden. 

@code
private void NuitrackManager_onHandsTrackerUpdate(nuitrack.HandTrackerDatahandTrackerData)
{
	bool active = false;
	bool press = false;

	foreach (nuitrack.UserHands userHands in handTrackerData.UsersHands) 
	{
		if (currentHand == Hands.right && userHands.RightHand != null)
		{
			baseRect.anchoredPosition = new Vector2(userHands.RightHand.Value.X * Screen.width, -userHands.RightHand.Value.Y * Screen.height);
			active = true;
			press = userHands.RightHand.Value.Click;
		}
		else if (currentHand == Hands.left && userHands.LeftHand != null)
		{
			baseRect.anchoredPosition = new Vector2(userHands.LeftHand.Value.X * Screen.width, -userHands.LeftHand.Value.Y * Screen.height);
			active = true;
			press = userHands.LeftHand.Value.Click;
		}
	}
}
@endcode

<li> In the <i>Pointer.cs</i> script, add fields for hands and <i>RectTransform</i>, as well as for the background, default sprite and "click" sprite. 

@code
using UnityEngine.EventSystems;
using UnityEngine.UI;

public class Pointer : MonoBehaviour
{
	public enum Hands { left = 0, right = 1 };
	 
	[SerializeField]
	Hands currentHand;
	 
	[Header ("Visualization")]
	[SerializeField]
	RectTransform baseRect;

	[SerializeField]
	Image background;
	 
	[SerializeField]
	Sprite defaultSprite;
	 
	[SerializeField]
	Sprite pressSprite;
}
@endcode

<li> Depending on the state of a hand, the pointer will be either displayed or hidden. To visualize the clenched hand, let's use the "click" sprite and replace this in the <b>Image</b> component.

@code
background.enabled = active;
background.sprite = active && press ? pressSprite : defaultSprite;
@endcode

@note
Learn more about ?: operator at the [Microsoft website] (https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/conditional-operator). 

<li> In Unity, create a <b>Canvas</b> to display our gallery. In the <b>Canvas</b>, create the <b>RHand</b> and <b>LHand</b> Images to display the pointers: <b>UI → Image</b>. Set up the <b>Camera</b>: in the <b>Canvas</b>, select <b>Render Mode → Screen Space Camera</b>; <b>Render Camera → Main Camera</b>. 

@image html images/Ugallery_2.png Canvas settings
@image latex images/Ugallery_2.png Canvas settings

<li> Drag-and-drop the <i>Pointer.cs</i> script to the <b>LHand</b> and <b>RHand</b> Images.
<li> Drag-and-drop the <b>Image</b> (sprite used to visualize a hand) to <b>RHand</b> and <b>LHand</b>. Set the following settings: <b>Rect Transform → Top Left Alignment</b> so that that the origin of coordinates of the pointer is in the upper left corner.

@image html images/Ugallery_3.png LHand and RHand settings 
@image latex images/Ugallery_3.png LHand and RHand settings 

<li> In Unity, specify the settings of a right hand: <b>Current Hand → Right</b>, make a reference to <b>BaseRect</b>. For a left hand, do the same thing. Create a reference to the <b>Image</b> component: <b>Background → Image</b>. Set the sprite for the "press" pointer: <b>Press Sprite → HandDown</b>.

@image html images/Ugallery_4.png Pointer settings
@image latex images/Ugallery_4.png Pointer settings

<li>  Run the project. The pointers should be displayed and move according to the user's movements. Also, the "press" sprite should appear when the user clenches his hand. 
</ol>

@image html images/Ugallery_5.gif Moving pointers
@image latex images/Ugallery_5.gif Moving pointers

@section gallery_create Creating a Gallery

<ol>
<li> In Unity, add an object for scrolling the content in our gallery to the <b>Canvas</b>: <b>GameObject → ScrollView</b>. Edit the <b>ScollView</b> settings: in the <b>Scroll Rect</b>, untick <b>Vertical</b>. For the <b>Scroll Rect</b>, set the alignment along the edges so that it's stretched up to the <b>Canvas</b> edges (even if you resize the screen, it will fill up the entire screen). 

@image html images/Ugallery_6.png Scroll View settings
@image latex images/Ugallery_6.png Scroll View settings

<li> For the <b>Content</b>, set the Top Left alignment so that it does not move to the side and the origin of coordinates is at the top left of the <b>ScrollRect</b>. 

@image html images/Ugallery_7.png Content settings
@image latex images/Ugallery_7.png Content settings

@note
We disable <b>Vertical</b> in our project because we would like to scroll our gallery only horizontally. However, you can create a gallery with a vertical scroll or a vertical and horizontal scroll, if you'd like to. 

<li> Create a new script <i>GalleryControl.cs</i>. In this script, we'll define the settings and types of control for our gallery. Add the fields for the image display mode, control elements and additional configuration settings of our gallery. The gallery supports two modes: previewing and viewing images. 

@code
public class GalleryControl : MonoBehaviour
{
	enum ViewMode { Preview, View };
	ViewMode currentViewMode = ViewMode.Preview;
	 
	[Header("Visualization")]
	 
	[SerializeField] ScrollRect scrollRect;
	[SerializeField] Sprite[] spriteCollection;
	[SerializeField] RectTransform content;
	[SerializeField] GameObject imageItemPrefab;
}
@endcode

<li> Set the number of columns and rows displayed on a page in the gallery view mode (you can set any positive numbers that you want). Set the variables to store a page size, number of pages and size of an image in the preview mode. 

@code
[Range(1, 10)]
[SerializeField] int rowsNumber = 3;
[Range(1, 10)]
[SerializeField] int colsNumber = 4;

Vector2 pageSize;
int numberOfPages = 0;

Vector2 defaultSize;
@endcode

<li> In the <i>Start</i> method, fill the gallery with pictures. 

@code
void Start()
{
	pageSize = new Vector2(Screen.width, Screen.height);
	defaultSize = new Vector2(Screen.width / colsNumber, Screen.height / rowsNumber); // calculate the size of an image in the preview mode

	Vector2 halfAdd = new Vector2(defaultSize.x / 2, -defaultSize.y / 2); 

	int imagesOnPage = rowsNumber * colsNumber;
	numberOfPages = (int)Mathf.Ceil((float)spriteCollection.Length / imagesOnPage); // divide the total number of pictures by the number of pictures on one page and round it up

	int imageIndex = 0;

	for (int p = 0; p < numberOfPages; p++) // iterate over the pages
	{
		int imagesOnCurrentPage = Mathf.Min(spriteCollection.Length - p * imagesOnPage, imagesOnPage); // set the number of images on a current page

		for (int i = 0; i < imagesOnCurrentPage; i++) // fill the current page with images
		{
			// Create an image object from the prefab and make it child to the content
			GameObject currentItem = Instantiate(imageItemPrefab);
			currentItem.transform.SetParent(content.transform, false);

			// Calculate and specify the position on the content
			RectTransform currentRect = currentItem.GetComponent<RectTransform>();
			currentRect.sizeDelta = defaultSize;

			float X = pageSize.x * p + defaultSize.x * (i % colsNumber);
			float Y = defaultSize.y * (i / colsNumber);

			currentRect.anchoredPosition = new Vector2(X, -Y) + halfAdd;

			// Drag-and-drop to the sprite
			Image currentImage = currentItem.GetComponent<Image>();
			currentImage.sprite = spriteCollection[imageIndex];
			imageIndex++;
		}
	}

	content.sizeDelta = new Vector2(Screen.width * numberOfPages, Screen.height); // set the content size
}
@endcode

<li> In Unity, drag-and-drop the <i>GalleryControl.cs</i> script to the <b>Canvas</b>. Drag-and-drop: <b>Scroll Rect → Scroll View</b>,  <b>Content → Content</b>. Create an <b>Image</b> object for an image: <b>Content →  GameObject → UI → Image</b>. Create a prefab and drag-and-drop the <b>Image</b> object to this prefab. Drag-and-drop this prefab to <b>GalleryControl</b> (<b>Gallery Control → Image Item Prefab</b>). In the <b>GalleryControl</b> settings, set the desired number of columns and rows with images.

@image html images/Ugallery_8.png Gallery Control settings
@image latex images/Ugallery_8.png Gallery Control settings

@note
The gif below shows a quick way to fill your gallery with images:
</ol>

@image html images/Ugallery_9.gif Fill your gallery with images in seconds! 
@image latex images/Ugallery_9.gif Fill your gallery with images in seconds! 

@section gallery_pages Turning the Pages

<ol>
<li> To add the function of turning the pages to our gallery, add the fields with the values that define the speed of turning the page, offset step for the <i>ScrollRect</i> element and the current page number. 

@code
[Header("Scroll")]
 
[Range(0.1f, 10)]
[SerializeField] float scrollSpeed = 4f;
 
float scrollStep = 0;

int currentPage = 0;
@endcode

<li> In the <i>GalleryControl.cs</i> script in the <i>Start</i> method, subscribe the <i>NuitrackManager_onNewGesture</i> method to the <i>onNewGesture</i> event of the <b>NuitrackManager</b> component to receive events of gestures. Unsubscribe from this event in the <i>OnDestroy</i> method.

@code
void Start()
{
	NuitrackManager.onNewGesture += NuitrackManager_onNewGesture;
}

	private void OnDestroy()
	{
		NuitrackManager.onNewGesture -= NuitrackManager_onNewGesture;
	}
@endcode

<li> Calculate the scrolling step in the <i>Start</i> method.

@code
if (numberOfPages > 1)
	scrollStep = 1f / (numberOfPages - 1); // 1/(n-1) given that the Scrollbar takes values from 0 to 1 and one page is already displayed
@endcode

<li> First, check the gallery mode (View, Preview) in the <i>NuitrackManager_onNewGesture</i> method, then define the gesture type and, depending on the result, increment or decrement the number of the current page. To ensure that the page number is not out of range, let's set the value in the range  from 0 to the total number of pages in the <i>Mathf.Clamp</i> function.

@code
private void NuitrackManager_onNewGesture(nuitrack.Gesture gesture)
{
	switch (currentViewMode)
	{
		case ViewMode.Preview:
 
			if (gesture.Type == nuitrack.GestureType.GestureSwipeLeft)
			currentPage = Mathf.Clamp(++currentPage, 0, numberOfPages);

			if (gesture.Type == nuitrack.GestureType.GestureSwipeRight)
			currentPage = Mathf.Clamp(--currentPage, 0, numberOfPages);

		break;
	}
}
@endcode

@note
<b>Nuitrack</b> supports the following types of gestures: Waving, Push, Swipe Up, Swipe Down, Swipe Left, Swipe Right. 

<li> In the <i>Update</i> method, add smooth turning of pages to the current page.  

@code
private void Update()
{
	case ViewMode.Preview:
		scrollRect.horizontalScrollbar.value = Mathf.Lerp(scrollRect.horizontalScrollbar.value, scrollStep * currentPage, Time.deltaTime * scrollSpeed); // move the content until the current page is displayed 
	break;
}
@endcode

<li> Run the project. You should see a gallery with images that can be scrolled to the left or to the right. 
</ol> 

@image html images/Ugallery_10.gif Scrolling the gallery
@image latex images/Ugallery_10.gif Scrolling the gallery

@section gallery_interaction Adding Interaction with Gallery Elements and the "Click" event

<ol>
<li> Create a new script <i>ImageItem.cs</i>. In this script, we'll describe the interaction with images in our gallery. Inherit it not from the <i>MonoBehaviour</i> class but from the [<i>Selectable</i>](https://docs.unity3d.com/ScriptReference/UI.Selectable.html) class so that it conforms to the general rules of interaction with the UI system in Unity. 
<li> In Unity, drag-and-drop this script to the <b>ImageItem</b>, set the lighting of images in the gallery depending on the pointer position. 

@image html images/Ugallery_11.png Lighting settings
@image latex images/Ugallery_11.png Lighting settings

<li> To interact with elements in the UI, we need to know, which element the user points to. If you use the  mouse pointer, the process is performed using the built-in Unity  components [<i>StandaloneInputModule</i>](https://docs.unity3d.com/Manual/script-StandaloneInputModule.html), [<i>EventSystem</i>](https://docs.unity3d.com/ScriptReference/EventSystems.EventSystem.html) and [<i>GraphicRaycaster</i>](https://docs.unity3d.com/Manual/script-GraphicRaycaster.html). In our case, we use our custom pointers, and we have to determine their logic of interaction. Similarly to the mouse pointer, to get the interface element that is located under our  pointer, we use <i>RayCast</i> for the UI system that "penetrates" the <b>Canvas</b> and all the elements and returns a list of all the "penetrated" items. 

To implement  <i>RayCast</i>, we need a camera from which we will throw a beam, a variable storing the current element, a variable storing the data on the point ([<i>PointerEventData</i>](https://docs.unity3d.com/ScriptReference/EventSystems.PointerEventData.html)) selected by the user, and a list of elements acting as a temporary results storage <i>RayCast</i>. 

@code
[Header("Raycasting")]
 
[SerializeField]
Camera cam;
 
ImageItem selectedButton;
 
PointerEventData eventData = new PointerEventData(null);
List<RaycastResult> raycastResults = new List<RaycastResult>();
@endcode

<li> If the pointer is active, we can determine which element the user is pointing to. First, define the point at which the ray will be thrown. Then project the position of the pointer relative to the screen: to do this, call  the <i>WorldToScreenPoint</i> camera method. Also, define the pointer offset, which we will need later. Clear the <i>RayCast</i> list of elements from the previous frame and perform raycasting using <i>RaycastAll</i>. Then iterate the list in a loop to find the element with the <i>ImageItem</i> component for interaction.

@code
if (!active)
    return;

Vector2 pointOnScreenPosition = cam.WorldToScreenPoint(transform.position);
eventData.delta = pointOnScreenPosition - eventData.position;
eventData.position = pointOnScreenPosition;
 
raycastResults.Clear(); // clear the raycasting results from the previous frame 

EventSystem.current.RaycastAll(eventData, raycastResults);
 
ImageItem newButton = null;
 
for (int q = 0; q < raycastResults.Count && newButton == null; q++)
      newButton = raycastResults[q].gameObject.GetComponent<ImageItem>();
 
if (newButton != selectedButton)
{
	if (selectedButton != null)
		selectedButton.OnPointerExit(eventData);
 
		selectedButton= newButton;
 
	if (selectedButton != null)
		selectedButton.OnPointerEnter(eventData);
}
@endcode

<li> In Unity, set the camera that will be used in <b>Pointer</b> for raycasting: <b>RHand и LHand → Pointer → Raycasting → Cam → Main Cam</b>.

@image html images/Ugallery_12.png Raycasting settings
@image latex images/Ugallery_12.png Raycasting settings
 
<li> In the <i>Pointer.cs</i> script, add the "press / release" event handling.  The "click" event will be performed if the user's hand moves at a speed lower than the one specified in <i>dragSensitivity</i>. This is necessary to get rid of "phantom" clicks when the user actively moves his hand. 

@code
else if (selectedButton != null)
{
	if (press)
	{
		if (eventData.delta.sqrMagnitude < dragSensitivity)
		{
			eventData.dragging = true;
			selectedButton.OnPointerDown(eventData);
		}
	}
	else if (eventData.dragging)
	{
		eventData.dragging = false;
		selectedButton.OnPointerUp(eventData);
	}
}
@endcode

<li> In the <i>ImageItem.cs</i> script, define the "click" event. 

@code
public class ImageItem : Selectable
{
	public delegate void Click(ImageItem currentItem);
	public event Click OnClick;
}
@endcode

<li> Override the <i>OnPointerUp</i> method: when the hand is released, the click will be counted. 

@code
public override void OnPointerUp(PointerEventData eventData)
{
	OnClick(this);
	InstantClearState();
 
	base.OnPointerUp(eventData);
}
@endcode

<li> In the <i>GalleryControl.cs</i> script in the <i>Start</i> method, add a field for <i>canvasGroup</i>, and also define the parameters for full-screen image view. 

@code
[SerializeField] CanvasGroup canvasGroup;

[Header("View")]
[SerializeField] RectTransform viewRect;

Vector2 defaultPosition;
 
[Range(0.1f, 16f)]
[SerializeField] float animationSpeed = 2;
 
ImageItem selectedItem = null;

bool animated = false;
float t = 0; // current animation time
@endcode

<li> Subscribe to the "click" event. 

@code
for (int i = 0; i < imagesOnCurrentPage.Length; i++)
{
...
	currentImage.sprite = spriteCollection[imageIndex];
	imageIndex++;

	ImageItem currentImageItem = currentItem.GetComponent<ImageItem>();
	currentImageItem.OnClick += CurrentImageItem_OnClick;
}
@endcode

<li> Add the mode check: from the preview mode, the switching to the view mode is performed. If a user clicks on the picture, the picture is thrown to the <i>viewRect</i>, so that it does not fade like the other pictures on the content (in <b>Scroll Rect</b>). Using the [<i>canvasGroup.interactable</i>](https://docs.unity3d.com/Manual/class-CanvasGroup.html) variable, disable interactivity of all  pictures left from the content (the actions of the <i>canvasGroup</i> script are applied to all its children, that is, when we disable its interactivity, then interactivity is disabled for all its children, too).

@code
private void CurrentImageItem_OnClick(ImageItem currentItem)
{
	if (currentViewMode == ViewMode.Preview)
	{
		t = 0;
		currentViewMode = ViewMode.View;
		selectedItem = currentItem;

		canvasGroup.interactable = false;
		selectedItem.interactable = false;

		selectedItem.transform.SetParent(viewRect, true);
		defaultPosition = selectedItem.transform.localPosition;
	}
}
@endcode

<li> In the <i>Update</i> method, determine the characteristics of the animation in the gallery: when you click and switch to the view mode, the image opens in full screen. 

@code
private void Update()
{
	switch (currentViewMode)
	{
		case ViewMode.View:
 
			if (t < 1)
			{
				// Add the delta multiplied by the specified speed to the current animation time
				t += Time.deltaTime * animationSpeed;

				// Shift the transparency of the remaining pictures to 0 exponentially from t
				canvasGroup.alpha = Mathf.Lerp(canvasGroup.alpha, 0, t);

				// Stretch the picture to the full screen size exponentially from t
				selectedItem.image.rectTransform.sizeDelta = Vector2.Lerp(selectedItem.image.rectTransform.sizeDelta, pageSize, t);
				// Move the picture to the center of the screen exponentially from t
				selectedItem.transform.localPosition = Vector2.Lerp(selectedItem.transform.localPosition, Vector2.zero, t);
			}
 
		break;
	...
	}
}
@endcode

<li> Add the animation in case the "preview" mode is selected. 

@code
...
case ViewMode.Preview:
 
	if (animated)
	{
		if (t > 0)
		{
			// Roll back the time from 1 to 0
			t -= Time.deltaTime * animationSpeed;

			canvasGroup.alpha = Mathf.Lerp(1, canvasGroup.alpha, t);

			selectedItem.image.rectTransform.sizeDelta = Vector2.Lerp(defaultSize, selectedItem.image.rectTransform.sizeDelta, t);

			selectedItem.transform.localPosition = Vector2.Lerp(defaultPosition, selectedItem.transform.localPosition, t);
			selectedItem.transform.localRotation = Quaternion.Lerp(Quaternion.identity, selectedItem.transform.localRotation, t);
			selectedItem.transform.localScale = Vector3.Lerp(Vector3.one, selectedItem.transform.localScale, t);
		}
		else
		{
			// Make the image a child of the content
			selectedItem.transform.SetParent(content, true);
			// Return interactivity to all elements
			selectedItem.interactable = true;
			canvasGroup.interactable = true;
			// Discard the selected image and stop the animation
			selectedItem = null;
			animated = false;
		}
	}
	else
		scrollRect.horizontalScrollbar.value = Mathf.Lerp(scrollRect.horizontalScrollbar.value, scrollStep * currentPage, Time.deltaTime * scrollSpeed);

break;
@endcode

<li> Add the actions to be performed when exiting the view mode.

@code
private void NuitrackManager_onNewGesture(nuitrack.Gesture gesture)
{
	case ViewMode.View:
 
		// If there was a swipe up, then switch to the preview mode and start the animation
		if (gesture.Type == nuitrack.GestureType.GestureSwipeUp)
		{
			currentViewMode = ViewMode.Preview;
			animated = true;
		}

	break;
}
@endcode

<li> In Unity, add the <b>Canvas Group</b> component, which we need to make the background  transparent when the image opens in the view mode: <b>Unity → Scroll View → Add Component → Canvas Group</b>. In the <b>Canvas</b>, set black as the background color (to make our gallery even more beautiful). Add the <b>Panel</b> object to the Canvas so that we have an empty rectangle, in which we will put the picture in the view mode: <b>Canvas → GameObject → UI → Panel</b> and delete the <b>Image</b> component. To correctly display the gallery and pointers, <b>Canvas</b> should have the following hierarchy: 
<ul>
<li> Scroll View
<li> View
<li> RHand
<li> LHand
</ul>
<li> Drag-and-drop the area that will contain an image in the view mode to the Canvas: <b>Gallery Control → View Rect → View (Rect Transform)</b>.

@image html images/Ugallery_13.png Setting the View Rect
@image latex images/Ugallery_13.png Setting the View Rect

<li> Run the project. Images will now change color depending on the cursor position. After the click, the image will open in the view mode. You can close the image with a swipe up. 
</ol>

@image html images/Ugallery_14.gif Interactive gallery 
@image latex images/Ugallery_14.gif Interactive gallery 

@section gallery_dragging Dragging, zooming and rotating the Images in View Mode

<ol>
<li> In the <i>ImagItem.cs</i> script, add the handling for One-Touch and Multi-Touch events. Add fields for storing current touches, initial position, rotation, and the image scale.

@code
List<PointerEventData> touches = new List<PointerEventData>();

Vector3 startCenter;
Vector3 startPosition;

Vector3 startScale;
float startHandDistance;

float startAngle;
Quaternion startRotation;
@endcode

<li> Override the <i>OnPointerExit</i> method: if the hand is outside the picture, then the touch is canceled. 

@code
public override void OnPointerExit(PointerEventData eventData)
{
	touches.Remove(eventData);
	base.OnPointerExit(eventData);
}
@endcode

<li> Since the change in the image state will be calculated based on the changes made since the start of capture until the current moment, determine the method for saving the initial image state.

@code
void UpdateInitialState()
{
	if (OneTouch)
	{
		startCenter = touches[0].position;  
	}
	else if (MultiTouch)
	{
		startCenter = (touches[0].position + touches[1].position) / 2;
		startHandDistance = (touches[0].position - touches[1].position).magnitude;

		Vector3 pointRelativeToZero = touches[1].position - touches[0].position;
		startAngle = Mathf.Atan2(pointRelativeToZero.x, pointRelativeToZero.y) * Mathf.Rad2Deg;
	}

	startScale = transform.localScale;
	startPosition = transform.localPosition;
	startRotation = transform.localRotation;
}
@endcode

<li> Override the <i>OnPointerDown</i> method: if the hand is pressed, then the touch is added.

@code
public override void OnPointerDown(PointerEventData eventData)
{
	if (!touches.Contains(eventData))
	{
		touches.Add(eventData);
		UpdateInitialState();
	}

	base.OnPointerDown(eventData);
}
@endcode

<li> Override the <i>OnPointerUp</i> method: when the hand is unclenched, the image is no longer held.

@code
public override void OnPointerUp(PointerEventData eventData)
{
	touches.Remove(eventData);
	UpdateInitialState();
	...
}
@endcode

<li> Add support for the [<i>IDragHandler</i>](https://docs.unity3d.com/ScriptReference/EventSystems.IDragHandler.html) interface for handling drag-and-drop.

@note
In this project, you can move images not only with hands, but also with the mouse pointer. 

<li> Define the <i>OnDrag</i> method: it will perform dragging, scaling and rotating of an image. For one pointer, only drag-and-drop is available. For two pointers, all actions are available.

@code
public void OnDrag(PointerEventData eventData)
{
	if (interactable || !eventData.dragging)
		return;
 
	if(OneTouch)
	{
		// Take the initial saved position and add the offset from that moment
		Vector3 currentCenter = touches[0].position;
		transform.localPosition = startPosition + (currentCenter - startCenter);
	}
	else if(MultiTouch)
	{
		// Calculate the center between two hands and add the center offset from start of capture
		Vector3 currentCenter = (touches[0].position + touches[1].position) / 2;
		transform.localPosition = startPosition + (currentCenter - startCenter);

		// Calculate the change in distance between two hands
		float currentHandDistance = (touches[0].position - touches[1].position).magnitude;
		transform.localScale = startScale * Mathf.Abs(currentHandDistance / startHandDistance);

		// Calculate the angle of one hand relative to the other
		Vector3 pointRelativeToZero = touches[1].position - touches[0].position;
		float angle = Mathf.Atan2(pointRelativeToZero.x, pointRelativeToZero.y) * Mathf.Rad2Deg;

		// Apply rotation 
		transform.localRotation = startRotation * Quaternion.Euler(0, 0, startAngle - angle);
	}
}
@endcode

<li> In the <i>Pointer.cs</i> script, add calling the <i>OnDrag</i> method of the current element to perform drag-and-drop. 

@code
if (press)
	selectedButton.OnDrag(eventData);
@endcode

<li> Run the project. The image in the preview mode can be dragged with one or two hands, scaled and rotated with two hands. 

@image html images/Ugallery_15.gif The owl is coming!
@image latex images/Ugallery_15.gif The owl is coming!

Congratulations, you've just created a gallery with images that you can control with gestures!
</ol>
*/
