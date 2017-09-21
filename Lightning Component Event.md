### Component Event
The ceEvent.evt component event has one attribute. We’ll use this attribute to pass some data in the event when it’s fired.
```sh
<!--c:ceEvent-->
<aura:event type="COMPONENT">
    <aura:attribute name="message" type="String"/>
</aura:event>
```
### Notifier Component
The c:ceNotifier component uses aura:registerEvent to declare that it may fire the component event.
The button in the component contains an on-click browser event that is wired to the fireComponentEvent action in the client-side controller. The action is invoked when you click the button.
```sh
<!--c:ceNotifier-->
<aura:component>
    <aura:registerEvent name="cmpEvent" type="c:ceEvent"/>
    <h1>Simple Component Event Sample</h1>
    <p><lightning:button
        label="Click here to fire a component event"
        onclick="{!c.fireComponentEvent}" />
    </p>
</aura:component>
```
The client-side controller gets an instance of the event by calling cmp.getEvent("cmpEvent"), where cmpEvent matches the value of the name attribute in the <aura:registerEvent> tag in the component markup. The controller sets the messageattribute of the event and fires the event.
### Handler Component
The c:ceHandler handler component contains the c:ceNotifier component. The <aura:handler> tag uses the same value of the name attribute, cmpEvent, from the <aura:registerEvent> tag in c:ceNotifier. This wires up c:ceHandler to handle the event bubbled up from c:ceNotifier.

When the event is fired, the handleComponentEvent action in the client-side controller of the handler component is invoked.
```sh
<!--c:ceHandler-->
<aura:component>
    <aura:attribute name="messageFromEvent" type="String"/>
    <aura:attribute name="numEvents" type="Integer" default="0"/>
    <!-- Note that name="cmpEvent" in aura:registerEvent
     in ceNotifier.cmp -->
    <aura:handler name="cmpEvent" event="c:ceEvent" action="{!c.handleComponentEvent}"/>
    <!-- handler contains the notifier component -->
    <c:ceNotifier />
    <p>{!v.messageFromEvent}</p>
    <p>Number of events: {!v.numEvents}</p>

</aura:component>
```
The controller retrieves the data sent in the event and uses it to update the messageFromEvent attribute in the handler component.
```sh
/* ceHandlerController.js */
{
    handleComponentEvent : function(cmp, event) {
        var message = event.getParam("message");

        // set the handler attributes based on event data
        cmp.set("v.messageFromEvent", message);
        var numEventsHandled = parseInt(cmp.get("v.numEvents")) + 1;
        cmp.set("v.numEvents", numEventsHandled);
    }
}
```

