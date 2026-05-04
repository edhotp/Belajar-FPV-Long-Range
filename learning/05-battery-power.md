# Modul 5 — Battery & Power System

> **Tujuan modul:** memahami kimia battery, cara baca spec, perhitungan waktu terbang, dan kenapa **Li-Ion** jadi standar LR.

---

## 5.1 Konsep Dasar

```mermaid
flowchart LR
    Batt[Battery] --> V[Voltage<br/>Volt = tekanan listrik]
    Batt --> C[Capacity<br/>mAh / Ah = jumlah listrik]
    Batt --> P[Power<br/>Watt = V × A]
    Batt --> E[Energy<br/>Wh = V × Ah]
    Batt --> Disc[Discharge Rate<br/>C-rating]
```

### Istilah penting
| Istilah | Arti | Contoh |
|---|---|---|
| **S** | Cell in series | 6S = 6 sel berderet |
| **P** | Parallel | 2P = 2 sel paralel (capacity ×2) |
| **mAh** | Capacity | 4200 mAh = 4.2 Ah |
| **C-rating** | Max discharge | 30C × 4.2Ah = 126A max |
| **Wh** | Energy | 21.6V × 8.4Ah = 181 Wh |

---

## 5.2 LiPo vs Li-Ion vs LiHV

```mermaid
flowchart TD
    Chem[Battery FPV] --> LiPo[LiPo<br/>Lithium Polymer]
    Chem --> Liion[Li-Ion<br/>Lithium Ion silinder]
    Chem --> LiHV[LiHV<br/>High Voltage LiPo]
    
    LiPo --> P1[+ Discharge tinggi 50-150C]
    LiPo --> P2[+ Ringan]
    LiPo --> P3[- Energy density rendah]
    LiPo --> P4[- Cepat habis]
    LiPo --> Use1[Racing, Freestyle]
    
    Liion --> I1[+ Energy density 2-3x LiPo]
    Liion --> I2[+ Lifecycle panjang 500+ cycles]
    Liion --> I3[- Discharge rendah 10-20C]
    Liion --> I4[- Berat per sel]
    Liion --> Use2[Long Range]
    
    LiHV --> H1[+ Voltase puncak 4.35V/cell]
    LiHV --> H2[+ Sedikit lebih powerful]
    LiHV --> H3[- Cycle life lebih pendek]
    LiHV --> Use3[Mini-LR, racing tertentu]
```

### Tegangan nominal vs full vs empty
| Kimia | Empty (datasheet) | **Practical Cut-off** | Nominal | Full |
|---|---|---|---|---|
| LiPo | 3.30 V/cell | **3.50 V/cell** | 3.70 V/cell | 4.20 V/cell |
| Li-Ion (NMC) | 2.50 V/cell | **3.30 V/cell** | 3.60 V/cell | 4.20 V/cell |
| LiHV | 3.40 V/cell | **3.60 V/cell** | 3.80 V/cell | 4.35 V/cell |

> **Penting:** kolom *Empty* adalah batas teoretis dari datasheet. Untuk **kesehatan baterai jangka panjang**, pakai **Practical Cut-off** sebagai trigger RTH/landing. Li-Ion yang sering didischarge ke 2.5V akan cepat rusak (capacity drop, internal resistance naik). Set `vbat_min_cell_voltage = 3.30V` di Betaflight (lihat [Modul 7](07-failsafe-gps-rescue.md)).

---

## 5.3 Kimia Li-Ion Populer untuk LR (2026)

| Cell | Capacity | Max Discharge | Berat | Catatan |
|---|---|---|---|---|
| **Molicel P42A** | 4200 mAh | 35–45A | 70 g | **Standar industri LR** |
| **Molicel P45B** | 4500 mAh | 45A | 70 g | Upgrade P42A |
| **Samsung 50S** | 5000 mAh | 25A | 70 g | Capacity tinggi |
| **Samsung 40T** | 4000 mAh | 35A | 67 g | Klasik LR |
| **LG M50LT** | 5000 mAh | 7.3A | 70 g | Capacity max, discharge rendah |

