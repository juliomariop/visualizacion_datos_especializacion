# 📊 Clase 2 — Diccionario de gráficas, indicadores e incertidumbre (14 de noviembre de 2026)

> Segunda sesión de **Visualización de Datos y Gerencia de la Información** (Esp. en Analítica de Datos, UNAULA), por Teams (~2 h 24 min grabados, con descanso a mitad). Primera parte: el «diccionario» de gráficos por tipo de dato con el caso del profe (*Golden State*) y el de los estudiantes (*CONECTA*). Al final: explicación del taller de la S2.
>
> **Fuentes:** *(clase)* = lo dicho en la grabación · *(presentación)* = `presentacion_sesion_02.pdf` · *(notebook)* = `sesion_02_guiada.ipynb` y `sesion_02_practica_proyecto_A.ipynb` · *(datos)* = comprobado en el CSV/GeoJSON, no dicho en clase. Lo dudoso va marcado con ❓.

---

## 🧭 Lo esencial de esta clase
- **Primero el mensaje, después la gráfica.** El corazón del curso: saber cuál es la idea que se quiere comunicar y escoger la gráfica que mejor la apoye, sin sesgar la realidad. No graficar 10–20 figuras esperando que el lector «autoconcluya».
- **Indicador ≠ KPI:** el indicador es una medida monitoreable; un KPI es un indicador **con meta y trazabilidad**. En Golden State el indicador es la **razón precio/ingreso** (años de ingreso para pagar la vivienda); en CONECTA, la **tasa de cancelación** (promedio de `cancelo` × 100).
- **El diccionario por tipo de dato:** cantidades → barras; distribuciones → histograma/densidad/acumulada/violín; proporciones → barras apiladas y Sankey; relaciones → dispersión/contornos/burbujas/correlación; tiempo → líneas, dispersión conectada y polares; geografía → puntos y coropletas; incertidumbre → intervalos de confianza.
- **Múltiplos pequeños** (subplots): la misma gráfica repetida por segmento con los mismos ejes → comparación inmediata.
- **Incertidumbre:** el error estándar de una proporción es `√(p(1−p)/n)` → **n pequeña = error grande**; siempre tomar la n más grande posible.
- **Entrega del taller S2** por correo, asunto `DATAVIZ sesion 2` + nombre del grupo; el profe pidió respetar el nombre del archivo (ver la última sección).

---

## 1. Repaso y marco

- **De la S1** *(clase)*: se partió de un informe con gráficos «feítos» que se clasificaron (fea / mala / errónea) y con ellos se conocieron las variables tal cual, sin limpieza. Recuerdo del profe sobre gráficos que confunden: 5 ejes superpuestos en una diapositiva para «correlacionar un montón de cosas» terminan exagerando relaciones y transmitiendo un mensaje alejado de la realidad.
- **De la S2** *(clase)*: ver el **diccionario de gráficas** (la parte más importante de la clase), definir un **indicador** en cada caso, **múltiplos pequeños** («la traducción bonita de subplot») e **incertidumbre**. Bibliografía del catálogo de gráficos: un enlace que pasó el profe Leandro con muchos tipos de gráficas, y el **Dataviz Project** para explorar más *(clase)*.
- **Hacia dónde apunta:** las sesiones 2 y 3 preparan la **S4**, donde se escribe el **SCQA** (situación, complicación, pregunta y respuesta) del caso propio. La S5 es de mapas y la S6, recopilatoria *(clase)*.

## 2. Indicador vs. KPI *(clase)*

| Concepto | Definición |
|---|---|
| **Indicador** | Una medida que se puede monitorear |
| **KPI** | Un indicador **con meta y trazabilidad**: se le da seguimiento contra un objetivo |

- En los casos del curso los datos son estáticos → se definen **indicadores**, no KPIs. Existen muchos otros (OKR, etc.), pero la distinción que importa es la de la meta.

## 3. El indicador de Golden State: razón precio/ingreso *(clase + presentación, diap. 7)*

