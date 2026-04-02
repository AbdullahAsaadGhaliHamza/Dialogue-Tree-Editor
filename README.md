# Dialogue Tree Editor

<img width="1920" height="1080" alt="Dialogue Tree Editor" src="https://github.com/user-attachments/assets/faa0b01d-3daf-46d5-9c4d-16438c8e984b" />

A visual node editor for writing NPC dialogue trees. You write the lines, it handles the structure. No internet required, no accounts, no installs. One HTML file.

Exports to Ink, Twine/Twee, and JSON.

---

## Getting started

Download `dialogue_tree_editor.html` and open it in Chrome, Firefox, or Edge. That is it. Everything runs locally in the browser.

---

## Building a tree

### Adding nodes

Right-click anywhere on the canvas to open the node menu. Four types:

- **Start** — one per tree, this is the entry point
- **NPC line** — something the NPC says
- **Player choice** — a branch point where the player picks a response
- **End** — closes a branch

### Connecting nodes

Every NPC/Start node has a small circle port on its right edge. Drag from that port to another node's left-edge port to create a connection. Player choice nodes have one port per choice row — each one wires independently to a different destination.

Click any connection line to delete it.

### Editing a node

Click a node to select it. The right panel shows all editable fields:

- **Type** — change node type after creation
- **Speaker name** — who is talking (used in Ink and Twine export)
- **Dialogue line** — what they say
- **Choices** — for Player nodes, add/remove/rename choices
- **Condition** — optional logic check (e.g. `player.rep > 5`), passed through to export
- **Set flags** — comma-separated flags to set when this node is reached (e.g. `met_dealer, quest_active`)

### Deleting

Select a node and press `Delete`, or use the Delete button in the panel. Right-clicking a node also gives a delete option.

---

## Navigation

| Action | How |
|---|---|
| Pan | Hold `Space` and drag, or middle-click drag |
| Zoom | Scroll wheel |
| Fit everything on screen | Toolbar: Fit view |
| Move a node | Drag it |
| Select a node | Click it |
| Deselect | Click empty canvas |

---

## Exporting

Pick a format in the top-right tab group (JSON / Ink / Twine), then click **Export**. The output opens in a modal, copy it from there.

### JSON

Clean structure with node IDs, `next` pointers, condition fields, and flags arrays. Leaf nodes have an empty `choices` array. Designed to slot into a custom parser or game engine without modification.

```json
{
  "id": "my_dialogue",
  "name": "My Dialogue",
  "nodes": [
    {
      "id": "n1",
      "type": "npc",
      "speaker": "Dealer",
      "line": "You got coin?",
      "condition": null,
      "flags": [],
      "next": "n2"
    },
    {
      "id": "n2",
      "type": "player",
      "speaker": "Player",
      "line": null,
      "condition": null,
      "flags": [],
      "choices": [
        { "text": "What do you know?", "next": "n3" },
        { "text": "Forget it.", "next": "n4" }
      ]
    }
  ]
}
```

### Ink

Valid `.ink` script. Each node becomes a stitch (`= node_id`). NPC lines use `Speaker: line` format. Player choices use `*` syntax with diverts. Conditions wrap nodes in `{ condition: }` blocks. Copy the output into Inky or drop it into a Unity project using the Ink runtime.

### Twine / Twee

Twee notation for Twine 2 with SugarCube header. Each node is a `:: node_id` passage. NPC lines use `''Speaker:''` bold formatting. Player choices use `[[text|target]]` link syntax. Conditions use `<<if>> <<endif>>`. Flags use `<<set $flag to true>>`. Import into Twine via File > Import, or compile with Tweego.

---

## Saving and loading

**Save** downloads a `.dtree.json` file to your machine. This is the editor's own format and preserves the full graph including node positions, zoom level, and pan position.

**Load** reopens a `.dtree.json` file. Any unsaved work in the current session will be replaced.

The save file is plain JSON so you can inspect or version-control it without the editor open.

---

## Keyboard shortcuts

| Key | Action |
|---|---|
| `Ctrl+S` | Save project |
| `Delete` | Delete selected node |
| `Space + drag` | Pan canvas |

---

## Tips

- Build left to right. Start node on the left, End nodes on the right. Keeps wires readable.
- One Start node per file. The exporter assumes a single entry point.
- Conditions and flags are passed through as plain strings. The editor does not validate them, write them in whatever syntax your engine expects.
- For Ink: add your `VAR` declarations manually after export if you need variable tracking.
- For JSON: the `flags` field exports as an array split on commas, so `met_dealer, quest_active` becomes `["met_dealer", "quest_active"]`.
- Player choice nodes do not have a single outgoing port. Each choice row has its own wire. NPC nodes have one wire out.

---

## Files

```
dialogue_tree_editor.html    the editor, fully self-contained
README.md                    this file
```
