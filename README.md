# corne-wireless-view-zmk-config

Configuración personalizada para el teclado dividido y ergonómico Corne Wireless, utilizando el framework [ZMK Firmware](https://zmk.dev/). Esta configuración está optimizada para una experiencia inalámbrica utilizando los microcontroladores `nice!nano v2` y el adaptador `nice_view` para la pantalla.

## Descripción

Este repositorio contiene los archivos de configuración necesarios para programar el teclado Corne Wireless (modelo de 6 columnas) con ZMK Firmware. La configuración incluye características para incrementar el alcance inalámbrico, activar modos de sueño profundo y un diseño de teclas en capas, pensado para maximizar la eficiencia y ergonomía al escribir en este teclado dividido.

### Componentes

- **Teclado**: Corne Wireless de 6 columnas
- **Microcontrolador**: nice!nano v2
- **Adaptador de pantalla**: nice_view

## Archivos de configuración

### `build.yaml`

Este archivo define la matriz de GitHub Actions para compilar el firmware automáticamente para el Corne Wireless. Se incluyen configuraciones para ambos lados del teclado:

- **Lado izquierdo**: `board: nice_nano_v2`, `shield: corne_left nice_view_adapter nice_view`
- **Lado derecho**: `board: nice_nano_v2`, `shield: corne_right nice_view_adapter nice_view`

### Configuración inalámbrica (`corne.conf`)

El archivo `corne.conf` contiene configuraciones específicas para el teclado Corne Wireless, incluyendo ajustes de potencia, tiempos de rebote y opciones para optimizar la duración de la batería.

- **Alcance inalámbrico aumentado**: Se incrementa la potencia de transmisión (`CONFIG_BT_CTLR_TX_PWR_PLUS_8=y`) para mejorar el alcance.
- **Debouncing optimizado**: Mejora la precisión al reducir el tiempo de rebote al presionar o soltar una tecla.
- **Modos de ahorro de energía**: Configuración opcional para habilitar el sueño profundo.

### Keymap Personalizado (`corne.keymap`)

El `keymap` para el Corne Wireless está estructurado en varias capas para una mayor eficiencia y adaptabilidad. Cada capa tiene una función específica, incluyendo una disposición en **Dvorak** para la capa principal, un **teclado numérico**, una **capa de símbolos** y una capa de ajustes (con acceso a la conectividad Bluetooth, control de brillo, entre otros). A continuación, se explica cada capa y los comportamientos adicionales de las teclas especiales.

#### 1. Capa Principal: **Dvorak**

Esta es la capa predeterminada, diseñada en el diseño Dvorak para mayor ergonomía y velocidad de escritura.

- **Teclas de Función**:
  - `TAB`, `BKSP` (Retroceso), `ESC` y `CTRL` están posicionadas estratégicamente en los bordes para su fácil acceso.
  - **GUI** (tecla de Windows o Command en Mac), **LWR** (acceso a la capa inferior), **RSE** (acceso a la capa superior) y **ALT** son teclas de función en la fila inferior.
  
- **Tap Dance**:
  - **TD_CAPS**: Alterna entre Shift izquierdo y Bloq Mayús.
  - **TD_ALT**: Alterna entre Alt derecho e izquierdo.

#### 2. Capa Inferior: **Numpad**

Activa un teclado numérico y funciones relacionadas con Bluetooth y control del sistema.

- **Teclas de Bluetooth**:
  - `BTCLR` limpia todas las conexiones Bluetooth, mientras que `BT1`, `BT2`, etc., seleccionan dispositivos Bluetooth emparejados del 1 al 5.

- **Control del Sistema**:
  - `LFT`, `DWN`, `UP`, `RGT` controlan la navegación en pantalla, permitiendo desplazarse sin necesidad de ratón.

#### 3. Capa Superior: **Símbolos**

Esta capa proporciona un acceso rápido a caracteres especiales y comandos del sistema.

- **Caracteres Especiales**:
  - La capa contiene símbolos básicos y avanzados como `!`, `@`, `#`, `$`, `%`, `^`, `&`, y más.
  - **Teclas de Paréntesis y Corchetes**:
    - `(`, `)`, `{`, `}`, `[`, `]` están disponibles para programación y edición de texto.
  - **Teclas de Línea de Comandos**:
    - `-`, `=`, `\`, `~`, y `|` permiten operaciones comunes en la línea de comandos.

- **Tap Dance**:
  - **TD_QST**: Alterna entre el símbolo `?` y el comando macro `ud_qst` para insertar el carácter `¿`.
  - **TD_BSLS**: Alterna entre `\` y `|`.
  - **TD_LPLB** y **TD_RPRB**: Alterna entre paréntesis izquierdo y `«`, y paréntesis derecho y `»` respectivamente.

#### 4. Capa de Ajustes (condicional): **Both**

La capa de ajustes se activa en combinación cuando están activas las capas **Lower** y **Raise**. Aquí se incluyen ajustes de Bluetooth y control de brillo de la pantalla.

- **Control de Brillo**:
  - `BR+` y `BR-` aumentan y disminuyen el brillo respectivamente.
  - `AUTO` ajusta automáticamente el brillo.

- **Opciones de Bluetooth**:
  - `BT_CLR` borra las conexiones Bluetooth existentes.
  - `BT_SEL 0` a `BT_SEL 4` permiten cambiar entre dispositivos emparejados.
  - `BT_PRV` y `BT_NXT` permiten navegar entre dispositivos emparejados en orden.

#### Macro y Funciones Personalizadas

Dentro del keymap, también se definen **macros y comportamientos tap dance personalizados**:

- **Tap Dances**:
  - **TD_CAPS**: Shift izquierdo o Bloq Mayús, dependiendo de si se toca o se mantiene.
  - **TD_SFBT**: Alterna entre F1 y el modo Bootloader para facilitar la programación.
  - **TD_ALT**: Alterna entre Alt derecho y Alt izquierdo, dependiendo de la duración de la pulsación.
  - **TD_QST**: Alterna entre `?` y el carácter `¿`.
  - **TD_BSLS**: Alterna entre `\` y `|`.
  - **TD_LPLB** y **TD_RPRB**: Alterna entre paréntesis izquierdo y `«`, y paréntesis derecho y `»`.

- **Macros**:
  - **ud_qst**: Inserta `¿` mediante una combinación de teclas.
  - **ud_exc**: Inserta `¡`, para español y otros idiomas con caracteres de exclamación invertidos.

---

### `west.yml`

Este archivo manifiesto permite utilizar West para gestionar el repositorio ZMK Firmware, y establece las referencias de los repositorios de ZMK necesarios para compilar el firmware.

## Instrucciones de Uso

### Requisitos

- [Git](https://git-scm.com/)
- [West](https://docs.zephyrproject.org/latest/guides/west/index.html) (para clonar y compilar el proyecto)
- Herramientas de programación compatibles con ZMK (por ejemplo, `nRF Connect`)

### Pasos para la Compilación

1. **Clonar el repositorio**:
   ```bash
   west init -l config
   west update
   ```

2. **Compilar el firmware**:
   ```bash
   west build -p -b nice_nano_v2 -- -DSHIELD=corne_left
   west build -p -b nice_nano_v2 -- -DSHIELD=corne_right
   ```

3. **Cargar el firmware**: Usa una herramienta compatible para cargar el firmware compilado en los módulos `nice!nano v2` de tu Corne Wireless.

## Contribuciones

Para contribuir, realiza un fork del repositorio y crea un pull request con tus cambios. Nos encanta recibir sugerencias de mejora y configuraciones alternativas.

## Créditos

Basado en el trabajo de [ZMK Contributors](https://zmk.dev/).
