---
generator: 'Doxygen 1.8.6'
title: 'Nuitrack: Creating your First Unity Project using Nuitrack SDK'
---

::: {#top}
::: {#titlearea}
+-----------------------------------+-----------------------------------+
| ![Logo](nuitrack.png)             | ::: {#projectname}                |
|                                   | Nuitrack  [1.3.5]{#projectnumber} |
|                                   | :::                               |
|                                   |                                   |
|                                   | ::: {#projectbrief}               |
|                                   | 3D Skeleton Tracking Middleware   |
|                                   | :::                               |
+-----------------------------------+-----------------------------------+
:::

::: {#navrow1 .tabs}
-   [Main Page](index.html)
-   [Related Pages](pages.html)
-   [Modules](modules.html)
-   [Classes](annotated.html)
-   ::: {#MSearchBox .MSearchBoxInactive}
    [ ![](search/mag_sel.png){#MSearchSelect} ]{.left}[
    [![](search/close.png){#MSearchCloseImg}](javascript:searchBox.CloseResultsWindow()){#MSearchClose}
    ]{.right}
    :::
:::
:::

::: {#side-nav .ui-resizable .side-nav-resizable}
::: {#nav-tree}
::: {#nav-tree-contents}
::: {#nav-sync .sync}
:::
:::
:::

::: {#splitbar .ui-resizable-handle style="-moz-user-select:none;"}
:::
:::

::: {#doc-content}
::: {#MSearchSelectWindow onmouseover="return searchBox.OnSearchSelectShow()" onmouseout="return searchBox.OnSearchSelectHide()" onkeydown="return searchBox.OnSearchSelectKey(event)"}
[[ ]{.SelectionMark}All](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Classes](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Namespaces](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Functions](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Variables](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Typedefs](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Enumerations](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Enumerator](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Properties](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Events](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Groups](javascript:void(0)){.SelectItem}[[ ]{.SelectionMark}Pages](javascript:void(0)){.SelectItem}
:::

::: {#MSearchResultsWindow}
:::

::: {.header}
::: {.headertitle}
::: {.title}
Creating your First Unity Project using Nuitrack SDK
:::
:::
:::

::: {.contents}
::: {.toc}
### Table of Contents

-   [Setting up the Environment](#environment)
-   [Setting up the Build](#build)
-   [Initializing the Nuitrack SDK, Setting up the Scene and Checking
    the Skeleton Detection](#initialize)
-   [Creating Objects for Skeleton
    Visualization](#skeleton_vizualization)
-   [Getting Data, Converting and Scaling the
    Joints](#getting_data_scaling)
:::

::: {.textblock}
In this very first tutorial, you'll learn how to create a basic Unity
project using Nuitrack SDK. As a result, you'll get a simple app with an
option of skeleton detection and tracking, which can be modified into a
full-fledged VR app.

You can find the finished project in **Nuitrack SDK**: **Unity 3D →
NuitrackSDK.unitypackage → Tutorials → First Project**

::: {.image}
![](Ubasic_image10.gif)
:::

[]{#environment .anchor} Setting up the Environment
===================================================

1.  As a first step, download Unity [from the official web
    site](https://unity3d.com/get-unity/download).

    Note
    :   To ensure stable operation of a project, we recommend you to use
        Unity 5.5.0f3.

2.  Create a new project.

    ::: {.image}
    ![](Ubasic_image1.png)
    :::

3.  To create a project, select the appropriate platform, for example,
    Android: **File → Build Settings → Android** and then select
    **Switch Platform**.

    Note
    :   In this tutorial we create an Android project, however, you can
        create a project using Nuitrack SDK for any platform that you
        like.

4.  Download and install **SDK** and **JDK**. To do this, select **Edit
    → Preferences → External Tools** and click **Download** in the
    relevant sections. After that, your browser will open the web sites,
    where you can download the required **SDK** and **JDK**.

    ::: {.image}
    ![](Ubasic_image3.png)
    :::

5.  And now, here is the most interesting part that will allow us to
    create projects with skeleton detection and tracking. Download
    **Nuitrack SDK** [from the official web site](https://nuitrack.com/)
    and import it to the project. To import **Nuitrack SDK**, select
    **Import Package → Custom Package →
    {Root}/NuitrackSDK/Unity3d/Nuitrack.unitypackage** by right-clicking
    in the **Project** tab.

    Note
    :   **What sensors can I use with my app?**\
        You can use any of the supported sensors for your application
        (see the list of supported sensors at the [Nuitrack official
        website](https://nuitrack.com/)). To learn more about **VicoVR**
        sensor, visit [the official VicoVR web
        site](https://vicovr.com/). If you\'d like to create apps with
        VicoVR sensor, you need to [download VicoVR
        app](https://play.google.com/store/apps/details?id=com.vicovr.manager).
        To get some inspiration for creating your own VR apps, you can
        check the list of our [VR apps made with
        VicoVR](https://vicovr.com/apps/vr).

6.  As a result, you will have a configured development environment with
    all the necessary components for the build.

    ::: {.image}
    ![](Ubasic_image2.png)
    :::

[]{#build .anchor} Setting up the Build
=======================================

1.  Now that we have configured the environment, we need to determine
    some important features of the project build.\
    First of all, save the empty scene: **File → Save Scenes**
2.  Add the scene to the project: **File → Build Settings → Add Open
    Scenes** or select **{Saved Scene}** in the **Project** tab and
    drag-and-drop it to the **Scenes in Build** field of the **Build
    Settings** window.
3.  Do not forget to put you company name and product name in the
    relevant fields: **File → Build Settings → Player Settings → Company
    Name, Product Name**.
4.  In the **Identification** section, select a name for the build
    (**Bundle Identifier**) as follows:
    **com.(CompanyName).(ProductName)**. Keep in mind that you won\'t be
    able to build the project with the default name.
5.  Also, select minimum API level in this section for correct operation
    of Nuitrack: **Minimum API Level → Android 4.4 'Kit Kat' (API
    Level 19)**.

    ::: {.image}
    ![](Ubasic_image5.png)
    :::

    Note
    :   To reduce the size of a final .apk file, select **ARMv7** type
        of architecture in the **Device Filter** section.

6.  Now that everything is set up, let\'s build the project: **File →
    Build Settings → Build**

    Note
    :   Hotkeys:\
        **Ctrl + Shift + B** - Build Settings\
        **Ctrl + B** - Build\

7.  If the build was successful, a new window will open with your
    application in the .apk format, which can be run on the Android
    device. When you run the app on your device, you will see the image
    as shown below:

    ::: {.image}
    ![](Ubasic_image4.png)
    :::

8.  During the project build, all errors and warnings (if any) are
    displayed in the **Console**.

    Note
    :   After the first build, when you have a ready-made .apk file, you
        can perform build and installation on the device in one click
        using **Ctrl + B**. To do this, activate **Developer Mode** in
        the settings of the mobile device and connect it to the computer
        via USB. Then press **Ctrl + B** in Unity. As a result, the
        project will be built, installed and launched on your mobile
        device.

[]{#initialize .anchor} Initializing the Nuitrack SDK, Setting up the Scene and Checking the Skeleton Detection
===============================================================================================================

1.  Now that your environment is set up, you're set to start doing
    work.\
    In the **Project** tab, select **Asset → Nuitrack → Prefabs** and
    drag-and-drop the **NuitrackScripts.prefab** object to the scene or
    to the **Hierarchy** tab. There must be only one object in the
    scene. This object contains the scripts that interact with Nuitrack.
2.  Set up the characteristics of the **NuitrackScripts** object by
    right-clicking on it. Check **Skeleton Tracker Module On** in the
    **Nuitrack Manager** section in the **Inspector** tab. This very
    module takes care of detection and tracking of a user\'s skeleton.\

    ::: {.image}
    ![](Ubasic_image7.png)
    :::

    As you can see, there are several more modules in this field. We\'ll
    cover them in the following tutorials.

3.  Create an empty C\# Script: **Create C\# Script** by right-clicking
    in the Project tab.
4.  To get information about joints, we first need to detect the user.
    First, let\'s check whether the user is in the frame or not using
    the *CurrentUserTracker.CurrentUser! = 0* condition. The result will
    be displayed as a \'User found\' or \'User not found\' message,
    which is stored in the *message* variable and displayed by the
    *OnGUI()* method. See the link at the end of the document to learn
    more about Execution Order of Event Functions.

    ::: {.fragment}
    ::: {.line}
    [using]{.keyword} UnityEngine;
    :::

    ::: {.line}
    :::

    ::: {.line}
    [public]{.keyword} [class ]{.keyword}NativeAvatar : MonoBehaviour
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    [string]{.keywordtype} message = [\"\"]{.stringliteral};
    :::

    ::: {.line}
    :::

    ::: {.line}
    [void]{.keywordtype} Update()
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    [if]{.keywordflow} (CurrentUserTracker.CurrentUser != 0)
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    message = [\"User found\"]{.stringliteral};
    :::

    ::: {.line}
    }
    :::

    ::: {.line}
    [else]{.keywordflow}
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    message = [\"User not found\"]{.stringliteral};
    :::

    ::: {.line}
    }
    :::

    ::: {.line}
    }
    :::

    ::: {.line}
    :::

    ::: {.line}
    [// Display the message on the screen]{.comment}
    :::

    ::: {.line}
    [void]{.keywordtype} OnGUI()
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    GUI.color = Color.red;
    :::

    ::: {.line}
    GUI.skin.label.fontSize = 50;
    :::

    ::: {.line}
    GUILayout.Label(message);
    :::

    ::: {.line}
    }
    :::

    ::: {.line}
    }
    :::
    :::

    Note
    :   Save changes in the script code so that they take effect in the
        Unity editor.

5.  Create an empty object on the scene so that Unity can execute
    **NativeAvatar.cs**: **GameObject → Create Empty** and then
    drag-and-drop the script to that object.
6.  Build the project (Ctrl + B) and check the app operation on your
    device.
7.  If everything is done correctly, at this stage the user will be
    detected. Depending on the result, you will see a message \'User
    found\' or \'User not found\' in the scene in the upper right
    corner. Time to move onto more complex things!

    ::: {.image}
    ![](Ubasic_image6.png)
    :::

    Note
    :   **How can I get the full log from the device?**\
        To get the Unity app log from your mobile device, install the
        Android Debug Bridge (adb) into your computer. Run the following
        command in the console: adb logcat -s Unity. When you run the
        application on your phone, the log will be displayed in the
        console.

[]{#skeleton_vizualization .anchor} Creating Objects for Skeleton Visualization
===============================================================================

1.  Now that we initialized Nuitrack and checked the detection of the
    user, we can go to the next stage, which presupposes obtaining data
    on the skeleton and visualization of the joints.
2.  Let\'s determine the joints of the skeleton, which we need to get:
    declare the
    *[nuitrack.JointType](group__SkeletonTracker__group__csharp.html#ga659db18c8af0cb3d660930d7116709ae "Joint index meaning (please note that LeftFingertip, RightFingertip, LeftFoot, RightFoot are not used..."){.el}*
    and *GameObject* arrays. Each element of the *GameObject* array will
    correspond to a specific *typeJoint*. In the app, the joint will be
    rendered in the form of the *PrefabJoint* that we selected.

    ::: {.fragment}
    ::: {.line}
    [public]{.keyword} nuitrack.JointType\[\] typeJoint;
    :::

    ::: {.line}
    GameObject\[\] CreatedJoint;
    :::

    ::: {.line}
    [public]{.keyword} GameObject PrefabJoint;
    :::
    :::

3.  In Unity, select the required number of joints in the object
    characteristics: **Native Avatar → Type Joint** and specify the
    joint types that you need. Drag-and-drop the prefab that will be
    used for displaying the user\'s joints (for example, a white sphere)
    to the **Prefab Joint**.

    ::: {.image}
    ![](Ubasic_image9.png)
    :::

    Note
    :   To create a prefab in Unity (for example, a sphere), select
        **GameObject → 3D Object → {Sphere}**. Scale down the sphere
        that appeared on the scene so that its size is about 10 cm (1
        Unity unit \~ 1 m). Create a prefab in the **Project** tab:
        **Right Click → Create → Prefab**. Then, drag-and-drop the
        created sphere to the prefab. To learn more about prefabs, see
        the link at the end of this tutorial.

4.  Let\'s create objects for visualization of joints using the saved
    prefab: create an array of the same dimension as the *typeJoint*
    array: *CreatedJoint = new GameObject\[typeJoint.Length\]*. After
    that, we start to create their copies on the scene in the loop using
    the Instantiate function. Now, we define copies of the objects as
    children of the object that contains the script. As a result, the
    position of the parent object will correspond to the position of the
    sensor.

    ::: {.fragment}
    ::: {.line}
    [void]{.keywordtype} Start()
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    CreatedJoint = [new]{.keyword} GameObject\[typeJoint.Length\];
    :::

    ::: {.line}
    :::

    ::: {.line}
    [for]{.keywordflow} ([int]{.keywordtype} q = 0; q \<
    typeJoint.Length; q++)
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    CreatedJoint\[q\] = Instantiate(PrefabJoint);
    :::

    ::: {.line}
    CreatedJoint\[q\].transform.SetParent(transform);
    :::

    ::: {.line}
    }
    :::

    ::: {.line}
    :::

    ::: {.line}
    message = [\"Skeleton created\"]{.stringliteral};
    :::

    ::: {.line}
    }
    :::
    :::

5.  Build the project (Ctrl + B) and check the app operation on your
    device. If everything is done correctly, the spheres from the prefab
    will be located in the position of the object with the script. Since
    they are yet located at one point, they will look like a single
    sphere.

    ::: {.image}
    ![](Ubasic_image8.png)
    :::

[]{#getting_data_scaling .anchor} Getting Data, Converting and Scaling the Joints
=================================================================================

1.  As soon as we created the objects for visualization of the joints,
    let\'s match their positions with the positions of the joints of a
    real user\'s skeleton.
2.  Get data on the detected skeleton using
    *CurrentUserTracker.CurrentSkeleton*. After this, we process the
    information about the joints in the loop.
3.  To receive data on the required joint, call the *GetJoint* function
    from the obtained *skeleton* specifying the *typeJoint\[q\]*.
4.  Let\'s calculate the joint position by calling the *ToVector3()*
    function from the current joint: *Vector3 newPosition = 0.001f \*
    joint.ToVector3()*. To learn more about Vectors in Unity, see the
    link at the end of this tutorial.

    Note
    :   Keep in mind that 1 Unity unit is about 1 m, so we need to
        adjust the obtained data. To do that, multiply the received
        values by 0.001 (convert m to mm).

5.  Set the calculated position *newPosition* to the *CreatedJoint
    \[q\]* object, which is matched with the *typeJoint \[q\]:
    CreatedJoint\[q\].transform.localPosition = newPosition*.

    ::: {.fragment}
    ::: {.line}
    [void]{.keywordtype} Update()
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    [if]{.keywordflow} (CurrentUserTracker.CurrentUser != 0)
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    nuitrack.Skeleton skeleton = CurrentUserTracker.CurrentSkeleton;
    :::

    ::: {.line}
    :::

    ::: {.line}
    message = [\"Skeleton found\"]{.stringliteral};
    :::

    ::: {.line}
    :::

    ::: {.line}
    [for]{.keywordflow} ([int]{.keywordtype} q = 0; q \<
    typeJoint.Length; q++)
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    nuitrack.Joint joint = skeleton.GetJoint(typeJoint\[q\]);
    :::

    ::: {.line}
    Vector3 newPosition = 0.001f \* joint.ToVector3();
    :::

    ::: {.line}
    CreatedJoint\[q\].transform.localPosition = newPosition;
    :::

    ::: {.line}
    }
    :::

    ::: {.line}
    }
    :::

    ::: {.line}
    [else]{.keywordflow}
    :::

    ::: {.line}
    {
    :::

    ::: {.line}
    message = [\"Skeleton not found\"]{.stringliteral};
    :::

    ::: {.line}
    }
    :::

    ::: {.line}
    }
    :::
    :::

    Note
    :   For correct detection of the user, the camera in Unity should be
        set at a distance of approximately 2-3 m from the object, which
        contains the script. Make sure the camera is facing the object.

6.  And now we are down to the final stretch! Build the project and run
    the app on your mobile device. Now, if everything was done properly,
    your skeleton will be detected, tracked and displayed in the app.
    Good job!

    ::: {.image}
    ![](Ubasic_image10.gif)
    :::

Now you know how to create a Unity project using Nuitrack SDK. From now
on, you can develop a plenty of apps with skeleton tracking! That's all
for now. Though this is a very simple example, it covers a large set of
basic subjects important to understanding how to use Unity with Nuitrack
SDK. In the next tutorials, you\'ll learn how create more complex and
functional Unity apps with skeleton tracking.

**Useful links:**

-   [Learn more about
    Prefabs](https://docs.unity3d.com/560/Documentation/Manual/Prefabs.html)
-   [Learn more about Vector3
    struct](https://docs.unity3d.com/ScriptReference/Vector3.html)
-   [Information on Execution Order of Event
    Functions](https://docs.unity3d.com/Manual/ExecutionOrder.html)
:::
:::
:::

::: {#nav-path .navpath}
-   [Unity Tutorials](UnityTutorials_page.html){.el}
-   Generated on Thu Oct 11 2018 12:24:39 for Nuitrack by
    [![doxygen](doxygen.png){.footer}](http://www.doxygen.org/index.html)
    1.8.6
:::
