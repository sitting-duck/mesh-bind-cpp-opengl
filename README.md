### mesh-bind-cpp-opengl

# Introduction
I wrote this for a Computer Animation course at University of Texas at Dallas in 2013. This project is pretty academeic in the sense that we reinvent the wheel and "roll our own" for many of the components involved in 3D animation. Modern SWE probably would use more pre-built components rather than writing one's own matrix, camera, 3D transformation classes and so on than what is done here. </br>

It's written in C++ and uses OpenGL to create and render scenes, freeglut and glui are used for frontend components. These we just use, we don't reinvent the wheel for the GUI components. The purpose of the project I believe was basically to break down and really understand the components of a 3D engine more or less. 

# The Program Output
Here's what the actual program itself looks like when it runs:

![image](https://user-images.githubusercontent.com/1289702/174928400-69a6ea47-b5b2-4616-b61f-0a1346cca11a.png)

It basically loads a skeleton file, which is just a text file containing a list of joints in 3D space and their connections, and a mesh file, which is just a text file containing a long list of vertices and their weights to each joint they are connected to. 
You can see that this mesh is basically a wasp. It's got some controls for rotating the mesh, moving the camera around etc, but we'll get to that later. 
At the center of all this there is the Scene

![image](https://user-images.githubusercontent.com/1289702/174928428-43fc37d5-f51a-4007-ac0c-737455e64117.png)

The Scene class is simple enough. It contains all the logic for parsing the skeleton and the mesh files and drawing them. 
There are only 4 state items in this class, a Model, a Camera, a ModelFactory, and a number for keeping track of the current joint. The user can iterate through the joints using this counter and rotate them.

![image](https://user-images.githubusercontent.com/1289702/174928471-f5541a08-36fa-447c-b7f4-dfa1fd819745.png)

# The Model and its Components
Let's take a quick look at the Model class.

![image](https://user-images.githubusercontent.com/1289702/174928524-d817b29e-5b1c-40d0-af1b-dcdc6c2e4892.png)

Models contain a Skeleton, the set of joints, and a Mesh, the skin looking bit, the mesh of all the vertices. You can see we keep ModelFactory as a friend class,

![image](https://user-images.githubusercontent.com/1289702/174928542-c6be5d76-3a33-4f0b-b9da-e664e08998c0.png)

ModelFactory is also quite simple, it creates a Model from skeleton file and a mesh file. You need both a skeleton and a mesh to build a model. 
Mesh has it's own MeshFactory class

![image](https://user-images.githubusercontent.com/1289702/174928572-f7c370f9-f02b-4d23-89af-89c74167e0b7.png)

![image](https://user-images.githubusercontent.com/1289702/174928590-a3880cdb-850b-4eb0-bc94-1ea8ec2439fb.png)

Skeleton it's own SkeletonFactory class and so on. <br>
Skeletons are composed of BallJoints <br>

![image](https://user-images.githubusercontent.com/1289702/174928615-3f527021-9ca7-4bec-9765-7561768da937.png)

BallJoints have a few mathematical helper classes, Matrix3, Vector3, Vertex, and Triangle. Triangles are also heavily featured in the Mesh class. <br>

# The Parsers
The SkeletonFactory uses SkelParser <br>
![image](https://user-images.githubusercontent.com/1289702/174928658-68df7088-9b7f-4c79-9c2f-36cf1f72d74c.png)

As well, the MeshFactory uses the MeshParser <br>
![image](https://user-images.githubusercontent.com/1289702/174928677-5526189c-28b3-4f64-91aa-ec16aeb8dc30.png)

All Parser classes use a convenience class called Tokenizer that tokenizes text files. ie. It takes a file, opens it, extracts the text, and returns the text as a list of useful "tokens". Tokens are usually separated by a "delimiter", often a space, or a "comma-space" as in the case of "comma separated values" files or .csv files. <br>

![image](https://user-images.githubusercontent.com/1289702/174928700-607b08fe-c6e5-47fc-bf7a-248cdf963cdc.png)

# Class Diagram
![image](https://github.com/sitting-duck/mesh-bind-cpp-opengl/blob/main/docs/bind-mesh-cpp-class-diagram.png)

# Related Tutorials I wrote: 
A solution for the build error ``Error 1 error MSB8020`` should you run into it: <br>
https://ashley-tharp.medium.com/error-1-error-msb8020-the-build-tools-for-v142-platform-toolset-v142-cannot-be-found-6ca7939ff442?sk=efa4d9add48fb279935f4767745afd3f

Tutorial for the tool I used to create the UML Diagrams <br>
https://ashley-tharp.medium.com/my-first-impression-using-software-ideas-modeler-uml-modeling-tool-for-c-35a6a720c25c?sk=6dc7d433a192a6b0433b143a0fb2ecbf