### Konfigurasi pack populer
```mermaid
flowchart LR
    A[6S2P P42A] --> A1[6 series x 2 paralel<br/>= 12 cell]
    A1 --> A2[V: 21.6V nom 25.2V max]
    A1 --> A3[Capacity: 8.4Ah 8400mAh]
    A1 --> A4[Energy: 181 Wh]
    A1 --> A5[Berat: ~840g]
    A1 --> A6[Max discharge: ~70A]
```

---

## 5.4 Perhitungan Waktu Terbang

### Rumus dasar
$$
T_{\text{terbang (menit)}} \approx \frac{C_{\text{Wh}}}{P_{\text{avg (W)}}} \times 60 \times \eta
$$

- $C_{\text{Wh}} = V_{\text{nom}} \times C_{\text{Ah}}$
- $P_{\text{avg}}$ = daya rata-rata cruise
- $\eta$ ≈ 0.85 (jangan kosongkan 100%)

### Contoh: 7" LR dengan 6S2P P42A
- Energy: 21.6V × 8.4Ah = **181 Wh**
- Cruise power: ~130 W (efisien, throttle 30–40%)

$$
T \approx \frac{181}{130} \times 60 \times 0.85 \approx 71 \text{ menit teoretis}
$$

> Realistis **45–55 menit** dengan margin RTH.

### Cara mengukur cruise power kamu
1. Pasang **current sensor** di FC (BF: `set ibat_scale=...`).
2. Terbang cruise stabil 5 menit.
3. Lihat **W rata-rata** di OSD atau blackbox.

---

## 5.5 Memilih Battery untuk Build Kamu

```mermaid
flowchart TD
    Q1{Misi Anda?} --> Race[Racing]
    Q1 --> Free[Freestyle]
    Q1 --> LRshort[LR 5-15km]
    Q1 --> LRlong[LR 20km+]
    
    Race --> RB[LiPo 6S 1100-1300mAh<br/>120C]
    Free --> FB[LiPo 6S 1300-1500mAh<br/>100C]
    LRshort --> SB[Li-Ion 6S2P P42A<br/>180Wh]
    LRlong --> LB[Li-Ion 6S3P / 6S4P<br/>270-360Wh atau LiPo high cap]
```

### Aturan emas untuk LR
1. **Power-to-weight** sweet spot: drone bisa hover di **50% throttle** dengan battery terpasang.
2. Kalau hover < 30% throttle: terlalu ringan → boros / unstable.
3. Kalau hover > 60%: terlalu berat → tidak akan efisien.

---

## 5.6 Charger & Storage

### Charger yang direkomendasikan
- **ISDT Q8 Max** (1000W, support semua kimia).
- **HOTA D6 Pro / D6+** (650W dual port).
- **ToolkitRC M6D / M8** (mid-range, dual port).

### Mode charging
```mermaid
flowchart LR
    A[Charge Mode] --> Bal[Balance Charge<br/>setara semua cell]
    A --> Fast[Fast Charge<br/>tanpa balance, riskan]
    A --> Stor[Storage Mode<br/>3.80V/cell, untuk simpan >2 hari]
    A --> Disc[Discharge<br/>untuk simpan jangka panjang]
```

### Aturan charging Li-Ion (BERBEDA dari LiPo!)
- **Max charge rate: 1C** (P42A 4.2Ah → **4.2A max aman**).
- **Stop di 4.20V/cell** (tidak boleh overcharge).
- **Storage: 3.70–3.80 V/cell**.

> 🚨 **WARNING SAFETY**: 1C adalah **batas aman maksimum** untuk Li-Ion silinder. **JANGAN charge melebihi 1C** kecuali kamu paham thermal management & punya monitoring suhu sel. Charging > 1C bisa menyebabkan **thermal runaway, ledakan, atau kebakaran** terutama pada pack rakitan rumahan dengan spot welding sub-optimal. Untuk pemula: stick di **0.5–1C saja**.

> **JANGAN** charge Li-Ion seperti LiPo cepat 5C. Bisa **terbakar / meledak**.

