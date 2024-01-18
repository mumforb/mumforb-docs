# Syntax

NestJS comes with a variety of syntactical sweeteners that were unusual at first glance. This is a partial inventory.

## Exclamation marks in Schemas/Entities

Used to correct a typescript error: `Property 'item' has no initializer and is not definitely assigned in the constructor.`  It comes into play when the Typescript `strictPropertyIntialization` flag is marked as true. This means that all properties "should" be initialized by the constructor. A quick workaround is to add the `!` postfix (e.g. `item!`), which asserts that this class property doesn't need to be explicitly initialized by a constructor.