# Bevy Sprity

The ultimate bevy Aseprite plugin. This plugin allows you to import aseprite files into bevy, with 100% unbreakable
hot reloading. You can also import static sprites from an aseprite atlas using slices with functional pivot offsets!

# Supported Aseprite Features

-   Animations
-   Tags
-   Frame Duration, Repeat, and Animation Direction
-   Layer Visibility
-   Blend Modes
-   Static Slices and Pivot offsets

# Features

-   Hotreload anything, anytime, anywhere!
-   Full control over animations using Components.
-   Oneshot animations and events when they finish.

# Installation

```rust

```

# Example

![Example](docs/example.gif)

There are two main Bundles added by this plugin, `AsepriteAnimationBundle` and `AsepriteSliceBundle`.

```rust
use bevy::prelude::*;
use bevy_sprity::prelude::*;

...

// Load the an animation from a aseprite file
fn spawn_demo_animation(mut cmd : Commands, server : Res<Assetserver>){
    cmd.spawn(AsepriteAnimationBundle {
        aseprite: server.load("player.aseprite"),
        transform: Transform::from_translation(Vec3::new(15., -20., 0.)),
        // set a custom animation speed multiplier
        animation_speed: AnimationSpeed(0.5),
        // you can override the default sprite settings here
        // the `rect` will be overriden by the animation
        sprite: Sprite {
            flip_x: true,
            ..default()
        },
        ..default()
    })
        // insert a optional animation to play, if none is provided, all frames will be played in `forward` direction
        .insert(AnimationTag::from("walk-right"));
}

// Load a static slice from a aseprite file
fn spawn_demo_static_slice(mut cmd : Commands, server : Res<Assetserver>){
    cmd.spawn(AsepriteSliceBundle {
        slice: "ghost_blue".into(),
        // you can override the default sprite settings here
        // the `rect` will be overriden by the slice
        // if there is a pivot provided in the aseprite slice, it will also be overwritten
        // and changes the origin of rotation.
        sprite: Sprite {
            flip_x: true,
            ..default()
        },
        aseprite: server.load("ghost_slices.aseprite"),
        transform: Transform::from_translation(Vec3::new(32., 0., 0.)),
        ..default()
    });
}

// animation events - tell me when the animation is done
// this is useful for one shot animations like explosions
fn despawn_on_finish(mut cmd : Commands){
    for event in events.read() {
        match event {
            AnimationEvents::Finished(entity) => cmd.entity(*entity).despawn_recursive(),
            // you can also listen for loop cycle repeats
            AnimationEvents::LoopCycleFinished(_entity) => (),
        };
    }
}
```
