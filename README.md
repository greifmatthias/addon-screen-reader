# Storybook Screen Reader Addon

[![NPM Version](https://img.shields.io/npm/v/storybook-screen-reader.svg)](https://www.npmjs.com/package/storybook-screen-reader)

A screen reader simulation addon for Storybook that helps developers test accessibility during component development. It uses industry-standard libraries to provide accurate accessible name computation and role announcements.

![Screen Reader Demo](screenshots/screen-reader-example.gif)

## Features

- 🎯 **W3C AccName Spec** - Uses `dom-accessibility-api` for accurate accessible name computation
- 🏷️ **ARIA Role Detection** - Uses `aria-query` for proper role mapping
- 🔊 **Voice Reader** - Uses Web Speech API to announce focused elements
- 📝 **Text Reader** - Displays announcements in the addon panel
- 🎯 **Focus Tracking** - Automatically tracks focus changes (Tab, arrow keys, clicks)
- 📢 **Live Regions** - Announces `aria-live` region updates
- 🌐 **Deep Shadow DOM Support** - Traverses into shadow roots to find the actual focused element (like real screen readers)
- 🎨 **Spectrum Web Components UI** - Built with Lit + SWC for the addon panel
- 🔄 **Story Navigation** - Automatically resets when switching between stories

---

## Comparison: This Plugin vs macOS VoiceOver

| Feature | This Plugin | VoiceOver (macOS) | Notes |
|---------|:-----------:|:-----------------:|-------|
| **Accessible Name Computation** | ✅ W3C AccName spec | ✅ Native | We use `dom-accessibility-api` which implements the full W3C spec |
| **Accessible Description** | ✅ `aria-describedby` | ✅ Native | Full support via `dom-accessibility-api` |
| **ARIA Roles** | ✅ 40+ roles | ✅ All roles | We use `aria-query` for accurate role detection |
| **Focus Tracking** | ✅ `focusin` event | ✅ Native | Tracks Tab, Shift+Tab, clicks |
| **Shadow DOM Traversal** | ✅ Deep active element | ✅ Native | Finds actual focused element inside shadow roots |
| **Arrow Key Navigation** | ✅ `aria-activedescendant` | ✅ Native | For menus, listboxes, trees |
| **Live Regions** | ✅ `aria-live` | ✅ Native | Announces dynamic content changes |
| **Form States** | ✅ required, invalid, readonly | ✅ Native | Comprehensive form feedback |
| **Button States** | ✅ pressed, expanded | ✅ Native | Toggle buttons, accordions |
| **List Position** | ✅ "Item X of Y" | ✅ Native | Lists, tabs, options |
| **Table Navigation** | ⚠️ Basic | ✅ Full | We announce row/column counts |
| **Checkbox States** | ✅ checked, mixed, unchecked | ✅ Native | Tri-state support |
| **Slider Values** | ✅ value, min, max, valuetext | ✅ Native | Full range announcements |
| **Virtual Buffer Mode** | ❌ Not supported | ✅ Native | NVDA/JAWS feature, not VoiceOver |
| **Reading All Content** | ❌ Focus-only | ✅ Browse mode | We only announce focused elements |
| **OS Integration** | ❌ Browser only | ✅ System-wide | VoiceOver works across all apps |
| **Braille Output** | ❌ Not supported | ✅ Native | Hardware requirement |

### What We Implement

| Category | Supported Features |
|----------|-------------------|
| **Roles** | button, link, checkbox, radio, switch, textbox, searchbox, combobox, listbox, slider, spinbutton, heading, img, listitem, option, menuitem, tab, tabpanel, navigation, main, banner, contentinfo, complementary, region, article, form, search, dialog, alertdialog, alert, status, log, progressbar, meter, tooltip, tree, treeitem, grid, table, menu, menubar, toolbar, group, separator |
| **States** | checked, pressed, expanded, selected, invalid, required, readonly, disabled |
| **Properties** | aria-label, aria-labelledby, aria-describedby, aria-valuenow, aria-valuemin, aria-valuemax, aria-valuetext, aria-level, aria-activedescendant, aria-modal |
| **Live Regions** | aria-live (polite/assertive), role="alert", role="status", role="log" |

---

## Accuracy & Trust Level

| Use Case | Reliability | Recommendation |
|----------|:-----------:|----------------|
| Quick dev feedback | ✅ **High** | Great for catching obvious issues |
| ARIA role verification | ✅ **High** | Accurate role mapping via `aria-query` |
| Accessible name testing | ✅ **High** | W3C spec via `dom-accessibility-api` |
| Focus order testing | ✅ **High** | Reliable focus tracking |
| Live region testing | ✅ **Medium** | Basic support, may miss edge cases |
| Full WCAG compliance | ⚠️ **Medium** | Use with real screen readers |
| Production sign-off | ❌ **Low** | Always test with VoiceOver/NVDA |

### Recommended Testing Workflow

```
1. 🛠️  Development    →  Use this plugin (instant feedback)
2. 🔍  Code Review    →  Use axe-core / Storybook a11y addon  
3. ✅  QA Testing     →  Use REAL VoiceOver / NVDA
4. 👥  User Testing   →  Involve actual screen reader users
```

---

## Installation

```bash
npm install storybook-screen-reader
```

or

```bash
yarn add storybook-screen-reader
```

## Setup

Add the addon to your `.storybook/main.ts`:

```ts
import type { StorybookConfig } from 'storybook/internal/types';

const config: StorybookConfig = {
  addons: [
    // ... other addons
    'storybook-screen-reader',
  ],
};

export default config;
```

## Usage

1. Open Storybook and navigate to any story
2. Click the **"Screen Reader"** tab in the addons panel
3. Enable **Voice Reader** and/or **Text Reader**
4. Navigate through your component:
   - **Tab** / **Shift+Tab** - Move between focusable elements
   - **Arrow keys** - Navigate within menus, listboxes, etc.
   - **Click** - Focus any element

The addon will announce each focused element with its role, name, and state.

## Announcement Examples

| Element | Announcement |
|---------|-------------|
| Button | "Button, Submit. Press Space or Enter to activate." |
| Toggle Button | "Button, Dark mode, pressed. Press Space or Enter to activate." |
| Link | "Link, Learn more. Press Enter to follow." |
| Checkbox | "Checkbox, Accept terms, not checked. Press Space to toggle." |
| Radio | "Radio button, Option A, selected." |
| Switch | "Switch, Notifications, on. Press Space to toggle." |
| Text Field | "Text field, Email, required. Empty." |
| Invalid Field | "Text field, Password, required, invalid entry. Contains: ****" |
| Slider | "Slider, Volume. Value: 75. Range: 0 to 100." |
| Heading | "Heading level 2, Welcome" |
| Tab | "Tab, Settings, selected. 2 of 4." |
| List Item | "Home. List item 1 of 5." |
| Menu Item | "Menu item, Copy" |
| Dialog | "Modal Dialog, Confirm deletion" |
| Alert | "Alert: Your session will expire in 5 minutes" |
| Progress | "Progress bar, Uploading. 45 percent" |
| Table | "Table, User data. 10 rows, 4 columns." |

---

## Technical Details

### Libraries Used

| Library | Purpose |
|---------|---------|
| `dom-accessibility-api` | W3C Accessible Name and Description computation |
| `aria-query` | ARIA role definitions and element-to-role mapping |
| `lit` | Web component framework for addon panel |
| `@spectrum-web-components` | Adobe Spectrum design system components |

### Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Storybook Manager                         │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  screen-reader-panel (Lit + SWC)                     │   │
│  │  └── Toggle switches for voice/text output          │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Storybook Preview (iframe)                │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  ScreenReader class                                  │   │
│  │  ├── Focus tracking (focusin event)                 │   │
│  │  ├── ARIA attribute monitoring (MutationObserver)   │   │
│  │  ├── Live region monitoring (MutationObserver)      │   │
│  │  ├── Name computation (dom-accessibility-api)       │   │
│  │  ├── Role detection (aria-query)                    │   │
│  │  └── Speech synthesis (Web Speech API)              │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

---

## Limitations

This addon is a **development aid**, not a replacement for testing with real screen readers:

| What We Can't Do | Why |
|------------------|-----|
| Access OS Accessibility Tree | Browser security restrictions |
| Virtual/Browse mode | VoiceOver reads all content, we only track focus |
| Braille output | Hardware requirement |
| System-wide announcements | Browser sandbox |
| 100% VoiceOver parity | Different underlying technology |

**Always test with actual assistive technology before shipping:**
- VoiceOver (macOS/iOS)
- NVDA (Windows, free)
- JAWS (Windows)
- TalkBack (Android)

---

## Compatibility

- Storybook 10.x
- React, Vue, Angular, Web Components, HTML
- Works with Shadow DOM

> **Note:** Version 2.x requires Storybook 10. For Storybook 8.x, use version 1.x.

## Contributing

Issues and PRs welcome! [GitHub Repository](https://github.com/TarunAdobe/addon-screen-reader)

## Credits

Originally created by [Víctor Lara](https://github.com/vlaraort/addon-screen-reader). 
Updated and maintained by [Tarun Tomar](https://github.com/TarunAdobe).

## License

MIT
