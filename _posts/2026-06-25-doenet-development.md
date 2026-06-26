---
title: Getting Started with Doenet Development
tags: doenet development git github
authors: 
- clontz
comments: true
---

So much of the open-source MathEd-tech space wouldn't exist without
faculty who learned just enough about software engineering to create
the next big app. One of the most exciting apps in this space (though
disclosure: I'm a consultant on their recent NSF award) is the
[Doenet project](https://beta.doenet.org) (which we've talked about
[before](/tags/#doenet)).

Doenet can do a lot, especially for freshman-level courses,
but there's still so much left to do (e.g. as of writing,
I'm craving the ability to find the RREF of a matrix to write
activities for my linear algebra courses).

So my friend [Melissa Lynn][0] and I put together a quick workshop today on
how to get started as a contributor to the
[Doenet codebase on GitHub](https://github.com/Doenet/DoenetML)
using just your web browser (no installs!). The recording is included
below, and Melissa's notes are copied below as well.

[0]: https://wp.stolaf.edu/mscs/mscs-faculty-staff-listing/#:~:text=and%20Computer%20Science-,Melissa%20Lynn,-Assistant%20Professor%20of

<iframe width="420" height="315"
src="https://www.youtube.com/embed/zUsyBq_wUvU">
</iframe>

If you have questions, come join a [drop-in Zoom call](/events/)
to connect with a community member! We're always excited to get more
people involved with all aspects of the project.

---

### Melissa's notes

Assume have codespace setup for [DoenetML](https://github.com/Doenet/DoenetML)

*   [DoenetApps](https://github.com/Doenet/DoenetApps): AWS, website, users, logging in, assignments
    
*   [DoenetML](https://github.com/Doenet/DoenetML): editor, formatter, converter, components
    

Testing using dev server ([Development section of Quickstart](https://github.com/Doenet/DoenetML)):

*   In terminal,
    
    *   `npm run build`
        
    *   `npm run dev`
        
*   To change starting content in editor, edit file `/packages/doenetml/dev/testCode.doenet`
    

Components:

*   [https://github.com/Eyuelwoldeh/DoenetML-documentation](https://github.com/Eyuelwoldeh/DoenetML-documentation)
    
*   Component definitions:
    
    *   `/DoenetML/packages/doenetml-worker-javascript/src/components`
        
*   How components render:
    
    *   `/DoenetML/packages/doenetml/src/Viewer/renderers` 
        
*   Register components (one line to say that component exists):
    
    *   `/DoenetML/packages/doenetml-worker-javascript/src/ComponentTypes.js`
        

Basic concepts:

*   Interactivity is facilitated by a directed acyclic graph of dependencies between values connected to different components. For example,
    ```xml
    <mathInput name="x"/>
    <math name="y">$x</math>
    ```
    The value of the math component named “y” depends on the value of the component mathInput named “x”. When x is updated, the dependencies in the directed acyclic graph signal that the value of y will also need to be updated.

*   **Attribute**: attributes allow you to change the behavior of a component, for example:
    ```xml
    <point draggable="false"/>
    ```
    draggable is an attribute of the point component, that controls whether the user can drag the point or not. 

*   **State variable**: state variables are stored values associated with the component. If a state variable is public, you can access its value, for example:
    ```xml
    <point name="P">(5,2)</point>
    $P.x
    ```
    `x` is a state variable of the point, giving its x-component. When state variables are not public, they might be used to render the component or to otherwise maintain its state.

*   Often, an attribute will have a corresponding state variable, and there can be significant overlap between the attributes and state variables (which can be confusing). Think of an attribute as being an input to the component, a state variable is a stored value, and a public state value is an output.
    
*   When defining a state variable, **returnDependencies** is used to define the dependencies needed to build the directed acyclic graph.
    
*   Nodes in the DAG are state variables (not components).
    
*   **inverseDefinition** is only used when there’s user interaction that requires “backwards” updates, for a simple example:
    ```xml
    <text name="t"/>
    <textInput>$t</textInput>  
    ```
    (updates to text changing the value of textInput would use the forward definition, while updates to the textInput use the inverse definition)

*   The forward definitions always work, but the inverse definition may not (in some situations, it may be hard to decide how to invert - for example, if the textInput had references to multiple text components, `$t $s`, which one to update?)
    
*   Use a desired value because the inverse definition is passing a suggestion, but it might not be possible. For example, a constraint might prevent the desired value from being valid.
