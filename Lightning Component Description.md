# Lightning Component
Lightning is all about components. You can build applications by assembling components created by you and other developers. A component can contain other components, as well as HTML, CSS, JavaScript, or any other Web-enabled code. This enables you to build apps with sophisticated UIs. We should always keep in mind Lightning Component’s modular approach while creating a Lightning App. It is always a best practice to use component based development approach. 
##### 1.	Increases
a. Developer Productivity
b. Feature availability
c. Application Scalability
##### 2.	Decreases
a. Application Complexity
b. Time to Delivery
c. Overall Cost
### Lightning Component Bundle
Each Lightning Component is made up of a markup, JavaScript controller, a Helper, a Renderer 
and more (Component Bundle).

![ScreenShot](https://github.com/ckprajkumar/sfdc/blob/master/Picture3.png?raw=true)

#### Controller
*	Use Controllers to listen to user events and other events like Component Event, Application Event.
*	Delegate your business logic to helper methods.
*	Do not trigger DML operation on component initialization. If you are doing DML in init(), you are creating a CSRF(Cross-Site Request Forgery).
*	Do not modify DOM in Controller. If you modify DOM in Controller, it will call renderer method which will end in no result.
#### Helper
Always write your business logic in helper functions because
*	Helper functions may be called from any other java-script in the component bundle.
*	Whenever a component runs Lightning Framework creates an instance of the Controller, an instance of the Renderer for each component but creates only one copy of the Helper and passes the reference of the Helper into every Controller instance and every Renderer instance. Below picture will make you understand this well.

![ScreenShot](https://github.com/ckprajkumar/sfdc/blob/master/Picture4.png?raw=true)

#### Renderer
*	Use Renderer whenever you want to customize default rendering, rerendering, afterrendering and unrendering behavior for a component.
*	Do not fire an event in renderer, firing an event in a renderer can cause an infinite rendering loop.
*	If you need to directly manipulate DOM elements in your component, you should do it in the component’s renderer
#### Events
*	Always use events to implement communication between components.
*	Always try to use a component event instead of an application event, if possible. Component events can only be handled by components above them in the containment hierarchy so their usage is more localized to the components that need to know about them. Below picture will make you understand better about component event.
*	Application events are best used for something that should be handled at the application level, such as navigating to a specific record.

![ScreenShot](https://github.com/ckprajkumar/sfdc/blob/master/Picture5.png?raw=true)

