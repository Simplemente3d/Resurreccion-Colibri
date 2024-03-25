# Impresora Colibrí Pro
Esta impresora se puede usar a través de la herramienta Octoprint y cualquier laminador moderno.

## Prerequisitos
 - Dispositivo para instalar Octoprint, este debe poderse mantener encendido durante la operación de la impresora.
 - Cable USB A Macho a B.
 - Instalar laminador Ultimaker Cura. Se pueden usar diferentes laminadores, pero esta guía sólo se enfocará en Cura.
 - Nivelación a través del panel de control de la impresora.

## 1. Instalar Octoprint en dispositivo.
Instalar Octoprint en dispositivo. Guías:
  - [Raspberry Pi](https://octoprint.org/download/#octopi)
  - [Mac](https://octoprint.org/download/#installing-manually)
  - [Windows](https://octoprint.org/download/#windows-installer)

Después de hacer la configuración inicial de Octoprint, conectar el cable USB desde el dispositivo a la Colibrí.
La opción de conectar debe estár disponible, sin embargo al conectarse debe dar el error:
*error*
Para poder conectarse exitosamente, debemos instalar el plugin ["Better GRBL Support"](https://plugins.octoprint.org/plugins/bettergrblsupport/) de Octoprint.
Al instalarse, la UI cambia y ahora al dar clic en conectar, se debe observar lo siguiente:
*screenshot*

La impresora se desconecta despues de un tiempo de inactividad, esto es normal.

## 2. Crear Perfil nuevo en Cura
Cura requiere configuraciones específicas para poder generar gcodes válidos. A estas configuraciones les llamamos perfiles.
Agregar impresora
Establecer límites.

### Límites
La impresora tiene un origen en el centro de la cama. Y soporta instrucciones de movimiento hasta los siguientes valores. Si una instrucción de movimiento `G0` o `G1` excede alguno de estos valores, la impresora rechaza la instrucción y causa un traslado al centro de la cama.

|Eje| Valor mínimo | Valor Máximo |
|---|-----|------|
| X | 123 | -140 |
| Y | 80  | -80  |
| Z | 0   | ?    |

Agregar g-código de inicio:
```
G90 ; home and center
G21 ; set units to mm
G4 P10; wait for nozzle to heat up
```
Agregar g-código final:
```
G1 E-1 F300 ;retract
G91 ; set absolute movement
G0 Z5 ; pull nozzle up a little
G0 F7200 X-130 Y-80 ; move head to back corner
```
###
Sabor de G-Code
En la opción de G-Code Flavor seleccionar `Makerbot`. La Colibrí no soporta instrucciones `G92` este sabor genera G-code que no depende de dichas instrucciones.


## 3. Crear scripts de postprocesamiento
Algunas instrucciones de la laminación no son soportadas y enviarlas causa que la impresora se detenga totalmente. Para quitarlas del archivo gcode, usamos scripts de postprocesamiento, particularmente el script de "Buscar y Reemplazar" o "Search and Replace"
  - Ir a Extensiones
  - Scripts de Postprocesamiento
  - Agregar las siguientes intstrucciones de Buscar y Reemplazar

|Script| Buscar | Reemplazar |
|----|--------|-----------|
| 1  | `M82`  | `;R M82`  |
| 2  | `M104` | `;R M104` |
| 3  | `M105` | `;R M105` |
| 4  | `M106` | `;R M106` |
| 5  | `M107` | `;R M107` |
| 6  | `M109` | `;R M109` |
| 7  | `M116` | `;R M116` |
| 8  | `M126` | `;R M126` |
| 9  | `M127` | `;R M127` |
| 10 | `G92`  | `;R G92`  |

Usamos la letra `R` como indicador de que la línea fue modificada en postprocesamiento.
*Ver soporte de códigos G 

Despues de agregar los scripts se debe tener una indicación visual como siguientes.

## 4. Laminar prueba
Laminar archivo de prueba y subir a octoprint.

## 5. Imprimir
Antes de de iniciar la impresión, se debe elegir la temperatura deseada en el panel de control de la impresora dado que la temperatura configurada en el laminador no tiene efecto. Despues de alrededor 5 minutos, la boquilla debe estar a la temperatura deseada. En este momento, nos podemos conectar a la impresora desde Octoprint e iniciar la impresion.
1. Elegir temperatura.
2. Permitir que se caliente la boquilla.
3. Subir archivo gcode a Octoprint.
4. Clic en conectar.
5. Clic en imprimir.

# Anexo

## Impresión desde SD y USB
La impresión desde SD y USB no ha sido exitosa. Se podría analizar un archivo GCODE creado por el software constructor, para determinar las instrucciones necesarias para iniciar la impresión. Si cuenta con uno, por favor compartirlo a simplemente3dgdl@gmail.com.

## Soporte de instrucciones de arco G2 y G3
La impresora soporta arcos, por lo que se puede usar el plugin "Arc Welder" de Cura. Sin embargo, en pruebas la extrusión se veía afectada al cambiar entre instrucciones de arco y líneas.

## Soporte de códigos G


| Gcode|Soportada|Nota |
|------|:--------|-----|
| G0 |  ☑️ |  |
| G1 |  ☑️ |  |
| G2 |  ☑️ |  |
| G3 |  ☑️ |  |
| G4 |  ☑ |  |
| G5-G16 | ❎  |  |
| G17-G21 | ☑️  |  |
| G22-G27 | ☑️  |  |
| G28 | ❎  | Error de lectura de EEPROM |
| G29 | ❎  |  |
| G30 | ❎  | Error de lectura de EEPROM |
| G31-40 | ❎  |  |
| G41-G52 | ❓| |
| G53 | ☑️ |  |
| G54-G59 | ❎  | Error de lectura de EEPROM |
| G60-G79 | ❎  |  |
| G80 | ☑️  |  |
| G81-G89 | ❎  |  |
| G90  | ☑️  | Un G90 es normalmente un cambio a coordenadas absolutas. En la colibrí esta instrucción causa un _home_, llevar los ejes a 0 y centra la boquilla en la cama. Es posible que esto sea causado por no poder leer/escribir de la EEPROM. Dado que el _home_ normal G28 no funciona, se usa G90 como el la instrucción de _home_. |
| G91  | ☑️  |  |
| G92  | ☑️  | Falla para E, si no está acompañada de uno de los 3 ejes, X, Y o Z |
| G93-94  | ☑️  |  |
| G95-G133  | ❎  |  |

## Soporte de códigos M
| Gcode|Soportada|Nota |
|------|:--------|-----|
| M0-M1 |  ☑️ | La impresora la acepta, pero no funciona como debería |
| M2 |  ☑️ | Causa una acción de _home_ y apaga los motores. |
| M3-M5 |  ☑️ |  |
M6-M7 |  ❎ |  |
| M8  | ☑️  |  |
| M9 |  ☑️ | La impresora la acepta, pero no funciona como debería |
| M10-M20  | ❎  |  |
| M21 | ❓| |
| M22  | ☑️  |  |
| M23-M29 | ❓| |
| M30  | ☑️  |  |
| M31-M81 | ❓| |
| M82-M83  | ❎  |  |

## Soporte de código S
Los códigos S para control de temperatura, ejemplo `S200`, son aceptados pero no parecen tener efecto.

