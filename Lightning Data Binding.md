### Data Binding Between Components

An expression to initialize attribute values in the component based on attribute values of the container component. There are two forms of expression syntax, which exhibit different behaviors for data binding between the components.
```sh
<!--c:parent-->
<aura:component>
    <aura:attribute name="parentAttr" type="String" default="parent attribute"/>
    <!-- Instantiate the child component -->
    <c:child childAttr="{!v.parentAttr}" />
</aura:component>
```
Consider a c:parent component that has a parentAttr attribute. c:parent contains a c:child component with a childAttr attribute that’s initialized to the value of the parentAttr attribute. We’re passing the parentAttr attribute value from c:parent into the c:child component, which results in a data binding, also known as a value binding, between the two components.
### Unbound Expressions
Use the {#expression} syntax for unbound expressions when you pass an expression from a parent component to a child component, unless you require bidirectional data binding. Bound expressions with {!expression} create a bidirectional data binding that’s expensive for performance. Bound expressions can also create hard-to-debug errors due to the propagation of data changes through nested components.
```sh
<c:child childAttr="{!v.parentAttr}" />
```
{#v.parentAttr} is an unbound expression. Any change to the value of the childAttr attribute in c:child doesn’t affect the parentAttr attribute in c:parent and vice versa.
### Bound Expressions
```sh
<c:childExpr childAttr="{#v.parentAttr}" />
```
to
```sh
<c:child childAttr="{!v.parentAttr}" />
```
{!v.parentAttr} is a bound expression. Any change to the value of the childAttr attribute in c:child also affects the parentAttr attribute in c:parent and vice versa.
#### Summary of the differences between the forms of expression syntax
##### {#expression} (Unbound Expressions)
Data updates behave as you would expect in JavaScript. Primitives, such as String, are passed by value, and data updates for the expression in the parent and child are decoupled.
Objects, such as Array or Map, are passed by reference, so changes to the data in the child propagate to the parent. However, change handlers in the parent aren’t notified. The same behavior applies for changes in the parent propagating to the child.
##### {!expression} (Bound Expressions)
Data updates in either component are reflected through bidirectional data binding in both components. Similarly, change handlers are triggered in both the parent and child components.
### Change Handlers and Data Binding
You can configure a component to automatically invoke a change handler, which is a client-side controller action, when a value in one of the component's attributes changes.
When you use a bound expression, a change in the attribute in the parent or child component triggers the change handler in both components. When you use an unbound expression, the change is not propagated between components so the change handler is only triggered in the component that contains the changed attribute.
```sh
<!--c:childExpr-->
<aura:component>
    <aura:attribute name="childAttr" type="String" />
    <aura:handler name="change" value="{!v.childAttr}" action="{!c.onChildAttrChange}"/>
    <p>childExpr childAttr: {!v.childAttr}</p>
    <p><lightning:button label="Update childAttr" onclick="{!c.updateChildAttr}"/></p>
</aura:component>
```
For More Information: 
https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/expr_data_binding.htm






