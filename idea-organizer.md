# Idea Organizer System

This system organizes a flood of ideas into a clean, evolving Markdown document, using category theory and motivation psychology to reduce overwhelm while preserving every idea’s essence. It’s built to handle single thoughts or dense paragraphs, with modes for organizing and discussing, all driven by a prompt.

## The Prompt
```
Organize my ideas—I’ll keep adding. This is ORGANIZE mode, default response. Use category theory, motivation psychology, reduce overwhelm, keep ALL info/essence. Categorize each idea, write a sharp label capturing core intent (length tuned to cognitive clarity, per latest psych/science), then append my exact quote snippet relevant to that idea right after. If idea spans categories, note overlaps in label. For multi-idea inputs, extract snippets per category, append full quote once in a ‘Source Quotes’ section with ID (e.g., P1). Insert into an updated output doc (Markdown code block), group related ideas under collapsible headers, nest hierarchically if related. Sub-categorize dynamically if clusters grow dense, based on math/psych thresholds (e.g., chunking limits), not fixed counts. Don’t lose my energy/essence. Revise doc each time, blend my voice with your structure. If no tag, assume ORGANIZE unless context screams discussion. Exception: If I start with "@DD", switch to DISCUSSION mode—respond directly, no doc update unless I flag doc changes clearly (e.g., meta-feedback on labels/categories), then adjust doc as directed. If meta-feedback’s unclear, ask for specifics, don’t guess. Stay in DISCUSSION until I say "@OO", then shift back to ORGANIZE mode and integrate the next idea into the doc. Got it?
```

## How It Works

### Modes
- **ORGANIZE (default, `@OO`)**: Feed ideas—single or paragraph-long. They’re categorized, labeled, and slotted into a Markdown doc with exact quote snippets. Full quotes are stored once in a “Source Quotes” section with IDs (P1, P2, etc.).
- **DISCUSSION (`@DD`)**: Chat about ideas, get feedback, or critique the doc (e.g., “labels suck”). Responses are direct, no doc changes unless you flag it (e.g., “add this to the doc” or “fix categories”). Vague flags prompt a request for clarity.

### Idea Handling
- **Single Idea**: Gets a label, quote snippet, and category (e.g., “Idea Flow System: ‘need a way to track ideas’ (P1)”).
- **Multi-Idea Paragraphs**: Split into distinct ideas, each with a label and snippet (e.g., “Dynamic Sorting: ‘sort my thoughts’ (P1)”). Full paragraph stored once in Source Quotes.
- **Hierarchy**: Related ideas nest (e.g., “Sub-Categorization” under “Organization”). Subcategories form dynamically when clusters get dense, guided by psych (e.g., 7±2 items).
- **Overlaps**: Ideas fitting multiple categories get noted (e.g., “Cross-Category Link: ‘connects sorting and display’”).

### Doc Structure
- Markdown code block, updated per ORGANIZE input.
- Collapsible headers group ideas (e.g., `## Idea Management`).
- Source Quotes section holds full inputs, tied to P1, P2 IDs—keeps doc lean, essence intact.
- Example:
  ```markdown
  ## Idea Management
  - Idea Flow System: "need a way to track ideas" (P1)
    - Dynamic Sorting: "sort my thoughts" (P1)
  - Clarity Focus: "keep it all clear" (P1)

  ## Source Quotes
  - P1: "need a way to track ideas, sort my thoughts, and keep it all clear"
  ```

### Essence & Overwhelm
- Raw voice preserved in Source Quotes—exact words, no dilution.
- Labels are sharp but don’t cut essence, tuned to psych for clarity (e.g., 4±2 words).
- Scannable via headers, nesting, and psych-driven limits (e.g., ~7 top-level categories).

### Feedback in `@DD`
- Discuss ideas or doc (e.g., `@DD Should sorting be visual?”). Response is direct, no doc touch.
- Flag changes (e.g., “add visual sorting to doc” or “labels too vague, fix them”), and the doc updates with a new Pxx quote.
- Vague flags (e.g., “this sucks”) trigger a “what’s off?”—no guessing.

## Example Flow
1. **Input**: `@OO Need a way to track ideas, sort my thoughts, and keep it all clear.`
2. **Doc**:
   ```markdown
   ## Idea Management
   - Idea Flow System: "need a way to track ideas" (P1)
     - Dynamic Sorting: "sort my thoughts" (P1)
   - Clarity Focus: "keep it all clear" (P1)

   ## Source Quotes
   - P1: "need a way to track ideas, sort my thoughts, and keep it all clear"
   ```
3. **Input**: `@DD What if sorting used colors?`
4. **Response**: “Colors could cue categories—fast, visual. Maybe tie to priority?”
5. **Input**: `@DD Add that to the doc.`
6. **Doc**:
   ```markdown
   ## Idea Management
   - Idea Flow System: "need a way to track ideas" (P1)
     - Dynamic Sorting: "sort my thoughts" (P1)
       - Color-Based Sorting: "sorting used colors" (P2)
   - Clarity Focus: "keep it all clear" (P1)

   ## Source Quotes
   - P1: "need a way to track ideas, sort my thoughts, and keep it all clear"
   - P2: "sorting used colors"
   ```

## Why It’s Solid
- Scales with idea floods—paragraphs split cleanly, no bloat.
- Preserves raw voice—quotes untouched in Source Quotes.
- Scannable—hierarchy, headers, psych-driven limits.
- Flexible—`@DD` chats, tweaks, or adds ideas without `@OO` hassle.