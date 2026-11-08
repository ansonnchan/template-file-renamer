# Template File Renamer

> Status: Early planning

A destination-first, folder-aware tool for moving and consistently renaming files. It is intended to be manually triggered: choose a file, choose where it belongs, review the proposed name, and approve the change.

## Motivation

A filename only needs to be unique inside its destination folder. The same name may be perfectly valid elsewhere, and different folders often need different naming conventions. This tool therefore chooses the destination before generating or validating a filename.

## Proposed workflow

1. Trigger the tool on a file.
2. Choose a destination using a fuzzy picker with recent and favorite folders.
3. Load the naming template associated with that folder.
4. Infer available fields from the file and destination context.
5. Preview the destination and proposed filename.
6. Approve the move, adjust the fields, or cancel.
7. Record the operation so it can be undone.

## Planned features

### Destination-first naming

Names are proposed for a specific destination rather than in isolation. Collision checks only inspect the selected destination folder.

### Per-folder templates

Folders can define their own naming rules, for example:

| Folder | Example template |
| --- | --- |
| Resumes | `{company}_{role}_{date}` |
| Assignments | `A{number}_{topic}` |
| Lecture notes | `{course}_{unit}_{topic}` |

### Context-aware fields

The tool may infer values such as:

- The current date
- A course code from the parent folder
- The next assignment or document number
- Basic metadata extracted from the file

Every inferred value remains editable before the file is moved.

### Safe collision handling

When the proposed name already exists in the destination, the tool can suggest a suffix such as `-2` or request another name. It should compare file contents first and warn when an identical file already exists instead of creating a duplicate.

### Preview and undo

Every operation shows the original and proposed locations before it runs. Completed operations are logged so recent moves and renames can be reversed.

## Design principles

- **Manual by default:** The user initiates and approves each operation.
- **Folder aware:** Naming rules belong to destinations.
- **Predictable:** Explicit templates and visible inferred values take priority over opaque automation.
- **Non-destructive:** Existing files are never silently overwritten.
- **Reversible:** Moves and renames can be undone.
- **Local first:** Files and configuration should not require a hosted service.

## Initial non-goals

- Continuously watching or automatically organizing the Downloads folder
- Silently moving files without confirmation
- Replacing a general-purpose file manager

An automatic download classifier could eventually call this tool after determining a destination, but classification is outside the initial scope.

## Open design questions

- Finder Quick Action, menu-bar app, command-line tool, or a combination
- Configuration format for folder templates
- How much metadata extraction belongs in the first version
- macOS-only versus cross-platform support
- Whether content-aware suggestions should be optional
