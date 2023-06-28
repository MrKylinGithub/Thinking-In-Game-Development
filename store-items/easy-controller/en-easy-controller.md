# Easy Controller

## Engaging Hook(Mix Cut)

It's a powerful tool package designed specifically for Cocos Creator 3.8 or above, offering a range of features and functionalities to enhance your game development process.

A simple drag-and-drop operation will allow you to control camera and character actions smoothly on your phone and computer.

Alright, now that we know what EasyController has offered, let's explore how to use it in your projects. Here's a step-by-step guide to get you started quickly.

## Greeting (Facing Camera)

Hi Everyone, Welcome to my channel, Kylin is here.

Today, I have an exciting tool package to share with you, which will improve your productivity a lot.

It's called Easy Controller, and wrote by myself. You can get it for free on the Cocos Store.

In this video, we are going to learn how it works and demonstrate how to use it to speed up your game development.

So, without further ado, let's get started.

## How to get it

First up, We need to get it from the Cocos Store.

to do that, let's Open up the Cocos Dashboard.

once you are in the dashboard, switch to the **Store** tab.

Here we search for 'EasyController' in the search bar and  press Enter. Look, it's right here. Listed among the results.

Next, click on it to access the detail page.

you can see it's free, so click the 'get' button to have it.

Click the 'Create Project' button and give it a unique name.

Now, click the 'confirm', you can see it starts downloading.

Once the download is complete, you will get this alert.

click the `Open Project` button to open it directly in Cocos Creator.

you can also cancel here, and head over to the `Project` tab. you will find your newly created project listed here.

Just click on to open it.

## Project Structure

It's worth noting that, please ensure you have installed the corresponding Cocos Creator version. it's 3.8 here as you can see.

It will take a while to loading and compiling something when you first open a project.

Alright, it's ok now.

Here we can see, there are several folders under the assets.

Their names indicate their purposes. so there is no need for further explanation.

we just need to focus on the `kylins_easy_controller` folder, it includes all the content that can be reused in your project.

let's open the `rooster_jump` scene under the `scenes` folder.

here, we can see a plane, a rooster and some objects.

click the play button on the top to run.

now, you can control a rooster to move and jump smoothly in the scene.

[5 seconds show with music](music)

Next, let me show you how to use the easy-controller step by step, and finally, we will re-build all things in this demo scene.

## How to use

First, let's create a new scene and name it "test" to show the steps.

### Introduce to Joystick prefab

#### create

The most important thing in this tool package is the joystick, it provides all the abilities to control the camera and character.

To use the joystick, we just need to drag the ui_joystick_panel prefab from `assets` window to the `Hierarchy`. then you can find, a new node has been created under a Canvas. and its name is the same as the prefab, ui_joystick_panel.

Don't feel weird about the automatically generated Canvas node, it is used to render all 2d objects in Cocos Creator.

When attempting to create a 2d object and there is no existing Canvas node, a new Canvas node will be created by the Cocos Creator engine automatically.

Look at here, in the scene, we can see nothing. that because it's in the 3D mode.

Click the button at the left-top corner of the scene editor to switch the mode to 2D.

now we can see what the joystick panel looks like.

#### content

Expand the `ui_joystick_panel` node, we can find there are 4 sub-nodes here.

the `checker_camera`, `checker_movement`, `ctrl`, and `buttons`.

the `checker_camera` node is used to indicate the area of camera operations. when users drag on it using a mouse or a finger, the joystick will dispatch a 'camera_rotate' event, you can listen to this event to control your camera in your script.

you can enable its sprite component to see the area visually, here, it is in white.

It also supports multi-touch points to zoom the camera distance on mobile devices.

bye the way, you can use the mouse wheel to zoom the camera on desktops too.

the `checker_movement` node is used to indicate the area of the movement controller, when users tap on it, the joystick controller will appear.

you can also enable its sprite component to see the area, look, it is in red.

the `ctrl` node is the movement controller. when you tap on the `checker_movement` node, it appears, then the pointer will follow your finger or mouse position on the screen when you move them.

