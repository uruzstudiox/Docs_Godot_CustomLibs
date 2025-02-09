# Orbit3D Sınıfı

**Tanım**\
Bağlı nesnenin belirlenen eksende, kendi local ekseni etrafında dönmesini
veya düğümün merkezi etrafında yörüngesel hareket yapmasını sağlayan bir
hareket düğümüdür.
\
Bu düğüm bir **referans noktası** gibi davranır.
- Eğer bağlı nesne `Vector3(0, 0, 0)` konumundaysa, **yanlızca kendi ekseni**
**etrafında döner.**
- Eğer bağlı nesne    `Vector3(0, 0, 0)` **dışında bir konumdaysa,**
**yörüngesel hareket oluşturur.**

**Temel Alınır**\
`extends Movement3D`

**Konum**\
`res://libs/movement_libs/modules/orbit_3d`

**İçerik**
- [Numaralandırmalar](#numaralandırmalar)
- [Değişken Tanımları](#değişken-tanımları)
- [Yöntemler](#yöntemler)
    1. [rotate_on_axis (void)](#rotate_on_axis-void)


## Numaralandırmalar
- **Not: `Orbit3D` sınıfı, mevcut haliyle herhangi bir numaralandırma**
**içermez.**

## Değişken Tanımları
- **Not: `Orbit3D` sınıfı, mevcut haliyle herhangi bir üye değişkeni**
**içermez.**

**[Başlığa Dön](#orbit3d-sınıfı)**

## Yöntemler

### rotate_on_axis (`void`)

**Tanım:**
Düğümü, belirlenen eksende ve belirlenen dönüş hızına göre döndürür.
\
**Tanıma Ait Notlar:**
- Mevcut hareketi ters yönde gerçekleştirmek için:
    - Hıza negatif bir değer verilebilir.
    - `reverse_speed_x`, `reverse_speed_y` ve `reverse_speed_z` değerleri
        `true` olarak ayarlanabilir.

**Parametreler:**
- `delta` (`float`): Saniye cinsinden geçen süre.

**Parametrelere Ait Notlar:**
- `delta`, Godot’un `_process(delta)` veya `_physics_process(delta)`
fonksiyonlarında çerçeve süresi olarak kullanılır. Dönüş hareketinin
kare hızından bağımsız olması için dönüş miktarı delta ile çarpılarak
hesaplanmalıdır.

**Dönüş:**
- Yok (`null`).

**Örnek Kullanım:**
```GDScript
extends Orbit3D


func _process(delta: float) -> void:
    if not is_moving or movement_speed == Vector3.ZERO:
        return

    rotate_on_axis(delta)
```
**Örneğe Ait Notlar:**
- Bu betik `Orbit3D` sınıfına eriştiği için **Inspector Paneli** üzerinden
değişkenler (`is_moving`, `movement_speed`, `reverse_speed`) kontrol 
edilebilir.
- Ayrı bir düğüm oluşturmak yerine, **Node menüsü aracılığıyla sahneye**
**doğrudan eklenebilir.**

**[Başlığa Dön](#orbit3d-sınıfı)**

---
