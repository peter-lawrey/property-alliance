## SJSU CS Wiki Use Cases

A copy of the SJSU CS wiki curated by Cay Horstmann.
Retrieved from the Internet Archive.

Version 1.3 last modified by Administrator on 28/10/2007 at 21:35

---

## bean-properties Project Analysis

The [bean-properties project](https://bean-properties.dev.java.net/) boasts that
it can provide what native language properties aim at today, without any changes to the language.
Provided here is a response to their list of
[10 Things that are considerably harder/impossible to achieve with get/set methods or the property keyword](https://bean-properties.dev.java.net/10things.html).

The content of the 10 Things document appears annotated below.
The version displayed here was retrieved from bean-properties on October 25, 2007.

### 1. Proper Declerative Programming/User Defined Annotations

> The samples of the bean properties highlight the use of annotations quite a
> bit, however the annotations are only used during the first time a bean is
> bound. After that all of the data is stored with no need for reflection in the
> bean context/property context. The cool aspect if that 3rd party can define
> their own annotations for their own uses such as DB related annotations and
> plug them in by overriding the default bean container.

Native properties do this better with a new type of annotation target defined for properties.

### 2. Generic Typesafe Code On Properties

> The ability to invoke myMethod(myBean.myProperty); allows you to pass a bean
> property to a generic method that doesn't need to know much about said
> property. A method that accepts a color might not care if you will later use
> said color as foreground or background.<br/>Probably the best example of this
> is the addListener method that now accepts a
> pointer to the property e.g. addListener(property, listener); Rather than the
> previous implementation that had
> addProppertyChangeListner("propertyName", listener); Notice that the
> propertyName is neither checked by the compiler nor properly documented.

Native properties achieve this with a property literal syntax.

### 3. Don't Write A Listener Ever Again/4 Listener Scopes

> In order to implement observability, probably the most common bean feature
> of all you need to implement at least 4 methods (2 for add and 2 for remove)
> and write significant amounts of code in order to fire events in every set
> method. A pain to say the least&#8230; You get that pretty much for free in the new
> bean properties and as an added bonus you can observe property changes in 2
> additional scopes (property context and bean context) which allow whole new use
> cases.

With native properties a bound property can be defined optionally to generate the same code
without all the bloat associated with a property object, allowing for more
efficient code.

### 4. Typesafe Binding Without Bytecode Generation

> Binding to anything be it a database or a GUI is remarkably hard, this is
> made harder still by invoking get/set methods dynamically in order to
> extract/push the data within said property. The current implementations of both
> the GUI binding API and the database binding API require no bytecode
> manipulation whatsoever and most of the code relies on plain and simple
> polymorphism. Yes there is an invocation of Field.get() there but when compared
> to the scale of reflection or generated bytecode a typical implementation of
> these features would require&#8230; The difference is quite bit.

Native properties give the polymorphism with a property literal syntax as well.

### 5. UI Factories/Property Sequence & I18n

> <a href="/web/20071224192747/https://bean-properties.dev.java.net/tutorial3.html">Automatically
> generating a UI</a> from a pure data bean has been done before and failed,
> there are many reasons for that failure and all of them have been solved by the
> bean properties. UI factories become not only possible to develop but also
> remarkably easy to develop and localize thanks to the design of the bean info.

UI factories are possible with native properties as well.

### 6. Virtual Properties

> Virtual properties allow us to dynamically add a property to a preexisting
> bean without modifying its code. The property would not be visible statically
> (e.g. bean.virtual won't compile) but it will be visible dynamically in the
> many bean context methods that allow detecting properties (e.g.
> bean.getContext().getPropertiesArray() would return virtual properties as
> well).<br/>This allows use cases that were previously impossible such as database primary
> keys that have no business purpose don't need to be defined in the bean, they
> can be dynamically added for the DB mapping layer only. It allows custom fields
> defined at runtime to have the right name/attributes and Java type!

This is
possible today with custom BeanInfo as well. This pattern can be implemented
independently of beans-info on top of the existing Java framework and on top of
native properties.

### 7. No More BeanInfo

> Few people ever bother to write bean info objects since they are such a
> pain, they are limited and annoying. They can't be extended and generalized
> like proper objects and its very hard to add additional meta data&#8230; No wonder
> their meta data looks like an early 90's API (monochrome 16x16 icon?). The lack
> of ability to specify anything elaborate not to mention localize the
> information has eliminated the very idea of meta-data associated with beans!!!<br/>What are beans if not coarse grained objects with additional meta-data? We
> ended up dumbing down our beans and tools because we just didn't want to deal
> with this terrible construct. No more, meta-data can be defined easily and
> mostly checked by the compiler where applicable. It exists in runtime and can
> easily be extended by 3rd parties to contain additional information.

All of this is possible and doable more efficiently with first-class properties.

### 8. Context Objects/Meta Data State

> The state loaded by bean info is only half the story, comparison of beans
> and actions on a bean sometimes need to apply a feature to a scope of the whole
> bean/property rather than a single object instance. Since every bean and every
> property has a single global context object associated with it using this
> object as part of your API becomes trivial. E.g. bindToGui(PropertyContext p);
> would work as bindToGUI(bean.property.getContext()); Notice that this is
> checked by the compiler and can be used to apply functionality to the given
> property on all of the instances of the bean.

Creating centralized references to all beans is dangerous and leads to potential memory
leak issues, unnecessary overhead and other problems. Centralized bean management
and organization should be a decision made at the project level.

### 9. User Definable Objects/Property Code Reuse

> Building your own implementation of Property is remarkably easy! This is far
> more powerful than implementing a get/set method since this code can be
> refactored and reused as much as you want. Several examples of such reusable
> properties exist in the code base, an SQLProperty was an interesting experiment
> while the LegacyDelegateProperty seems like a remarkably useful tool in order
> to expose functionality of an underlying legacy bean. The ability to define
> your own reusable properties is what differentiates this framework from
> getter/setters.

Reusable property code can be achieved by using delegation or in more complex cases AOP
with annotations.

### 10. The Bean Container

> EJB's and lightweight IoC frameworks have benefited from central management
> for years and now a simplified version of these ideas is within reach. Since
> all beans pass through a central point a great deal of the code can be
> generalized. Event handling is probably the simplest and most powerful allowing
> the greatest performance benefits from central management. However the
> flexibility is only limited by your imagination, the container can be
> subclassed and replaced (without changing a single line of bean code including
> BeanContainer.bind(this) calls!). This allows you to tailor in custom logic for
> every bean binding which is an excellent point for runtime bytecode
> instrumentation if someone desires or for custom bean initialization code&#8230;
> Common methods such as equals, toString etc. and some special methods such as
> toMap have generic reasonably efficient implementations within the container
> allowing for a great deal of reuse.

This is outside the scope of comparing properties as a
language feature to property objects. This is a framework concern.
Bean-properties as a lightweight framework can be a perfectly liable choice for
a project, whether sitting on top of the current Java or on top of a Java with
native properties. Bean-properties is not liable as an addition to the core
Java framework.



## Comments

None
