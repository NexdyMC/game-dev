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

## ⚙️ Pengaturan Inspector

### 1. Sprite2D (Visual)
Agar gambar tidak terlihat buram (blur) saat di-zoom, gunakan settingan berikut:
- **Texture:** Masukkan asset gambar Anda.
- **CanvasItem > Texture > Filter:** Pilih `Nearest` (Sangat disarankan untuk Pixel Art).
- **Offset > Centered:** `On`.

### 2. Project Settings
Atur resolusi layar di **Project > Project Settings > Display > Window**:
- **Viewport Width:** `1152`
- **Viewport Height:** `648`
- **Stretch Mode:** `canvas_items`
- **Aspect:** `keep`

---

## 📜 Logika Script (GDScript)

Script ini ditempelkan pada node **Camera2D**. Menggunakan variabel `@export` sehingga kecepatan dapat diubah langsung melalui editor tanpa menyentuh kode.

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
