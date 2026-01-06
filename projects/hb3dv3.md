# hb3dv3

A Geometry Wars-inspired space shooter that got out of hand.

**24,000+ lines of Rust + WGPU**

![hb3dv3 gameplay](/assets/images/hb3dv3-1.png)

## What It Is

Started as "I want to make a twin-stick shooter." Ended up as a GPU-accelerated physics playground featuring:

- **N-body gravitational simulation** - Black holes, neutron stars, white holes with real orbital mechanics
- **Negative mass physics** - Anti-gravity projectiles that curve *away* from gravity wells
- **Frame-dragging effects** - Rotating black holes create ergospheres that twist trajectories
- **Fractal bullet patterns** - 11 patterns including golden ratio icosahedral geometry
- **524,288 GPU particles** - Compute shader particle system feeling gravitational pull
- **Real-time fractal skybox** - Mandelbrot, Julia, Burning Ship, Newton fractals rendered live
- **9 unique weapons** - From chain lightning to implosion launchers

![hb3dv3 combat](/assets/images/hb3dv3-2.png)

## Tech Stack

- **Rust** - Core engine
- **wgpu** - Modern GPU API (Vulkan/Metal/DX12)
- **GPU Compute Shaders** - Physics and collision detection
- **Custom ECS-like architecture** - Entity management with phased updates

## Why

I wanted to see what happens when you stop saying "that's probably too complex" and just build the thing.

## Links

- [GitHub Repository](https://github.com/sqrew/hb3dv3)

---

[← Back to projects](/projects) · [← Back home](/)
