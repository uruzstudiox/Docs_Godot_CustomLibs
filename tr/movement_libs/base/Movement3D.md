# Movement3D Sınıfı

**Tanım**\
`Movement3D`, 3D sahnelerde temel hareket mekanizmasını sağlayan en temel düğümdür.

**Temel Alınır**\
`extends Node3D`

**Konum**\
`res://libs/movement_libs/base/movement_3d.gd`

**İçerik**
- [Numaralandırmalar](#numaralandırmalar)
- [Değişken Tanımları](#değişken-tanımları)
    1. [@export ile Tanımlanan Değişkenler](#export-ile-tanımlanan-değişkenler)
- [Yöntemler](#yöntemler)
    1. [start_movement (void)](#start_movement-void)
    2. [stop_movement (void)](#stop_movement-void)
    3. [get_direction_value (int), static](#get_direction_value-int-static)

## Numaralandırmalar

- **Not: `Movement3D` sınıfı, mevcut haliyle herhangi bir numaralandırma**
**içermez.**

## Değişken Tanımları
`Movement3D` sınıfı, bazı durumlar için üye değişkenleri tanımlar.

### `@export` ile Tanımlanan Değişkenler

| **Değişken Adı**   | **Değişken Tipi**   | **Varsayılan Değer** | **Açıklama** |
|--------------------|---------------------|----------------------|----------------------|
| `is_moving`        | `bool`              | `false`              | Hareketin açık olup olmadığını belirler. |
| `movement_speed`   | `Vector3`           | `Vector3.ZERO`       | Hareket yönünü ve hızını belirler. |
| `reverse_speed_x`  | `bool`              | `false`              | X eksenindeki hareket yönünü tersine çevirir. |
| `reverse_speed_y`  | `bool`              | `false`              | Y eksenindeki hareket yönünü tersine çevirir. |
| `reverse_speed_z`  | `bool`              | `false`              | Z eksenindeki hareket yönünü tersine çevirir. |

**[Başlığa Dön](#movement3d-sınıfı)**

## Yöntemler

### start_movement (`void`)

**Tanım:**
Bu yöntem, hareketi başlatmak için '`is_moving`' değişkenini true olarak
ayarlar.

**Parametreler:**
- Yok.

**Dönüş:**
- Yok (`null`).

**Örnek Kullanım:**
```GDScript
extends Node3D

@export var movement_node: Movement3D = null


func _ready() -> void:
    movement_node.start_movement()  # Hareket düğümü için hareketi başlatır.
```
**Örneğe Ait Notlar:**
- `movement_node` nesnesi bir Movement3D örneği olmalıdır ve sahnede atanmalıdır.

**[Başlığa Dön](#movement3d-sınıfı)**

---

### stop_movement (`void`)

**Tanım:**
Bu yöntem, hareketi başlatmak için '`is_moving`' değişkenini false olarak
ayarlar.

**Parametreler:**
- Yok.

**Dönüş:**
- Yok (`null`).

**Örnek Kullanım:**
```GDScript
var movement = Movement3D.new()
movement.stop_movement()
```

**[Başlığa Dön](#movement3d-sınıfı)**

---

### get_direction_value (`int`), `static`

**Tanım:**
Ters hız durumunu (`reverse_speed`) baza alarak hareket yönünü 1 veya -1
olarak döndürür.
- Tersine çevrim yoksa (`reverse_speed` = `false`), fonksiyon `1` 
 döndürecektir.
- Tersine çevrim varsa (`reverse_speed` = `true`), fonksiyon `-1` 
 döndürecektir.

**Parametreler:**
- `reverse_speed` (`bool`): Hareketin ters yönde olup olmadığını belirler.

**Dönüş:**
- `int`: 1 veya -1

**Örnek Kullanım:**
```GDScript
class_name CustomMovement
extends Movement3D


fumc _process(delta: float) -> void:
    if not is_moving or movement_speed == Vector3.ZERO:
        return
    else:
        rotate_on_y(delta)


func rotate_on_y(delta: float) -> void:
    if movement_speed.y != 0.0:
        rotate_x(
            delta * movement_speed.y * get_direction_value(reverse_speed_y)
        )
```

**[Başlığa Dön](#movement3d-sınıfı)**

---
