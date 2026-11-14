# Diccionario de datos — CONECTA S.A., `conecta_clientes_mes.csv`

CONECTA S.A. es un operador de telecomunicaciones en Bogotá con **dos líneas de negocio**: **Hogar** (internet fijo en la vivienda) y **Móvil** (línea de celular con datos). Los datos son **sintéticos** (creados para el curso) sobre la base territorial real de la ciudad: las 112 UPZ y sus 19 localidades.

**Unidad de observación.** Cada fila es **un cliente en un mes** (cliente-mes). Un cliente que estuvo activo los doce meses aparece en doce filas; uno que llegó o se fue durante el año aparece solo en los meses en que estuvo activo.

**Periodo.** De noviembre de 2025 a octubre de 2026 (doce meses), con cerca de 30 000 clientes activos por mes, sumando las dos líneas.

**Formato del archivo.** Texto separado por comas (CSV), codificación UTF-8, punto como separador decimal y una fila de encabezados. Conviene leer `mes` como texto para conservar el formato AAAA-MM: `pd.read_csv(..., dtype={"mes": str})`.

**Archivo complementario.** `upz.geojson`: los polígonos de las 112 UPZ con su código, su nombre, su localidad y los hogares proyectados para 2026.

## Los segmentos de clientes

| Segmento | `linea` | `modalidad` | Qué significa `plan` | Qué significa `ingreso_mensual_cop` |
|---|---|---|---|---|
| Hogar | Hogar | Pospago (siempre) | Velocidad contratada: Básico, Estándar o Plus | Tarifa del plan; fija mientras el cliente no cambie de plan |
| Móvil pospago | Móvil | Pospago | Datos incluidos en el plan: Básico, Estándar o Plus | Tarifa del plan; fija mientras el cliente no cambie de plan |
| Móvil prepago | Móvil | Prepago | Paquete de recarga habitual: Básico, Estándar o Plus | Lo que el cliente recargó en el mes; varía de un mes a otro |

**Supuestos del caso.** Los precios, la mezcla de segmentos y la existencia de clientes prepago con paquetes de recarga son supuestos del caso, no cifras del mercado colombiano.

## Columnas

| Columna | Qué es | Tipo | Unidad o valores |
|---|---|---|---|
| `cliente_id` | Identificador del cliente; se repite en cada mes en que el cliente está activo | Texto (identificador) | `CNX-` seguido de un número |
| `mes` | Mes al que corresponde la fila | Fecha (mes) | AAAA-MM, de 2025-11 a 2026-10 |
| `codigo_upz` | Código oficial de la UPZ del cliente (en Hogar, donde está el servicio; en Móvil, la dirección registrada). Es un código, no una cantidad: no se suma ni se promedia | Entero (código) | Código de la UPZ, como en `upz.geojson` |
| `upz` | Nombre de la UPZ del cliente | Texto | Nombre oficial de la UPZ |
| `localidad` | Localidad a la que pertenece la UPZ | Texto | Nombre de la localidad |
| `estrato` | Estrato socioeconómico de la vivienda del cliente (en Móvil, la de la dirección registrada) | Entero ordinal | 1 a 6 |
| `latitud` | Latitud del punto del cliente (servicio en Hogar; dirección registrada en Móvil) | Decimal | Grados decimales |
| `longitud` | Longitud del mismo punto | Decimal | Grados decimales |
| `linea` | Línea de negocio del cliente | Texto (categoría) | Hogar o Móvil |
| `modalidad` | Forma de pago | Texto (categoría) | Pospago (paga una tarifa mensual) o Prepago (recarga) |
| `plan` | Plan del mes; su significado depende del segmento (ver la tabla de segmentos) | Texto (categoría) | Básico, Estándar o Plus |
| `fecha_alta` | Fecha en que el cliente contrató el servicio | Fecha | AAAA-MM-DD |
| `antiguedad_meses` | Meses transcurridos desde la fecha de alta hasta el mes de la fila | Entero | Meses |
| `ingreso_mensual_cop` | **Lo que el cliente pagó a CONECTA en ese mes**: es el ingreso de la empresa por ese cliente, **no el ingreso del hogar**. Con contrato es la tarifa del plan; en prepago, la recarga del mes | Decimal | Pesos colombianos (COP) por mes |
| `uso_datos_gb` | Datos consumidos por el cliente en el mes (medidos; no es un tope) | Decimal | Gigabytes (GB) por mes |
| `quejas_mes` | Quejas que el cliente registró en el mes | Entero (conteo) | Número de quejas |
| `nps` | Respuesta del cliente a la pregunta de recomendación de la encuesta del mes («¿qué tan probable es que recomiende CONECTA?»); vacío cuando no hay respuesta | Entero | 0 (nada probable) a 10 (muy probable) |
| `cancelo` | Indica si en ese mes el sistema de CONECTA registró una cancelación para el cliente | Entero (0/1) | 1 = se registró una cancelación; 0 = no |
| `tipo_cancelacion` | Tipo de cancelación con que quedó registrada en el sistema; vacío cuando `cancelo` = 0 | Texto (categoría) | Categoría del sistema |

## La jerga del caso

| Término | Cómo se usa en el curso |
|---|---|
| Cliente-mes | Una fila: un cliente en un mes. Contar filas no es contar clientes |
| UPZ y localidad | Las dos escalas territoriales de Bogotá en los datos: 112 UPZ agrupadas en 19 localidades (polígonos de la Secretaría Distrital de Planeación y del DADEP, Datos Abiertos Bogotá). La UPZ es la unidad más pequeña del caso |
| Estrato | Clasificación de la vivienda de 1 a 6 que trae cada cliente; en el caso se usa como aproximación a la capacidad de pago (estratificación por manzana de la Secretaría Distrital de Planeación, Datos Abiertos Bogotá) |
| Segmento | La combinación de línea y modalidad: Hogar, Móvil pospago o Móvil prepago |
| Pospago y prepago | Pospago: el cliente paga una tarifa mensual por un plan. Prepago: el cliente recarga cuando quiere y no tiene contrato |
| Tarifa frente a ingreso del hogar | `ingreso_mensual_cop` es lo que CONECTA recibe del cliente, no lo que gana el hogar |
| Antigüedad | Meses que lleva el cliente con CONECTA |
| Respuesta de recomendación (`nps`) | La respuesta individual de 0 a 10. El indicador que las empresas llaman NPS se calcula a partir de estas respuestas; en el curso se trabaja con la respuesta |

Fuente: CONECTA S.A., datos sintéticos del curso (nov. 2025 – oct. 2026). Base territorial: Secretaría Distrital de Planeación y Departamento Administrativo de la Defensoría del Espacio Público, Datos Abiertos Bogotá.
