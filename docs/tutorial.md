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

First you are going to start by creating a 2d scene which or game will be set in then you are going to give the scene the name game or however you prefer it.


Then were gonna start by making our player. You are going to make a new scene and call it player. So our player is going to be needing different abilities like moving jumping, collecting and dying. Also our player is going to be animated. Our player is going to be using assets for this that. In this tutorial you can get online and for the different abilities were going to have to write a script. 
So lets start by the players animation. When you choose your assets your gonna go and animate them so that lets say your so youre gonna add a  new node which is called 2d animated body thats were your going to add the animation so you can choose yourself what kind of animation you want but in this case we got jumping, running, and just being idle. Then you choose them and if you go on autoplay for the idle one youre charackter will always move that way unless you change it but lets say you want to move in any type of way for that youre gonna have to write your script. 

```
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

# Result

# What could go wrong?
