# Modul 4 — Sistem Video: Analog, DJI O4, Walksnail, HDZero

> **Tujuan modul:** memahami pilihan sistem video FPV terbaru (2026), kelebihan/kekurangan, dan cara memilih untuk LR.

---

## 4.1 Big Picture: Bagaimana Video FPV Bekerja?

```mermaid
flowchart LR
    Cam[Kamera FPV] --> VTX[Video Transmitter<br/>VTX]
    VTX -- gelombang radio --> VRX[Video Receiver<br/>VRX di Goggles]
    VRX --> Display[Layar Goggles<br/>OLED / Micro-OLED]
    Display --> Eye[Mata Pilot]
```

3 hal yang **paling penting** dalam sistem video LR:
1. **Latency** — delay dari objek di depan kamera sampai muncul di mata pilot.
2. **Range** — jarak maksimum sebelum video pecah/freeze.
3. **Graceful degradation** — apa yang terjadi saat sinyal jelek?

---

## 4.2 Dua Filosofi: Analog vs Digital

```mermaid
flowchart TD
    Video[Video FPV] --> Analog[Analog<br/>5.8 GHz CVBS]
    Video --> Digital[Digital<br/>HD ke goggles]
    
    Analog --> A1[+ Latency super rendah <20ms]
    Analog --> A2[+ Murah]
    Analog --> A3[+ Graceful: pecah pelan-pelan]
    Analog --> A4[- Resolusi rendah CVBS 480p]
    Analog --> A5[- Noise statis]
    
    Digital --> D1[+ HD 1080p, jernih]
    Digital --> D2[+ Recording lokal kualitas tinggi]
    Digital --> D3[- Latency lebih tinggi 20-40ms]
    Digital --> D4[- Cliff effect: tiba-tiba freeze]
    Digital --> D5[- Lebih mahal]
```

### Kapan pilih Analog?
- Budget terbatas.
- Suka **feel klasik** (banyak pilot LR veteran tetap analog).
- Mau frekuensi 1.2 GHz untuk extreme LR (legal di beberapa negara).

### Kapan pilih Digital HD?
- Mau pengalaman immersive HD.
- Suka filming/cinematic.
- Punya budget lebih.

---

## 4.3 Tabel Lengkap Sistem Video 2026

| Sistem | Resolusi | FPS | Latency | Range Tipikal* | Harga (USD) |
|---|---|---|---|---|---|
| **Analog 5.8 GHz** | 480p CVBS | – | <20 ms | 5–20 km dgn patch | $30–80 (VTX) |
| **Analog 1.2/1.3 GHz** | 480p CVBS | – | <20 ms | 30–60+ km | $80–150 (butuh izin) |
| **DJI O3 Air Unit** | 1080p | 100 | 30–40 ms | 8–20+ km | $230 |
| **DJI O4 Air Unit Pro** | 1080p | 100 | ~22 ms | 15–30+ km | $230 |
| **DJI O4 Air Unit Lite** | 1080p | 60 | ~28 ms | 8–15 km | $160 |
| **Walksnail Avatar HD Pro** | 1080p | 100 | ~18 ms | 10–20+ km | $180–220 |
| **Walksnail Moonlight\*\*** | 1080p / 4K record | 100 | ~22 ms | 10–20+ km | $250 |

\*\*Walksnail Moonlight: spec & harga berdasarkan announcement 2025. Verifikasi availability & spec final saat membeli.
| **HDZero LR Whoop V2** | 720p | 60–90 | <20 ms | 5–15 km | $130–180 |
| **HDZero Race V3** | 720p | 90 | <20 ms | 3–8 km | $130 |
| **Caddx Vista (legacy)** | 720p | 60 | ~28 ms | 4–8 km | $150 (used) |

\*Range bergantung pada antena, lingkungan, ketinggian, dan power output.

---

## 4.4 DJI O4 Air Unit Family (Terbaru 2024–2026)

DJI rilis **O4 generation** sebagai pengganti O3, dengan dua varian.

