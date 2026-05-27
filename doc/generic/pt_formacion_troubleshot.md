# Problem description

The integration test freezes due to a protocol and port mapping mismatch between the legacy JodConverter client and the selected Testcontainers LibreOffice image. `jodconverter:2.2.1` relies on the OpenOffice/LibreOffice UNO (Universal Network Objects) protocol, which expects a raw TCP socket connection to a headless Office instance running with the `--accept="socket,host=...,port=...;urp;"` flag. However, the container image `linuxserver/libreoffice:25.8.1` is designed to provide a web-based GUI environment (typically via KasmVNC or similar technologies), and port 8082 is exposing a `Data WebSocket Server` expecting an HTTP/WebSocket handshake. When `OpenOfficeJodConverterClient` attempts to open a raw UNO connection on this port, the WebSocket server does not send the expected UNO handshake, causing the legacy client's socket connection to block and wait indefinitely.

## Solution 1

Migrate to a modern JodConverter REST implementation and compatible container.

### Technical Implementation

Replace the legacy `com.artofsolving:jodconverter:2.2.1` and `net.sf.jodreports` dependencies with a modern, actively maintained library such as `org.jodconverter:jodconverter-local` or `jodconverter-rest`. Instead of a raw UNO socket, switch the Testcontainers setup in `ContainerIT` to use a dedicated REST-based image (e.g., `jodconverter/jodconverter-rest`) and refactor `OpenOfficeJodConverterClient` to communicate via standard HTTP POST requests. This eliminates raw socket fragility and standardizes the container interface.

## Solution 2

Replace the container image with a raw headless LibreOffice instance exposing a UNO socket.

### Technical Implementation

Retain the legacy JodConverter dependencies but replace the `linuxserver/libreoffice` Testcontainers image in `ContainerIT` with a lightweight, bare-bones Linux image (e.g., Alpine or Ubuntu with `libreoffice-headless` installed). Configure the container's entrypoint or command to start LibreOffice explicitly exposing the UNO API over TCP: `soffice --headless --accept="socket,host=0.0.0.0,port=8082;urp;" --nofirststartwizard --nologo`. This provides the exact raw TCP socket that the `OpenOfficeJodConverterClient` requires to establish its `connection.connect()`.

## Solution 3

Mock the external PDF conversion client during integration tests.

### Technical Implementation

Bypass the entire containerized LibreOffice execution for this specific test scope. In `CreateSignatureContainerIT`, use a mocking framework (like Spring Boot's `@MockBean`) to inject a mock of `OpenOfficeJodConverterClient`. Configure the mock to intercept the `convert` method and immediately return a dummy byte array representing a PDF. Remove the LibreOffice container initialization from `ContainerIT` (or disable it for this test profile). This isolates the domain logic test from infrastructure rendering issues.

## Comparative (pros and cons)

| Pros/Cons | Solution 1 | Solution 2 | Solution 3 |
| :--- | :--- | :--- | :--- |
| Pro 1 | Eliminates obsolete legacy dependencies | Validates actual ODT to PDF generation | Unblocks CI/CD pipeline immediately |
| Pro 2 | Uses standard HTTP REST communication | Requires no changes to application code | Dramatically reduces test execution time |
| Pro 3 | Container setup is officially supported | Maintains exact current runtime behavior | Eliminates container memory overhead |
| Con 1 | Requires refactoring client application code | Still relies on unmaintained legacy libraries | Fails to test the actual PDF conversion logic |
| Con 2 | May require migration effort for ODT templates | Custom container scripting can be brittle | Requires maintaining mock configurations |
| Con 3 | Introduces new HTTP overhead | Raw UNO sockets remain fragile under load | False positives if production template fails |
| Complexity | High | Med | Low |