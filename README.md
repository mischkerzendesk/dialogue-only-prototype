# Dialogue-Only Use Cases Prototype

An interactive prototype demonstrating the "Dialogue only" scoping feature for AI agent use cases.

## Overview

This prototype showcases a UX pattern for preventing "dead end" scenarios when context-dependent intents (like "Affirmative" and "Negative") are matched outside of their intended dialogue flows.

## The Problem

When use cases like "Affirmative" (yes) or "Negative" (no) are active globally:

```
Customer: "Yes"
AI Agent: [Matches "Affirmative" use case]
AI Agent: [No reply configured → dead end]
```

The bot doesn't know what the customer is saying "yes" to because the context only exists within a dialogue flow.

## The Solution

**Dialogue-only scoping** restricts certain use cases to only trigger inside dialogue flows:

```
[Inside a dialogue about product selection]
AI Agent: "Would you like the red or blue version?"
Customer: "Yes, the red one"
AI Agent: [Matches "Affirmative" within dialogue context]
AI Agent: [Continues the dialogue flow properly]
```

## Features Demonstrated

### 1. Warning Banner
System proactively identifies use cases that may need dialogue-only scoping:
- Shows count of flagged use cases
- "Review" button to bulk-apply the setting

### 2. Use Cases Table
- Visual indication of dialogue-only status with tags
- Highlights use cases with 0 replies (potential dead-end risk)
- Shows active/inactive status

### 3. Bulk Review Modal
- Select multiple use cases to mark as dialogue-only
- Shows metadata: reply count and linked dialogues
- Dismiss option for false positives

### 4. Individual Use Case Settings
- Checkbox to enable/disable dialogue-only scoping
- Contextual helper text explaining the behavior
- Warning hint when a use case has no replies and is linked to dialogues

## Use Cases Shown

1. **Affirmative** - Customer confirmations (yes, correct, that's right)
2. **Negative** - Customer rejections (no, wrong, not that)
3. **Choose color** - Selection from presented options

## Running the Prototype

Simply open `index.html` in a web browser. No build process or dependencies required.

```bash
open index.html
```

Or visit the [live demo](https://your-github-username.github.io/dialogue-only-prototype/).

## Interactions

### List View
- Click the warning banner's "Review" button to open the bulk modal
- Click on "Affirmative" or "Negative" rows to view details

### Modal
- Check/uncheck use cases to include in the bulk action
- Click "Apply" to mark selected use cases as dialogue-only
- Click "Dismiss" to hide the warning banner

### Detail View
- Toggle the "Dialogue only" checkbox to scope the individual use case
- Click the back arrow to return to the list

## Design System

The prototype uses a custom design system with:
- **Font:** DM Sans
- **Primary color:** Indigo (accent actions)
- **Status colors:** Green (active), Amber (warnings), Purple (dialogue tags)
- **Layout:** Fixed sidebar + main content area
- **Interactions:** Smooth transitions and animations

## Technical Details

- Pure HTML, CSS, and vanilla JavaScript
- No external dependencies
- Responsive design
- Accessible form controls
- State management for dialogue-only flags

## Related Documentation

For the complete user guide, see:
- [Dialogue-Only Use Cases Guide](./docs/dialogue-only-use-cases-guide.md)

## Version History

- **v1.0** (2026-03-25) - Initial prototype
  - Warning banner/nudge
  - Bulk review modal
  - Individual use case detail view
  - State persistence within session

## License

This prototype is for demonstration purposes.

## Contact

For questions or feedback, please open an issue in this repository.