- **Recordatorio del caso:** comité de inversión, perfil financiero no técnico, debe elegir un **top 150** de distritos para invertir en el trimestre. La brecha de la S1: la vivienda en la bahía cuesta ~2× la del interior, pero el ingreso solo crece ~1.3× → oportunidad.
- **Indicador:** `razón = valor de la vivienda / ingreso anual` → «cuántos años de ingreso se necesitan para pagar la vivienda». Por debajo de 1 sería un salario «súper alto»; lo normal es una distribución de 2, 3, 4… n años.
- **Su distribución** *(clase)*: casi normal, sesgada a la derecha, pico en ~4.5 años; un atípico en ~12. El **5 % más bajo** está en razón ≤ 2.66 (~1 000 distritos): las «gangas».
- ⚠️ Escoger solo con el umbral del 5 % puede llevar a distritos no atractivos (desiertos, zonas sobre- o subpobladas): el umbral **orienta**, no decide.

## 4. El diccionario de gráficas

| Si quiere mostrar… | Gráficas | Cuidado principal |
|---|---|---|
| **Cantidades** | Barras, puntos, barras agrupadas, apiladas, mapas de calor | No recortar el eje a lo loco (sobredimensiona); barras **horizontales** con etiquetas largas; con cientos de categorías, top N o Pareto |
| **Distribuciones** | Histograma, densidad (KDE), acumulada, cajas, violín, Q–Q | El ancho de los bins cambia el mensaje; la acumulada facilita leer percentiles; el boxplot puede ocultar la forma real |
| **Proporciones** | Barras apiladas al 100 %, Sankey | La torta se defiende solo con 2–3 categorías |
| **Relaciones x–y** | Dispersión, contornos/hexbin, burbujas, matriz de correlación | Una «masa negra» de puntos no deja ver la densidad: usar color o transparencia |
| **Tiempo** | Líneas, dispersión conectada, coordenadas polares | No toda línea es serie de tiempo (edad ↔ valor no lo es); las polares sirven para **estacionalidad** con varios años |
| **Geografía** | Mapa de puntos, coropletas | El mapa da contexto (concentración) que ninguna otra gráfica da |
| **Incertidumbre** | Barras/intervalos de confianza | n pequeña ⇒ error estándar grande |

### 4.1 Cantidades *(clase + presentación, diap. 12)*
- El diagrama de **barras** es lo más recomendado para comparar categorías. En Golden State: «isla» sale con una razón ~14–16, un valor tan alto que hace ruido (n muy pequeño → variabilidad).
- **Barras agrupadas** añaden una variable (nivel de ingreso): en el interior, ingreso medio y alto pagan casi igual de años; en las demás zonas se nota la escala por ingreso.
- **Barras apiladas** muestran también la composición (cantidad de distritos por zona y nivel de ingreso): el interior y «<1 h del mar» concentran la mayoría de distritos para escoger.
- **Horizontal vs. vertical:** misma información rotada; con textos largos, la horizontal se lee natural. Con muchas variables (ej. importancia de 500 variables), graficar el **top 10/15** y dejar una «ruptura de eje» con las últimas para recordar que hay más.

### 4.2 Distribuciones *(clase + presentación, diap. 15)*
- **Histograma:** ojo con los bins; con bins más grandes el pico «baja». En Golden State la media del valor de vivienda ronda ~150–200 k USD y hay un valor atípico en 500 000 (se revisa en S3).
- **En n vs. en %:** con n cuesta comparar; con porcentaje (densidad) la comparación es directa.
- **Violín:** el boxplot «con forma»: la línea interna es la mediana y el ancho muestra dónde se acumulan los datos. En Golden State se nota que incluso en el interior hay distritos con razón ~14. «Isla» desaparece por su n minúsculo.
- **Densidad (KDE):** el histograma con «infinitos bins» → línea continua en proporción; atenúa picos exagerados del conteo.
- **Distribución acumulada (CDF):** la mejor para leer percentiles: el 50 % está en ~4.5 años, el 80 % por debajo de ~6.5–7. Basta trazar una línea horizontal.
- **Q–Q plot:** cuantil contra cuantil con la recta de la normal de referencia; si la nube se aleja de la recta (como hacen los 500 k), la distribución no es normal / tiene colas.
- **Boxplot:** puede esconder la distribución real — dos conjuntos distintos pueden dar la misma caja (ejemplo de una charla que el profe citó: el boxplot igualaba distribuciones muy distintas); si se usa, mostrar también los puntos o el violín.

