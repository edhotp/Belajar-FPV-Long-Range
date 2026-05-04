# Modul 6 — Build & Setup Pertama

> **Tujuan modul:** panduan praktis merakit drone FPV LR 7" dari nol, lengkap dengan tools, urutan kerja, dan setup software.

---

## 6.1 Shopping List Lengkap (Build 7" LR HD)

| Komponen | Rekomendasi 2026 | Estimasi (USD) |
|---|---|---|
| Frame 7" | iFlight Chimera7 Pro V2 / GepRC Mark5 LR | $80 |
| FC + ESC AIO | SpeedyBee F405 V4 50A AIO | $90 |
| Motor 2806.5 1300KV (×4) | T-Motor F90 / iFlight Xing2 | $80 |
| Propeller HQ 7×4×3 V1S (×8) | HQProp / Gemfan 7035 | $15 |
| RX | RadioMaster RP4TD Dual Band | $40 |
| GPS | Matek M10Q-5883 | $30 |
| VTX HD | DJI O4 Air Unit Pro | $230 |
| Battery | Molicel P42A 6S2P pack | $80 |
| Antena VTX | dipole bawaan + tambahan stub | $0 |
| Antena RX | T-antenna kit | $5 |
| XT60 + AWG14 wire | – | $5 |
| **Total drone-side** | | **±$655** |

### Ground side (sekali beli, pakai bertahun)
| Item | Rekomendasi | USD |
|---|---|---|
| Radio TX | RadioMaster Boxer ELRS Dual Band | $200 |
| Goggles | DJI Goggles 3 / N3 | $300–500 |
| Charger | ISDT Q8 Max + 12V PSU | $150 |

---

## 6.2 Tools yang Dibutuhkan

```mermaid
flowchart LR
    Tools[Tools Wajib] --> Sold[Soldering Iron<br/>TS100/Pinecil]
    Tools --> Solder[Solder + Flux<br/>60/40 atau lead-free]
    Tools --> Hex[Hex Driver Set<br/>1.5/2.0/2.5/3.0mm]
    Tools --> Pliers[Tang + Cutter]
    Tools --> Tweezer[Tweezer ESD]
    Tools --> Multi[Multimeter]
    Tools --> Heat[Heat shrink + heat gun]
    Tools --> Tape[Double tape + zip tie]
    Tools --> Loctite[Loctite 243 blue<br/>untuk motor screws]
```

### Optional tapi sangat membantu
- **Smoke stopper** (sekring 1A untuk first power on, hindari drone meledak).
- **Solder fume extractor**.
- **PCB holder / vise**.

---

## 6.3 Urutan Build (Workflow)

```mermaid
flowchart TD
    A[1. Unbox & verifikasi part] --> B[2. Solder XT60 + capacitor ke ESC]
    B --> C[3. Solder motor wires ke ESC]
    C --> D[4. Test ESC standalone smoke stopper]
    D --> E[5. Mount FC+ESC ke frame]
    E --> F[6. Solder camera + VTX + RX + GPS]
    F --> G[7. Routing kabel rapi + secure dengan zip tie]
    G --> H[8. Mount motor + propeller test direction]
    H --> I[9. Power on + flash firmware]
    I --> J[10. Konfigurasi Betaflight / iNav]
    J --> K[11. Bind RX + test failsafe]
    K --> L[12. Bench test motor direction]
    L --> M[13. Antenna mount terakhir]
    M --> N[14. Pre-flight check]
```

---

## 6.4 Soldering Tips

```mermaid
flowchart LR
    A[Tinning pad dulu<br/>flux + sedikit solder] --> B[Tinning kawat<br/>flux + solder]
    B --> C[Pertemukan,<br/>panaskan bersamaan]
    C --> D[Hasil: shiny smooth<br/>bukan dull lumpy]
```

### Suhu solder
- **Ujung kawat tipis (signal wire):** 280–320°C
- **Pad besar (battery, motor):** 350–400°C
- **Diam terlalu lama** = pad terbakar atau lift!

### Cold joint vs good joint
| Cold (jelek) | Good |
|---|---|
| Dull, abu-abu | Mengkilap silver |
| Bentuk bola | Cone / vulkan |
| Mudah lepas | Solid |

---

## 6.5 Wiring Checklist Khusus DJI O4

```mermaid
flowchart LR
    O4[DJI O4 Air Unit Pro] --> P1[6-pin JST-GH 1.0mm<br/>ke FC]
    P1 --> Pin1[+ Power 7-26V]
    P1 --> Pin2[GND]
    P1 --> Pin3[TX UART]
    P1 --> Pin4[RX UART]
    P1 --> Pin5[SBUS opsional]
    P1 --> Pin6[GND]
```

### Pasang capacitor!
**Wajib:** pasang **35V 1000–2200 µF low-ESR cap** di pad battery sebelum pasang DJI Air Unit. DJI menulisnya di manual resmi — **tanpa cap, VTX bisa rusak**.

---

## 6.6 Software Setup — Betaflight

### Instalasi awal
1. Download **Betaflight Configurator 10.10+** dari <https://betaflight.com>.
2. Connect FC via USB-C.
3. Tab **Firmware Flasher** → pilih target board (mis. `SPEEDYBEEF405V4`) → versi **4.5+** → flash.
4. Reconnect.

### Konfigurasi awal (urutan penting)

