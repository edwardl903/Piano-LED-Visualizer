# Quick reference — connecting to your Pi

Use this when you power the Pi back on and need to find it on the network again.

## Default credentials

| What | Value |
|------|-------|
| SSH username | `plv` |
| SSH password | `visualizer` |
| Hotspot name | `PianoLEDVisualizer` |
| Hotspot password | `visualizer` |
| Hostname | `pianoledvisualizer.local` |

---

## First boot (or Pi not on home Wi‑Fi yet)

The Pi starts its own Wi‑Fi hotspot. Connect from your Mac/PC/phone:

1. Join Wi‑Fi **`PianoLEDVisualizer`** (password `visualizer`).
2. Open the web UI: **http://192.168.4.1**
3. SSH (optional): `ssh plv@192.168.4.1`

---

## Put the Pi on your home Wi‑Fi (one-time setup)

While connected to the Pi hotspot:

1. Open **http://192.168.4.1** in a browser.
2. Go to the **Network** tab in the left menu.
3. Click refresh on the Wi‑Fi list.
4. Click **Connect** next to your home network, enter the password, confirm.
5. The hotspot will turn off — your Mac will disconnect. That is normal.
6. Reconnect your Mac to your **home Wi‑Fi**.

### Find the Pi's new IP address

**On the Pi** (SSH while still on hotspot, or LCD if you have one):

```bash
hostname -I
```

Use the **first number** (IPv4). Example output:

```
10.0.0.194 2601:189:...
```

→ use **`10.0.0.194`** (yours will differ).

**From your Mac** (same home Wi‑Fi):

```bash
ping pianoledvisualizer.local
```

If that fails, scan your network (adjust subnet if needed):

```bash
nmap -sn 10.0.0.0/24
```

Or check your router's connected-devices list for `pianoledvisualizer`.

---

## Day-to-day access (Pi already on home Wi‑Fi)

Replace `<pi-ip>` with the address from `hostname -I` (e.g. `10.0.0.194`).

| Task | How |
|------|-----|
| Web UI | **http://<pi-ip>** |
| SSH | `ssh plv@<pi-ip>` |
| Upload MIDI files | Web UI → **Songs management**, or copy to `Songs/` on the Pi |
| SFTP | `sftp://plv@<pi-ip>` → `/home/plv/Piano-LED-Visualizer/Songs/` |

Both your computer and the Pi must be on the **same Wi‑Fi network**.

---

## SSH tips

```bash
ssh plv@<pi-ip>
# password: visualizer
```

**If SSH hangs:** the Pi is not reachable. Try the hotspot steps above, or confirm the IP with `hostname -I`.

**If you are already on the Pi** and run `ping pianoledvisualizer.local`, it will show `127.0.0.1` — that is normal (the Pi pinging itself). Run `ping` from your **Mac**, not from the Pi, to test network access.

**Enable SSH on the pre-built image:** place an empty file named `ssh` (no extension) on the SD card boot partition before first boot. See [wifi_setup.md](wifi_setup.md).

---

## Bluetooth MIDI piano (optional)

Run these **on the Pi** via SSH, not on your Mac.

1. Put the piano in Bluetooth pairing mode.
2. Scan and connect:

```bash
sudo bluetoothctl
power on
agent on
default-agent
scan on
# wait for [NEW] Device ... Your-Piano-Name
scan off
pair XX:XX:XX:XX:XX:XX
trust XX:XX:XX:XX:XX:XX
connect XX:XX:XX:XX:XX:XX
exit
```

3. Check MIDI ports: `aconnect -l`
4. Web UI → **Ports** → set **Input** to the Bluetooth/piano port.

USB MIDI is usually more reliable than Bluetooth.

---

## Common gotchas

- **MIDI files on your laptop do not sync to the Pi.** Copy or upload them to the Pi's `Songs/` folder (web UI or SFTP). Only `.mid` files show in the song list.
- **Hotspot vs home Wi‑Fi:** use `192.168.4.1` on the hotspot; use `<pi-ip>` on home Wi‑Fi.
- **IP may change** if your router reassigns addresses. Run `hostname -I` on the Pi again if the old IP stops working.

---

## Related docs

- [Wi‑Fi setup (SD card method)](wifi_setup.md)
- [Manual installation](manual_installation.md)
- [Bluetooth / Synthesia](btconnection.md)
- [External device connections](external_devices.md)
