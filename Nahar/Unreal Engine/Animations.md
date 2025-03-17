# Animations
#gamedev


### Creating an Animation Blueprint

- Purpose: Manage logic for animations (e.g., state machines, blending).
- **Steps**:
    1. Right-click in the Content Drawer.
    2. Select **Animation > Animation Blueprint**.
    3. Choose the appropriate **Skeleton** for the animation.
    4. Open the new Animation Blueprint and configure it.

#### Key Components:

- **Event Graph**: Handles logic (e.g., variables, function calls).
- **Anim Graph**: Controls animation blending and state transitions.
- **State Machine**: Manages animation states (e.g., idle, walk, run, jump).


### Setting Up a State Machine

- Open the **Anim Graph** and create a **State Machine**:
    1. Add states (e.g., `Idle`, `Run`).
    2. Connect states with transition rules.
    3. Define conditions (e.g., speed > 0 for transitioning to `Run`).
	    1. Blend spaces for walk->run transitions 

### Blending Animations

- Use **Blend Spaces** to combine animations based on input:
    1. Right-click and create a **Blend Space** or **2D Blend Space**.
    2. Set up input axes (e.g., Speed, Direction).
    3. Assign animations to corresponding areas in the Blend Space.

### Using the Animation Blueprint

- Assign the Animation Blueprint to the skeletal mesh:
    1. Open the Skeletal Mesh asset.
    2. Set the **Animation Mode** to **Use Animation Blueprint**.
    3. Assign the created Animation Blueprint.


### Testing Animations in the Game

- Add the skeletal mesh to a **Character Blueprint**.
- In the Character Blueprint:
    1. Set up input bindings for movement (e.g., WASD keys).
    2. Update the speed variable in the Event Graph to reflect player input.
    3. Use this variable in the Animation Blueprint to drive state transitions or blending.



### Root Motion

- Enables animations to dictate character movement instead of code.
- Set in the Skeletal Mesh asset:
    1. Enable **Root Motion** in the animation settings.
    2. In the Animation Blueprint, ensure movement is handled correctly.

### Using Animation Montages

- Ideal for one-off actions (e.g., attacks, emotes).
- **Steps**:
    1. Create an Animation Montage from an animation asset.
    2. Trigger it in code or Blueprints.

### Connecting Animations to Gameplay Logic

- Use **Blueprint** or **C++** to interact with the Animation Blueprint:
    - Update variables (e.g., `Speed`, `IsJumping`).
    - Trigger specific animations using functions.


## Best Practices

- **Organize Assets**: Keep skeletal meshes, animations, and Blueprints in clear folders.
- **Optimize Blend Spaces**: Limit the number of axes and animations for better performance.
- **Test Early**: Regularly test animations in the game environment to catch issues.