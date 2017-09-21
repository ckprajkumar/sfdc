### Scenario 1: The response is not available in the cache (or has expired)
[Image]
* The component calls a server method
* The framework checks if the response is available in the cache
* The response isn’t available in the cache or is expired (cached response age > expiration age)
* The framework calls the server method
* The server returns the response
* The framework caches the response
* The framework calls the action callback function providing the server response
### Scenario 2: The response is available in the cache and doesn’t need to be refreshed
[image]
* The component calls a server method
* The framework checks if the response is available in the cache
* The response is available, and doesn’t need to be refreshed (cached response age <= refresh age)
* The framework calls the action callback function, providing the cached response
### Scenario 3: The response is available in the cache and needs to be refreshed
[image]
* The component calls a server method
* The framework checks if the response is available in the cache
* The response is available in the cache and needs to be refreshed (cached response age > refresh age)
* The framework calls the action callback function providing the cached response
* The framework calls the server method to get a fresh response
* The server returns the response
* The framework updates the cache with the new response
* If the server response is different from the cached response, the framework calls the action callback function for the second time with the updated response

