---
title: 2D Game tutorial 
---

# Goal

In this tutorial, you will learn how to create a simple  2D Game with Godot that has a Player who can move, jump and pick up elements like coins.

# Previous Knowledge

We'll assume you already know the basics of a game like the platforms, the world, the player and etc. and also that you know some basics on how to make a player move with code.

# What you'll learn

We'll assume you already know the basics you can easily start making a world, whats gonna be new is the assets the scenes, the scripts. This tutorial is gonna show you how you can make a Game with a player and a world and you are going to be need assets for this that you can either get online for free or you can make them yourself. Later on your then gonna use this assets for the worldbuilding and the player. And to move this players gonna code a script. 

# Tutorial

Game

First, start by creating a new 2D scene. This scene will be where your whole game takes place. Name the scene Game (or any name you prefer).

Player

Next, create another new scene and name it Player.
Your player will need several abilities:

• moving
• jumping
• collecting items
• dying

Your player will also be animated. You will use character assets from the internet, and you will write scripts to control all the abilities.

Player animation

After choosing your character assets, add a new node called AnimatedBody2D (or AnimatedSprite2D, depending on your Godot version). This is where you create and manage the animations.

For this tutorial, make three animations:

• idle
• run
• jump

Set the idle animation to autoplay so the character plays it when nothing is happening. When the player starts moving or jumping, the script will switch to the correct animation.

Player movement

To make the player move, add a script to the player node. Godot already provides a basic movement template for 2D characters.

In the script, you control:

• movement speed
• jump force
• gravity, so the player falls back down

By default, the character moves using the arrow keys. You can change this to W, A, S, D in Project Settings → Input Map.

Collision

Now add a new node called CollisionShape2D to the player.

This gives your character a physical shape in the game world. Choose a shape that fits your player, such as a rectangle or capsule.

Collision allows your player to:

• stand on platforms
• hit walls
• interact with moving objects
• touch coins and collect them

Without a collision shape, the player would fall through the level and could not interact with anything.

After these steps, your player will be animated, movable, and able to interact with the world.

``` player movement
extends CharacterBody2D


const SPEED = 130.0
const JUMP_VELOCITY = -400.0


@onready var animated_sprite: AnimatedSprite2D = $AnimatedSprite2D


func _physics_process(delta: float) -> void:
	# Add the gravity.
	if not is_on_floor():
		velocity += get_gravity() * delta

	# Handle jump.
	if Input.is_action_just_pressed("jump") and is_on_floor():
		velocity.y = JUMP_VELOCITY

	# Get the input direction: -1, 0, 1
	var direction := Input.get_axis("move_left", "move_rigth")
	
	# Flip the Sprite
	if direction > 0:
		animated_sprite.flip_h = false
	elif direction < 0:
		animated_sprite.flip_h = true
		
	#  Play animations 
	if is_on_floor():
		if direction == 0:
			animated_sprite.play("idle")
		else:
			animated_sprite.play("run")
	else:
		animated_sprite.play("jump")
	
	# Apply movement
	if direction:
		velocity.x = direction * SPEED
	else:
		velocity.x = move_toward(velocity.x, 0, SPEED)

	move_and_slide()
 ```")
```

World and TileMap

If you start the game now, your character will fall off the map because there is no ground.

Go back to your Game scene. Add a TileMap node. Use the tile assets you imported earlier. Select the blocks from your tileset and draw the level however you like.

Now you have platforms. But if you press play, the character will still fall through the tiles. This happens because the tiles do not yet have collision.

To fix this, open the TileSet editor and add a physics layer to your tiles. This works similar to a CollisionShape2D. You define the collision shape for each block. Once this is set, your player will stand on the tiles instead of falling through them.

Falling off the map and dying

If the player walks off the platform, they will still fall forever. To fix this, you need a killzone.

Create a new scene and name it Killzone.

Add these nodes:

• Area2D as the main node
• CollisionShape2D as a child

Stretch the collision shape into a long horizontal line under your map. This will detect when the player falls below the level.

Place the Killzone scene inside your Game scene under the level.

Making the player die

Right now, even if the player touches the killzone, nothing happens. So you need to add a script.

Attach a script to the Killzone. Connect the body_entered signal from the Area2D. When the player enters the killzone, you start a Timer.

Add a Timer node as a child of the Killzone. Set it to one-shot mode.

The logic works like this:

• Player enters killzone
• body_entered signal triggers
• Timer starts
• When timer finishes, reload the current scene

Reloading the scene will restart the game. The player will spawn again at the original starting position.

This way:

• The player can stand on platforms
• The player falls if they walk off
• If they fall too far, they die
• The scene reloads automatically

Now your game has a working world, ground collision, and a death system.

``` killzone
extends Area2D



@onready var timer: Timer = $Timer



func _on_body_entered(body: Node2D) -> void:
	print("You died!")
	timer.start()
	


func _on_timer_timeout() -> void:
	get_tree().reload_current_scene()



```
# Result

# What could go wrong?
