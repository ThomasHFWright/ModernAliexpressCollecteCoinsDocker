# AliExpress Coin Collector - Mobile Edition

An automated tool to help collect daily coins on AliExpress with **realistic mobile device simulation** to avoid detection.

## Features

- **Mobile Device Simulation**: Emulates Android smartphone (Samsung Galaxy S23 / Google Pixel 7)
- **Touch Events**: Uses touch/tap interactions instead of mouse movements
- **Mobile User Agent**: Authentic mobile browser fingerprint
- **Anti-Detection**: Advanced fingerprinting protection and human-like behavior
- Automatic login to AliExpress account
- Optional region change to Korea (for maximum coin rewards)
- Collects daily coins with realistic human-like behavior
- Secure credential management using environment variables
- Scheduled mode for automated daily collection

## Device Configuration

The script now simulates a **real mobile device**:
- **Device**: Samsung Galaxy S23 (Poland locale) or Google Pixel 7 (US locale)
- **Screen**: 412x915 pixels, 3x DPI
- **Platform**: Android 13
- **Browser**: Chrome Mobile 120
- **Touch**: Enabled with maxTouchPoints=5
- **Memory**: 8GB RAM, 8-core CPU

## Prerequisites

- Docker and Docker Compose
- A valid AliExpress account

## Installation

### Docker Compose

1. Download `docker-compose.yml`, or copy and modify from below:
```YAML
services:
  aliexpress-coin-collector:
    # Build directly from GitHub so Portainer can deploy this stack
    # using only this compose file + environment variables.
    build:
      context: https://github.com/ThomasHFWright/ModernAliexpressCollecteCoinsDocker.git
      dockerfile: Dockerfile
    environment:
      ALIEXPRESS_EMAIL: ${ALIEXPRESS_EMAIL:-}
      ALIEXPRESS_PASSWORD: ${ALIEXPRESS_PASSWORD:-}
      LOCALE: ${LOCALE:-poland}
      USE_KOREA: ${USE_KOREA:-false}
      SCHEDULE: ${SCHEDULE:-true}
    command: ["python", "main.py", "--headless"]
    shm_size: "1gb"
    restart: unless-stopped
```

2. Copy the env file and set values:
   ```bash
   cp .env.example .env
   ```
3. Build and run:
   ```bash
   docker compose up --build
   ```

`docker-compose.yml` is configured to build directly from this GitHub repository, which works well in Portainer as a stack. You can:
- Upload/paste only `docker-compose.yml`
- Define variables in Portainer's **Environment variables** section (or provide a `.env` file)
- Deploy without manually cloning the repo on the host

The container reads these environment variables from `.env` (Docker always runs with `--headless`):
- `LOCALE` (`poland` or `us_east`)
- `USE_KOREA` (`true`/`false`)
- `SCHEDULE` (`true`/`false`)

### What the Script Does

The script will:
1. **Simulate Android mobile device** (Samsung Galaxy S23 or Pixel 7)
2. Navigate to the AliExpress coin collection page
3. Log in with your credentials from the environment variables.
4. (Optional) Change region to Korea for maximum coin benefits (if `--use-korea` is enabled)
5. Collect the daily coins using touch/tap simulation
6. Close the browser when complete

### Mobile Simulation Features

- ✅ **Touch Events**: All interactions use mobile touch/tap gestures
- ✅ **Mobile User Agent**: Authentic Android Chrome mobile browser
- ✅ **Mobile Viewport**: 412x915 resolution (typical mobile phone)
- ✅ **Device Properties**: Proper mobile hardware fingerprint (8GB RAM, 8-core CPU)
- ✅ **Anti-Detection**: Canvas/WebGL fingerprinting protection
- ✅ **Human Behavior**: Realistic delays, swipes, and touch patterns

## Security

- Your credentials are stored locally in the `.env` file, which should NEVER be committed to Git
- The `.gitignore` file includes `.env` to prevent accidental commits
- The script uses environment variables instead of hardcoded credentials

## Troubleshooting

If you encounter issues:

- **Login Failures**: Ensure your credentials in the `.env` file are correct
- **Element Not Found Errors**: AliExpress may have updated their website. Please create an issue on GitHub

## Legal Disclaimer

This tool is provided for educational purposes only. Use at your own risk. The creator is not responsible for account suspensions, lost coins, or other issues that may arise from automated interactions with AliExpress. Always review the AliExpress Terms of Service before using automation tools.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

[MIT License](LICENSE)
