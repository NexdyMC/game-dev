Tentu! Berikut adalah draf `README.md` yang dirancang dengan estetika bersih, profesional, dan mudah dipahami untuk repositori GitHub kamu.

---

# 🚪 Animated Door Toggle System (Godot 4)

Sistem pintu interaktif sederhana menggunakan **GDScript** dan **Tweening** di Godot Engine 4. Proyek ini mendemonstrasikan cara membuat mekanisme *toggle* (saklar) antara tombol UI dan objek di dalam game world secara halus.

---

## 🛠 Struktur Node
Penyusunan hierarki node sangat penting agar script dapat berjalan dengan benar. Pastikan struktur di panel **Scene** kamu terlihat seperti ini:

| Node | Type | Deskripsi |
| :--- | :--- | :--- |
| `World` | **Node2D** | Root dari scene (Tempat script ditempel). |
| ├── `PenutupPintu` | **Sprite2D** | Objek pintu yang akan bergerak secara vertikal. |
| ├── `ButtonToggle` | **Button** | Tombol interaktif untuk memicu animasi. |
| │   └── `IconTexture` | **TextureRect** | Ikon status di dalam tombol (On/Off). |
| ├── `AnimationPlayer`| **AnimationPlayer**| (Opsional) Untuk keperluan animasi tambahan. |
| └── `Pintu` | **Sprite2D** | Background atau bingkai pintu statis. |

---

## ⚙️ Pengaturan Inspector
Agar script berjalan tanpa error, perhatikan pengaturan pada **Inspector** berikut:

1.  **Script Placement**: Tempelkan file script pada node **World**.
2.  **Texture Path**: Pastikan file gambar berikut ada di folder `res://`:
    * `btn_green.png` (Status Off)
    * `btn_red.png` (Status On)
3.  **Positioning**: 
    * Nilai awal `PenutupPintu` diatur secara otomatis oleh script ke `y = 9` saat game dimulai.

---

## 📜 Kode Utama (`World.gd`)
Script ini menangani logika perubahan tekstur dan pergerakan halus menggunakan `create_tween()`.

```gdscript
extends Node2D

@onready var pintu = $PenutupPintu
@onready var icon_texture = $ButtonToggle/IconTexture 
@onready var button_toggle = $ButtonToggle

# Preload Asset
var img_off = preload("res://btn_green.png")
var img_on = preload("res://btn_red.png")

# [Posisi Buka, Posisi Tutup]
var P_PenutupPintu = [213, -100]
var is_door_open: bool = false

func _ready():
	pintu.position.y = 9
	icon_texture.texture = img_off
	button_toggle.pressed.connect(_on_button_pressed)

func _on_button_pressed():
	is_door_open = !is_door_open
	var tween = create_tween()
	
	if is_door_open:
		# Animasi Membuka
		tween.tween_property(pintu, "position:y", P_PenutupPintu[0] , 0.5)
		icon_texture.texture = img_on
	else:
		# Animasi Menutup
		icon_texture.texture = img_off
		tween.tween_property(pintu, "position:y", P_PenutupPintu[1], 0.5)
```

---

## 🚀 Cara Penggunaan
1.  **Clone** repositori ini atau salin script ke project Godot kamu.
2.  Sesuaikan **Node Path** di bagian `@onready` jika nama node kamu berbeda.
3.  Ubah nilai dalam array `P_PenutupPintu` untuk mengatur seberapa jauh pintu bergeser sesuai kebutuhan desain level kamu.
4.  Jalankan scene (`F6`) dan klik tombol untuk melihat hasilnya!

---
*Dibuat dengan ❤️ untuk komunitas pengembang game Godot.*