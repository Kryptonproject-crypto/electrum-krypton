# Electrum-Krypton — Lightweight wallet for Krypton (KYP)

```
Licence:      MIT
Language:     Python (>= 3.10)
Based on:     Electrum 4.8 (spesmilo/electrum)
Coin:         Krypton (KYP)
Platforms:    Linux · Windows · macOS · Android
```

**Electrum-Krypton** is the official light wallet for the
[Krypton (KYP)](https://github.com/Kryptonproject-crypto/Krypton) blockchain.
It is a fork of [Electrum](https://github.com/spesmilo/electrum): fast to start,
no full chain download, deterministic seed backup, and SPV verification of your
own transactions against block headers.

It talks to a [KryptonX](https://github.com/Kryptonproject-crypto/KryptonX)
server, which indexes the chain from a Krypton full node.

---

## Features

- **Instant sync** — no need to download the blockchain; balances come from an
  Electrum (KryptonX) server, verified locally by SPV.
- **Native SegWit** — generates `kyp1…` (bech32, P2WPKH) addresses.
- **Deterministic backup** — a single seed phrase restores every address.
- **Cross-platform** — desktop (Linux/Windows/macOS) and Android.
- **Biometric unlock** and payment authentication on mobile.
- **Live KYP/USD price** via CoinPaprika.

---

## Krypton specifics

| Parameter              | Value                                   |
|------------------------|-----------------------------------------|
| **Ticker**             | KYP                                     |
| **Address type**       | Native SegWit, bech32 `kyp1…` (P2WPKH)  |
| **Derivation**         | `m/0h` (Electrum native-segwit standard), master key is a `zpub` |
| **Node P2P port**      | 8369                                    |
| **Node RPC port**      | 8370                                    |
| **Price feed**         | CoinPaprika (`kyp-krypton-1`), USD      |

> The wallet ships with **CoinPaprika as the only exchange-rate provider** and
> **USD** as the fiat currency, since KYP is priced there. Other Electrum
> providers return Bitcoin rates and do not apply to Krypton.

---

## Getting started (desktop)

### Requirements

Python ≥ 3.10 and the dependencies listed in the repo.

```bash
git clone https://github.com/Kryptonproject-crypto/electrum-krypton.git
cd electrum-krypton
python3 -m pip install -r contrib/requirements/requirements.txt --user
```

### Run from source

```bash
./run_electrum
```

That launches the GUI. For headless / scripting use, the same binary drives a
daemon:

```bash
./run_electrum daemon -d                 # start the daemon
./run_electrum load_wallet -w <path>     # load a wallet
./run_electrum getbalance -w <path>      # query balance
./run_electrum stop                      # stop the daemon
```

---

## Building the Android APK

The Android app is a QML build produced with `buildozer` inside the provided
Docker image. Build on an x86_64 machine with Docker (a Raspberry Pi is too
limited for the Qt-for-Android toolchain).

```bash
cd contrib/android
# see contrib/android/Readme.md for the full, reproducible Docker build
./make_apk.sh qml arm64-v8a release
```

The app is branded **Krypton Wallet** (launcher name, biometric prompt, and
notifications).

---

## Connecting to a server

By default the wallet connects to a public KryptonX server. To run your own,
set up [KryptonX](https://github.com/Kryptonproject-crypto/KryptonX) against a
`kryptond` node **with `txindex=1`**, then point the wallet at its host and TCP
port in the network settings.

---

## Restoring a wallet

Choose **restore from seed** and enter your seed phrase. The wallet re-derives
your `kyp1…` addresses, subscribes to the server, and rebuilds history and
balance automatically.

> If a restored wallet shows a **0 balance** while a block explorer shows funds,
> the server's node is almost certainly running **without `txindex=1`** — it can
> return address history but not the transactions needed for SPV verification.
> This is a server-side configuration issue, not a wallet problem. See the
> KryptonX README.

---

## Security notes

- Your seed is the only backup. Write it down and keep it offline. Anyone with
  the seed controls the funds.
- The wallet verifies your transactions by SPV (Merkle proofs against block
  headers); it does not blindly trust the server's balance answer.
- On mobile, enable biometric authentication and payment confirmation in
  **Preferences → Security**.

---

## Licence

MIT. Electrum-Krypton is a fork of [Electrum](https://github.com/spesmilo/electrum)
(author Thomas Voegtlin and the Electrum developers), adapted to the Krypton
network.
