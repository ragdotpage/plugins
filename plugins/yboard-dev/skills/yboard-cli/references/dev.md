# schema0 dev

Start the mobile development server (Expo).

## Usage

```bash
schema0 dev
```

## Behavior

Uses `node-pty` to spawn Expo with `--go` flag (Expo Go mode). The QR code is always rendered in the output regardless of the environment.

**Important:** The mobile device must be on the same network as the development machine.

## Displaying the QR Code

`schema0 dev` outputs a QR code and a URL like `exp://192.168.x.x:8081`. The agent MUST display the QR code output to the user so they can scan it with Expo Go on their phone.

## Requirements

- Must be run from the project root (expects `apps/native/` to exist)
- Requires authentication (`schema0 whoami`)
- The API must be deployed first (`schema0 deploy --platform mobile`) — dev mode calls the deployed backend
- Mobile device must be on the same network as the development machine
