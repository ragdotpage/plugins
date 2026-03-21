# yboard dev

Start the mobile development server (Expo).

## Usage

```bash
yboard dev
```

## Behavior

- **TTY (interactive terminal):** Starts Expo with `--go` flag (Expo Go mode). Displays a QR code directly in the terminal for the user to scan.
- **Non-TTY (agent/CI environment):** Starts an ngrok tunnel on port 8081, sets `EXPO_PACKAGER_PROXY_URL`, and starts Expo. Prints `TUNNEL_URL=<url>` to stdout.

## Non-TTY: Showing the QR Code

In non-TTY environments (e.g. when an agent runs this command), Expo's built-in QR code will not be rendered. Instead:

1. Run `yboard dev` — it will print `TUNNEL_URL=exp://abc123.ngrok.io` to stdout
2. Extract the tunnel URL from the `TUNNEL_URL=` line
3. Run `yboard qr <tunnel-url>` to generate a terminal QR code
4. Show the QR code to the user so they can scan it with Expo Go

### Example

```bash
# Start the dev server (tunnel mode in non-TTY)
yboard dev
# Output includes: TUNNEL_URL=exp://abc123.ngrok.io

# Generate QR code from the tunnel URL:
yboard qr "exp://abc123.ngrok.io"
```

## Requirements

- Must be run from the project root (expects `apps/native/` to exist)
- Requires authentication (`yboard whoami`)
- The API must be deployed first (`yboard deploy --platform mobile`) — dev mode calls the deployed backend
- **Non-TTY requires ngrok:** Install from https://ngrok.com/download
