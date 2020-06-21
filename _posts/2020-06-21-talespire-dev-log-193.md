---
layout: post
title: TaleSpire Dev Log 193
description:
date: 2020-06-21 16:31:32
category:
tags: ['Bouncyrock', 'TaleSpire']
---

Hi folks.

I've been poking a little at the occlusion culling, so I thought I'd show something I found handy.

We are not moving to Unity's scriptable rendering pipeline yet. This means much of the work I'm doing right now is finding out how we should hook into Unity's traditional rendering stack.

I really like [BatchRendererGroup](https://docs.unity3d.com/ScriptReference/Rendering.BatchRendererGroup.html) as it lets us set up batches and not have to update them per frame[0]. We can then map the batch-id to a specific zone and kind of asset. This now means that any active camera (or one that is told to render) will render the things we have submitted in those batches.

We really want to use lower-poly meshes for the shadows and occlusion-culling, so what we should put in those batches is actually the low poly-meshes (which we are going to call occlusion-meshes from now on). However, we don't want to render the mesh itself to the final scene, we just want it to cast shadows in the main view and populate a depth buffer for us to use later.

Now those used to Unity might think of layers. You can tag objects with a specific layer and then tell a camera to only render particular layers. The issue with this is that if you filter out an object, you also can't see it's shadows, so that won't do. Next, we could see that when [adding batches to the BatchRendererGroup](https://docs.unity3d.com/ScriptReference/Rendering.BatchRendererGroup.AddBatch.html) we can set the `ShadowCastingMode` to `ShadowsOnly`. Awesome, now we only get shadows in the main view… however, we then don't get any depth information in the depth buffer. Damn. What ended up working was the following:

- Make a shader with *only* a `ShadowCaster` pass [1].
- Make a material using this, and use that in `AddBatch`.
- Render the scene from the same orientation using a camera with a replacement shader and with a depth RenderTexture as the target.

What's nice about this is that lights will now use our occlusion-meshes, but those meshes won't show up in the final render.

With the depth buffer available, we now generate the depth-chain, as mentioned in a previous dev-log. We then have everything we need to get into the meat bit of the occlusion culling. Once we know what to draw, we will use [DrawMeshInstancedIndirect](https://docs.unity3d.com/ScriptReference/Graphics.DrawMeshInstancedIndirect.html) to submit the high-poly meshes for rendering. At this point, we make sure to set `receiveShadows` to false and the `ShadowCastingMode` to `Off`. This lets us receive the shadows we computed earlier without asking Unity to compute more from these more detailed meshes.

Right now, all the tests I'm doing are just with randomly placed spheres, so screenshots are a little meaningless, however, once we get into the using real assets, I'll start showing more.

Next, I need to work out a little mistake I've created in the depth-chain, refactor the tests to be a little easier to handle, and then get into the culling compute shader. Once I have this first test working, I'll need to do a lot of planning to work out how we want to handle the data in the TaleSpire. The goal is to find a balance where we do as little per-frame work and as small per-frame GPU uploads as possible.

Until next time,
Peace.

[0] and handle culling ourselves which is handy too
[1] here is the shader we used in our recent tests

```
// Very minimal
Shader "OccluderShadowCaster"
{
    SubShader
    {
        Pass
        {
            Tags {"LightMode"="ShadowCaster"}

            CGPROGRAM
            #pragma vertex vert
            #pragma fragment frag
            #pragma multi_compile_shadowcaster
            #pragma multi_compile_instancing
            #include "UnityCG.cginc"

            struct appdata
            {
                UNITY_VERTEX_INPUT_INSTANCE_ID
                float4 vertex : POSITION;
                float3 normal : NORMAL;
            };

            struct v2f {
                V2F_SHADOW_CASTER;
            };

            v2f vert(appdata v)
            {
                v2f o;
                UNITY_SETUP_INSTANCE_ID(v);
                TRANSFER_SHADOW_CASTER_NORMALOFFSET(o)
                return o;
            }

            float4 frag(v2f i) : SV_Target
            {
                SHADOW_CASTER_FRAGMENT(i)
            }
            ENDCG
        }
    }
}
```
