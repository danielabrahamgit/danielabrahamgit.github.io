---
layout: page
title: Collisions
permalink: /Projects/Collisions
---

#### Goal
The goal of this project was to accomplish two things:
1. Get comfortable doing graphics in C++
2. Simulate many circles colliding

Achieving these goals was a very fun process, especially the mathematics! Let us start with that

#### Theory
It turns out that it is really hard to predict how two moving balls will colide in the *wrong reference frame*. When analysing collisions, it is important to determine the *correct reference frame*. We define this to be the coordinate system such that a line drawn between the two colliding balls is orthogonal to the collision axis. Here is a diagram showing just that:
<p style="text-align:center;"><img src="../../images/collision.jpg" alt="Diagram" style="width:600px" class="center"></p> 
In short, using this new coordinate system turns a 2D collision into a 1D collision problem. From there, you can use the conservation of momentum and energy equations to solve for the final velocities of each ball.   

Applying the coordinate change will make our new x-axis be in the direction of the line between the centers (the new y-axis will just be orthogonal to the new x-axis). To do this coordinate change, it is helpful to just use a change of basis matrix and apply it on momentum vectors for each ball. Then, we can calculate the final velocities in the new coordinate frame, only to convert back to the old coordinate frame. That is, in short, how we can determine the final velocities of two colliding balls. 

#### Graphics and C++
Putting the theory into the code was a very fun process. There was the challenge of detecting the collisions, and then acting on them. Detecting a collision using a naive approach will be extremely inefficient for a large number of balls. There are techniques to overcome this, they are best highlighted in this video: <a href="https://www.youtube.com/watch?v=eED4bSkYCB8">Collision Detection Video</a>. I ended up using the **Sweep and Prune Algorithm**. This algorithm is simple yet extremely effective. The rendering time for these simulations were decreased dramatically thanks to this algorithm. The video linked above describes the algorithm very well.

I found on of the most time consuming parts of the entire project, ironically, was learning how to turn a pixel on and off using the C++ graphics library. This was my first time installing a library in C++, so I had a lot of learning to do. Here is another helpful video that helped me with the instalation and background information that I needed: <a href="https://www.youtube.com/watch?v=AUFZnA3lW_Q">C++ Graphics Library</a>   


#### Results
Here is an example where many small balls were arranged to make the word *Physics*. Then, 4 heavy balls distrubted the entire system, which is played in reverse after a few seconds.
<p style="text-align:center;"><img src="../../images/physics.png" alt="gravity" style="width:800px" class="center"></p> 

You can find the source code to this project on my github <a href="https://github.com/danielabrahamgit/collisionsplusplus">here</a>