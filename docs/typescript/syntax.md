---
layout: default
title: Syntax
parent: TypeScript
nav_order: 1
---

# Syntax

This Style Guide uses the [terminology convention]('/../../terminology/keywords.md') when using the phrases must, must not, should, should not, and may. All examples given are non-normative and serve only to illustrate the normative language of the style guide.

## Identifiers

Identifiers must use only ASCII letters, digits, underscores (for constants and structured test method names), and the '\(' sign. Thus each valid identifier name is matched by the regular expression `[\)\w]+`.

| Style          | Category                                                           |
| :------------- | :----------------------------------------------------------------- |
| UpperCamelCase | class / interface / type / enum / decorator / type parameters      |
| lowerCamelCase | variable / parameter / function / method / property / module alias |
| CONSTANT_CASE  | global constant values, including enum values                      |

### Abbreviations

Treat abbreviations like acronyms in names as whole words, i.e. use loadHttpUrl, not loadHTTPURL, unless required by a platform name (e.g. XMLHttpRequest).

### Dollar sign

Identifiers should not generally use `$`, except when aligning with naming conventions for third party frameworks. See below for more on using $ with Observable values.

### Type parameters

Type parameters, like in `Array<T>`, may use a single upper case character (`T`) or `UpperCamelCase`.

### prefix/suffix

Identifiers must not use `_` as a prefix or suffix.

#### Tip

If you only need some of the elements from an array (or TypeScript tuple), you can insert extra commas in a destructuring statement to ignore in-between elements:

```ts
const [a, , b] = [1, 5, 10]; // a <- 1, b <- 10
```

### Imports

Module namespace imports are `lowerCamelCase` while files are `kebab-case`, which means that imports correctly will not match in casing style, such as

```ts
import * as fooBar from "./foo-bar";
```

Some libraries might commonly use a namespace import prefix that violates this naming scheme, but overbearingly common open source use makes the violating style more readable. The only libraries that currently fall under this exception are:

