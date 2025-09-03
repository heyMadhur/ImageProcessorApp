# ImageProcessorApp

Lightweight JavaFX demo that applies tile-based image filters synchronously or asynchronously.

## Requirements
- JDK 21 (set JAVA_HOME)
- macOS (commands shown), Maven wrapper included
- Recommended IDE: VS Code or IntelliJ with JavaFX support

## Quick setup (macOS)
```bash
# build
./mvnw clean package

# run
./mvnw javafx:run
```

Or use your IDE: import as a Maven project and run class `com.my.app.HelloApplication`.

## What it does
- Loads an image from `src/main/resources`.
- Presents a small Java console menu and a JavaFX window.
  - Option 1 — Async tile-based processing (parallel).
  - Option 2 — Sync whole-image processing.
  - Option 3 — Exit.
- Progressive drawing shown on the canvas; processed images are saved to `output/filtered_<name>.png`.

## Important notes
- Tile size must evenly divide both image width and height; otherwise processing aborts (see `ImageProcessor`).
- Async uses a fixed thread pool (configured in `ImageProcessor`).
- Filters implement `com.my.app.filters.ImageFilter`. See `GreyScaleFilter` for an example.

## Extending
- Add new images to `src/main/resources` and update the path in `HelloApplication`.
- Implement new filters under `com.my.app.filters` and wire them in `HelloApplication` to use.

## Troubleshooting
- JavaFX module errors: ensure JDK 21 and run configuration uses the project module path.
- Missing images: confirm resource filename and location.

## Quick test plan — Synchronous vs Asynchronous (short)
- Prepare three test images in `src/main/resources`:
  - Small (≈800×600), Medium (≈1920×1080), Large (≈3000–4000 px on longest side).
- For each image:
  1. Run `./mvnw javafx:run` and choose Option 2 (synchronous). Note wall time and UI responsiveness.
  2. Run Option 1 (asynchronous) with a tile size that evenly divides the image. Note wall time, CPU usage, and UI responsiveness.
  3. Repeat each run 3–5 times and take the median time.
- What to record: total elapsed time, peak CPU utilization, memory use, whether the UI stayed responsive, and output image correctness.
- Expected observations (concise):
  - Synchronous: single-threaded, longer wall time, UI may block during processing.
  - Asynchronous: lower wall time on multi-core machines, higher CPU utilization, improved UI responsiveness (progressive drawing), but overhead from thread scheduling and possible diminishing returns if thread pool is too large or tile size is too small.
  - Final images should be identical for deterministic filters; watch for seam/artifacts only if filters depend on neighboring tiles beyond tile boundaries.
- Quick tips:
  - Adjust thread pool size in `ImageProcessor` for best throughput (matching CPU cores is a good start).
  - Use larger tiles to reduce scheduling overhead; smaller tiles increase parallelism but add

## License & Author
Made in India ❤️ by Madhur Gupta ✨