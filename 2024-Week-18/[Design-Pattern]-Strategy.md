Type: **Behavioral Pattern**

Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

Class Diagram Example:
```mermaid
classDiagram
    class Client

    class Context {
        -strategy
        +setStrategy(strategy)
        +doSomething()
    }
    Client --> Context

    class Strategy {
        <<interface>>
        +execute(data)
    }
    Context o--> Strategy

    class ConcreteStrategies {
        +execute(data)
    }
    Strategy <|.. ConcreteStrategies
    Client ..> ConcreteStrategies
```

## Code Example:
- Class Diagram
```mermaid
classDiagram
    class Navigator {
        -routeStrategy
        +buildRoute(A, B)
    }

    class RouteStrategy {
        <<interface>>
        +buildRoute(A, B)
    }
    Navigator o--> RouteStrategy

    class RoadStrategy
    RouteStrategy <|.. RoadStrategy

    class WalkingStrategy
    RouteStrategy <|.. WalkingStrategy
```
- Code

        class WalkingStrategy {
            public buildRoute(A, B) {};
        };

        class RoadStrategy {
            public buildRoute(A, B) {};
        };

        class Navigator {
            private routeStrategy;
            public setStrategy(_routeStrategy) {
                this.routeStrategy = _routeStrategy;
            };
            public buildRoute(A, B) {
                this.routeStrategy.buildRoute(A, B);
            };
        };

        // Usage
        const navigator = new Navigator();
        if (selectedStrategy ==== 'road') {
            navigator.setStrategy(new RoadStrategy());
        }
        if (selectedStrategy ==== 'walking') {
            navigator.setStrategy(new WalkingStrategy());
        }
        navigator.buildRoute(A, B);

Source: https://refactoring.guru/design-patterns/strategy
