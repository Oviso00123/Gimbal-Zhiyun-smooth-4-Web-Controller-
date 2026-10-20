Zhiyun Smooth 4 Web BLE Controller🚀 LIVE DEMO / PROVALO SUBITONon serve installare nulla! Se vuoi provare l'applicazione subito dal tuo telefono o PC, apri il link:👉 https://superlative-pavlova-45e320.netlify.app📑 Technical Documentation / Documentazione TecnicaTechnical notes for developers and makers who want to understand, test, or extend the Bluetooth protocol for the Zhiyun Smooth 4.Legend: ✅ Verified · 🟡 Likely · ❓ Not Verified1. 📡 Bluetooth ConnectionParameterValue / UUIDStatusDescriptionServicefee9 (0000fee9-0000-1000-8000-00805f9b34fb)🟡Standard service used by Weebill-S and Smooth 4 familyWrite Characteristicd44bc439-abfd-45a2-b575-925416129600✅Primary command handle (0x0012, writeValueWithoutResponse)Notificationsd44bc439-abfd-45a2-b575-925416129601...✅Status and telemetry notifications⚠️ Important: The gimbal accepts only one Bluetooth connection at a time. Make sure to close the official Zhiyun ZY Cami/Play app before connecting via browser.2. 📦 Message Packet StructurePackets consist of a 14-byte binary payload structured as follows:Plaintext┌────────┬─────────┬────────┬─────────┬────────┬─────────┬──────────────┬────────────────────┐
│ Header │ Payload │ Fixed  │ Counter │ Source │ Command │     Data     │  CRC16 (Little-E)  │
│ 24 3c  │  08 00  │ 18 12  │   SS    │   01   │   CC    │  D0 D1 D2    │       RR RR        │
└────────┴─────────┴────────┴─────────┴────────┴─────────┴──────────────┴────────────────────┘
BytesFieldMeaning & ValueStatus0 - 1HeaderMagic bytes: 24 3c (Start of Packet)✅2 - 3LengthPayload size (Little-Endian). Standard: 08 00 (8 bytes)✅4 - 5FixedFixed control header: 18 12✅6CounterIncremental sequence number (SS). Prevents duplicated packet execution🟡7SourceOrigin identifier: 01 (App to Gimbal). Replies use 10✅8CommandAxis or system command code (CC)✅9 - 11DataMotion data format: 10 + 16-bit Little-Endian position value✅12 - 13CRC16CRC16-XMODEM calculated across bytes 4 to 11 (Little-Endian)✅⚙️ CRC16 CalculationAlgorithm: CRC16 XMODEM (Polynomial 0x1021, Initial Value 0x0000, No Reflection, XOR Out 0x0000).Scope: Calculated strictly over the 8 payload bytes (18 12 through data byte D2).JavaScript// JavaScript implementation
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
🔍 Verified Payload ExamplesHex Packet PayloadCounter (SS)Command (CC)Decoded ValueAxis State243c08001812730102100008d87a73012048 (0x0800)Neutral / Stop243c0800181274010310d40e087d74013796 (0x0ED4)Up (Fast)243c080018123c0101102c01b59c3c01300 (0x012C)Down (Fast)3. 🕹️ Axis & Movement ControlCommand Code (CC)Target AxisMovement DirectionStatus01TiltUp / Down✅02PanLeft / Right🟡03RollClockwise / Counter-Clockwise🟡Raw Value Range: 0 to 4095Neutral Point: 2048 (Motors stationary)Active Speeds: 300 (Max Reverse) ⟵ 2048 ⟶ 3796 (Max Forward)💡 Keep-Alive Loop Requirement: The gimbal expects a continuous stream of motor update packets. A single command packet will only move the motor for a fraction of a second. To keep the gimbal moving continuously, repeat the current axis state every ~50ms to 70ms. Send 2048 to stop.4. 🧪 Extended Command Codes (Experimental) ❓Commands reverse-engineered from the Zhiyun Weebill-S protocol stack (same family). For read operations, set data bytes to 00 00 00.Plaintext┌──────┬─────────────────────────────────────────────────────────┐
│ Code │ Description                                             │
├──────┼─────────────────────────────────────────────────────────┤
│  04  │ Software / Firmware Version Read                        │
│  06  │ Battery Level Read (0 - 100%)                           │
│  22  │ Read Current Tilt Angle Position                        │
│  23  │ Read Current Roll Angle Position                        │
│  24  │ Read Current Pan Angle Position                         │
│  27  │ Set Mode (Data: 80 MM 00 | MM = 00:PF, 01:L, 02:F, 03:POV)│
└──────┴─────────────────────────────────────────────────────────┘
5. 🤝 How to Contribute & VerifyOpen the Live Demo.Connect your Zhiyun Smooth 4 and open the Diagnostic Console.Perform test movements or queries and copy the generated TX / RX log lines.Submit a GitHub Issue or Pull Request with your findings, firmware version, and device logs!📚 References & CreditKarim Jedda: Controlling a Zhiyun Smooth 4 gimbal with Raspberry Pi (Medium Guide)Peter Maguire: Zhiyun Weebill-S BLE Protocol Analysis & Bleebil Repo (Blog | GitHub)
