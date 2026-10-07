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
- Before painting, an LLM on fal.ai (`fal-ai/any-llm`, model set in Settings) turns the sketch's words plus the recent talk about the whole picture into a short English prompt, so you can speak any language. If that call fails, your words are used as they are.
- A sketch's typed "What is it?" text overrides speech. Tap a sketch's chip to see what was heard and the prompt that was used.
