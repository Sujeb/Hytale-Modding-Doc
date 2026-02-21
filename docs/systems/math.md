# Math Library

> **CONTEXT**: Documentation for Hytale's custom math classes.
> **SOURCE**: Analysis of `com.hypixel.hytale.math.vector`.

## 1. Overview

Hytale uses its own math library located in `com.hypixel.hytale.math`.
You must use these specific classes for API interoperability, not standard Java/LibGDX/JOML libraries unless converted.

## 2. Vectors

### Vector3d (Double Precision)
Used for World Coordinates, Entity Positions, and Physics.

*   **Class**: `com.hypixel.hytale.math.vector.Vector3d`
*   **Usage**:
    ```java
    Vector3d pos = new Vector3d(0, 10, 0);
    pos.add(1.0, 0.0, 1.0);
    double dist = pos.distance(otherPos);
    ```

### Vector3f (Float Precision)
Used for Rendering, Models, and relative offsets.

*   **Class**: `com.hypixel.hytale.math.vector.Vector3f`

### Vector3i (Integer Precision)
Used for Block Coordinates and Chunk indices.

*   **Class**: `com.hypixel.hytale.math.vector.Vector3i`

## 3. Quaternions

Used for Rotation (Entity orientation).

*   **Class**: `com.hypixel.hytale.math.quaternion.Quaternionf` (usually float-based for transforms)

## 4. Helper Methods

The classes typically support method chaining:
```java
Vector3d velocity = new Vector3d(0, 0, 0)
    .add(direction)
    .mul(speed);
```
