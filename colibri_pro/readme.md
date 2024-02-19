# Impresora Colibrí Pro

## Límites
La impresora tiene un origen en el centro de la cama. Y soporta instrucciones de movimiento hasta los siguientes valores

|Eje| Valor mínimo | Valor Máximo |
|---|-----|------|
| X | 123 | -140 |
| Y | 80  | -80  |
| Z | 0   | ?    |

Cuando una instrucción de movimiento `G0` o `G1` excede alguno de estos valores, la impresora rechaza la instrucción y causa un traslado al centro de la cama.

# Soporte de códigos
## Códigos G
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
| G91-94  | ☑️  |  |
| G95-G133  | ❎  |  |

## Códigos M
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
| M82  | ❎  |  |

## Código S
Los códigos S para control de temperatura, ejemplo `S200`, son aceptados pero no parecen tener efecto.