```mermaid
flowchart LR
    O4[DJI O4 Family] --> Pro[O4 Air Unit Pro]
    O4 --> Lite[O4 Air Unit Lite]
    
    Pro --> P1[1080p 100fps]
    Pro --> P2[E2E latency ~22ms]
    Pro --> P3[Dual antena coax + dipole]
    Pro --> P4[Output up to 1W region dependent]
    Pro --> P5[Built-in gyroflow stabilization]
    Pro --> P6[Kamera lebih besar, low-light bagus]
    
    Lite --> L1[1080p 60fps]
    Lite --> L2[Latency ~28ms]
    Lite --> L3[MIPI camera kecil]
    Lite --> L4[Lebih ringan & murah]
    Lite --> L5[Cocok 3-5 inch & whoops]
```

### Kompatibilitas Goggles
| Goggles | O3 | O4 |
|---|---|---|
| Goggles 2 / Integra | ✅ | ✅ (firmware update) |
| Goggles 3 | ✅ | ✅ (native) |
| **Goggles N3** (2024, terjangkau) | ✅ | ✅ (native) |
| Goggles V2 (lama) | ❌ | ❌ |

### Kenapa O4 Pro untuk LR?
- **Range terbaik di kelasnya** (15–30 km dengan antena bagus).
- **Dual diversity antena** (coax + dipole) bawaan.
- **Recording 1080p 100fps** ke microSD lokal.
- **Compatible Goggles 3** — display micro-OLED terbaik di pasaran.

> **Catatan:** DJI O3/O4 punya **DRM lock** ke akun & region. Pastikan beli unit dengan firmware region kamu.

---

## 4.5 Walksnail Avatar HD Pro & Moonlight

Saingan utama DJI di segmen HD digital.

### Walksnail Avatar HD Pro Kit (2024)
- 1080p 100 fps, latency ~18 ms (bersaing dengan DJI O4).
- **True diversity 2 antena RX** di goggles.
- Open ecosystem — banyak vendor 3rd party (Caddx, Foxeer, Speedybee).

### Walksnail Moonlight (2025)
- Kamera **Starlight low-light sensor** — terbang dini hari/sore tetap jelas.
- **Recording 4K 60 fps** di SD card lokal — kualitas mendekati GoPro mini.
- Latency ~22 ms.

```mermaid
flowchart LR
    Choose{Pilih HD VTX?} --> Q1{Budget?}
    Q1 -->|$160| Lite[DJI O4 Lite]
    Q1 -->|$180-230| Mid{Eko-sistem?}
    Mid -->|DJI Goggles| O4Pro[DJI O4 Pro]
    Mid -->|Open / mau ganti goggles| Avatar[Walksnail Avatar HD Pro]
    Q1 -->|$250+| Q2{Butuh low-light?}
    Q2 -->|Ya| Moon[Walksnail Moonlight]
    Q2 -->|Tidak, butuh range max| O4Pro
```

---

## 4.6 HDZero — Open Digital

**HDZero** = digital tapi dengan filosofi seperti analog: **low latency, graceful degradation**.

| Varian | Cocok untuk |
|---|---|
| HDZero Race V3 | Racing, freestyle |
| HDZero Freestyle V2 | Freestyle |
| **HDZero LR Whoop V2 / Mini LR** | **Long Range** |

### Kelebihan
- **Latency < 20 ms** (cocok untuk pilot suka feel responsif).
- **Gradual degradation** (tidak cliff freeze seperti DJI/Walksnail).
- Open ecosystem, banyak antena & VTX.

### Kekurangan
- Resolusi 720p (bukan 1080p).
- Range lebih rendah dari DJI O4.

---

## 4.7 Goggles Populer 2026

```mermaid
flowchart TD
    G[Goggles] --> Digital[Digital]
    G --> Analog[Analog]
    G --> Hybrid[Analog + Module Bay]
    
    Digital --> DJI3[DJI Goggles 3<br/>$500, dual micro-OLED]
    Digital --> DJIN3[DJI Goggles N3<br/>~$230, single LCD<br/>O4-only]
    Digital --> WSX[Walksnail Avatar Goggles X<br/>$400]
    Digital --> WSL[Walksnail Avatar Goggles L<br/>~$300, single panel]
    Digital --> HDZG[HDZero Goggles<br/>$500]
    Digital --> FatHDO3[Fatshark Dominator HDO3<br/>$500, OLED 1080p<br/>Walksnail-compatible]
    
    Analog --> Orqa[Orqa FPV.One<br/>$400-700]
    Analog --> Skyz04X[Skyzone 04X<br/>analog OLED]
    
    Hybrid --> Skyzone[Skyzone Cobra X<br/>~$300, bay untuk HDZ/Walksnail]
```

