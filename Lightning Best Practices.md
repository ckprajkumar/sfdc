# Lightning Components Best Practices
*	Caching data at the client side can significantly reduce the number of server round-trips and improve the performance of your Lightning components.
*	Access server data using either server actions or the Lightning Data Service
*	Server actions support optional client-side caching, and the Lightning Data Service is built on top of a sophisticated client caching mechanism
### Apex Server-Side Controller

*	Methods must be static and marked public or global. Non-static methods aren’t supported.
*	If a method returns an object, instance methods that retrieve the value of the object’s instance field must be public.
*	Use unique names for client-side and server-side actions in a component. A JavaScript function (client-side action) with the same name as an Apex method (server-side action) can lead to hard-to-debug issues. In debug mode, the framework logs a browser console warning about the clashing client-side and server-side action names

