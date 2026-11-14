# Resumen Clase 3 — ¿Podemos confiar en lo que vemos?

**Curso:** Visualización de Datos y Gerencia de la Información
**Sesión:** 3 de 9 (reunión del sábado 14 de noviembre de 2026, en dos partes)
**Caso de la sesión:** Golden State (vivienda California 1990) y CONECTA S.A. (telecomunicaciones Bogotá)

---

## 1. 🎯 De qué trata la sesión

Antes de llevar un hallazgo al comité hay que responder: **¿qué cuenta como una cancelación y en qué datos se puede confiar?** La sesión repasa la calidad del dato, construye una bitácora de limpieza con Golden State y luego aplica el mismo método a CONECTA. *(clase)*

La fe de erratas de la S2 arranca la clase: el Sankey contaba «sigue» a quien tiene una fila en octubre, así que **las 711 salidas de octubre aparecían como clientes que siguen**. Los datos estaban bien; falló la regla con que se contaron. La regla corregida clasifica al cliente por su última fila: se va quien termina con una cancelación que no es cambio de plan. *(clase / notebook guiado)*

> ⚠️ El curso no es de programación: el código denso viene dado para tomarlo como referencia; lo importante son las decisiones. *(clase)*

---

## 2. 📏 La calidad del dato: aptitud para el uso

- Un dato es de calidad si sirve para el uso que se le va a dar — no «perfecto», sino apto (Wang y Strong, 1996). *(presentación)*
- **Cuatro categorías de calidad** (diapositiva 10): intrínseca (el dato en sí), contextual (sirve para la tarea), representacional (cómo está escrito) y accesibilidad (se puede obtener). *(presentación)*
- En el taller: perfilar cada columna (tipo, % de nulos, valores distintos) y clasificar los problemas en esas categorías. *(notebook)*

---

## 3. 🚩 Valores límite, imposibles y diminutos (Golden State)

- **Valores límite**: casas en el tope de USD 500 001, ingresos en 15 o más, valores en el piso de USD 14 999 → la variable se «amontona» en el extremo: sospechoso. *(clase / presentación)*
- **Valores imposibles**: más de 10 personas por hogar no es un hogar plausible (criterio del curso). *(clase)*
- **Distritos diminutos**: menos de 100 habitantes → el promedio de 3-4 casas no representa una zona. *(clase)*
- **Sin comparables**: la isla (5 distritos) se excluye porque no hay con qué compararla (regla de la S2). *(notebook guiado)*
- De aquí salen las reglas numeradas de la bitácora (1 a 6) y la decisión: **se excluye**, con el conteo de distritos afectados y de «gangas» de la S2 que cada regla saca. El tope de USD 500 001 excluye 965 distritos pero ningún candidato: la regla que más saca no cambia nada. *(notebook guiado)*

---

## 4. 🕳️ Datos faltantes: ¿de qué depende que un dato falte?

- Tres mecanismos (van Buuren, 2018): **MCAR** (falta al azar), **MAR** (falta según lo que sí observamos), **MNAR** (falta por el valor que falta). *(clase / presentación)*
- **Cómo saber cuál**: comparar a quienes tienen el dato con quienes no. En `total_bedrooms` (207 nulos) se parecen en ingreso, valor, cuartos y razón, y la proporción sin dato es pareja entre zonas → **compatible con MCAR**: se conservan porque la razón precio/ingreso no los usa. *(notebook guiado)*
- En CONECTA el **NPS falta en ~78 %** de las filas y la respuesta **baja con las quejas** (≈ 22 % con 0 quejas → ≈ 15 % con 3 o más): **no es MCAR**, depende del comportamiento del cliente. *(clase / datos)*
- Qué hacer con los faltantes depende de la decisión, no de la estadística sola: conservar, excluir o usar la propia falta como dato. *(clase)*

---

## 5. 📓 La bitácora

- Una fila por regla: **problema, decisión, por qué y cuántos registros afecta** (la cifra la imprime el código, no se escribe a mano). *(clase / presentación, diapositiva 31)*
- Sirve para responderle al comité qué se tocó y por qué, y para reproducir la tabla limpia. *(clase)*

---

## 6. 📞 CONECTA: qué cuenta como cancelación

- `cancelo = 1` incluye el **«cambio de plan»**: el sistema cierra un contrato pero el cliente sigue con CONECTA. La tarea crea `perdida` (cancelación sin cambio de plan). *(notebook)*
- En los datos revisados: ~**2 464 filas** son «cambio de plan»; la pérdida de Hogar baja casi a la mitad y la del prepago no cambia (el prepago no tiene cambios de plan — se va sin avisar). *(notebook guiado)*

---

## 7. 🗺️ La tabla limpia y las UPZ candidatas

- El taller deja en `salidas/` la **tabla limpia** (`conecta_limpio_s3.csv`) y las **10 UPZ candidatas** (`upz_candidatas_s3.csv`): el 10 % peor por tasa de pérdida, solo UPZ con ≥ 80 clientes (con menos, la tasa es inestable — como Candelaria en la S2). *(notebook)*
- La S4 parte de esa tabla; la del curso es idéntica a la del equipo si aplicaron bien las reglas (el solucionario lo verifica). *(notebook)*

---

## 8. 📦 Entregas y logística (de la grabación)

- **S2 y S3 son dos entregables, ambos para el miércoles 18 de noviembre** (lo repite la profesora: «S2 y S3 para que queden que son 2 entregables… el miércoles, 18 de noviembre»). *(clase)*
- La entrega 1 venció el viernes 13 de noviembre a las 5 PM; quien no la haya enviado, «a más tardar mañana en la mañana». *(clase)*
- La clase del jueves es virtual, a las 6:00 PM (a confirmar si alguien está en desacuerdo). *(clase)*
- Entrega por correo del curso (mismo de la S2): `espanaliticadatos.ssemestre.g2@unaula.edu.co` *(clase / IMPORTANTE.txt)*

---

## 9. 🛠️ El taller de la sesión 3 (proyecto A: Retención)

1. **Perfil de calidad**: tipo, % de nulos y distintos por columna; clasificar problemas en las 4 categorías; columnas que repiten información (`localidad` se deduce de `codigo_upz`; `upz` también). *(notebook)*
2. **Reglas de limpieza** sobre `d`: duplicados exactos, cuenta `CNX-PRUEBA-01`, cuentas con pago 0 en todos los meses; localidad y plan oficiales (celda dada); coordenadas intercambiadas (latitud < −60) y coordenadas nulas. *(notebook)*
3. **¿Qué cuenta como cancelación?**: `perdida` sin «cambio de plan»; comparar por estrato y segmento; tabla de tipos; **ranking de UPZ** con las dos definiciones (¿qué pasa con La Esmeralda?). *(notebook)*
4. **NPS**: respuesta por número de quejas → no falta al azar; decidir qué hacer con él y para qué sirve todavía. *(notebook)*
5. **Guardar** la tabla limpia y las UPZ candidatas en `salidas/`. *(notebook)*

---

## ❓ Puntos por confirmar

- El enunciado menciona «el plazo acordado en clase»; la grabación confirma **miércoles 18 de noviembre** para S2 y S3 (misma fecha para ambos). Si el profesor dio otra hora límite, ajustar.

*Fuentes: transcripción de la reunión (dos partes), `presentacion_sesion_03.pdf`, `sesion_03_guiada.ipynb`, `sesion_03_practica_proyecto_A.ipynb`, `datos/diccionario_conecta.md`.*
