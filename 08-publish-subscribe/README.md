# 8 Publish-Subscribe Pattern

A pattern that allows objects to communicate with each other without requiring a direct reference. It is an extension of the Observer pattern. It uses a Broker and Messaging System similar to a Postal Service to send and receive Messages.
- Unity: see C#
- C#: `Singleton`, `delegate`
- C++: `Singleton`, `function pointer`

## Achievement System 

- Low-level systems depend on high-level systems. Not good!

```cpp
// code sample see Observer Bad Example
```

## Achievement System: Observer

- Now, the high-level system depends on the low-level systems.
- Improvement, but not perfect.

```cpp
// code sample see Observer Good Example
```

## Achievement System (Broker):

- Fully decoupled
- Anyone can publish and subscribe events

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <functional>

class Broker {
public:
    void subscribe(std::string topic, std::function<void(std::string)> callback) {
        subscribers_[topic].push_back(callback);
    }

    void publish(std::string topic, std::string message) {
        for (auto& callback : subscribers_[topic]) {
            callback(message);
        }
    }

private:
    std::map<std::string, std::vector<std::function<void(std::string)>>> subscribers_;
};

void fooCallback(std::string message) {
    std::cout << "Received message in fooCallback: " << message << std::endl;
}

void barCallback(std::string message) {
    std::cout << "Received message in barCallback: " << message << std::endl;
}

int main() {
    Broker broker;

    broker.subscribe("foo", fooCallback);
    broker.subscribe("bar", barCallback);

    broker.publish("foo", "Hello, world!");
    broker.publish("bar", "Goodbye, world!");

    return 0;
}
```