At the same time, it continuously dispatches `movement` events based on the relative position of the pointer and the center of the controller.

the `buttons` node is the root of buttons, you can see there are 5 buttons are predefined here, you can also add or remove depend on your needs. these buttons can be used to control the character's actions, such as jump, attack, skill etc.

In this demo, we just need to use one button to control the character's jump, so let's hide the others and only keep one visible.

### Script Component

OK, next, let's take a look at the script components of the joystick.

Select the `ui_joystick_panel` node, you can find the attached `UI_Joystick` component.

we are not going to delve deep into this script component today, just a simple introduction to make you get started easier.

here, we can see, it imported the `EasyControllerEvent` class from `EasyController`. let's jump into the `EasyController` file to see what it contains.

here, we can see, it defines two classes.

The `EasyControllerEvent` class is used to hold all events that should be dispatched by the joystick.

and the `EasyController` class is defined to effectively handle the listening and unlistening of events in code.

All the methods are defined in static, so that they can be conveniently called in any code file.

let's go back to the `UI_Joystick`, and search for 'EasyControllerEvent', you can see the code lines used to emit the events.

### Add a Character

OK,next, let me show you how to use this joystick panel to control you camera and character.

let's go back to the Cocos Creator.

For better understanding, let's construct a simple scene for testing.

switch to 3D mode.

create a plane, scale it to 20,1,20 and give it the ground material.

Then, find the `assets/rooster_man` folder, drag the `rooster_man_skin` into the `Hierarchy`, and double-click the generated node, and we can see a rooster with an umbrella in the scene.

### Use ThirdPersonCamera

now, we just need to use the ThirdPersonCamera script component to make your camera controllable.

select the Main Camera node, hit the "Add Component" button to add the 'ThirdPersonCamera' component to it.

Drag the `rooster_man` node on the `target` property.

Alright, click the play button to run.

Here we can see the camera is looking at the rooster in a proper distance.

Drag in the `checker_camera` area, you can see the camera is rotated.

we can also use the mouse wheel to zoom the camera.

If you are testing on mobile devices. you can use two fingers to press and move on the screen to zoom the camera.

### Use CharacterMovement

How about the movement controller?
Let's try it.

Ok, we can see, it is not working.

because we haven't listened and handled the movement events.

So, next, let me show you how to make the rooster move smoothly using the movement controller.

Let's go back to the Cocos Creator.

Look at here, this package offers a CharacterMovement script component to handle the character actions.

Now, let me demonstrate you how to use it.

Select the rooster_man node.

add CharacterMovement as its component.

let's take a look at the properties.

the `Main Camera` would refer to the main camera in the scene.

the `velocity` indicates the movement speed of the character.

the `jump velocity` indicates the start jump speed of the character

the `max-jump-times` indicates how many times the character can jump before landing.

if it set to 0, it represents the character can not jump.

let's set it to 1 to enable the jump ability.

the idle, move, jump begin, jump loop, and jump land animation clips are used to animate the character in each state.

OK, let's drag the corresponding animation clips onto the properties.

all have done, let's play it again.

you can see we can control the rooster to move and jump.

let's zoom the camera a bit closer to show you the detail about the movement controller, you can see the rooster moves at a speed according to the offset of the pointer.

we can see, with a small offset, it moves slowly. with a large offset, it moves fast.

## Use Physics System

the CharacterMovement component also supports the attached node interacting with Physics System in Cocos Creator.

let's add the RigidBody component to the rooster node, and add it a capsule collider. Adjust the properties of this collider to make it precisely surround the mesh. so it can perform accurate physics detection.

to make it works, we need to add a plane collider to the ground, to prevent the rooster from falling.

in the end, let's add some objects into the scene to make it interesting.

Alright, everything is done, let's start to run it again.

[play 10 seconds](xxx)

## Showcase and Conclusion

That's all for this video.

Until next time, happy game development.

I'm Kylin, see you!