- jQuery, using `$` prefix (you shouldn't use jQuery in your work normally)
- threejs, using the `THREE` prefix

### Constants

`CONSTANT_CASE` indicates that a value is intended to not be changed, and may be used for values that can technically be modified (i.e. values that are not deeply frozen) to indicate to users that they must not be modified.

```ts
const UNIT_SUFFIXES = {
  milliseconds: "ms",
  seconds: "s",
};
// Even though per the rules of JavaScript UNIT_SUFFIXES is
// mutable, the uppercase shows users to not modify it.
```

A constant can also be a static readonly property of a class.

```ts
class Foo {
  private static readonly MY_SPECIAL_NUMBER = 5;

  bar() {
    return 2 * Foo.MY_SPECIAL_NUMBER;
  }
}
```

If a value can be instantiated more than once over the lifetime of the program, or if users mutate it in any way, it must use `lowerCamelCase`.

If a value is an arrow function that implements an interface, then it can be declared `lowerCamelCase`.

### Aliases

When creating a local-scope alias of an existing symbol, use the format of the existing identifier. The local alias must match the existing naming and format of the source. For variables use `const` for your local aliases, and for class fields use the `readonly` attribute.

```ts
const { Foo } = SomeType;
const CAPACITY = 5;
****
class Teapot {
  readonly BrewStateEnum = BrewStateEnum;
  readonly CAPACITY = CAPACITY;
}
```

### Naming style

TypeScript expresses information in types, so names should not be decorated with information that is included in the type.

Some concrete examples of this rule:

- Do not use trailing or leading underscores for private properties or methods.
- Do not use the `opt_` prefix for optional parameters.
- Do not mark interfaces specially (`IMyInterface` or `MyFooInterface`) unless it's idiomatic in its environment. When introducing an interface for a class, give it a name that expresses why the interface exists in the first place (e.g. `class TodoItem` and `interface TodoItemStorage` if the interface expresses the format used for storage/serialization in JSON).
- Suffixing `Observables` with `$` is a common external convention and can help resolve confusion regarding observable values vs concrete values. Judgement on whether this is a useful convention is left up to individual teams, but should be consistent within projects.

### Descriptive names

Names must be descriptive and clear to a new reader. Do not use abbreviations that are ambiguous or unfamiliar to readers outside your project, and do not abbreviate by deleting letters within a word.

#### Exceptions:

- Variables that are in scope for 10 lines or fewer, including arguments that are not part of an exported API, may use short (e.g. single letter) variable names. Usefull in a loop for example.
- Variables that are more than 25 characters/columns can contains abbrevited words (e.g. `organizationsDatabaseResponse` -> `organizationsDbResponse` or `orgDatabaseResponse`)

## File encoding: UTF-8

For non-ASCII characters, use the actual Unicode character (e.g. `∞`). For non-printable characters, the equivalent hex or Unicode escapes (e.g. `\u221e`) can be used along with an explanatory comment.

```ts
// Perfectly clear, even without a comment.
const units = "μs";

// Use escapes for non-printable characters.
const output = "\ufeff" + content; // byte order mark
```

NOT
{: .text-red-200 }

```ts
// Hard to read and prone to mistakes, even with the comment.
const units = "\u03bcs"; // Greek letter mu, 's'

// The reader has no idea what this is.
const output = "\ufeff" + content;
```

## Comments & Documentation

### JSDoc vs Comments

There are two types of comments, JSDoc (`\** ... */`) and non-JSDoc ordinary comments (`// ...` or `/* ... */`).

- Use `\** JSDoc */` comments for documentation, i.e. comments a user of the code should read.
- Use `// line comments` for implementation comments, i.e. comments that only concern the implementation of the code itself.

JSDoc comments are understood by tools (such as editors and documentation generators), while ordinary comments are only for other humans.

### Document all top-level exports of modules

Use `/** JSDoc */` comments to communicate information to the users of your code. Avoid merely restating the property or parameter name. You should also document all properties and methods (exported/public or not) whose purpose is not immediately obvious from their name, as judged by your reviewer.

#### Exception

Symbols that are only exported to be consumed by tooling, such as @NgModule classes, do not require comments.

### Omit comments that are redundant with TypeScript

For example, do not declare types in @param or @return blocks, do not write @implements, @enum, @private etc. on code that uses the implements, enum, private etc. keywords.

### Do not use `@override`

Do not use `@override` in TypeScript source code.

`@override` is not enforced by the compiler, which is surprising and leads to annotations and implementation going out of sync. Including it purely for documentation purposes is confusing.

### Make comments that actually add information

For non-exported symbols, sometimes the name and type of the function or parameter is enough. Code will _usually_ benefit from more documentation than just variable names though!

- Avoid comments that just restate the parameter name and type, e.g.
  ```ts
  /** @param fooBarService The Bar service for the Foo application. */
  ```
- Because of this rule, `@param` and `@return` lines are only required when they add information, and may otherwise be omitted.
  ```ts
  /**
   * POSTs the request to start coffee brewing.
   * @param amountLitres The amount to brew. Must fit the pot size!
   */
  brew(amountLitres: number, logger: Logger) {
  // ...
  }
  ```

### Parameter property comments

A parameter property is when a class declares a field and a constructor parameter in a single declaration, by marking a parameter in the constructor. E.g. `constructor(private readonly foo: Foo)`, declares that the class has a `foo` field.

To document these fields, use JSDoc's `@param` annotation. Editors display the description on constructor calls and property accesses.

```ts
/** This class demonstrates how parameter properties are documented. */
class ParamProps {
  /**
   * @param percolator The percolator used for brewing.
   * @param beans The beans to brew.
   */
  constructor(private readonly percolator: Percolator, private readonly beans: CoffeeBean[]) {}
}
```

```ts
/** This class demonstrates how ordinary fields are documented. */
class OrdinaryClass {
  /** The bean that will be used in the next call to brew(). */
  nextBean: CoffeeBean;

  constructor(initialBean: CoffeeBean) {
    this.nextBean = initialBean;
  }
}
```

### Comments when calling a function

If needed, document parameters at call sites inline using block comments. Also consider named parameters using object literals and destructuring. The exact formatting and placement of the comment is not prescribed.

```ts
// Inline block comments for parameters that'd be hard to understand:
new Percolator().brew(/* amountLitres= */ 5);
// Also consider using named arguments and destructuring parameters (in brew's declaration):
new Percolator().brew({ amountLitres: 5 });
```

```ts
/** An ancient {@link CoffeeBrewer} */
export class Percolator implements CoffeeBrewer {
  /**
   * Brews coffee.
   * @param amountLitres The amount to brew. Must fit the pot size!
   */
  brew(amountLitres: number) {
    // This implementation creates terrible coffee, but whatever.
    // TODO #123: Improve percolator brewing.
  }
}
```

### Place documentation prior to decorators

When a class, method, or property have both decorators like `@Component` and JsDoc, please make sure to write the JsDoc before the decorator.

- **Do not write** JsDoc between the Decorator and the decorated statement.
  ```ts
  @Component({
    selector: "foo",
    template: "bar",
  })
  /** Component that prints "bar". */
  export class FooComponent {}
  ```
- Write the JsDoc block before the Decorator.
  ```ts
  /** Component that prints "bar". */
  @Component({
    selector: "foo",
    template: "bar",
  })
  export class FooComponent {}
  ```

