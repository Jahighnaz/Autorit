# Sketchboard

Draw with Apple Pencil on a 2048×1536 board. When you pause or move the pen away from a sketch, that sketch is sent to an image-to-image model on fal.ai and the painting is blended back into the board.

Single file: `index.html`. Open it in Safari (or any modern browser), add your fal.ai key under Settings, and start drawing.

- Strokes close to each other merge into one sketch; each sketch is painted on its own.
- A fast preview model runs first, then an optional refine pass with the same seed.
- Tap a sketch's chip to name it (improves results), repaint, revert or delete it.
- The API key and settings are stored only in the browser's localStorage.
