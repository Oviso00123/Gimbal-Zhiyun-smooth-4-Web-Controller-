🎥 Zhiyun Smooth 4 Web BLE Controller
A complete, modern Web App to remotely control the Zhiyun Smooth 4 gimbal via Bluetooth (Web BLE API).

Includes Gesture Control, Motion Routines, Air Mouse, Voice Commands, and Motion Tracking directly from any browser!

🚀 LIVE DEMO / PROVALO SUBITO
Non serve installare nulla! Se vuoi provare l'applicazione subito dal tuo telefono o PC, apri il link qui sotto:

👉 https://superlative-pavlova-45e320.netlify.app

📑 Technical Documentation / Documentazione Tecnica
Technical notes for developers and makers who want to understand, test, or extend the Bluetooth protocol for the Zhiyun Smooth 4.

Legenda:

✅ Verified / Verificato

🟡 Likely / Probabile

❓ Not Verified / Non verificato

1. 📡 Bluetooth Connection
Service UUID: fee9 (0000fee9-0000-1000-8000-00805f9b34fb) 🟡

(Standard service used by Weebill-S and Smooth 4 family)

Write Characteristic: d44bc439-abfd-45a2-b575-925416129600 ✅

(Primary command handle 0x0012, writeValueWithoutResponse)

Notifications Characteristic: d44bc439-abfd-45a2-b575-925416129601... ✅

(Status and telemetry notifications)

⚠️ Important: The gimbal accepts only one Bluetooth connection at a time. Make sure to close the official Zhiyun ZY Cami/Play app before connecting via browser.

2. 📦 Message Packet Structure
Packets consist of a 14-byte binary payload structured as follows:

Plaintext
24 3c | LL LL | 18 12 | SS | 01 | CC | D0 D1 D2 | RR RR
  ^       ^       ^     ^    ^    ^       ^          ^
Header Length  Fixed Count Src  Cmd    Data      CRC16
Byte Breakdown
Byte 0-1 Header: 24 3c (Magic bytes / Start of Packet) ✅

Byte 2-3 Length: 08 00 (Payload size in Little-Endian, 8 bytes) ✅

Byte 4-5 Fixed: 18 12 (Fixed control header) ✅

Byte 6 Counter: SS (Incremental sequence number, prevents duplicate packet execution) 🟡

Byte 7 Source: 01 (App to Gimbal. Replies use 10) ✅

Byte 8 Command: CC (Axis or system command code) ✅

Byte 9-11 Data: D0 D1 D2 (Motion data format: 10 + 16-bit Little-Endian position value) ✅

Byte 12-13 CRC16: RR RR (CRC16-XMODEM calculated across bytes 4 to 11 in Little-Endian) ✅

⚙️ CRC16 Calculation
Algorithm: CRC16 XMODEM (Polynomial 0x1021, Initial Value 0x0000, No Reflection, XOR Out 0x0000).

Scope: Calculated strictly over the 8 payload bytes (18 12 through data byte D2).

JavaScript
// JavaScript implementation
function crc16(bytes) {
  let c = 0;
  for (const b of bytes) {
    c ^= b << 8;
    for (let i = 0; i < 8; i++) {
      c = (c & 0x8000) ? ((c << 1) ^ 0x1021) & 0xFFFF : (c << 1) & 0xFFFF;
    }
  }
  return c;
}
Python
# Python implementation
def crc16(data, crc=0):
    for b in data:
        crc ^= b << 8
        for _ in range(8):
            crc = ((crc << 1) ^ 0x1021) & 0xFFFF if crc & 0x8000 else (crc << 1) & 0xFFFF
    return crc
🔍 Verified Payload Examples
Neutral / Stop (2048):

243c08001812730102100008d87a (Counter: 73, Command: 01)

Fast Up (3796):

243c0800181274010310d40e087d (Counter: 74, Command: 01)

Fast Down (300):

243c080018123c0101102c01b59c (Counter: 3c, Command: 01)

3. 🕹️ Axis & Movement Control
Command 01 — Tilt Axis (Up / Down) ✅

Command 02 — Pan Axis (Left / Right) 🟡

Command 03 — Roll Axis (Clockwise / Counter-Clockwise) 🟡

Movement Range & Speed
Raw Value Range: 0 to 4095

Neutral Point: 2048 (Motors stationary)

Active Direction: 300 (Max Reverse) ⟵ 2048 ⟶ 3796 (Max Forward)

💡 Keep-Alive Loop Requirement: The gimbal expects a continuous stream of motor update packets. A single command packet will only move the motor for a fraction of a second. To keep the gimbal moving continuously, repeat the current axis state every ~50ms to 70ms. Send 2048 to stop.

4. 🧪 Extended Command Codes (Experimental) ❓
Commands reverse-engineered from the Zhiyun Weebill-S protocol stack (same family). For read operations, set data bytes to 00 00 00.

04 — Read Software / Firmware Version

06 — Read Battery Level (0 - 100%)

22 — Read Current Tilt Angle Position

23 — Read Current Roll Angle Position

24 — Read Current Pan Angle Position

27 — Set Mode (Data: 80 MM 00 | MM = 00:PF, 01:L, 02:F, 03:POV)

5. 🤝 How to Contribute & Verify
Open the Live Demo.

Connect your Zhiyun Smooth 4 and open the Diagnostic Console.

Perform test movements or queries and copy the generated TX / RX log lines.

Submit a GitHub Issue or Pull Request with your findings, firmware version, and device logs!

📚 References & Credit
Karim Jedda: Controlling a Zhiyun Smooth 4 gimbal with Raspberry Pi (Medium Guide)

Peter Maguire: Zhiyun Weebill-S BLE Protocol Analysis & Bleebil Repo (Blog | GitHub)
