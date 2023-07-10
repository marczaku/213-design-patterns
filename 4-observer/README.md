# 4 Observer

A pattern that helps you decouple components by making sure that high-level classes can observe the results of low-level classes instead of the low-level class being required to know, which high-level classes need to be informed on changes.
- Unity: `UnityEvent`
- C#: `event`
- C++: manual implementation using `list` and Virtualization

## Bad Achievement Example

Example: We want to make a system where the Player gets an achievement when he achieves a velocity of more than `100`. The PhysicsEngine updates each actor's velocity based on their acceleration. So that's the class that can trigger the achievement, right?

```cpp
class PhysicsEngine{
    void updateVelocity(float deltaTime){
        for(auto actor : actors){
            actor.velocity += player.acceleration * deltaTime;
            if(actor.isPlayer && actor.velocity.magnitude > 100f){
                achievementManager.unlock(Achievement::RunningVeryFast);
            }
        }
    }
};
```

Problems:
- it's difficult to understand, why the `PhysicsEngine` has to change when an Achievement Changes
- the `PhysicsEngine` has detailed knowledge of how the Achievement works
- the `PhysicsEngine` needs a reference to the `achievementManager`
- if you were to port the `PhysicsEngine` to another Game
  - it would either not work
  - or you would have to introduce Achievements to that game as well
  - or you would have to make changes to the `PhysicsEngine`

```cpp
class Enemy{
    void takeDamage(float damage){
        static int deathCount = 0;
        health -= damage;
        if(health <= 0f){
            if(++deathCount == 100){
                achievementManager.unlock(Achievement::HundredKills);
            }
        }
    }
};
```

General Problem: Achievement Logic is fragmented in your whole code-base.
- difficult to understand
- difficult to update
- difficult to remove

## Observer Pattern

The Observer Pattern de-couples the `Enemy` from the `Achievement`-System by allowing classes to observe changes to it. That way, instead of the `Enemy` knowing about `Achievements`, only the `KillAchievement` needs to know, what `Enemies` to observe:

### Subject

The Subject is the class that can be observed (`Enemy`):

```cpp
class IDeathSubject{
private:
    std::vector<IDeathObserver*> deathObservers;
public:
    void addObserver(IDeathObserver* observer){
        deathObservers.push_back(observer);
    }
    void removeObserver(IDeathObserver* observer){
        deathObservers.erase(std::remove(deathObservers.begin(), deathObservers.end(), observer), deathObservers.end());
    }
    void raiseDeath(IDeathSubject* subject){
        for(auto observer : deathObservers){
            // notify observer that we died
            observer->notifyDeath(this);
        }
    }
};
```

```cpp
class Enemy : IDeathSubject {
    void takeDamage(float damage){
        static int deathCount = 0;
        health -= damage;
        if(health <= 0f){
            raiseDeath(this);
        }
    }
};
```

### Observer

The Observer is the class that can observe (`KillAchievement`):

```cpp
class IDeathObserver{
public:
    virtual ~IDeathObserver() {};
    virtual void notifyDeath(IDeathSubject& subject) = 0;
};
```

```cpp
class KillsAchievement : IDeathObserver{
    Enemy* enemy;
    int killCount;
public:
    KillsAchievement(Enemy* enemy, int killCount) : enemy{enemy} killCount{killCount} {
        enemy->addObserver(this);
    }

    void notifyDeath override (IDeathSubject& subject){
        if(--killCount == 0){
            // achievement logic
        }
    }
};
```

Advantages:
- kills achievements can easily be created for 100,1000,10000 Kills.
- all achievement logic can reside in one folder called achievements containing achievements classes.
- the achievement class can easily be serialized (saved) and deserialized (loaded) to track progress
- the enemy class remains clean and free of achievement logic

## Observer Composition

Always remember: Composition over Inheritance.\ We can make Observables a template class that can easily be reused whenever you want to make use of Observables in any class:

```cpp
class Enemy {
public:
    Observable<Kill*> onDeath;
};
```

```cpp
enemy->onDeath.addObserver(this);
```

## Observer Functions

Right now, Observers always need to define a function with the exact name expected by the `ISubject` (and provided by the `IObserver`-Interface) and it needs to inherit it from an Interface:

```cpp
class IDeathSubject{
// ...
    void raiseDeath(IDeathSubject* subject){
        for(auto observer : deathObservers){
            // notify observer that we died
            observer->notifyDeath(this);
        }
    }
};
// ...
class IDeathObserver{
public:
    virtual ~DeathObserver();
    virtual void notifyDeath(IDeathSubject& subject) = 0;
};
```

That's a bit annoying, since usually, the Observer only needs to provide exactly one function defined in the interface and that function might make sense to have different names in different contexts. Imagine, for example, a Game-Class that counts Deaths from TeamA and TeamB. Both player from TeamA and TeamB implement `IDeathSubject`. Wouldn't it be great, if you could somehow make it so that TeamA's `IDeathSubjects` invoke `onTeamADeath()` and TeamB's `IDeathSubjects` invoke `onTeamBDeath()`?

Or generally, wouldn't it be great, if the Observing class could just instruct the Subject, what function to call? In fact, we can make it so!

```cpp
class Observable {
public:
    void addObserver(std::function<void()> func) {
        observers_.push_back(func);
    }

    void notifyObservers() {
        for (auto& observer : observers_) {
            observer();
        }
    }

private:
    std::vector<std::function<void()>> observers_;
};
```

```cpp
class MyObservable : public Observable {
public:
    void doSomething() {
        // do something here...
        std::cout << "Observable Event Raised!\n";
        notifyObservers();
    }
};
```

```cpp
class MyObserver {
public:
    MyObserver(Observable& observable) {
        observable.addObserver([this] {this->update(); });
    }
    virtual void update() {
        std::cout << "Observer was notified!\n";
    }
};
```cpp

```cpp
void myFunction() {
    std::cout << "myFunction was called!\n";
}

int main() {
    MyObservable observable;
    MyObserver observer{ observable };

    observable.addObserver(myFunction);

    observable.doSomething();

    return 0;
}
```
