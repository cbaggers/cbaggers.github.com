---
layout: post
title: TaleSpire Dev Log 230
description:
date: 2020-09-28 09:41:09
category:
tags: ['Bouncyrock', 'TaleSpire']
---

With lights under control, I've turned my evil eye back to physics. Since I replaced the physics engine, gm-blocks and hide-volumes have not been working. This was expected, but it was time to fix them.

For sanity, I'm going to call Unity's old physics engine ClassicPhysics, their new one DotsPhysics, and our wrapper BouncePhysics.

Hide volumes make use of the fact that, in ClassicPhysics, we could resize colliders on the fly. We needed something similar in our new system. First up was a different problem, however. When we convert Unity's colliders to the form required by DotsPhysics, we remove the components. This means there isn't an obvious place for the programmer to set the values. This meant BouncePhysics needed its own version of these components.

I knocked together a Sphere, Cylinder, Capsule, and Box collider components. I modified the BouncePhysicsBody to walk the hierarchy, converting the ClassicPhysics components to their BouncePhysics equivalent, and registering all these new colliders.

During this, I noticed some issues in how my code handled baking of scale when the collider is subject to multiple transformations. This sucked for a good few hours, but I was able to find code that showed how to do it properly, and I got it fixed.

![custom component adjustment](/assets/videos/bounceColliders.gif)

With that done, I was able to hook up the hide volumes and get them working again \o/

Next, gm-blocks. There I found an interesting bug when deselecting them. First, the gm-block would be destroyed, and as that happened, it would unregister itself from BouncePhysics. The issue arose because, in DotsPhysics, you reset and pass all the RigidBodys every frame. This means that although I had unregistered the object from BouncePhysics, the RigidBody was still in the DotsPhysics collision world. I can't just remove that one body (the only option is Reset, which clears everything). Instead, I now set a flag inside the RigidBody to state that it is unregistered and then modified the collision queries to check for this flag when collecting results. This all takes place after the updates to motion have been calculated, so there is no risk of something bouncing off this unregistered body before the next frame, and by then, it will have been cleaned up.

Of course, this check (which is essentially a comparison of a ulong) is not free. I will probably have the system only use those new collectors on frames where something has been unregistered. Luckily in TaleSpire, we don't have much churn in the physics objects.

During this, it made sense to clean up some of the physics code in general. Until now, I had made the class that holds the board data responsible for the physics 'world'. This wasn't ideal, and the physics related code was very out of place. I decided to centralize it in the BouncePhysics classes. This does mean that, for now, we only allow only one board to be using physics at a time (which could matter when changing boards), but the trade-off was that I could make the API much more like ClassicPhysics. The API design is important because, as soon as I merge this branch, Ree needs to be able to get up to speed quickly. The more familiar it is, the better.

In amongst this, there was the usual smattering of bugs, confusion, and other things that slow one down. But overall, it's felt good to have some forward momentum.

Next, I really need to find out why dice in BouncePhysics are not behaving like they did in ClassicPhysics. It could be an issue in the conversion routines or, more likely, mistakes in the code than manages fixed-timestep and interpolation. I'll set up some test scenes where I can compare everything side-by-side, and we'll see what we can find out.

Alright, that's all from me for now.
Seeya later
