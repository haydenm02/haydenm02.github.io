---
title: High Level Graphics
date: 2023-04-28 16:00:00
categories: [Education, University]
tags: [c++, graphics]
---

{% include embed/youtube.html id='4M7YCrOvSBU' %}

For this assignment, we were given the basic building blocks of a road network simulation. 
The goal was to create a functioning traffic-light system for the vehicles, avoiding collisions or any unexpected behaviours. 
This project was completed using c++ utilising the open-source 3D graphics API [OpenSceneGraph](https://openscenegraph.github.io/openscenegraph.io/).


The animations for the vehicles were handled using the AnimationPath object provided by OSG. Along with a helper class, this allowed me to specify points on each road tile to create a path for each car.

```cpp
raaAnimationPointFinders apfs;
osg::AnimationPath* ap = new osg::AnimationPath();

// Add points
apfs.push_back(raaAnimationPointFinder("tile10", 3, pRoadGroup));
// ...

ap = createAnimationPath(apfs, pRoadGroup);
raaCarFacarde* pCar = new raaCarFacarde(g_pRoot, 
	raaAssetLibrary::getNamedAsset("vehicle", "car3"), ap, speed);
g_pRoot->addChild(pCar->root());
```

One of the main challenges for this project was to have the cars avoid collisions and follow the road signals. The approach I took to this problem was a simple yet effective one.
First off, if the car approaches a red light, stop. As the lights were placed in predictable locations on the left side of the road, this can simply be done by measuring the distance between the light and the car. If it's below a certain amount, we know we have approached the junction.

```cpp
// Loop through all traffic lights
for (auto i = raaTrafficSystem::lights().begin(); i != raaTrafficSystem::lights().end(); i++) {
	TrafficLightFacarde* trafficLight = *i;
	
	// Traffic light and car points
	osg::Vec3 lPos = trafficLight->root()->getBound().center();
	osg::Vec3 cPos = this->getWorldDetectionPoint();
	
	// Get distance between
	float xDif = lPos[0] - cPos[0];
	float yDif = lPos[1] - cPos[1];
	float totalDist = sqrtf((xDif * xDif) + (yDif * yDif));
	
	// Close to a light
	if (totalDist < 100) {
		switch (trafficLight->m_iTrafficLightStatus)
		{
		// Red or amber- stop
		case 1:
		case 2:
			m_dSpeed = 0;
			break;
		default:
			m_dSpeed = normalSpeed;
			break;
		}
	}
}
```

On top of this, if the car approaches another vehicle, my solution is to simply match their speed. If they are travelling normally, the car follows along behind without crashing. If the car has stopped at a light, both vehicles stop until it is safe to continue.

```cpp
// Loop through all cars
for (auto i = raaTrafficSystem::colliders().begin(); i != raaTrafficSystem::colliders().end(); i++) {
	raaCollisionTarget* collider = *i;
	
	// Do not collide with self
	if (collider == this) continue;

	// Traffic light and car points
	osg::Vec3 oPos = collider->getWorldDetectionPoint();
	osg::Vec3 cPos = this->getWorldDetectionPoint();

	// Get distance between
	float xDif = oPos[0] - cPos[0];
	float yDif = oPos[1] - cPos[1];
	float totalDist = sqrtf((xDif * xDif) + (yDif * yDif));
	
	raaCarFacarde* car = (raaCarFacarde*)collider;

	// If we are catching up to a car ahead of us, set the same speed
	if (totalDist < 100) {
		m_dSpeed = car->m_dSpeed;
	}
	else {
		m_dSpeed = normalSpeed;
	}
}
```

These functionalities combine to create a reliable, fluid traffic system. This project has given me the opportunity to work with graphics libraries as well as further develop my skills with c++.