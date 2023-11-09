# 7 State

A pattern that allows an object to alter its behavior when its internal state changes. The object will appear to change its class. This is very useful, when a classes behavior changes dramatically depending on some variables. For example a game that can be in Main Menu, Options and Game States or an enemy that may be in Exploring, Alerted and Attacking States.
- Unity: `Animator`
- C#: `interface`, Inheritance
- C++: `abstract`, Inheritance

## Input Example (Bad):

- Whenever you add a new state (like falling, attacking, ...)
- All other Key Inputs need to be updated

```cpp
void OnInput(Key key){
    if(key == Key.DownA){
        if(!isJumping && !isSliding){
            // jump
        }
    } else if(input == Key.DownB){
        if(!isJumping){
            // start sliding
        } else {
            // stomp
        }
    } else if(input == Key.UpB){
        if(isSliding){
            // stop sliding
        }
    }
}
```

## Input Example (Enum State):

- Improves the amount of unintended side effects
- But still leaves all the logic in one place

```cpp
State state;
void OnInput(Key key){
    switch(state){
        case State.Jumping: {
            if(key == Key.DownB){
                // stomp
            }
        } break;
        case State.Sliding: {
            if(key == Key.UpB){
                // stop sliding
            }
        } break;
        case State.Standing: {
            if(key == Key.DownA){
                // jump
            } else if(key == Key.DownB){
                // start sliding
            }
        } break;
    }
}
```

## Input Example (State Class):

- Much more extensible
- Code separated into multiple smaller classes

```cpp
class State{
public:
    virtual void enter() = 0;
    virtual void exit() = 0;
    virtual State* update() = 0;
    virtual State* onInput(Key key) = 0;
}
```

```cpp
State* currentState;

void OnInput(Key key){
    auto newState = currentState->onInput(key);
    if(newState != nullptr){
        switchState(newState);
    }
}

void switchState(State* newState){
    currentState.exit();
    currentState = newState;
    newState.enter();
}
```
