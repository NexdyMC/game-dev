# Godot 4.4.1: Simple 2D Edge Scrolling System

Repositori ini berisi implementasi dasar pembuatan game 2D di **Godot Engine v4.4.1**, dengan fokus pada pengaturan visual (Sprite2D) dan logika pergerakan kamera berdasarkan posisi kursor mouse (*Edge Scrolling*).

## 🚀 Fitur Utama
- **Visual Optimization:** Pengaturan Sprite2D untuk Pixel Art (Anti-blur).
- **Responsive Camera:** Kamera bergerak otomatis saat mouse menyentuh pinggir layar.
- **Custom Resolution:** Dioptimalkan untuk resolusi desktop **1152 x 648**.

---

## 🛠 Struktur Scene (Node Tree)

Penyusunan node dilakukan secara hierarkis untuk memastikan fungsionalitas yang tepat:
```
Main (Node2D)
├── Background (Sprite2D)
│   └── Transform: Position (576, 324)
└── MainCamera (Camera2D)
└── Script: "res://edge_scroll.gd"
```
---

## ⚙️ Pengaturan Node 2D & Inspector

### 1. Node2D (Root)
Sebagai node utama (Parent), pastikan setting berikut diperhatikan:
* **Transform > Position:** `(0, 0)` agar koordinat anak node tetap konsisten.
* **Process > Mode:** `Inherit` (Default).

### 2. Sprite2D (Visual Optimization)
Agar gambar *pixel art* tidak terlihat buram (blur), gunakan konfigurasi berikut di Inspector:
* **Texture:** Masukkan asset gambar Anda.
* **CanvasItem > Texture > Filter:** Ubah menjadi **`Nearest`**.
* **Offset > Centered:** `On`.
* **Transform > Position:** Sesuaikan dengan titik tengah resolusi (Contoh: `576, 324`).

### 3. Camera2D (Main Camera)
* **Enabled:** Pastikan dalam kondisi `On`.
* **Position:** Atur di tengah layar untuk memulai.
* **Script:** Tempelkan script pergerakan di sini untuk mengontrol `position.x`.

---

## 🖥️ Project Settings (Resolution)

Atur resolusi layar di **Project > Project Settings > Display > Window** untuk hasil terbaik:
* **Viewport Width:** `1152`
* **Viewport Height:** `648`
* **Stretch > Mode:** `canvas_items` (Menjaga ketajaman elemen 2D).
* **Stretch > Aspect:** `keep` (Menghindari distorsi gambar).

---

## 📜 Logika Script (GDScript)

Script ini ditempelkan pada node **Camera2D**. Menggunakan variabel `@export` sehingga kecepatan dapat diatur langsung melalui Inspector.

```gdscript
extends Camera2D

# Pengaturan kecepatan yang bisa diubah di Inspector
@export var speed_low: int = 200    # Kecepatan area kiri (0-200px)
@export var speed_medium: int = 500 # Kecepatan area kanan (925-1152px)

func _process(delta: float):
    # Mendapatkan posisi mouse saat ini di layar
    var mouse_x = get_viewport().get_mouse_position().x
    
    # Logika Gerak Kiri: 0px sampai 200px
    if mouse_x >= 0 and mouse_x <= 200:
        position.x -= speed_low * delta
        
    # Logika Gerak Kanan: 925px sampai 1152px (batas layar)
    elif mouse_x >= 925 and mouse_x <= 1152:
        position.x += speed_medium * delta