### 4.3 Proporciones *(clase)*
- **Tortas** muy parecidas no dejan comparar; mejor **barras apiladas al 100 %** (en cualquiera de las dos orientaciones: por zona con niveles de ingreso, o por nivel de ingreso con zonas — «la misma gráfica volteada»). En Golden State se concluye que a menor ingreso, más proporción de interior; la bahía y cerca del mar reparten parejo entre los niveles.
- **Sankey** (caso CONECTA): relaciona dos composiciones — base por segmento (hogar ~16 k, prepago ~15 k, pospago ~4 k *(clase)*) → destino (siguen, retiro voluntario, por mora, sin recarga). Lectura clave: del ~15 % que se va de forma voluntaria, más de la mitad es prepago; «sin recarga» solo tiene sentido para prepago. En telecom se usa también para ver **portabilidad** entre operadores (quién le quita clientes a quién).

### 4.4 Relaciones x–y *(clase + presentación, diap. 24)*
- **Dispersión:** el punto de referencia. La versión **negra** no deja distinguir la densidad; con **color o transparencia** se ve que la concentración está en ingresos ~25 k y casas ~100 k. En Golden State hay relación creciente ingreso→valor (con sus «tacaños» y «endeudados» como casos graciosos en las esquinas).
- **Contornos:** la misma relación, más limpia, cuando lo que importa es mostrar la concentración y no el detalle de cada punto.
- **Burbujas:** el tamaño añade una tercera variable; si además se colorea por categoría puede volverse un «salpicón». Con solape, pensar en escala logarítmica.
- **Matriz de correlación:** ingreso↔valor ≈ 0.7 (fuerte); población↔hogares alta (lógico); edad↔población ≈ −0.3, tenue y contraintuitiva (distritos más grandes, gente más joven — un «soplo», no una conclusión).

### 4.5 Tiempo *(clase)*
- **Líneas / serie de tiempo:** solo si el eje X es tiempo. La tasa de cancelación de CONECTA ronda el ~3 % y se ve estable de nov-2025 a oct-2026 *(clase)*.
- **Dispersión conectada:** cada punto es una fecha (quejas ↔ tasa de cancelación por mes). En CONECTA se ve que a mitad de año el churn **cae a su mínimo aunque las quejas siguen subiendo** → este tipo de gráfico **genera preguntas** para filtrar periodos y analizar.
- **Coordenadas polares:** cada eje es un mes y cada círculo un valor; con varios años permite ver **estacionalidad** (ej. «en junio siempre sube»). Si la serie fluctúa mucho se forma una «estrella» ilegible → ahí sí, la línea de siempre. Referencia citada: el gráfico de temperatura de la Tierra en espiral.

### 4.6 Geografía *(clase + presentación, diap. 33)*
- **Mapa de puntos:** contexto que ninguna otra gráfica da — concentración real. En Golden State: el **Valle Central concentra el 68 %** de las «gangas»; la **bahía de San Francisco** tiene el 21 % de los distritos pero solo el 3 % de las gangas; Los Ángeles y SF tienen muchos distritos y pocas gangas. Una ganga «a un año de salario» en mitad del desierto hay que evaluarla.
- **Coropletas:** la misma información coloreada por condado (los «municipios» de California). El tema a fondo llega en la S5; el taller ya trae el código del mapa para solo ejecutarlo.

### 4.7 Incertidumbre *(clase + presentación, diap. 41)*
- **Intervalo de confianza:** el valor oscila según media y desviación estándar; IC al 95 % ≈ 1.96 × error estándar. Para una **proporción**: `ee = √(p(1−p)/n)` (para la media, `σ/√n` *(clase)*).
- **Regla de oro:** el error estándar va **inverso a n** → muestra pequeña = bigotes largos; muestra grande = error casi cero.
- Ejemplo de clase: el % real de «gangas» es ~14.5; con muestras aleatorias de 500, algunos intervalos del 95 % capturan el valor real y otros no. Por eso, ante un estudio estadístico, siempre escoger la n más grande posible para no inducir error.

