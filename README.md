# RandomXMac-Miner
Native macOS Monero (XMR) miner for Intel and Apple Silicon, featuring RandomX mining, configurable pools, CPU controls, TLS support, live statistics, pool latency monitoring, and an activity log.

RandomXMac provides a modern interface for RandomX CPU pool mining, powered by bundled XMRig engines. Configure your pool and wallet, choose how many CPU threads to use, and monitor mining activity from one window.

## Features

- Native support for Intel and Apple Silicon Macs, including M1 and later.
- Configurable pool hostname, port, XMR receiving address, worker name, and password.
- TLS support with an optional certificate fingerprint.
- Adjustable mining CPU thread count.
- Live connection state, hash rate, accepted/rejected shares, and active mining time.
- Pool latency display after accepted shares.
- Activity log with export support.
- **Save settings** button and **Command-S** shortcut.
- **Reset to defaults** button.
- Visible developer-fee information and current mining recipient.
- Bundled mining engines and required non-system runtime libraries.

No separate XMRig, Homebrew, CocoaPods, Node.js, or OpenSSL installation is needed to run the packaged app.

## System requirements

| Hardware | Minimum macOS for mining with the bundled engine |
| --- | --- |
| Intel Mac (x86_64) | macOS 11 Big Sur |
| Apple Silicon Mac (ARM64) | macOS 13 Ventura |

RandomX uses several gigabytes of memory and can place a sustained load on your CPU. Choose a thread count suitable for your Mac and other running applications.

## Download and install

Open this repository's **Releases** section and select a packaged app download, if available. GitHub's automatically generated **Source code** archives contain the project, rather than a ready-to-run installation.

- For a `.dmg` download, open the disk image and drag **RandomXMac** into **Applications**.
- For an app `.zip`, extract it and move **RandomXMac.app** into **Applications**.

Release notes should identify the version, supported systems, and signing/notarization status. If no packaged release is available, build the app using the instructions below.

## Getting started

1. Enter your pool's hostname without a URL prefix such as `stratum://`.
2. Enter the pool's port and enable **TLS** if that port supports TLS.
3. Enter **your own public Monero receiving address**. Replace any prefilled address before mining.
4. Enter the worker name and pool password required by your pool. Many pools use `x` as the password.
5. Choose the number of logical CPU threads to use.
6. Click **Save settings** to keep your configuration, or **Start mining** to save it and begin mining.

Initialization may take time before a hash rate appears. Accepted shares also depend on pool difficulty.

Click **Stop** before changing settings. Closing the last window or quitting the app stops mining. Mining does not automatically start when the app opens.

### Saved settings and defaults

Pool, port, wallet, worker, TLS, and CPU settings are stored locally. The pool password remains session-only and is not saved in preferences.

**Reset to defaults** restores the defaults compiled into the app, including its default wallet, and resets the password to `x`. Check the receiving address again before starting.

### Pool latency

The latency value is XMRig's median accepted-share response time. It includes network transit and pool processing, rather than being a separate ICMP ping measurement. A value appears after an accepted share and is cleared when status is unavailable or mining stops.

### CPU selection

The selector controls mining worker threads and RandomX dataset initialization threads. It does not pin individual cores or guarantee an exact CPU usage percentage; the engine also uses supporting threads.

## Fees and payouts

The interface displays the configured application developer fee separately from XMRig's upstream donation.

- **Application developer fee:** configurable at build time; the supplied source defaults to **0%**.
- **XMRig upstream donation:** **1%**, separately enabled in the supplied configuration.
- **Pool fees and payout thresholds:** determined by the selected pool.

When enabled, the application fee assigns a portion of measured active mining time to the developer wallet. Switching recipients restarts the engine. A percentage of mining time does not guarantee the same percentage of earnings.

The selected pool handles balances and payouts. RandomXMac is a mining interface, not a wallet or a payout service. It uses RandomX mining; connecting to a multi-algorithm pool does not add automatic algorithm switching.


The example enables a 1% application fee in addition to XMRig's donation. Supported application fees range from 0–20%; an enabled fee requires a valid receiving address. A separate developer pool can be configured in the same file.

## Bundled components

The project supplies XMRig 6.26.0 executables for both architectures, with RandomX and non-system dependencies included. These include libuv, hwloc, and OpenSSL.

- `Sources/`: SwiftUI interface, configuration, status handling, and process supervision.
- `Scripts/`: engine embedding and macOS verification scripts.
- `Vendor/`: engine binaries, original archives, and dependency sources.
- `Resources/Licenses/`: third-party license notices.
- `Tests/`: portable process-supervisor tests.

Rebuilding XMRig and its dependencies requires additional build tools. Those tools are not required to build the interface around the supplied engines.

## Testing

Portable supervisor tests require Python 3 and a C compiler:

```sh
python3 Tests/test_supervisor.py
```

On a supported Mac with Xcode, run configuration checks and an offline RandomX benchmark:

```sh
bash Scripts/test-macos.sh
```

Before publishing a release, test installation, pool login, TLS, share reporting, saved settings, reset behavior, shutdown, and any enabled developer-fee phase on both architectures.

## Privacy and operation

- Use only a public receiving address. Never enter a wallet seed phrase or private key.
- Mining connects to the configured pool; the pool receives the credentials necessary for mining.
- The status API listens on loopback and uses a per-session authentication token.
- Engine configuration is written to a temporary file with restricted permissions and removed during normal shutdown. A hard crash may leave it until temporary storage is cleaned.
- Exported activity logs may contain wallet addresses and pool information. Review logs before sharing them.
- Mining can increase power use, heat, and fan activity. The app does not automatically pause when switching to battery power.

## Support and contributions

Use this repository's **Issues** tab to report bugs or suggest features. Include your macOS version, Mac architecture, app version, steps to reproduce, and relevant log excerpts with sensitive information removed.

Contributions and documentation improvements are welcome.

DONATION ADDRESS (XMR): 8BtKmGYtNSobhaJND6Jwm3YBXoxdpDAH3DBVtVmX6qHrfT2AAiCZU6SenxUXkbPsPagENwh96TA4PYJp5iYoiZEa8fdZJak

## License and acknowledgments

The application wrapper and supervisor are provided under **GPL-3.0-or-later**. See [LICENSE](LICENSE). Bundled components retain their respective licenses; see `Resources/Licenses` and the included source archives. Preserve the applicable notices and provide corresponding source materials when redistributing.

Powered by [XMRig](https://github.com/xmrig/xmrig) and [RandomX](https://github.com/tevador/RandomX), for mining [Monero](https://www.getmonero.org/).

RandomXMac is an independent project and is not an official Monero or XMRig application.

