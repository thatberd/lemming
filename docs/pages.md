# Pages

## Philosophy

Pages are the primary mechanism for defining the lemming user interface.

A page describes what widgets appear and how they are arranged. It does not describe how they behave or what they look like.

The page language is intentionally not a programming language. It contains:

- no variables
- no loops
- no conditionals
- no functions
- no scripting
- no runtime execution

Its purpose is only to describe UI structure.

## Compilation

Pages are compiled and cached before deployment. The firmware never interprets page source files at runtime.

The compiler validates page definitions before they reach the device.

This means:

- Invalid page definitions are caught at build time, not on the device.
- Page source files do not need to be stored on the SD card.
- The firmware only consumes compiled, validated page structures.

## Separation of Concerns

Applications provide widgets while the page language only describes composition.

| Concern       | Owner          |
| ------------- | -------------- |
| Widget logic  | Firmware (app) |
| Widget look   | Theme          |
| Layout        | Page           |
| Behavior      | Firmware       |

## Home Page

Users are encouraged to build their own home page by composing existing widgets.

The firmware provides widgets. Users arrange them. The firmware provides behavior. The page describes structure.

## Relationship to lmngdsl

lemming uses lmngdsl, a standalone project that provides a generic declarative UI description language.

lmngdsl is not coupled exclusively to lemming. It is intended to be reusable for other embedded or terminal-based interfaces.