### 4.8 Múltiplos pequeños *(clase + presentación, diap. 22)*
- La misma gráfica repetida por segmento **con los mismos ejes** → comparación directa sin una imagen «sucia» de densidades superpuestas.
- Ojo: el **umbral del 5 % se marca sobre la distribución total**, por eso no coincide con el 5 % de cada panel.

## 5. Reglas de presentación *(clase)*

- Pocos gráficos, **limpios**: «no confundamos lo simple con lo sustancioso». El sobrepensamiento se hace antes; ante la audiencia va la versión clara, con el **menor esfuerzo de lectura** posible.
- Cada gráfico = **una idea**. Quien presenta gobierna el mensaje: decide qué mostrar según el objetivo de la reunión; si muestra otra cosa, la audiencia se va «por ahí».
- **Qué evitar (los contraejemplos del cierre):** torta con muchas categorías, concentración sin números, **gráficos en 3D**, y el **doble eje** que sugiere relaciones espurias (parecía que al subir el churn bajaban los clientes… no).
- Ejemplos de comunicación directa: «5 años de ingreso cuesta la vivienda típica de California»; tabla de % de gangas por zona; comparación nov↔oct de la tasa por localidad (Kennedy sube ~3.2 → ~3.6 mientras otra baja) y el puente de clientes (activos iniciales + altas − bajas = activos finales).

## 6. El taller de la S2 (CONECTA, proyecto A) *(clase + notebook)*

- **Material:** `presentacion_sesion_02.pdf`, `sesion_02_guiada.ipynb` (todo lo visto en la presentación) y `sesion_02_practica_proyecto_A.ipynb` (el taller). Datos en `datos/`: `conecta_clientes_mes.csv` (la base cliente-mes, **solo necesitan esta y `upz.geojson`**, que son las que el notebook consulta) + `diccionario_conecta.md`, `housing.csv`, `cb_2018_us_county_20m.zip` (este último solo lo usa el notebook guiado del profe) *(clase)*.
- **Tareas:** 1) definir el indicador (tasa de churn = promedio de `cancelo` × 100) y barras por localidad y segmento; 2) distribuciones (`ingreso_mensual_cop` por segmento; `antiguedad_meses` según `cancelo`); 3) tiempo en múltiplos pequeños (top 6 localidades por clientes, 2 × 3, cada una vs. el promedio); 4) incertidumbre (muestra de 30 000 e intervalos por localidad); 5) relaciones (tasa vs. % prepago; tasa por # de quejas con intervalo) y el **mapa ya hecho** para solo ejecutar. Al final: preguntas de interpretación y el párrafo para llevar a la S4 *(notebook)*.
- **Estilo exigido:** «no vayan a ser como los gráficos feos y malos» — aplicar lo visto en las dos sesiones *(clase)*.
- **Entrega:** por correo, archivo `DATAVIZ_sesion_2_proyecto_A_grupo_<n>.ipynb`; el profe pidió respetar el nombre porque tiene que cerrar notas hacia el 23 *(clase)*. Fecha: en clase quedó «miércoles» de la semana siguiente; con viajes, hasta el domingo ❓ (la transcripción mezcla «18», «25», «octubre/noviembre» — confirmar en el chat del curso).
- **Recordatorio del caso CONECTA** *(clase)*: la base está sucia (datos mal escritos → 38 variantes para 19 localidades) — esa limpieza es el tema de la **S3**. Del análisis previo: churn ~3 % estable, pero con localidades por encima del promedio.

## 7. Próximas sesiones *(clase)*

| Sesión | Tema |
|---|---|
| S3 | Limpieza de datos («el curso de vida» en 2 horas) |
| S4 | SCQA: la recomendación al comité — lo más fuerte del curso |
| S5 | Mapas |
| S6 | Recopilatoria de todo lo anterior |
