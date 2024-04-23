---
title: 3D Games Development
date: 2023-01-09 16:00:00
categories: [Education, University]
tags: [c#, unity, games]
---
![Main Menu](images/2023-01-09-3d-games-development/main.jpg)

For this assignment, I was tasked with creating a 3D game in Unity, displaying competency with C# as well as various aspects of game development. This project was undertaken utilising the Unity engine.


My idea was simple, pick up a weapon, search the area and destroy all the bottles. A walkthrough of the key game sequences can be seen below:
![Main menu](images/2023-01-09-3d-games-development/door.jpg)
![Weapon pickup](images/2023-01-09-3d-games-development/pickup.jpg)
![Bottle next to a lantern](images/2023-01-09-3d-games-development/lighting.jpg)
![Bottle breaking](images/2023-01-09-3d-games-development/particles.jpg)
![Shooting the bell](images/2023-01-09-3d-games-development/bell.jpg)
![Rubber duckies](images/2023-01-09-3d-games-development/duck.jpg)

The main challenges I wished to undertake were: switching between items; creating
satisfying bottle-breaking action and efficiently tracking objectives.


# Items

My approach to the first challenge was to create a numbered hotbar-like system and an Item class. This would supply a game object and extra details such as the hotbar icon, allowing for a visual representation of the user's inventory.

```c#

// The usable item in the player's inventory
public class UsableItem : MonoBehaviour
{
    public Vector3 offset = new Vector3(0, 0, 0); // Offset from the player's center
    public Texture image; // Hotbar image

    public virtual void Use(Vector3 pos, Vector3 dir)
    {
        Debug.Log("No item functionality!");
    }
}
```
{: file="UsableItem.cs"}


This was then extended to provide the required functionality of the revolver:


```c#
public class Weapon : UsableItem
{
	// Sound and visual effects
    ParticleSystem gunParticles;
    public AudioClip sound;
	
	// Which objects we should be able to hit
	public LayerMask layerMask;
	
    private void Start()
    {
		// Tell the game we found the revolver to progress the objective
        GameManager.instance.RevolverFound(); 
    }
	
	// Override the use function so it can be used like any other item
    public override void Use(Vector3 pos, Vector3 dir)
    {
        audioSource.PlayOneShot(sound, 0.6f);
        gunParticles.time = 0;
        gunParticles.Play();

        RaycastHit hit;
        if (Physics.Raycast(pos, dir, out hit, 100.0f, layerMask))
        {
            Debug.Log("Hit something!");
            Shootable target = hit.collider.GetComponent<Shootable>();
            if (target != null) {
                Debug.Log("Hit something shootable!");
                target.Hit();
            }
        }
    }
}
```
{: file="Weapon.cs"}

# Bottles & Gameplay

In order to make bottles satisfying to break, I paired a delicate mix of particles with a royalty-free glass breaking sound effect, which I hand-picked to be believable for the use case. Thankfully, the model pack I chose provided separate halves of the bottle as models, saving me some time. 
I simply paired these alongside browny-orange spheres in a particle system to create a clean, visually interesting animation.

![Bottle breaking](images/2023-01-09-3d-games-development/bottle.jpg)
 
On top of the bottles themselves, I made sure to include sound effects for both the items, the movement and the bell. Including a music track alongside these, the game began to feel more fun and engaging. 

See below for a quick demo:

{% include embed/youtube.html id='iPg9R3kB7UY' %}

# Objectives

For a game to progress from start to finish, there has to be a way to track objectives. I settled upon a simple, flexible solution involving a static GameManager class. This would keep track of the objective stages sequentially as well as updating the UI when appropriate.

By utilising a static class for managing the game objectives, I could simply have the game objects call a function on the class whenever their objective has been satisfied. For example, when the player picks up the weapon in the church, the following line of code is executed:
```c#
GameManager.instance.RevolverFound();
```
{: file="Weapon.cs"}

This tells the game to activate the next objective on which the player can now progress. In order to give myself more freedom and make the system smoother, I decided upon tracking the bottles on runtime rather than hard-coding the value. At the beginning of the game, the following code would run, counting the instances of the bottles and setting the objective number:
```c#
numBottles = FindObjectsOfType<BottleShootable>().Length;
```
{: file="GameManager.cs"}

The bottles would then trigger this number to be decremented upon destruction:
```c#
GameManager.instance.BottleShot();
```
{: file="BottleShootable.cs"}

From this, the game plays smoothly without issues moving from one objective to the next.