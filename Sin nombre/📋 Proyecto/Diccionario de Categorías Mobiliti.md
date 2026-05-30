---
title: Diccionario de Categorías Mobiliti
tags:
  - mobiliti
  - categorias
  - diccionario
  - clasificador
  - sinonimos
  - muebles
  - oficina
fecha: '2025-05-30'
tipo: referencia
---
# Diccionario de Categorías Mobiliti

> Diccionario de clasificación de productos para la columna E de la hoja Mobiliti. Editar este JSON agrega/modifica categorías sin tocar código.

---

## Categorías del Template

### 1. Silla
**Términos clave:**
- silla, sillas, chair, chairs, task chair, office chair, desk chair
- silla operativa, silla ejecutiva, silla de trabajo, silla ergonómica, silla giratoria
- silla de visita, silla de espera, silla apilable, silla de conferencia
- aveza, caz83sw, cdk19gs, computer chair, operator chair, meeting chair
- cantilever chair, mesh chair, leather chair, executive chair

---

### 2. Mesas de Apoyo
**Términos clave:**
- mesa auxiliar, mesas de apoyo, occasional table, side table, coffee table
- moji, varna, jason, round table, mesa redonda, mesa lateral
- center table, auxiliar, apoyo, end table, nested table, console table
- cocktail table, tea table, lamp table, accent table, pedestal table
- mesa de centro, mesa de café, mesa de esquina, mesa de rincón

---

### 3. Escritorios
**Términos clave:**
- escritorio, escritorios, desk, desks, workstation, workstations
- bureau, mesa de trabajo, standing desk, escritorio ejecutivo
- reception desk, mostrador, front desk, i-key, ikey, veneer desk
- work bench, banco de trabajo, work table, office desk, computer desk
- manager desk, secretary desk, L-shaped desk, U-shaped desk, corner desk

---

### 4. Sillones
**Términos clave:**
- sofá, sofa, sillón, sillon, sillones, lounge, couch, settee
- f80, sf51, chaiselongue, loveseat, modular sofa, sofa modular
- ottoman, pouf, puff, sala de estar, sala estar, living room
- diván, canapé, chaise longue, chesterfield, daybed, bean bag
- recliner, reclinable, rocking chair, mecedora, butaca, armchair

---

### 5. Mesas de Juntas
**Términos clave:**
- sala de juntas, mesa de juntas, mesas de juntas, meeting room
- conference table, mesa de reunión, junta, juntas, tetris, lido
- boardroom table, discussion table, negotiation table, round table
- seminar table, training table, collaborative table, team table
- mesa de conferencias, mesa de directorio, mesa de negociación

---

### 6. Librero - Locker - Gabinete
**Términos clave:**
- librero, librería, estante, estantería, locker, lockers, taquilla
- casillero, gabinete, gabinetes, cabinet, cabinets, storage cabinet
- credenza, armario, wardrobe, closet, storage unit, shelving unit
- wall unit, display cabinet, china cabinet, bookcase, bookshelves
- sideboard, buffet, server, console cabinet, media cabinet, tv cabinet

---

### 7. Archiveros Moviles y Fijos
**Términos clave:**
- archivero, archiveros, archiveros móviles, archiveros fijos
- filing cabinet, file cabinet, mobile pedestal, mobile cabinet
- drawer unit, under-desk drawer, roll container, tambour cabinet
- plan chest, map cabinet, card index cabinet, lateral file
- vertical file, suspension file cabinet, cajonera, cajonera móvil

---

### 8. Phonebooths
**Términos clave:**
- phonebooth, phone booth, phonebooths, phone booths
- cabina telefónica, cabina acústica, privacy booth, acoustic booth
- focus room, work pod, meeting pod, phone pod, call pod
- isolation booth, soundproof booth, quiet room, huddle room
- call booth, telephone booth, cápsula, cápsula acústica

---

### 9. Multicontactos
**Términos clave:**
- multicontacto, multicontactos, power strip, surge protector
- outlet, tomacorriente, enchufe, power module, usb hub
- charging station, estación de carga, power dock, power bar
- extension lead, power outlet, electrical outlet, socket, wall socket
- desk power, in-desk power, charging hub, regleta, zócalo

---

### 10. Terminados
**Términos clave:**
- terminado, terminados, acabado, acabados, finish, finishes
- accessory, accesorio, accesorios, complemento, misceláneo
- miscellaneous, various, general, other, package, kit, set, bundle

---

### 11. Bancos 🆕
**Términos clave:**
- banco, bancos, banqueta, banquetas, taburete, taburetes
- stool, stools, bench, benches, ottoman, pouffe, pouf
- tuffet, footstool, step stool, bar stool, counter stool
- kitchen stool, backless stool, saddle stool, drafting stool
- ducky, sd32, banco de madera, banco metálico, banco tapizado

---

### 12. Cocineta 🆕
**Términos clave:**
- cocineta, cocinetas, cocina, kitchenette, mini kitchen
- kitchen unit, pantry, coffee station, coffee bar, tea station
- break room, staff room, dining area, lunch room, cafeteria
- canteen, food prep area, wet bar, sink unit, counter kitchen
- modular kitchen, kitchen cabinet, kitchen counter, kitchen island

---

### 13. Pizarrones 🆕
**Términos clave:**
- pizarrón, pizarrones, pizarra, pizarras, pizarra blanca
- pizarra magnética, pizarra de corcho, pizarra de tiza
- pizarra digital, pizarra interactiva, pizarra de vidrio, pizarra móvil
- whiteboard, whiteboards, dry erase board, marker board, magnetic board
- glass board, interactive whiteboard, smart board, flip chart, easel
- chalkboard, blackboard, notice board, bulletin board, cork board
- modit, dm24, tiza, marcador, pizarra acrílica, pizarra electrónica

---

## Cómo agregar una categoría

1. Editar `diccionario_categorias.json` en el proyecto
2. Agregar la nueva categoría con su lista de `terminos`
3. Ejecutar `python -m pytest test_clasificador.py` para verificar
4. La próxima ejecución del script usará la nueva categoría automáticamente

## Configuración del Clasificador

```json
{
  "config": {
    "umbral_fuzzy": 75,
    "case_sensitive": false,
    "normalizar_acentos": true,
    "priorizar_match_exacto": true,
    "default_category": "Terminados"
  }
}
```

| Parámetro | Descripción |
|-----------|-------------|
| `umbral_fuzzy` | Score mínimo (0-100) para aceptar fuzzy match |
| `normalizar_acentos` | Convierte `sofá` → `sofa` antes de buscar |
| `priorizar_match_exacto` | Busca substring (`in`) antes de fuzzy |
| `default_category` | Categoría cuando no hay match |
