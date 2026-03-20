# yboard dev

Start the mobile development server (Expo).

## Usage

```bash
yboard dev
```

## Behavior

- **TTY (interactive terminal):** Starts Expo with `--go` flag (Expo Go mode). Displays a QR code directly in the terminal for the user to scan.
- **Non-TTY (agent/CI environment):** Starts Expo with `--tunnel` flag. The tunnel URL is printed in the output.

## Non-TTY: Showing the QR Code

In non-TTY environments (e.g. when an agent runs this command), Expo's built-in QR code will not be rendered. Instead:

1. Run `yboard dev` and read the output
2. Extract the tunnel URL from the output (look for a URL like `exp://...` or an `https://` tunnel URL)
3. Run `yboard qr <url>` to generate a terminal QR code
4. Show the QR code to the user so they can scan it with Expo Go

### Example

```bash
# Start the dev server (tunnel mode in non-TTY)
yboard dev

# After extracting the tunnel URL from the output:
yboard qr "exp://u.expo.dev/..."
```

## Requirements

- Must be run from the project root (expects `apps/native/` to exist)
- Requires authentication (`yboard whoami`)
- The web platform must be deployed first (mobile calls the web API)
