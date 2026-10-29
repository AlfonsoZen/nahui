# nahUI — Menú de inicio (iOS)

## Contexto

nahUI es un videojuego conceptual para una actividad del **iOS Development Lab** (UNAM, servicio social): un speech de presentación de producto con tema de Día de Muertos. El PDF original de la actividad está en la raíz del repo.

La historia: una calavera recorre los 9 niveles del Mictlán (el inframundo mexica) acompañada de Tlalli, su xoloitzcuintle. *Nahui* significa "cuatro" en náhuatl, por los cuatro años que duraba el viaje; la "UI" es un guiño.

**Alcance de este repo: SOLO la pantalla de menú de inicio.** El resto de las pantallas del juego se simulan con imágenes en la presentación. No implementes niveles, inventario ni lógica de juego.

**Entrega: 30 de octubre de 2026.** Hay que priorizar algo que corra y se vea bien sobre algo completo.

## Referencia visual

La imagen del menú en la raíz del repo es la **fuente de verdad del diseño**. Ábrela y estúdiala antes de escribir código. Todo debe coincidir con ella:

- Pixel art de 16 bits estilo Terraria, con píxeles grandes y visibles. **Nunca debe verse suavizado.**
- Noche morada con niebla, un cerro o pirámide al fondo y pocas estrellas.
- Un camino de cempasúchil naranja brillante que va del borde inferior hacia el horizonte, en perspectiva.
- El xolo (gris oscuro, sin pelo, orejas grandes, collar azul) y la calavera (cabeza redonda gris-blanca, poncho café), **vistos de espalda**, sobre el camino.
- Pétalos de cempasúchil cayendo.
- UI de piedra gris tallada con bordes biselados y letras crema/naranja:
  - Logo "nahUI" arriba ("nah" en piedra gris, "UI" en naranja), con un glifo azteca a cada lado.
  - Botón principal "AVENTURA" debajo del logo.
  - Botón circular de engrane arriba a la derecha.
  - Botones circulares "INVENTARIO" (abajo a la izquierda) y "COLECCIÓN" (abajo a la derecha).

## Stack

- Swift 5.9+, **SwiftUI + SpriteKit** (una `SKScene` dentro de un `SpriteView`).
- iOS 17+, solo iPhone, solo vertical (portrait).
- **Sin dependencias externas.**
- Prioridad: que se vea igual a la referencia y corra fluido (60 fps).

## Lo que debe hacer la pantalla

1. **Fondo:** cielo, niebla y cerro. Puede ser una imagen estática; agrega un leve movimiento de niebla si sobra tiempo.
2. **Camino animado:** el xolo y la calavera caminan **en su lugar**; la sensación de avance la da el camino. Usa un desplazamiento continuo de los pétalos y las marcas del camino hacia el espectador, en loop sin costuras.
3. **Ciclo de caminar:** xolo y calavera con animación de 2 a 4 cuadros en loop (`SKAction.animate(with:timePerFrame:)`). Si todavía no hay sprites por cuadro, usa una sola imagen con un rebote vertical de 1–2 px y una ligera alternancia, que de espalda se ve convincente.
4. **Pétalos cayendo:** `SKEmitterNode` creado con el editor de partículas de Xcode (`Petals.sks`), con textura de pétalo pixelado, caída lenta, rotación y deriva lateral.
5. **UI en SwiftUI encima de la escena:** logo, botón AVENTURA, engrane, INVENTARIO y COLECCIÓN.
   - AVENTURA: al tocarlo suena un ladrido (`bark.wav`), el botón se hunde 2 px y la pantalla hace fade a negro.
   - Los demás botones: solo feedback visual al tocarlos (no navegan a ningún lado).
   - Un pulso sutil en AVENTURA para invitar a tocar.
6. **Tipografía:** Pixelify Sans (Regular y Bold), incluida en el bundle y registrada en `Info.plist` (`UIAppFonts`).

## Reglas de pixel art (obligatorias)

- Todas las texturas con `texture.filteringMode = .nearest`.
- Todas las imágenes de SwiftUI con `.interpolation(.none)` y `.resizable()`.
- Escalar en múltiplos enteros cuando sea posible.
- Nada de sombras suaves, desenfoques ni degradados de SwiftUI sobre los sprites.

## Assets

Van en `Assets.xcassets`. Si un asset no existe todavía, **genera un placeholder** (un rectángulo de color con el nombre) para que la escena corra, y avísalo en tu respuesta. No inventes arte final.

| Asset | Descripción |
|---|---|
| `bg_sky` | Cielo, niebla y cerro (sin personajes ni UI) |
| `path` | Camino de cempasúchil en perspectiva, repetible verticalmente |
| `xolo_walk_0…3` | Xolo de espalda, cuadros del ciclo de caminar |
| `skull_walk_0…3` | Calavera de espalda, cuadros del ciclo de caminar |
| `petal` | Pétalo individual, unos 8×8 px |
| `logo_nahui` | Logo con glifos laterales |
| `btn_stone_wide`, `btn_stone_round` | Fondos de botón de piedra (o dibújalos con shapes pixelados) |
| `bark.wav` | Sonido de ladrido |

Mientras no estén los sprites finales, se pueden recortar elementos de la imagen de referencia para prototipar.

## Estructura sugerida

```
nahUI/
  nahUIApp.swift
  Menu/
    MenuView.swift          // SwiftUI: SpriteView + overlay de UI
    MenuScene.swift         // SKScene: fondo, camino, personajes, pétalos
    StoneButton.swift       // Botón de piedra reutilizable
  Resources/
    Petals.sks
    Fonts/PixelifySans-Regular.ttf, PixelifySans-Bold.ttf
    Sounds/bark.wav
  Assets.xcassets
```

## Hitos (en este orden)

1. Proyecto creado, compila y muestra la escena vacía en vertical.
2. Fondo y camino con desplazamiento en loop.
3. Xolo y calavera animados sobre el camino.
4. Pétalos cayendo.
5. UI de SwiftUI encima (logo y botones) con Pixelify Sans.
6. Interacción de AVENTURA: ladrido y fade.
7. Pulido: comparar contra la referencia lado a lado y ajustar posiciones y escalas.

Después de cada hito, el proyecto debe compilar y correr. No avances con el siguiente si el anterior está roto.

## Grabación para el video

La pantalla se graba para el speech desde el simulador o un iPhone:

```bash
xcrun simctl io booted recordVideo menu.mp4
```

El loop debe verse bien durante al menos 15 segundos sin cortes visibles.

## Convenciones

- Código e identificadores en inglés; comentarios y textos de UI en español.
- Archivos pequeños y legibles: es un proyecto escolar que el equipo debe poder explicar.
- No agregues pantallas, navegación ni features fuera de este alcance.
