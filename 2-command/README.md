# 2 Command

Put a function into an object (class instance) to make it more shareable and configurable. This means, instead of directly calling a "Jump" function when pressing `X`, `X` Fires a `Command`-Object that has been assigned to a variable. In this case, a `JumpCommand`, but the user could also configure a `RunCommand` to be assigned to that key.

Advantages:
- you can pass those objects to other classes that control
  - recording of all Commands necessary to create a replay
  - network communication to other players, so they see you jump as well
  - pause/unpause, validation, queueing
- you have object-oriented objects and make use of all OOP principles
- those objects can be serialized and deserialized (saved and loaded)
- those objects can be replaced (e.g. for different key-mappings)

It is so popular and important that most languages allow function pointers and lambda functions nowadays to pass functions as objects.
- Unity: `SendMessage`
- C#: `delegate`
- C++: `std::function`, Function Pointers

Use Cases:
- Input Manager (e.g. allow re-binding Commands to Key Inputs)
- Decoupling of Input and Effect (e.g. a JumpPad could send a JumpCommand to the Player)
- Game-Play Recording (record all player input and re-simulate the game)
- Multiplayer Games (e.g. Sending `Cast Fire Ball` to other players)
- Input Validation (e.g. Backend Communication for `Join Clan`)

## Bad Input System Example

```cpp
class Player{
    
    void jump();
    void attack();
    void run();
    void crouch();

    void Input::onInput(){
        switch(event.cbutton.button){
        case SDL_GameControllerButton::SDL_CONTROLLER_BUTTON_X: {
            /* jump code*/ break;
        }
        case SDL_GameControllerButton::SDL_CONTROLLER_BUTTON_Y: {
            /* attack code*/ break;
        }
        case SDL_GameControllerButton::SDL_CONTROLLER_BUTTON_A: {
            /*run code*/ break;
        }
        case SDL_GameControllerButton::SDL_CONTROLLER_BUTTON_B: {
            /*crouch*/ break;
        }
        }
    }
};
```

Problem: It's hard coded
- what, if the player wants to change their input mapping?
  - e.g. jump on `SDL_CONTROLLER_BUTTON_RIGHT_SHOULDER`?
- what, if you want to add keyboard support?
- what, if the player is supposed to switch his character?
  - e.g. in FIFA, where the player can switch controls to another player
- what, if other triggers can make the player run/jump/crouch?
  - e.g. a loud explosion makes the player duck automatically

## Command Pattern (Tightly Coupled)

The first minimal variant of the Command-Pattern solves above problems. But these Commands only work for the issuing class:

```cpp
class BaseCommand {
public:
    virtual ~BaseCommand(){}
    virtual void execute() = 0;
};
```

```cpp
class JumpCommand : public Command{
public:
    virtual void execute() override {
        /* make player jump */
    }
};
```

```cpp
class Player{
    // this is where we store all commands for each button
    Command* buttonX;
    Command* buttonY;
    // ...

    // initialize the default input commands:
    Player() : buttonX{new JumpCommand{}} { }
    void jump();
    void attack();
    void run();
    void crouch();

    void Input::onInput(){
        switch(event.cbutton.button){
        case SDL_GameControllerButton::SDL_CONTROLLER_BUTTON_X: {
            // if button X is currently bound to a command execute it:
            if(buttonX) buttonX->execute(); 
            break;
        }
        // ...
        }
    }

    // this is, how we can bind new commands to the buttons:
    void rebindX(Command* newCommand){
        if(buttonX) delete buttonX;
        buttonX = newCommand;
    }
};
```

Problem: The Command only works for the Player right now.

## Command Pattern (Decoupled)

This more advanced approach passes the `Actor` as an Argument to the Execute function. This allows you to reuse these commands for any classes that inherit from the base class `Actor`:

```cpp
class BaseCommand {
public:
    virtual ~BaseCommand(){}
    virtual void execute(Actor& actor) = 0;
};
```

```cpp
class JumpCommand : public Command{
public:
    virtual void execute(Actor& actor) override {
       /* make actor jump */
    }
};
```

## Command Pattern (Undo & Redo)

This one is the most sophisticated approach (and at the same time even introduces an Undo/Redo functionality). All arguments needed by the command will be passed in the Command Constructor. Now, anyone can at anytime execute and undo this command.

```cpp
class BaseCommand {
public:
    virtual ~BaseCommand(){}
    virtual void execute() = 0;
    virtual void undo() = 0;
};
```

```cpp
class MoveCommand : public Command{
    Actor* actor;
    Vector2 position, oldPosition;
public:
    MoveCommand(Actor* actor, Vector2 position) : actor{actor}, position{position} {}
    virtual void execute() override {
       oldPosition = actor.position;
       actor.position = position;
    }
    virtual void undo() override {
       actor.position = oldPosition;
    }
};
```

Now, you can track the executed commands:
- in a queue
  - if you want to queue commands like in an RTS
- in a list
  - if you want to save them for a replay
- on a network stream
  - if you want to update the commands over a network
- on a stack
  - if you want to allow players to undo and redo their commands