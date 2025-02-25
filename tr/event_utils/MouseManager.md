# MouseManager Sınıfı

**Tanım**\
`MouseManager`, fare ile ilgili işlemleri yönetmek için statik yöntemler sağlayan bir yardımcı sınıftır.

**Temel Alınır**\
`extends Script`

**Konum**\
`res://libs/event_utils`

**İçerik**
- [Numaralandırmalar](#numaralandırmalar)
    1. [MouseMode](#mousemode)
- [Değişken Tanımları](#değişken-tanımları)
- [Yöntemler](#yöntemler)
    1. [mouse_mode (void), static](#mouse_mode-void-static)
    2. [get_mouse_delta (Vector2), static](#get_mouse_delta-vector2-static)
    3. [mouse_speed (Vector2), static](#mouse_speed-vector2-static)
    4. [mouse_speed_x (float), static](#mouse_speed_x-float-static)
    5. [mouse_speed_y (float), static](#mouse_speed_y-float-static)

## Numaralandırmalar

### MouseMode
`MouseManager` sınıfı, fare imlecinin durumunu belirlemek için `MouseMode` adlı bir enum tanımlar.

| **Enum Değeri** | **Açıklama** |
|-----------------|--------------|
| `CAPTURED` | İmleç ekranın ortasına sabitlenir, hareketleri algılanır ancak görünmez.
| `CONFINED` | İmleç ekran sınırları içinde tutulur ve görünür kalır. |
| `CONFINED_HIDDEN` | İmleç ekran sınırları içinde tutulur ve görünmez olur. |
| `HIDDEN` | İmleç tamamen gizlenir ancak hareket algılanmaya devam eder. |
| `VISIBLE` | İmleç görünür ve normal şekilde hareket eder. |


## Değişken Tanımları
- **Not: `MouseManager` sınıfı mevcut haliyle yanlızca statik yöntemler içerir, bu yüzden herhangi bir üye değişkeni içermez.**

**[Başlığa Dön](#mousemanager-sınıfı)**

## Yöntemler

### mouse_mode (`void`), `static`

**Tanım**:
Girilen mod değerine göre fare imlecinin durumunu belirler. Temel amacı imleç durum kontrolü için merkezi bir kontrol sağlamaktır.

**Parametreler:**
- `mode` (`MouseMode`): [`MouseMode`](#mousemode) sınıfında tanımlı enum değerlerinden biri.
    
**Dönüş:**
- Yok (`null`).

**Örnek Kullanım:**
    
```GDScript
extends Node


func _input(event: InputEvent) -> void:
    if event is InputEventMouseButton:
        if event.button_index == MOUSE_BUTTON_LEFT and event.pressed:
            hide_mouse()  # Fareyi gizle.
        else:
            confine_mouse()  # Fareyi görünür yap ve sınırlandır.


func hide_mouse() -> void:
    MouseManager.mouse_mode(MouseManager.MouseMode.HIDDEN)
    

func confine_mouse() -> void:
    MouseManager.mouse_mode(MouseManager.MouseMode.CONFINED)
```

**[Başlığa Dön](#mousemanager-sınıfı)**

---

### get_mouse_delta (`Vector2`), `static`

**Tanım:**
Mouse hareket ediyorsa göreceli konumunu döndürür, aksi halde `Vector2.ZERO` (0) döndürür.

**Parametreler**
- `event` (`InputEvent`): Godot'un giriş olay nesnesi.

**Dönüş:**
- `Vector2`: Fare Hareketinin x ve y eksenindeki değişimi.

**Örnek Kullanım:**
    
```GDScript
extends Node


func _input(event: InputEvent) -> void:
    var mouse_delta = MouseManager.get_mouse_delta(event)

    if Input.is_action_pressed("LeftClick"):
        control_with_mouse(mouse_delta, ...)


func control_with_mouse(mouse_delta: vector2, ...) -> void:
    ...
```
**Örneğe Ait Notlar:**
- "`LeftClick`" Input Map'te tanımlanmalı.

**[Başlığa Dön](#mousemanager-sınıfı)**

---

### mouse_speed (`Vector2`), `static`

**Tanım:**
Fare hareketinin x ve y eksenlerindeki ölçekli hızını döndürür.

**Parametreler:**
- `mouse_delta` (`Vector2`): Fare hareketinin ham değeri.
- `speed` (`Vector2`): X ve Y eksenleri için hız çarpanı. Varsayılan değer: `Vector2(0.5, 0.5)`
- `speed_multiplier` (`float`): Genel hız çarpanı. Varsayılan değer: `0.001`

**Dönüş:**
- `Vector2`: X ve Y eksenlerinde ölçeklenmiş hız.

**Örnek Kullanım:**
    
```GDScript
extends Node3D

@export var mouse_speed: Vector2 = Vector2(1, 1)


func _unhandled_input(event: InputEvent) -> void:
    var mouse_delta: Vector2 = MouseManager.get_mouse_delta(event)

    if Input.is_action_pressed("LeftClick"): 
        rotate_node_manually(mouse_delta)


## Verilen fare hareketi değerine göre bu düğümün x ve y
## eksenlerindeki rotasyonunu ayarlar.
func rotate_node_manually(mouse_delta: Vector2)
    var scaled_speed: Vector2 = MouseManager.mouse_speed(
            mouse_delta,
            mouse_speed
        )   

    rotation.y -= scaled_speed.x
    rotation.x -= scaled_speed.y
```
**Örneğe Ait Notlar:** 
- "`LeftClick`" Input Map'te tanımlanmalı.
- Varsayılan değer (`speed_multiplier` = `0.001`) kullanarak ölçeklenmiş x ve y
hızını alıyoruz.

**[Başlığa Dön](#mousemanager-sınıfı)**

---

### mouse_speed_x (`float`), `static`

**Tanım:**
X eksenindeki fare hızını ölçekleyerek döndürür.
Tek eksen hareketi için optimize edilmiştir.

**Parametreler:**
- `mouse_delta` (`Vector2`): Fare hareketinin ham değeri.
- `speed` (`float`): X ekseni için hız çarpanı. Varsayılan değer: `0.5`
- `speed_multiplier` (`float`): Genel hız çarpanı. Varsayılan değer: `0.001`

**Dönüş:**
- `float`: X ekseninde ölçeklenmiş hız.

**Örnek Kullanım:**
    
```GDScript
extends Node3D

@export var mouse_speed_x: float = 1.0


func _unhandled_input(event: InputEvent) -> void:
    var mouse_delta: Vector2 = MouseManager.get_mouse_delta(event)

    if Input.is_action_pressed("LeftClick"):
        rotate_node_manually(mouse_delta)


## Verilen fare hareketi değerine göre bu düğümün y eksenindeki
## rotasyonunu ayarlar.
func rotate_node_manually(mouse_delta: Vector2)
    var scaled_speed_x: float = MouseManager.mouse_speed_x(
            mouse_delta,
            mouse_speed_x
        )   

    rotation.y -= scaled_speed_x
```
**Örneğe Ait Notlar:**
- "`LeftClick`" Input Map'te tanımlanmalı.
- Varsayılan değer (`speed_multiplier` = `0.001`) kullanarak ölçeklenmiş y 
hızını alıyoruz.

**[Başlığa Dön](#mousemanager-sınıfı)**

---

### mouse_speed_y (`float`), `static`

**Tanım:**
Y eksenindeki fare hızını ölçekleyerek döndürür.
Tek eksen hareketi için optimize edilmiştir.

**Parametreler:**
- `mouse_delta` (`Vector2`): Fare hareketinin ham değeri.
- `speed` (`float`): Y ekseni için hız çarpanı. Varsayılan değer: `0.5`
- `speed_multiplier` (`float`): Genel hız çarpanı. Varsayılan değer: `0.001`

**Dönüş:**
- `float`: Y ekseninde ölçeklenmiş hız.

**Örnek Kullanım:**
    
```GDScript
extends Control


func _gui_input(event: InputEvent) -> void:
    var mouse_delta: Vector2 = MouseManager.get_mouse_delta(event)

    if Input.is_action_pressed("RightClick"):
        adjust_vertical_movement(mouse_delta)


## Bu fonksiyon, verilen fare hareketi değerine göre bu düğümün dikey
## konumunu ayarlar.
func adjust_vertical_movement(mouse_delta: Vector2) -> Void:
    var scaled_speed_y: float = MouseManager.mouse_speed_y(mouse_delta)

    rect_position.y += scaled_speed_y
```
**Örneğe Ait Notlar:**
- "`RightClick`" Input Map'te tanımlanmalı.
- Varsayılan değerler (`speed` = `0.5`, `speed_multiplier` = `0.001`)
kullanarak ölçeklenmiş y hızını alıyoruz.

**[Başlığa Dön](#mousemanager-sınıfı)**

---
