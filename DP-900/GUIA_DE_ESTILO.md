# Guía de Estilo y Formato - Proyecto DP-900

Este documento establece las directrices y reglas estrictas de formato Markdown que deben aplicarse a todos los archivos del repositorio **DP-900: Microsoft Azure Data Fundamentals**. 

El objetivo es mantener una estructura limpia, modular y 100% compatible con los visores estándar de GitHub, Docsify o GitBook.

---

## 1. Estructura de Directorios y Archivos

El repositorio sigue una arquitectura modular. Cada lección o laboratorio debe estar contenido en su propia carpeta raíz, y los recursos gráficos deben estar aislados en una subcarpeta específica.

- **Módulos Teóricos / Laboratorios:** Cada concepto o laboratorio tiene su propia carpeta principal (ej. `01_Analisis_a_gran_escala/`).
- **Archivo Markdown:** Dentro de la carpeta principal habrá un único archivo `.md` con el nombre exacto de la lección.
- **Imágenes:** Todas las imágenes pertenecientes a un archivo `.md` deben almacenarse obligatoriamente en una subcarpeta llamada `imagenes/` ubicada dentro del mismo directorio de la lección.

---

## 2. Limpieza de Código (Regla de Cero Marcas Internas)

- [cite_start]Queda estrictamente prohibido el uso de marcas, etiquetas de citación o metadatos internos generados por IA (ej. `[cite: 1]`, ``, etc.).
- Los archivos deben ser texto plano en formato Markdown puro, listos para producción.

---

## 3. Índice y Anclajes Internos (Enlaces de Navegación)

Para garantizar que el índice de contenidos (Tabla de Contenidos) funcione correctamente en GitHub y visores compatibles, los enlaces ancla deben generarse siguiendo estas reglas matemáticas:
- Eliminación total de letras mayúsculas.
- Eliminación de acentos y caracteres especiales (ej. `á` -> `a`, `ñ` -> `n`).
- Sustitución de los espacios en blanco por guiones medios (`-`).
- Eliminación de signos de puntuación (comas, puntos, paréntesis).

**Ejemplo correcto:**
- Título original: `## 4.2 Crea una casa en el lago (lakehouse)`
- Enlace en el índice: `- [4.2 Crea una casa en el lago (lakehouse)](#42-crea-una-casa-en-el-lago-lakehouse)`

---

## 4. Referencia a Imágenes

Las imágenes nunca deben incrustarse mediante URLs externas temporales ni rutas absolutas. Se utilizará siempre la ruta relativa apuntando a la subcarpeta `imagenes/` local.

**Formato correcto:**
```markdown
![Descripción o Alt Text de la imagen](imagenes/nombre_del_archivo.png)
```

---

## 5. Bloques de Código

Todo fragmento de código (SQL, Python, Kusto, JSON, Bash, etc.) debe estar correctamente encapsulado utilizando tres acentos graves y la etiqueta del lenguaje correspondiente para aplicar el resaltado de sintaxis correcto.

**Ejemplo correcto:**
```sql
SELECT DATENAME(dw, lpepPickupDatetime) AS Day,
       AVG(tripDistance) AS AvgDistance
FROM taxi_rides 
GROUP BY DATENAME(dw, lpepPickupDatetime);
```

*Nota para editores:* Si el propio documento necesita explicar cómo hacer bloques de Markdown, se debe encapsular el documento padre con cuatro acentos graves (` ```` `) para evitar la rotura del renderizado.

---

## 6. Alertas, Notas y Consejos (Callouts)

Se utilizará la sintaxis de alertas nativas de GitHub (Markdown Alerts) para destacar información importante, consejos técnicos o requisitos del sistema. Los tipos permitidos son:

**Nota general:**
> [!NOTE]
> Utiliza este bloque para información general, aclaraciones de conceptos o requisitos de licencias.

**Consejo de buenas prácticas:**
> [!TIP]
> Utiliza este bloque para trucos, atajos de interfaz o sugerencias tácticas que mejoren el flujo de trabajo.

**Información crítica:**
> [!IMPORTANT]
> Utiliza este bloque para advertencias críticas, destrucción de recursos (limpieza) o acciones irreversibles.

---

## 7. Navegación de Pie de Página (Footer)

Todos los archivos `.md` (teoría y laboratorios) deben finalizar con un bloque de navegación estándar, separado por una línea horizontal (`---`). Los enlaces deben usar rutas relativas para subir un nivel (`../`) y apuntar al archivo correspondiente.

**Estructura obligatoria del Footer:**
```markdown
---

⬅️ **Anterior:** [Nombre del recurso anterior](../Carpeta_Anterior/Archivo_Anterior.md)

🏠 **Inicio del módulo:** [README](../README.md)

➡️ **Siguiente:** [Nombre del siguiente recurso](../Carpeta_Siguiente/Archivo_Siguiente.md)
```
🏠 **Inicio del módulo:** [README PRINCIPAL](DP900/readme.md)
*(Ajustar el nivel de subida `../` o `../../` según la profundidad de la carpeta actual respecto a la raíz del proyecto).*