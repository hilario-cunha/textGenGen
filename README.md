# TextGenGen

TextGenGen is an MPS plugin for generation of TextGen definitions derived from Editor definitions,
so that the text generated by TextGen is the same as the one rendered on the screen by the Editor.

## Getting started

The plugin is distributed via the JetBrains MPS Marketplace. It is recommended to install the plugin
from there.

However, if you want to make any modification to the plugin, you could find useful these guides:

**To build the plugin:**
- Open the *TextGenGen* project in MPS
- Expand the `TextGeGEn.build` item in the project explorer
- Right-click the `TextGenGen` item with a spider icon
- Trigger `Run TextGenGen`
- The plugin ZIP file is located in the directory hierarchy of the project (`build/artifacts/TextGenGen`)

**To install the built plugin:**
- Open MPS
- Go to **File** > **Settings** > **Plugins** > **Install plugin from disk...** and locate the ZIP archive
- After MPS restart, two options should appear in the context menu after right clicking on any module:
    - *Save as text* - Exports a textual representation of all nodes in a module as they are rendered on the screen
    - *Generate TextGen* - Generates TextGen definitions from Editor definitions

## Release log

Changes brought by **version 1.1**, 11/2019:
- Fix spacing between elements of a ref-node list
- Add support for editor cells representing AST references
- Implementation of enumeration support
- Refactoring of deprecated parts

Major changes brought by **version 1.0**, 9/2019:
- Generation of TextGen removes the previously generated TextGen and does not ask for a prefix anymore
- Newly supported aspects of the Editor (so that the generated TextGen respects them):
    - *Show if* at an Editor cell
    - Editor components (the `#alias#` component too)
    - *Empty cell models* at a child reference list and/or a child reference
- Improved implementation of indentation and spacing generation
- Refactoring: redesign of the plugin core to a more pure form, documentation of the code

**Version 0.4**
- Support of simple Editor constructs
- The design is basically prepared to be extended by support of more complex Editor constructs

## Development notes

### Design overview

#### Plugin Core

The entry-point object of the plugin core is *EditorToTextGenConvertor*, which
provides generation of TextGen for a given Editor. This object uses the
*EditorCellToTextGenConvertor* object to generate TextGen chunks corresponding to
individual Editor cells. This object uses the Gang-of-Four Builder pattern:
it delegates generation of smaller, general chunks of the TextGen to the
*TextGenBuilder* object.

*TextGenBuilder* is intended to be Editor-independent so it should not
contain any knowledge of the Editor structure and should understand
the TextGen structure only.

The *utils* virtual package contains some helper classes:
- *CustomStyleContainer*: A wrapper for styling from Editor cells
- *Value*, *Constant*, *Query*: Wrappers for (run-time evaluated) conditions
- *NameProvider*: A generator of unique identifiers
- *StatementsBuffer*: A holder for TextGen statements which executes some
basic optimizations of the inserted statements
- *Utils*: General utility methods
