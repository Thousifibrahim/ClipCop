

[![JavaFX](https://img.shields.io/badge/JavaFX-24.0.1-blue?style=for-the-badge&logo=javafx&logoColor=white)](https://openjfx.io/)
[![Java](https://img.shields.io/badge/Java-24-red?style=for-the-badge&logo=openjdk&logoColor=white)](https://openjdk.org/projects/jdk/24/)
[![Jackson](https://img.shields.io/badge/Jackson-2.18.0-orange?style=for-the-badge&logo=jackson&logoColor=white)](https://github.com/FasterXML/jackson)
[![Maven](https://img.shields.io/badge/Build%20with-Maven-red?style=for-the-badge&logo=apache-maven&logoColor=white)](https://maven.apache.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)
[![Platform: Windows | macOS | Linux](https://img.shields.io/badge/Platform-Windows%20%7C%20macOS%20%7C%20Linux-brightgreen?style=for-the-badge)](https://github.com/Thousifibrahim/ClipCop)

# ClipCop
open-source clipboard manager implemented with Java 24 and JavaFX. ClipCop provides persistent clipboard history, fast quick-paste access, and full-text search while keeping a small runtime footprint.

Repository: https://github.com/Thousifibrahim/ClipCop

![ClipCop Main Window](02.png)

## Project status
Production-ready prototype. Maintenance and incremental improvements ongoing.

## Key capabilities
- Persistent history for text clipboard entries (default: 100 items, 30 days retention)
- Quick Paste popup for immediate access to the top 10 recent entries
- Full-text, incremental search across history
- Light and dark themes implemented in CSS
- Cross-platform: Windows, macOS, Linux
- Local persistence: JSON file (data/clipboard_history.json)
- Minimal memory usage through bounded history and optional compression

## Technical stack
- Language: Java 24
- UI: JavaFX 24.0.1 (FXML-based views + CSS)
- JSON serialization: Jackson 2.18.0
- Build system: Apache Maven
- Packaging: javafx-maven-plugin (supports javafx:run and jlink-based runtime images)
- Main class (sample pom configuration): `com.clipboard.MainApp`

## Architecture and components
- com.clipboard
  - Application entry point and DI/bootstrap
  - Controllers: JavaFX controllers handling view events
  - Services:
    - Clipboard listener service: captures clipboard events and normalizes text
    - History manager: bounded queue with deduplication, expiry, and persistence
    - Search service: incremental filtering with relevance ordering
    - Quick paste controller: keyboard navigation and paste logic
  - Persistence:
    - JSON schema (array of entries): each entry contains id, text, createdAt, and optionally metadata (source application).
    - Default data location: `data/clipboard_history.json`
- Resources:
  - FXML views under `src/main/resources/com/clipboard/`
  - Styling in `styles.css`
  - Static assets (icons, screenshots)

## Configuration and tuning
- History retention and size:
  - Default constants are defined in the history manager (maxEntries = 100, retentionDays = 30). These can be adjusted by changing the configuration class or exposing a settings file.
- Persistence format example:
  ```json
  [
    {
      "id": "b4f1c2d3",
      "text": "Example clipboard text",
      "createdAt": "2026-02-09T12:34:56Z",
      "source": "com.example.editor"
    }
  ]
  ```
- JVM / runtime recommendations:
  - Default JVM arguments are compatible with Java 24.
  - Recommended for desktop: Xmx configured per user environment (e.g., -Xmx256m) — adjust if working with very large clipboard content collections.
- Platform specifics:
  - macOS: user may need to grant Accessibility or Automation permissions for clipboard capture in some cases
  - Linux: Wayland support may vary depending on compositor; X11 environments behave predictably

## Build, run, and package

Prerequisites
- JDK 24 (OpenJDK or Oracle)
- Apache Maven (3.8+ recommended)

Development run
1. Clone:
   ```bash
   git clone https://github.com/Thousifibrahim/ClipCop.git
   cd ClipCop
   ```
2. Ensure the history file exists:
   ```bash
   mkdir -p data
   echo "[]" > data/clipboard_history.json
   ```
3. Run with Maven:
   ```bash
   mvn clean javafx:run
   ```

Build and package options
- Standard build:
  ```bash
  mvn clean package
  ```
- Create a runtime image (jlink) — plugin configured in pom.xml:
  ```bash
  mvn javafx:jlink
  ```
- Create an executable jar (if needed) — ensure javafx modules are on module path or use the javafx-maven-plugin runtime

CI and testing
- Unit tests: place non-UI tests under `src/test/java`
- UI tests: consider TestFX or equivalent for JavaFX UI testing
- CI: integrate Maven lifecycle with GitHub Actions/other CI to run `mvn -DskipTests=false test package`

Logging, telemetry, and diagnostics
- Logging: use a lightweight logging facade (e.g., java.util.logging or SLF4J with a simple backend) for diagnostic messages
- Diagnostics: provide a verbose startup flag to dump configuration and data file path for troubleshooting
- Telemetry: current implementation includes no telemetry; any telemetry work should be opt-in and clearly documented

Security & privacy
- Clipboard contents are sensitive. By default, history is stored locally in `data/clipboard_history.json`.
- Recommendations for production:
  - Add optional encryption-at-rest for the history file
  - Provide a secure, opt-in cloud sync with end-to-end encryption if synchronization is required
  - Implement per-entry redaction or exclusion rules (e.g., do not store content matching regex patterns like credit card numbers)

Developer workflow
- Use feature branches: `feature/<short-description>`
- Follow conventional commits or a project-specific commit message style
- Tests required for logic changes; UI-only tweaks should include manual verification steps in PR description
- Release process:
  - Update version in `pom.xml`
  - Tag release in git and attach artifacts if distributing via GitHub Releases

Contributing
- Fork -> create branch -> implement change -> open pull request
- Provide tests for behavioral changes (history manager, search, persistence)
- Document configuration changes and migration steps, if any

Troubleshooting
- Maven not found: ensure Maven is installed and on PATH (`mvn -v`)
- JavaFX module errors: ensure JavaFX dependencies present in pom.xml and use `javafx-maven-plugin` for running
- Class version mismatch: verify `java -version` reports JDK 24
- JSON parsing errors: verify `data/clipboard_history.json` exists and contains a valid JSON array (`[]`)

Project layout
- src/main/java/com/clipboard — Java source code
- src/main/resources/com/clipboard — FXML, CSS, images
- data — runtime JSON storage for clipboard history
- pom.xml — build and plugin configuration

License
This project is licensed under the MIT License. See the  MIT `LICENSE` file for full terms.

Contact
Thousif Ibrahim  
GitHub: https://github.com/Thousifibrahim  
LinkedIn: https://www.linkedin.com/in/smd-thousif-ibrahim-29050421b
