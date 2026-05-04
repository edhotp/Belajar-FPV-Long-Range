# Modul 8 — First Flight & Tuning

> **Tujuan modul:** sukses melewati maiden flight tanpa crash dan paham dasar tuning PID + filter.

---

## 8.1 Sebelum Maiden — Final Checklist

```mermaid
flowchart TD
    A[Pre-Maiden Check] --> B[Battery fresh charged + balance]
    B --> C[Prop kencang, arah benar, tidak crack]
    C --> D[GPS sat >= 10, home lock]
    D --> E[Failsafe stage 2 = GPS Rescue]
    E --> F[Voltage failsafe armed]
    F --> G[Telemetry RSSI/LQ stabil]
    G --> H[OSD warna terbaca]
    H --> I[Mag terkalibrasi pagi itu]
    I --> J[Cuaca: angin < 15 km/h, no rain]
    J --> K[Lokasi: lapangan terbuka 200m radius]
    K --> L[Punya observer]
    L --> READY[Siap Maiden]
```

> **Hindari maiden flight di area kecil, hari pertama beli, sore mau gelap.** Beri diri kamu **kondisi optimal** untuk sukses pertama kali.

---

## 8.2 Maiden Flight Protocol

```mermaid
sequenceDiagram
    participant Pilot
    participant Drone
    
    Pilot->>Drone: Power on, tunggu beep stable
    Pilot->>Drone: GPS Lock confirmed
    Pilot->>Drone: Arm di mode ANGLE / HORIZON
    Pilot->>Drone: Spool up perlahan
    Drone->>Pilot: Lift off 30 cm hover
    Pilot->>Drone: Hold 5 detik, cek getaran
    Pilot->>Drone: Naik 2 m, hover lagi
    Pilot->>Drone: Sedikit roll/pitch ringan
    Pilot->>Drone: Land, disarm
    Note over Pilot,Drone: Cek hardware: motor panas? screws kendur?
```

### Mode terbang pertama
- **Pemula: ANGLE mode** (drone auto-level, tidak bisa flip).
- **Setelah nyaman: HORIZON** (level + bisa flip dengan stick max).
- **ACRO** (full manual) → **setelah 20+ jam simulator**.

---

## 8.3 Memahami Mode Terbang

```mermaid
flowchart LR
    Mode[Flight Modes] --> Angle[ANGLE<br/>auto-level, max tilt 45°]
    Mode --> Horizon[HORIZON<br/>level + flip on max stick]
    Mode --> Acro[ACRO / RATE<br/>full manual, no level]
    Mode --> Pos[POSHOLD<br/>iNav: hold position GPS]
    Mode --> Cruise[CRUISE<br/>iNav: heading hold + auto altitude]
    Mode --> WP[WAYPOINT<br/>iNav: misi pre-program]
    Mode --> RTH[RTH<br/>return to home]
```

### Untuk LR, yang sering dipakai:
- **ANGLE** atau **HORIZON** untuk smooth cinematic.
- **ACRO** untuk freestyle moment.
- **CRUISE / POSHOLD** (iNav) untuk istirahat tangan.
- **RTH** kalau ragu.

---

## 8.4 Tuning Dasar — Apa itu PID?

**PID = Proportional, Integral, Derivative** — algoritma feedback yang bikin drone stabil.

```mermaid
flowchart LR
    Stick[Pilot Stick] --> Setpoint[Setpoint angular rate]
    Gyro[Gyro reading] --> Error["Error = Setpoint - Actual"]
    Error --> P["P: respons proporsional"]
    Error --> I["I: hilangkan steady-state error"]
    Error --> D["D: redam overshoot"]
    P & I & D --> Mix[Sum + Mixer]
    Mix --> ESC[ESC]
    ESC --> Motor
    Motor -.feedback gerakan.-> Gyro
```

| Term | Fungsi | Kalau terlalu tinggi |
|---|---|---|
| **P** | Respons utama | Oscillation cepat |
| **I** | Hilangkan drift | Bouncy / wobble lambat |
| **D** | Redam overshoot | Motor panas, jittery |

---

## 8.5 Tuning Filter (PALING PENTING dulu!)

```mermaid
flowchart TD
    A[Sebelum tuning PID] --> B[Cek noise gyro<br/>blackbox / bidirectional DShot]
    B --> C[Set Dynamic Filter ON]
    C --> D[RPM Filter ON wajib bidirectional DShot]
    D --> E[Notch filter sesuai motor noise]
    E --> F[Lalu tuning P, D, FF]
```

### Default Betaflight 4.5+ untuk 7" LR
Default sudah cukup baik untuk pemula. Yang penting:
- **Bidirectional DShot ON**.
- **RPM Filter ON**.
- **Dynamic Notch ON**.
- **Looptime 4 kHz** (cukup, hemat CPU).

> **Untuk pemula:** **JANGAN otak-atik PID di awal**. Default + filter sudah 90% bagus. Tuning baru perlu kalau ada problem nyata.