---

## 5.7 Konektor

| Konektor | Current | Cocok untuk |
|---|---|---|
| **XT30** | 15–30A | Tiny whoop, 3" mini |
| **XT60** | 60A continuous | **Standar 5"–7" LR** |
| **XT90 / AS150** | 90–150A | Cinelifter, 10" heavy |
| **MR30** | 30A | Mini freestyle |

### Wiring
- **AWG 12 atau 14** untuk power 6S (jangan terlalu kecil).
- **Solder bersih**, tidak ada cold joint.
- **Heat shrink** semua sambungan.

---

## 5.8 Battery Safety

```mermaid
flowchart TD
    Safe[Safety Battery] --> Store[Storage]
    Safe --> Use[Penggunaan]
    Safe --> Damage[Damage Handling]
    
    Store --> S1[LiPo bag tahan api]
    Store --> S2[Storage voltage 3.80V/cell]
    Store --> S3[Suhu kamar, kering]
    
    Use --> U1[Cek puffed/swollen sebelum pakai]
    Use --> U2[Jangan melebihi C-rating]
    Use --> U3[Jangan over-discharge < 3.0V/cell]
    
    Damage --> D1[Crash keras: cek puffed]
    Damage --> D2[Bocor / asap: kubur di pasir/garam, jangan air]
    Damage --> D3[Disposal: kosongkan via discharge mode]
```

### Tanda battery rusak
- **Puffed (gembung)** → buang.
- **Voltase tidak balance > 0.1V antar cell** → cek atau buang.
- **Internal resistance naik drastis** (cek di charger) → mendekati end-of-life.
- **Suhu naik abnormal saat charge** → bahaya, hentikan.

### Crash protocol
1. Setelah crash keras, **diamkan battery 15 menit** sebelum disentuh.
2. Cek visual (puffed? bocor cairan?).
3. Kalau aman, charge perlahan & monitor suhu.
4. Kalau ragu — **buang dengan benar**.

---

## 5.9 Voltage Sag & ESR

**Voltage sag** = drop voltase saat draw ampere tinggi (contoh punch throttle).

$$
V_{\text{sag}} = V_{\text{rest}} - (I \times \text{ESR}_{\text{total}})
$$

- **ESR (Internal Resistance)** kecil = sag kecil.
- Li-Ion ESR lebih tinggi dari LiPo → **lebih banyak sag** saat punch.

### Implikasi untuk LR
- Jangan **full throttle berturut-turut** dengan Li-Ion (akan sag drastis).
- **Cruise smooth** — power demand stabil 100–150W.
- Set **min cell voltage** di Betaflight 3.3 V/cell (Li-Ion) untuk RTH.

---

## 5.10 Power System Wiring (Big Picture)

```mermaid
flowchart LR
    Batt[6S Battery<br/>22.2V nom] --> XT60
    XT60 --> ESC[4-in-1 ESC<br/>+ Capacitor]
    ESC -- VBAT --> FC[Flight Controller]
    ESC -- BEC 5V --> FC
    ESC -- BEC 9V/12V --> VTX[VTX]
    FC -- 5V --> RX[ELRS RX]
    FC -- 5V --> GPS[GPS module]
    FC -- 5V --> Cam[Camera]
```

### Capacitor wajib!
**Pasang capacitor low-ESR** (35V, 1000–2200µF) di pad battery:
- Mengurangi voltage spike (lindungi VTX & FC).
- **Wajib** untuk DJI O3/O4 (DJI rekomendasikan!).
- **Wajib** untuk semua build 6S.

---

## 🔗 Referensi

- Molicel datasheet — <https://www.molicel.com/product/>
- Oscar Liang — *Li-Ion vs LiPo for FPV* — <https://oscarliang.com/li-ion-vs-lipo/>
- ChrisRosser — *Long Range FPV Battery Guide* (YouTube).
- Jeff Bourke — *Li-Ion Pack Building Guide* (YouTube).

---

**Selanjutnya** ➡️ [Modul 6: Build & Setup Pertama](06-build-setup.md)
