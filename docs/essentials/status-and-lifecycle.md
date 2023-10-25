---
title: Status and Lifecycle
sidebar_position: 9
---

# Status and Lifecycle Management in DevCycle

In DevCycle, Features are associated with **Statuses** to indicate their current position in the Development LifeCycle. The statuses are a succinct way to understand a Feature's state, and each status has its own unique properties.

## Statuses

Features in DevCycle can exist in one of three Statuses to indicate their current LifeCycle stage:

- **In Progress**
- **Completed**
- **Archived**

Each status comes with its unique properties, affecting how a Feature behaves, can be interacted with, or is displayed in the dashboard.

**Status changes are not automatic and are maintained by the user.**

### In Progress

When a Feature is created, it starts in this status. While a Feature is "In Progress," you can modify everything, have as many variations as possible, and have complex targeting rules.

### Completed

One could consider Feature "Completed" once it has been tested, approved, and is ready for release or has been fully released. When the user changes to the Complete status the Feature enters a semi-readonly state, limiting some editability. All users will be served a single variation. 

### Archived

The "Archived" status is designed to clean up the dashboard and the codebase by essentially putting the Feature into a read-only mode and hiding it from the standard dashboard views. It can still be retrieved from archived status and reverted to In Progress.

## LifeCycle

### Completing a Feature

When a Feature is marked as "Completed," the following changes are implemented:

- Status changes to "Complete."
- A 'Release Variation' must be chosen which will be served to all users.
- Past environment statuses are preserved.
- Variables section will display only one single variation.
- Variable values can still be modified, and environments can be toggled on and off.

#### Cleanup Steps/Checklists for Variables

Upon completing a Feature, you will see cleanup steps/checklists for each Variable. You can choose to "Keep" or "Cleanup" a Variable.

- **Keep**: Marks the Variable as permanent, which implies that DevCycle will manage the value of this Variable until otherwise specified.
  
- **Cleanup**: Provides a checklist to help you know when it's safe to remove the Variable from the Feature.

### Reverting to In Progress

The "Completed" status is reversible by clicking "Revert to In Progress."

- Previous variations return, but any new changes made during the "Completed" state remain.
- Past targeting rules will not be restored.

### Archiving a Feature

When a Feature is archived:

- All Variables are detached.
- The Feature is put into a read-only mode.
- Status changes to "Archived."
- All environments for the Feature are turned off.

#### Warning and User Interactions

If Variables are seen in the code or evaluations are still incoming, a warning will be displayed in the archive modal. You can opt to archive Variables along with the Feature.

#### Restoring a Feature

You have the option to restore an archived Feature. You'll be asked if you want to reassociate past Variables when moving back to "In Progress."

By understanding and utilizing these statuses and lifecycle functionalities, teams can maintain a clean, organized codebase and dashboard, making Feature management in DevCycle efficient and effective.