> 🚨 **WARNING SERIUS**: Tuning PID tanpa pemahaman dasar bisa menyebabkan **oscillation tak terkendali → crash mahal** dalam hitungan detik di udara. Aturan aman:
> - **0–20 jam terbang**: pakai default firmware, **JANGAN ubah PID**.
> - **20–100 jam**: baru boleh tweak rates & expo (bukan PID).
> - **100+ jam + paham blackbox**: baru pelajari PID/filter tuning.
>
> Builder berpengalaman pun masih sering pakai default Betaflight 4.5+ untuk LR — karena memang sudah bagus.

---

## 8.6 Trim & Calibrate

### Yang harus dikalibrasi sebelum maiden:
1. **Accelerometer** — drone di permukaan datar → tab Setup → Calibrate Accel.
2. **Magnetometer (mag)** — putar drone semua arah → Calibrate Mag.
3. **ESC throttle range** — Bluejay/BLHeli throttle calibration.
4. **Subtrim** — kalau drone drift sedikit, trim di radio (bukan PID!).

### Stick rates
Pemula: pakai **rates default RC link**, pelan-pelan naikkan kalau sudah nyaman.

---

## 8.7 Membaca OSD

```mermaid
flowchart TD
    OSD[OSD Layout LR] --> Top[Atas: Heading + GPS sat]
    OSD --> Left[Kiri: Voltage + mAh + Watts]
    OSD --> Right[Kanan: RSSI/LQ + Altitude]
    OSD --> Bot[Bawah: Distance + Home Arrow + Timer]
    OSD --> Center[Tengah: Crosshair + AHI]
```

### Reading OSD saat terbang
- **Glance method**: lihat OSD setiap 5–10 detik.
- Prioritas: **Voltage** (kalau warning, RTH segera) > **LQ** > **Distance** > **GPS arrow** > **Battery used (mAh)**.
- **Aturan 50%**: kalau battery atau jarak sudah 50% target, **mulai pulang**.

---

## 8.8 Crash Protocol

```mermaid
flowchart TD
    Crash[Drone Crash] --> A[Disarm segera]
    A --> B[Cek visual lokasi crash via goggles last frame]
    B --> C[Walk ke lokasi]
    C --> D{Cek kerusakan}
    D --> P["Prop pecah → ganti"]
    D --> M["Motor panas → cek ESC"]
    D --> F["Frame retak → ganti"]
    D --> B2["Battery puffed → buang aman"]
    D --> C2["Camera retak → ganti"]
    D --> Soft["Soft crash → cek baut"]
```

### Kalau drone hilang
1. Cek **last GPS coordinate** di OSD recording (DJI/Walksnail rekam!).
2. Pakai **Find My / tracker** kalau ada.
3. Drone **beeper** akan terus bunyi (kalau lost model active).
4. Cari di sektor downwind (angin sering dorong drone).

---

## 8.9 Logbook & Improvement

> **Catat tiap penerbangan**:
> - Tanggal, lokasi, durasi.
> - Battery yang dipakai (capacity awal/akhir).
> - Cuaca & angin.
> - Catatan tuning / problem.
> - Distance terjauh.

Pakai aplikasi: **Speedybee app**, **Betaflight Blackbox Explorer**, atau spreadsheet manual.

---

## 8.10 Tuning Lanjutan (kalau sudah nyaman)

### Tools
- **Betaflight Blackbox Explorer** — analisis log.
- **PIDtoolbox** — visualisasi noise & PID.
- **Setpoint Response** plot — analisa respons.

### Workflow tuning iteratif
```mermaid
flowchart LR
    A[Test flight + record blackbox] --> B[Buka di Blackbox Explorer / PIDtoolbox]
    B --> C[Cek noise level di gyro]
    C --> D[Adjust filter atau P/D]
    D --> E[Save preset, test lagi]
    E --> A
```

> Ini **advanced**. Pemula bisa skip dan stick dengan default Betaflight 4.5+ — sudah sangat bagus untuk 7" LR.

---

## 📝 Quiz Modul 8

1. Mode terbang apa yang **paling cocok** untuk maiden flight pemula?
2. Apa yang **lebih penting** dituning duluan: PID atau Filter?
3. Apa fungsi **RPM Filter** dan apa prasyaratnya?
4. Apa "**aturan 50%**" untuk terbang LR?
5. Kalau drone crash, apa langkah pertama setelah disarm?

---

## 🔗 Referensi

- Betaflight Tuning Guide — <https://betaflight.com/docs/wiki/tuning>
- Joshua Bardwell — *PID Tuning Master Class* (YouTube playlist).
- UAVTech — *Tuning with PIDtoolbox* (YouTube).
- Chris Rosser — *Long Range Flight Tips* (YouTube).

---

**Selanjutnya** ➡️ [Modul 9: Misi Long Range](09-long-range-mission.md)
