# DependencyInjection
Swift dependency injection.

Dependency Injection(DI) is a design pattern in software design wich provide testable code and loose couple code for us.
Here are three major patterns of DI.
1. Constructor injection.
2. Property injection.
3. Method injdection.

We are discussing constructor injection in this article.

## Class diagram
![DI](/Dependency%20Injection.png)

## Sample Code
```swift
protocol Service {
    static var shared: Service { get }
    func getImage(url: String) -> UIImage?
}

class RealService: Service {
    static var shared: Service = RealService()

    func getImage(url: String) -> UIImage? {
        guard let url = URL(string: url),
            let data = try? Data(contentsOf: url) else {
            return nil
        }
        return UIImage(data: data)
    }
}

class MockService: Service {
    static var shared: Service = MockService()

    func getImage(url: String) -> UIImage? {
        return nil
    }
}

class DataManager {
    static let shared = DataManager()
    var service: Service
    init(service: Service = RealService.shared) {
        self.service = service
    }

    func getImage() -> UIImage? {
        return service.getImage(url: "https://avatars2.githubusercontent.com/u/5814856?s=40&v=4")
    }
}

let dataManager = DataManager.shared
let image = dataManager.getImage()
print(image)

let mockManager = DataManager(service: MockService())
let mockImage = mockManager.getImage()
print(mockImage)
```

## Console log
```
Optional(<UIImage: 0x6000018212d0>, {40, 40})
nil
```