```mermaid
flowchart TD
    F1[Ports: enable UART<br/>untuk RX, GPS, VTX] --> F2[Configuration:<br/>set DShot600, ESC protocol]
    F2 --> F3[Receiver: CRSF, mapping channel]
    F3 --> F4[Modes: Arm switch, Beeper, GPS Rescue]
    F4 --> F5[Failsafe: GPS Rescue mode]
    F5 --> F6[Motors: test direction prop OFF]
    F6 --> F7[OSD: enable RSSI, Battery, GPS]
    F7 --> F8[GPS: enable, set baud]
    F8 --> F9[VTX: setup channel via VTX Tables]
    F9 --> F10[Save & Reboot]
```

### Setting penting untuk LR
- **Looptime:** 8 kHz / 4 kHz (default OK).
- **Gyro filter:** default RPM filter on (butuh bidirectional DShot).
- **Min cell voltage:** 3.3V (Li-Ion) atau 3.5V (LiPo).
- **Failsafe stage 2:** GPS Rescue.
- **Beeper:** ON saat failsafe & lost model.

---

## 6.7 Software Setup — iNav (Alternatif untuk LR)

iNav lebih kaya untuk LR: **waypoint mission, fixed wing support, return-to-home matang**.

### Kelebihan iNav untuk LR
- **PosHold mode** tanpa drift bahkan tanpa pilot input.
- **Mission planner** (waypoint).
- **Safehome** (multiple home points).
- **Soaring detection** (untuk efisiensi power).

```mermaid
flowchart LR
    iNav[iNav Configurator] --> Setup[Setup tab: kalibrasi]
    Setup --> Ports[Ports: RX, GPS, VTX, OSD]
    Ports --> Modes[Modes: Angle, NavPosHold, NavRTH, NavWP]
    Modes --> Failsafe[Failsafe: RTH]
    Failsafe --> Mission[Mission Control: waypoint]
```

### Pemula: pilih Betaflight atau iNav?
| Kalau kamu mau... | Pilih |
|---|---|
| Freestyle + sesekali GPS Rescue | Betaflight |
| LR misi waypoint, autonomous | iNav |
| Fixed wing | iNav |
| Acro feel terbaik | Betaflight |

---

## 6.8 Bind ELRS RX

```mermaid
sequenceDiagram
    participant TX as Radio TX (ELRS)
    participant RX as RX di Drone
    
    Note over TX,RX: Flash firmware sama versi (3.x)
    TX->>TX: Lua script: set Bind Phrase
    RX->>RX: Flash dengan Bind Phrase sama
    Note over TX,RX: Power on keduanya
    TX-->>RX: Auto bind via UID hash
    RX-->>TX: Telemetry kembali
    Note over TX,RX: LED RX solid = bound
```

### Tips bind
- **Bind phrase** di TX & RX harus **identik** (case-sensitive).
- Update firmware via **ELRS Configurator** (<https://www.expresslrs.org/quick-start/installing-configurator/>).
- Setelah bind, set **Packet Rate 50 Hz** dan **TX Power 100 mW** untuk start.

---

## 6.9 Test Motor Direction

> **Wajib lepas propeller!** Test direction tanpa prop.

### Aturan motor direction (Betaflight default: "Props Out")
```mermaid
flowchart LR
    M1[Motor 1: Rear Right<br/>CCW] 
    M2[Motor 2: Front Right<br/>CW]
    M3[Motor 3: Rear Left<br/>CW]
    M4[Motor 4: Front Left<br/>CCW]
```

Di tab **Motors** Betaflight:
1. Centang "I understand the risks" (prop OFF!).
2. Slide motor 1 → harus putar CCW.
3. Kalau salah → ubah di tab **Configuration → Motor Direction** atau via BLHeli Configurator.

---

## 6.10 Pre-flight Bench Test

```mermaid
flowchart TD
    A[USB connected, lihat OSD di Goggles] --> B[Cek RSSI/LQ stabil]
    B --> C[GPS satellite count >8]
    C --> D[Battery dipasang, voltage di OSD benar]
    D --> E[Arm switch test: lihat motor beep]
    E --> F[Disarm via switch dan failsafe simulasi]
    F --> G[Walk away 50m: lihat LQ tetap >90%]
    G --> H[Siap maiden flight!]
```

### Test failsafe (PENTING)
1. Arm drone (di bench, prop OFF, dipegang).
2. **Matikan radio TX**.
3. Pastikan motor **stop dalam 1–2 detik** (failsafe stage 1).
4. Untuk LR: aktifkan **GPS Rescue** dan test simulasi (drone dipegang outdoor, GPS lock, switch ke GPS Rescue mode → motor harus mengarahkan ke home).

---

## 📝 Quiz Modul 6

1. Apa fungsi **smoke stopper** dan kapan dipakai?
2. Kenapa **wajib** pasang kapasitor untuk DJI O4?
3. Apa beda Betaflight dan iNav untuk LR?
4. Apa setting failsafe stage 2 yang direkomendasikan untuk LR?
5. Kenapa motor direction harus dites **tanpa propeller**?

---

## 🔗 Referensi

- Betaflight Wiki — <https://betaflight.com/docs>
- iNav Wiki — <https://github.com/iNavFlight/inav/wiki>
- Joshua Bardwell — *FPV Drone Build Tutorial* (YouTube playlist).
- Oscar Liang — *Build a 7 inch Long Range Drone* — <https://oscarliang.com/7-inch-long-range/>

---

**Selanjutnya** ➡️ [Modul 7: Failsafe & GPS Rescue](07-failsafe-gps-rescue.md)
