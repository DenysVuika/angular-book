## Interfaces

An _interface_ is a description of the actions that an object can do.

You might already be familiar with _interfaces_ in other programming languages like C# and Java,
or _contracts_ in Swift.

Interfaces are not part of the ECMAScript.
It is a level of abstraction supported by TypeScript to improve the type-checking process, and not converted to JavaScript code.

Here's an example of an interface describing generic **Text** component:

```ts
interface TextComponent {

    text: string;
    render(): void;

}
```

Now you can use the interface above to describe the requirement of having the **text** property that is a string and a **render** method:

```ts
class PlainTextComponent implements TextComponent {

    text: string;

    render() {
        console.log('rendering plain text component');
    }

}
```

We are using `implements` keyword to wire class with a particular interface.
It is not important in what order class members are defined as long as all properties and methods the interface requires
are present and have required types.

Let's create another class that implements `TextComponent` interface partially:

```ts
class RichTextComponent implements TextComponent {
    text: string;
}
```

Upon compilation TypeScript will produce the following error:

```text
error TS2420: Class 'RichTextComponent' incorrectly implements interface 'TextComponent'.
Property 'render' is missing in type 'RichTextComponent'.
```

You can use multiple interfaces delimited by a comma:

```ts
class RichTextComponent implements TextComponent, OnInit, OnDestroy {
    // ...
}
```

The example above shows a class that must implement three different interfaces to compile.
