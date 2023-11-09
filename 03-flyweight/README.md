# 3 Flyweight

The idea here is, if you have a thousand objects containing a lot of data, then you could put the information that they all have in common into a separate class and then share one instance of that class across all instances instead of each instance having their own copy of said redundant information.
- Unity: `ScriptableObject`, `UnityEngine.Object`
- C#: `class`
- C++: Pointers, References

## Bad Army Example

```cpp
class Soldier{
private:
    Mesh characterMesh;
    Mesh weaponMesh;
    Texture armorTex;
    Texture skinTex;
    Texture weaponTex;
    Vector3 position;
    float health;
    float strength;
};
```

Problem for 1000 Instances:
- 1000 Character Meshes
- 1000 Weapon Meshes
- 1000 Armor Textures
- ...
- 1000 Positions
- 1000 Healths

Redundancy:
- They all have the same Mesh, why do we have 1000 copies of that?
- They all have the same Weapon, why do we have 1000 copies of that?
- They all have the same Armor, why do we have 1000 copies of that?
- They all have different Positions, so that's fine to have copies of

## Flyweight

```cpp
// wrap shared data into a separate object:
class SoldierData {
private:
    Mesh characterMesh;
    Mesh weaponMesh;
    Texture armorTex;
    Texture skinTex;
    Texture weaponTex;
};
```

```cpp
class Soldier{
private:
    // shared instance:
    SoldierData* data;
    Vector3 position;
    float strength;
    float health;
};
```

Improvement for 1000 Instances:
- 1 Character Mesh
- 1 Weapon Mesh
- 1 Armor Texture
- ...
- 1000 Positions
- 1000 Healths