### Rekomendasi pemula
- **Budget tipis + HD:** DJI Goggles N3 (~$230, single panel LCD; hanya kompatibel dengan DJI O4 Air Unit / Avata 2 / Neo).
- **All-in untuk LR HD:** DJI Goggles 3 (dual micro-OLED 100Hz, kompat O3 & O4).
- **Hybrid (mau analog + HD module):** Skyzone Cobra X.
- **Analog purist:** Orqa FPV.One atau Skyzone 04X.

> ⚠️ **Disclaimer rekomendasi:** pilihan goggles di atas adalah opini berdasarkan kebiasaan komunitas & review publik per **2026**, **bukan endorsement berbayar**. Harga adalah perkiraan retail global (USD) yang bisa bervariasi per region & toko. Kenyamanan goggles **sangat personal** (IPD, jarak mata, bentuk wajah) — kalau memungkinkan, **coba langsung** atau pinjam dulu sebelum beli. Pastikan juga goggles cocok dengan ekosistem VTX yang kamu pakai (DJI O3/O4, Walksnail, HDZero, analog). DJI Goggles N3 **tidak kompatibel dengan O3 Air Unit lama** — hanya O4 / Avata 2 / Neo.

---

## 4.8 Antena Video LR

```mermaid
flowchart TD
    A[Antena Video] --> Drone[Sisi Drone Omni]
    A --> Goggles[Sisi Goggles]
    
    Drone --> CP[Circular Polarized<br/>Pagoda, AXII, Lollipop]
    Drone --> Lin[Linear<br/>untuk DJI O4 dipole]
    
    Goggles --> Patch[Patch Directional<br/>8-13 dBi]
    Goggles --> Omni[Omni untuk diversity]
```

### Setup goggles diversity yang umum
- **Patch directional** di kiri/depan (gain tinggi, fokus ke arah drone).
- **Omni circular** di kanan/belakang (cover semua arah).
- Goggles otomatis switch ke yang sinyal lebih kuat.

### Tips antena DJI O4 / Walksnail
- Pakai **dua antena bawaan beda orientasi** (coax + dipole vertikal).
- **Jangan satukan** dua antena di lokasi yang sama (multipath kanselasi).
- Pastikan **konektor MMCX/U.FL** terpasang kencang — sering lepas saat crash.

---

## 4.9 Power Output VTX & Regulasi

| Region | Max VTX power 5.8 GHz |
|---|---|
| Indonesia (umum) | 25 mW (legal); 200 mW–1 W common di praktik LR |
| EU | 25 mW (CE); pita amatir bisa lebih |
| USA (HAM license) | up to 1.5 kW di 5.8 GHz amatur |
| Free-fly area | tergantung lokal |

> **Patuhi regulasi & jangan ganggu pilot lain di airfield.** Naikkan power **bertahap** sesuai kebutuhan.

---

## 4.10 OSD (On-Screen Display)

```mermaid
flowchart LR
    FC[Flight Controller] --> OSD[OSD chip<br/>MAX7456 atau BF OSD]
    OSD --> Vid[Video stream]
    Vid --> Goggles
```

OSD menampilkan info di atas video:
- **RSSI / LQ** (RC link quality).
- **Voltase battery & mAh consumed**.
- **GPS jarak ke home & arah**.
- **Altitude, speed, heading**.
- **Flight timer**.

### Yang **wajib** ada di OSD untuk LR
1. RSSI / LQ
2. Voltase per cell
3. Jarak ke home
4. Arah panah ke home (home arrow)
5. Flight time
6. GPS satelit count
7. **Warning flag** (low voltage, low LQ)

---

## 🔗 Referensi

- DJI O4 Air Unit Pro/Lite — <https://www.dji.com/o4-air-unit-pro>
- Walksnail — <https://www.walksnail.com/>
- HDZero — <https://www.hd-zero.com/>
- Oscar Liang — *FPV Video Systems Comparison* — <https://oscarliang.com/fpv-video-system/>
- Joshua Bardwell — *DJI O4 Review* (YouTube).

---

**Selanjutnya** ➡️ [Modul 5: Battery & Power System](05-battery-power.md)
