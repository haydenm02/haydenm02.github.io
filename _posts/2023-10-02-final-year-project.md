---
title: Final Year Project
date: 2023-10-02 16:00:00
categories: [Education, University]
tags: [graphics, linux, windows, c++]
---

The aim of this project was to investigate cross-platform support in software development.


# Overview

One of my passions in this industry is the development of games. An issue I've noticed is prevelant especially in this sector is compatibility across systems. Most software is built for one operating system- Windows. Specifically, the latest build of Windows.
This raises a problem when your system is not on this version, or even Windows at all. The options are either switch operating systems, or don't use the software.


Tools such as Wine (and Proton on top of this) partially solve this problem. They allow most software and games to run on Unix-based operating systems. The compatibility is pretty good and it's getting better every day, however it is not perfect and it is still another layer on top of running natively. 
If games coupld be developed with cross-platform support in mind, it would allow for (mostly) quick and easy builds for any required system.

This is where my project comes in. If I could develop a simple game and compile it for both Windows and Linux, I could show the potential for software development with a focus on cross-platform support from the ground up.

# Development

The first goal was creating an OpenGL scene. 

In order to create this, I utilised GLFW and Glad (https://glad.dav1d.de/) to set up an environment which would compile seamlessly to other platforms.
From this, I had a simple scene with a basic shape:

![Triangle](images/2023-10-02-final-year-project/triangle.png)

After this, I encapsulated my object data in to a GameObject class, storing position and rotation vectors as well as the revelant IDs for the object's vertex buffers.

I then implemented an MVP (model, view, projection) matrix in order to have the capacity to draw and manipulate objects in a 3D space:

![Cubes](images/2023-10-02-final-year-project/cubes.png)

This has all been rendering logic with no extra libraries or tools used, so it would not have an impact on cross-platform support.


My next goal would be to load object files and textures from the file system. This would be a little trickier to manage as I would have to ensure the implementation would work across various systems.
I decided upon utilising C functions in order to open and read data from files. This would mean I would not be relying on any external libraries which may be inacessable elsewhere:

```c++
bool readOBJ(
	const char* filePath,
	std::vector<glm::vec3> &outVertices,
	std::vector<glm::vec2> &outUV,
	std::vector<glm::vec3> &outNormals
	) {

	std::cout << "(OBJ) Reading " << filePath << "\n";
	
	/*
	*  --- OBJ file format ---
	*  # comment
	*  v vertex (X, Y, Z)
	*  vt vertex texture coordinate (U, V)
	*  vn normal of vertex (X, Y, Z)
	*  f face (A/B/C A/B/C A/B/C), A=Vertex, B=TextureCoord C=Normal
	*  usemtl mtlib (not important at the moment){
	*/
	std::vector<glm::vec3> tmpVert, tmpNorm;
	std::vector<glm::vec2> tmpUV;
	std::vector<unsigned int> indVert, indUV, indNorm;

	// Open the file
	FILE* file = fopen(filePath, "r");
	if (file == NULL) {
		std::cout << "Could not read OBJ! " << filePath << "\n";
		return false;
	}

	while (1) {
		char lineHeader[128]; // 128 byte header
		// %s to get a word
		int res = fscanf(file, "%s", lineHeader);
		if (res == EOF) break; // If end of file, break

		// Process the different parameters
		if (strcmp(lineHeader, "v") == 0) {
			// Vertices
			glm::vec3 vertex;
			// Read the vertex points, add them to the temp list
			int result = fscanf(file, "%f %f %f\n", &vertex.x, &vertex.y, &vertex.z);
			if (result == 0) std::cout << "fscanf 0 on vertex\n";
			tmpVert.push_back(vertex);
		}
		else if (strcmp(lineHeader, "vt") == 0) {
			// Vertex texture coord
			glm::vec2 uv;
			int result = fscanf(file, "%f %f\n", &uv.x, &uv.y);
			if (result == 0) std::cout << "fscanf 0 on UV\n";
			tmpUV.push_back(uv);
		}
		else if (strcmp(lineHeader, "vn") == 0) {
			// Normals
			glm::vec3 normal;
			int result = fscanf(file, "%f %f %f\n", &normal.x, &normal.y, &normal.z);
			if (result == 0) std::cout << "fscanf 0 on normal\n";
			tmpNorm.push_back(normal);
		}
		else if (strcmp(lineHeader, "f") == 0) {
			unsigned int vInd[3], uvInd[3], normInd[3];
			int matches = fscanf(file, "%d/%d/%d %d/%d/%d %d/%d/%d\n", &vInd[0], &uvInd[0], &normInd[0], &vInd[1], &uvInd[1], &normInd[1], &vInd[2], &uvInd[2], &normInd[2]);
			if (matches != 9) {
				// Simple obj file reading only, possibly an area to expand
				std::cout << "Too many faces!\n";
				return false;
			}
			indVert.push_back(vInd[0]);
			indVert.push_back(vInd[1]);
			indVert.push_back(vInd[2]);
			indUV.push_back(uvInd[0]);
			indUV.push_back(uvInd[1]);
			indUV.push_back(uvInd[2]);
			indNorm.push_back(normInd[0]);
			indNorm.push_back(normInd[1]);
			indNorm.push_back(normInd[2]);
		}
	}

	fclose(file);

	std::cout << "Finished reading data!\n";
	// We have finished reading the data, start procesing it:
	for (unsigned int i = 0; i < indVert.size(); i++) {
		// OBJ indexing starts at 1, c++ at 0. We need to -1 to get the referenced index
		// Face: VERTEX/UV/NORMAL V/U/N V/U/N
		
		// Vertex
		glm::vec3 vertex = tmpVert[indVert[i] - 1];
		outVertices.push_back(vertex);

		// UV
		glm::vec2 uv = tmpUV[indUV[i] - 1];
		outUV.push_back(uv);
		
		// Normals
		glm::vec3 norm = tmpNorm[indNorm[i] - 1];
		outNormals.push_back(norm);
	}

	// We successfully read the data
	return true;
}
```

From this, I was able to implement custom objects with textures in to my scene:

![Textured OBJ](images/2023-10-02-final-year-project/textured.png)

After implementing some game logic and input handling (done through GLFW), I successfully had a simple game which would compile for multiple systems:

![Game](images/2023-10-02-final-year-project/game.png)

This project shows that with care for the implementation of functions and development of cross-platform libraries, software and games have the potential of fast and easy distribution across many systems.