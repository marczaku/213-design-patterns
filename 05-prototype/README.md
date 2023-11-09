# 5 Prototype

A pattern that allows you to define objects as a template/prefab/prototype of others. For example, you might create a prototype for one kind of monster and then clone it 100 times to spawn one hundred monsters.
- Unity: `Object.Instantiate`, `Prefab`, `Prefab Variant`
- C#: `IClonable`
- C++: manually implement a `virtual Clone()` function

## Spawner Example:

Example: We want to have a Spawner that can spawn enemies. But we don't want to create a new Spawner class each time we create a new Type of Enemy.

Solution: Pass an Enemy-Prototype with a `clone()` function to the Spawner and clone that prototype. Like a Unity-Prefab.

```cpp
class EnemySpawner {
    Enemy* prototype;
public:
    EnemySpawner(Enemy* prototype) : prototype{prototype} {}
    Enemy* spawn(){
        return prototype.clone();
    }
};
```

## Variant Example

Imagine your Designer wants to create Hundreds of Items. At some point, he might want to create slight variations of some items:

### Redundant Example

```json
{
    "id" : "weapon.knife",
    "type" : "Melee",
    "hands" : 1,
    "damageType" : "Physical",
    "damage" : 20
}
```

```json
{
    "id" : "weapon.longKnife",
    "type" : "Melee",
    "hands" : 1,
    "damageType" : "Physical",
    "damage" : 30
}
```

```json
{
    "id" : "weapon.goldenDagger",
    "type" : "Melee",
    "hands" : 1,
    "damageType" : "Physical",
    "damage" : 50
}
```

A lot of redundancy. And it will make it more difficult for the designer, if your game were to change somehow. e.g. Knives are now supposed to deal "Cut" damage instead of "Physical".

### Better Solution

Allow your weapons to define a Prototype. The Prototype's values will be used first and then overridden by the values defined for the new item:

```json
{
    "id" : "weapon.knife",
    "type" : "Melee",
    "hands" : 1,
    "damageType" : "Physical",
    "damage" : 20
}
```

```json
{
    "id" : "weapon.longKnife",
    "prototype" : "weapon.knife",
    "damage" : 30
}
```

```json
{
    "id" : "weapon.goldenDagger",
    "prototype" : "weapon.knife",
    "damage" : 50
}
```

Unfortunately, the Json Parsers I know don't natively support this. But it is not too difficult to implement this Feature yourself. Unity supports this as Prefab Variants.