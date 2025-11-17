# NTiOS Shortcut: Quick Note Capture

This shortcut captures a note from anywhere on iOS and appends it to a
central "NTiOS Inbox" note inside the **Notes** app. It also supports
optional tagging and voice dictation so you can trigger it hands-free
through Siri.

## Prerequisites

- iPhone or iPad running iOS / iPadOS 16 or later.
- Apple Notes with an existing note titled `NTiOS Inbox` (create it in
your preferred folder beforehand).
- Shortcuts app installed (it ships with iOS).

## Build the Shortcut

Follow these steps in the Shortcuts app. The numbers correspond to the
order of actions in the shortcut editor.

1. **Get Current Date**  
   No configuration needed. This action timestamps each captured note.

2. **Format Date**  
   - Date: `Provided Input` (from step 1).  
   - Format: `Custom`.  
   - Date Format String: `yyyy-MM-dd HH:mm`.  
   This yields a concise timestamp such as `2024-06-06 18:42`.

3. **Ask for Input**  
   - Prompt: `What should I capture?`.  
   - Default Answer: leave blank.  
   - Input Type: `Text`.  
   This displays a text box (or dictation prompt if you invoke via Siri)
   to capture the note body.

4. **Choose from Menu** (optional tagging)  
   - Prompt: `Tag this note?`.  
   - Add menu items such as `Work`, `Personal`, `Idea`, and `No Tag`.  
   - For each non-"No Tag" option, add a **Text** action containing the
     tag (e.g., `#work`).  
   - For "No Tag", add a **Text** action with an empty value.  
   The chosen branch should always end with a **Text** action so the
   downstream steps receive a tag string.

5. **Text** (assemble the final payload)  
   Configure the text block with the following template:
   ```
   [[Formatted Date]] [[Tag]]
   [[Asked Text]]
   ---
   ```
   - Insert `Formatted Date` from step 2.  
   - Insert the tag from step 4 (or leave blank if you skipped the menu).  
   - Insert the text input from step 3.  
   The horizontal rule (`---`) visually separates each entry in the
   target note.

6. **Append to Note**  
   - Note: `NTiOS Inbox`.  
   - Account: choose the Notes account/folder that holds the inbox note.  
   This action adds the formatted text to the end of your inbox note.

7. **Show Result** (optional confirmation)  
   Display the text that was appended so you can confirm the capture when
   running the shortcut from the Shortcuts app.

## Enhancements

You can extend the base shortcut in a few useful ways:

- **Hands-free capture**: In the shortcut settings, enable "Show in
  Share Sheet" and "Use as Quick Action". Then assign a Siri phrase such
  as “Capture note.” When triggered through Siri, the `Ask for Input`
  action switches to voice dictation.
- **Sync to other services**: After the **Append to Note** action, you
  can add integrations such as "Save File" (to Dropbox) or "Add Row to
  Numbers" depending on your workflow.
- **Markdown formatting**: If you prefer Markdown-style headings, update
  the text template with `##` or `-` list prefixes.
- **Daily journal split**: Replace the single `Append to Note` action
  with "Append to File" targeting a daily `.txt` file in iCloud Drive.

## Import Checklist

Before sharing or importing the shortcut to another device, tap the
shortcut menu (•••) → **Details** and verify:

- "Allow Running While Locked" is enabled if you plan to trigger it from
  the lock screen.
- "Privacy" permissions for the Notes app are granted on first run.
- The `NTiOS Inbox` note exists on the destination device.

Once created, test the shortcut from the widget or Siri to confirm it
appends entries exactly as expected.
