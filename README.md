# Optimizador de Configuración de Home Theater

**Geometric & Directional Acoustic Setup Optimization — v1.3 (ES)**

Herramienta interactiva que, a partir de las dimensiones de la sala y el
sistema elegido (2.0 / 5.1 / 7.1 / Dolby Atmos 5.1.2), propone la colocación
de los altavoces: posición, ángulo, orientación (toe-in) y potencia
aproximada del amplificador — y visualiza qué tan bien esa disposición
cubre la zona de escucha.

Esta es la versión en español del proyecto original, pensada para el
mercado hispanohablante. El código y la lógica son idénticos a la versión
original; solo cambia el idioma de la interfaz.

## Honestamente, qué es esto (y qué no es)

El programa calcula **geometría y direccionalidad convencional**, no la
física completa del sonido en una sala. **No modela**:

- reflexiones en paredes, techo y suelo;
- absorción por materiales (alfombras, cortinas, muebles);
- difracción e interferencia;
- modos resonantes de la sala.

Por eso en la interfaz se llama *Geometric & Acoustic Setup Optimization*,
no *Acoustic Field Optimization*. Esto último es el objetivo de una etapa
futura, mucho más compleja, cuando se incorpore un modelo físico real del
campo acústico.

## Qué hace hoy

- **Geometría de la sala** — ancho, largo, altura del techo.
- **4 configuraciones típicas**: Estéreo 2.0, 5.1, 7.1, Dolby Atmos 5.1.2.
- **Zona de escucha** en lugar de un solo punto — ancho del sofá, 3 puntos
  de muestreo (borde izquierdo / centro / borde derecho).
- **Arrastrar** el punto de escucha y cada altavoz directamente en el plano.
- **Conos de directividad** — cobertura orientativa de cada altavoz (más
  ancha en surround/rear, más estrecha en los frontales).
- **Orientación manual (toe-in)** de cada altavoz, independiente de su
  posición — botones ±5°, con reinicio al apuntado automático.
- **Fijar posición** — si un altavoz no puede ir en el lugar ideal (por
  ejemplo, hay una ventana), se puede arrastrar a un lugar posible y
  fijarlo: el optimizador ya no lo tocará, pero recalculará el resto.
- **Acoustic Score multifactorial** en vez de un número mágico:
  - **Geometry** (25%) — coincidencia de posición y orientación con el estándar
  - **Coverage** (25%) — proporción de puntos de la zona de escucha que
    realmente caen dentro del cono de directividad de cada altavoz
  - **SPL Proxy** (20%) — aproximación por distancia (no un cálculo físico de SPL)
  - **Distance** (20%) — uniformidad de las distancias entre canales
  - **Symmetry** (10%) — simetría de los pares de canales espejados (L/R, LS/RS, etc.)
- **Optimizador** — búsqueda estocástica local que mueve los altavoces no
  fijados para maximizar el Score total.
- **Vista 2D y vista 3D** (SVG puro, sin WebGL) — la vista 3D es solo
  lectura; para mover elementos, se usa el plano 2D.
- **Cálculo aproximado de potencia** del amplificador según el volumen de
  la sala.

## Inicio rápido

```bash
npm install
npm run dev
```

Se abrirá una dirección como `http://localhost:5173`.

Compilación para producción:

```bash
npm run build
npm run preview
```

## Estructura del proyecto

```
src/
  components/
    HomeTheaterPlanner.jsx   # componente principal — toda la lógica y el markup
  main.jsx                   # punto de entrada de React
  index.css                  # reset mínimo de estilos
```

## Hoja de ruta

**Próximo:**
- Zonas de exclusión explícitas en el plano (marcar "aquí hay una ventana/puerta").
- Salas de forma libre, no solo rectangulares.
- Atenuación gradual (no binaria) del Coverage hacia el borde del cono.
- Arrastrar elementos directamente en la vista 3D (requiere invertir
  correctamente la proyección en perspectiva).

**Más adelante:**
- Medición de la sala con la cámara del teléfono (AR) en vez de introducir
  las dimensiones a mano.
- Comparar el cálculo geométrico con una medición real hecha con el
  micrófono del teléfono (de forma honesta: no reemplaza un micrófono de
  medición calibrado).
- Modelo físico completo del campo acústico (reflexiones, absorción, modos
  de la sala) — y entonces sí, el cambio de nombre a *Acoustic Field
  Optimization*.

## Licencia

MIT — ver [LICENSE](./LICENSE).
