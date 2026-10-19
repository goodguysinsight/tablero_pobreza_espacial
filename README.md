# Análisis espacial de la pobreza en el Perú

Tablero interactivo de análisis exploratorio de datos espaciales (AEDE) de la pobreza monetaria: matriz de pesos espaciales, rezago espacial, I de Moran global, diagrama de dispersión de Moran y clústeres LISA (I de Moran local).

## Niveles

| Nivel | Unidades | Datos | Años |
|---|---|---|---|
| Provincial | 196 provincias | INEI, mapas de pobreza monetaria provincial y distrital 2013 (Anexo N° 5) y 2018 (Anexo N° 4) | 2013, 2018 y cambio 2013-2018 |
| Departamental | 25 departamentos | ENAHO, réplica propia: pobreza, pobreza extrema, brecha, severidad, gasto real y vulnerabilidad | 2004-2025 (vulnerabilidad 2020-2025) |

## Contenido del tablero

- **I de Moran global** con su valor esperado, p-valor por 999 permutaciones y lectura (agrupamiento, dispersión o aleatorio).
- **Periodo (nivel provincial)**: 2013, 2018 o cambio 2013-2018 en puntos porcentuales. En el cambio, el mapa de valores usa la paleta rojo–azul oscuro (azul: la pobreza bajó; rojo: subió) y, en el LISA, Bajo-Bajo agrupa provincias donde la pobreza bajó más y Alto-Alto, donde bajó menos o subió.
- **Mapa de valores** (sextiles) y **mapa de clústeres LISA**: Alto-Alto (puntos calientes), Bajo-Bajo (puntos fríos), Alto-Bajo y Bajo-Alto (atípicos). Solo se colorean las unidades con p < 0.05.
- **Diagrama de dispersión de Moran**: valor estandarizado (z) frente al rezago espacial (Wz); la pendiente de la recta es el I de Moran.
- Nivel provincial: provincias en clústeres por departamento. Nivel departamental: evolución del I de Moran global.
- Tabla de detalle con filtros por tipo de clúster.

## Metodología

- **Matriz W**: contigüidad tipo reina (borde o vértice común), estandarizada por filas. Las listas de vecinos (`inputs/vecinos_reina_*.csv`) se derivan de los límites distritales del INEI; Putumayo (1608) se arma con sus distritos de origen. Callao solo es vecino de Lima.
- **I de Moran global**: I = z′Wz / z′z; valor esperado −1/(n−1); inferencia por 999 permutaciones.
- **I de Moran local (LISA)**: I<sub>i</sub> = z<sub>i</sub> Σ<sub>j</sub> w<sub>ij</sub> z<sub>j</sub> (Anselin, 1995), con permutación condicional; clúster si p < 0.05.
- Provincias: estimación puntual = centro del intervalo de confianza al 95% publicado por el INEI. Ambos mapas publican las mismas 196 provincias (incluida Putumayo), por lo que usan la misma matriz W; los nombres son los del mapa 2018.
- Con solo 25 departamentos la prueba tiene poca potencia: lo usual es que el I departamental de la pobreza no sea significativo; a nivel provincial el agrupamiento es claro (I = 0.436 en 2013 y 0.443 en 2018; 0.288 para el cambio 2013-2018; p = 0.001 en los tres casos).

## Archivos

| Ruta | Descripción |
|---|---|
| `index.html` | Tablero autocontenido (incluye datos y límites). |
| `data/datos.js` | Copia de los datos incluidos en `index.html`. |
| `stata/analisis_espacial_pobreza.do` | Cálculo en Stata/Mata: W, rezago, I de Moran global y LISA. |
| `inputs/vecinos_reina_provincias.csv`, `inputs/vecinos_reina_departamentos.csv` | Pares de unidades contiguas (matriz W en lista). |

## Actualizar

1. Ejecute primero `pobreza_departamental_2004_2025.do` (tablero de pobreza) y luego `stata/analisis_espacial_pobreza.do`, con `Bases/Anexo Estadístico_Pobreza 2013.xlsx`, `Bases/Anexo Estadístico_Pobreza 2018.xlsx` y los CSV de vecinos en `inputs/`.
2. Convierta las hojas `LISA_provincial`, `LISA_departamental` y `Moran_global` del Excel resultante al bloque `window.AE_DATA` de `index.html`.

## Publicar en GitHub Pages

Suba los archivos a un repositorio (por ejemplo, `tablero_pobreza_espacial`), y en **Settings → Pages** elija la rama `main`, carpeta `/ (root)`.

Fuente: INEI, mapas de pobreza monetaria provincial y distrital 2013 y 2018; ENAHO 2004-2025. Elaboración: Good Guys Insight.
