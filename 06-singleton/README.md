# 6 Singleton

A pattern that ensures that for a class only one instance is created and it allows global access to that instance.
- Unity: `ScriptableObject*` (*not really a Singleton, but a good replacement for it)
- C#: `public static`
- C++: `public static`

## Disclaimer:

This pattern is considered bad and an anti-pattern by modern interpretation and I strongly advise you not to use it for projects with a lifetime of more than 12 weeks.

## Inventory Example:

Example: The player has an inventory. Many systems need to access that inventory:
- items, when they're collected, so they can be added to it
- enemies, when they die, so the inventory can be added some extra gold
- shops, when the player chooses to buy items
- the auction house, when the player trades items
- an easter pop-up, when the player collects some free holiday gifts

But how can all those different classes find the Player Inventory?

```cpp
class PlayerInventory {
    static PlayerInventory* instance;

    PlayerInventory* LoadInventory(PlayerInventorySaveGame& saveGame){/*...*/}
public:
    static PlayerInventory* getInstance(){
        if(instance == nullptr){
            instance = LoadInventory(SaveGame.getInstance());
        }
        return instance;
    }
};
```

Now, any other class can easily access the inventory:

```cpp
class Shop{
public:
    void onPurchase(Item* itemPrototype){
        PlayerInventory.getInstance().add(itemPrototype.clone());
    }
}```

```cpp
class Enemy{
public:
    void onDeath(){
        PlayerInventory.getInstance().addGold(10);
    }
}
```

## Why is it good?
- It is easy to implement. And easy to use.
- Singletons only get instantiated IF and WHEN they are needed.
- You don't need to worry about the order in which they are instantiated in case of dependencies between your Singletons.

## Why is it bad?
Global access promotes tight coupling of classes.
- e.g. a Newbie might access the SoundManager from the PhysicsEngine. Just because it's easy to call `SoundManager.getInstance()`

You lose the power to control who accesses the instance.
- this makes it hard to understand, who breaks a class instance, since anyone can access it

You can't easily create multiple instances if there's need
- e.g. for the previous PlayerInventory: it will become very difficult, if you make the game a MultiPlayer game now.

You lose control over the order in which Singletons are instantiated.
- this can break your game, if for example the Sound System can only work if the Main Scene has been fully loaded already.

## What are alternatives?
- Have one Singleton that controls all other initialization
- Use Dependency Injection
- Use a Service Locator