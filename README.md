# Sketchboard

Draw with Apple Pencil on a 2048×1536 board. When you pause or move the pen away from a sketch, that sketch is sent to an image-to-image model on fal.ai and the painting is blended back into the board.

Single file: `index.html`. Open it in Safari (or any modern browser), add your fal.ai key under Settings, and start drawing.

- Strokes close to each other merge into one sketch; each sketch is painted on its own.
- A fast preview model runs first, then an optional refine pass with the same seed.
- Tap a sketch's chip to name it (improves results), repaint, revert or delete it.
- The API key and settings are stored only in the browser's localStorage.

## Talking while you draw

Tap **Listen** and talk about what you draw. The browser's speech recognition (Safari on iPad works) transcribes continuously:

- Words said while a sketch is being drawn, or up to 8 s before it starts, belong to that sketch. Words said within 6 s after its last stroke repaint it.
- If you are mid-sentence when the pen stops, painting waits (up to 5 s) for the sentence to end.
- Your talk is treated as commentary, not as the prompt. In the background an LLM on fal.ai (`fal-ai/any-llm`, model set in Settings) keeps a short **brief** of what you are making overall (theme, style, objects, standing instructions).
- When a sketch is painted, a second LLM step reads the brief plus what you said while drawing it and writes a visual description of just that sketch, plus one colour per object. A sketch may be a group (a still life of several fruits); its fill then starts from patches of those colours. You can speak any language; if the LLM fails, your words are used as they are.
- Both instructions (analysis prompt and master prompt) are editable in Settings.
- A sketch's typed "What is it?" text overrides speech. Tap a sketch's chip to see what was heard and the prompt that was used.

## Object types

The prompt LLM also names what kind of sketch it is, and each kind is prepared and improved differently (Settings: "Adapt … to the type of object"):

| Type | Fill under the lines | First pass strength | Improvement pass | Added to the prompt |
|---|---|---|---|---|
| lettering | colour | 0.75 | 0.35 | exact letter shapes, crisp edges |
| geometric (man-made) | colour | 0.8 | 0.4 | straight edges, sharp corners, perspective |
| organic (one natural thing) | noise | 0.88 | 0.45 | natural form, surface texture |
| group (still life) | noise | 0.92 | 0.45 | several distinct items |
| creature | noise | 0.88 | 0.4 | coherent anatomy |

The improvement pass repaints the first result lightly (inside the shape, lines faintly on top) instead of starting again from the sketch.

## Live mode

The **Sketch / Live** button switches modes. In Live mode the sketch you are drawing is sent to the fast model as a 512 px image while you draw (at most `Live frames per second`, newest state wins, one fixed seed per sketch), and each frame appears under your lines. When you pause, the final pass runs: by default an editing model (`fal-ai/flux-kontext/dev`) told to keep the shape and remove the lines; if fal rejects that request the fast model is used. Then the lines melt away. Every live frame is a paid request.
