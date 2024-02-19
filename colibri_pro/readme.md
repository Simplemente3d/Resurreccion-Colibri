#Impresora Colibrí Pro

##Límites
La impresora tiene un origen en el centro de la cama. Y soporta instrucciones de movimiento hasta los siguientes valores

|Eje| Valor mínimo | Valor Máximo |
|---|-----|------|
| X | 123 | -140 |
| Y | 80  | -80  |
| Z | 0   | ?    |

Cuando una instrucción de movimiento `G0` o `G1` excede alguno de estos valores, la impresora rechaza la instrucción y causa un traslado al centro de la cama.
