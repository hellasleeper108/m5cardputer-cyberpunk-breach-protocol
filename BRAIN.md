# Project Brain

## 2026-06-24 Cardputer ADV boot fix

- Symptom: firmware flashed successfully but the Cardputer ADV stayed on a black screen at power-on.
- Root cause found: `M5.config()` falls back to `board_M5AtomS3Lite` on ESP32-S3 if runtime board detection does not resolve the ADV. That can leave the app initialized on the wrong display/keyboard path.
- Fix: set `cfg.fallback_board = m5::board_t::board_M5CardputerADV;` before `M5Cardputer.begin(cfg);`.
- Hardware note: this unit reports `Embedded Flash 8MB (GD)` via esptool, so the project should not force `default_16MB.csv` or `board_upload.flash_size = 16MB`.
- Verification: rebuilt and flashed with PlatformIO to `/dev/ttyACM0`; user confirmed the app runs.

## 2026-06-24 Upstream merge notes

- Upstream added leaderboard/auth/WiFi code and website files. The PlatformIO build needed `bblanchon/ArduinoJson` added to `lib_deps`.
- Upstream code came from Arduino `.ino` style and relied on generated function prototypes; `src/main.cpp` needs explicit forward declarations for PlatformIO.
- Verification after merge: `pio run -e m5cardputer-adv` succeeds.

## 2026-06-24 Effects and difficulty pass

- Added procedural speaker tones for boot, hover, select, typing, success, and failure. This replaces the previous null WAV placeholders, so audio now works without bundled sample arrays.
- Added glitch burst rendering for boot/menu/gameplay transitions and occasional splash static.
- Added persisted difficulty settings: Easy, Normal, Hard, Nightmare. Difficulty controls grid size, target length, timer pressure, and score multiplier.
- Main menu now has HACK, DIFF, and LEADERBOARD. Enter on DIFF cycles difficulty; left/right on DIFF also cycles.
- Verification: `pio run -e m5cardputer-adv` succeeds and the final build was flashed to `/dev/ttyACM0`.
