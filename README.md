# Providing-a-unified-Swift-error-API

```swift
func perform<T>(_ expression: @autoclosure () throws -> T,
                errorTransform: (Error) -> Error) throws -> T {
    do {
        return try expression()
    } catch {
        throw errorTransform(error)
    }
}
```

### Usage
```swift
func loadSearchData(matching query: String) throws -> Data {
    let urlString = "https://my.api.com/search?q=\(query)"

    guard let url = URL(string: urlString) else {
        throw SearchError.invalidQuery(query)
    }

    return try perform(Data(contentsOf: url)) { error in
        return SearchError.dataLoadingFailed(url, underlying: error)
    }
}
```
