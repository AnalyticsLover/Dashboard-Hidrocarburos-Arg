# Dashboard Hidrocarburos Argentina

---

Este proyecto presenta un **dashboard interactivo** desarrollado con **Power BI**, integrado en una página web estática alojada en GitHub Pages. El análisis se basa en datos oficiales extraídos de [datos.gob.ar](https://datos.gob.ar), proporcionando una visión detallada y comparativa de la industria de hidrocarburos en Argentina.

El dashboard se divide en dos focos de análisis:

### 1. **Producción de Hidrocarburos**

- **Cobertura temporal:** 2009–2025, con énfasis en 2023 y 2024.
- **Variables analizadas:**
  - Producción total por año.
  - Comparativa por tipo de producción: agua, gas, condensado, gasolina estabilizada, entre otros.
  - Análisis por subtipo de reservorio no convencional.
  - Ubicación de pozos: offshore vs. onshore.
  - Top 5 yacimientos con mayor producción.

### 2. **Fracturación y Pozos**

- **Cobertura temporal:** 2009–2025.
- **Variables analizadas:**
  - Cantidad de fracturas por pozo.
  - Fracturas mínimas, máximas y promedio por pozo.
  - Uso de recursos en la fracturación: arena bombeada, agua inyectada, presión y potencia máxima de equipos.
  - Top 5 empresas con mayor cantidad de pozos.
- **Panel de filtros interactivo** que permite al usuario personalizar el análisis según:
  - Fechas.
  - Yacimientos.
  - Cuencas.
  - Formaciones productivas.

---

## 🧪 Datos y Procesamiento

- **Fuentes de datos:**
  - 1º Dataset : [Datos-Fracturas-Pozos-Hidrocarburos.csv](http://datos.energia.gob.ar/dataset/71fa2e84-0316-4a1b-af68-7f35e41f58d7/archivo/2280ad92-6ed3-403e-a095-50139863ab0d)
  - 2º Dataset :[Produccion-De-Petroleo-Por-Yacimiento.csv](http://datos.energia.gob.ar/dataset/590d1284-fd6d-4686-afd8-b3da5d90a6e9/archivo/745facdc-73dc-46d8-83d5-d027bdaa3210)
  - 3º Dataset: [Produccion-De-Gas-Por-Yacimiento.csv](http://datos.energia.gob.ar/dataset/590d1284-fd6d-4686-afd8-b3da5d90a6e9/archivo/ce479c85-2e8b-441e-9c68-9681597b3694)
  
- **Procesamiento:**
  - Limpieza y combinación de los dos datasets de producción (petroleo y gas), los cuales poseen la misma estructura, se realizó una operación merge resultando en un set de datos de más de 3.6 millones de registros y más de 15 columnas.
  - Limpieza adicional del dataset con información de pozos y fracturas, se cambiaron nombre de columnas, se crearon columnas nuevas limpias, se lidió con errores y valores null. Dataset final menor a 5000 registros y más de 20 columnas.

---

## 🛠 Tecnologías Utilizadas

- **Power BI Desktop**: Desarrollo del dashboard interactivo.
- **Power Query**: Limpieza y transformación de datos.
- **DAX**: Cálculo de métricas y KPIs.
  - Measures:
    - Produccion Primaria Total = CALCULATE( SUM('Producción'[cantidad]), 'Producción'[concepto] = "Producción Primaria (m3)")
      
    - Producción Total = SUM('Producción'[cantidad])
      
    - Produccion Total 2023 = CALCULATE(SUM('Producción'[cantidad]), YEAR(Calendario[Date]) = 2023)
      
    - Produccion Total 2024 = CALCULATE(SUM('Producción'[cantidad]), YEAR(Calendario[Date]) = 2024)
      
    - Variacion % produccion = DIVIDE([Produccion Total 2024] - [Produccion Total 2023], [Produccion Total 2023], 0)
      
    - Agua Inyectada Total (Mm3) = DIVIDE(SUM(Yacimientos[agua_inyectada_m3]), 1000000)
      
    - Arena Total Bombeada (ktn) = DIVIDE(SUM(Yacimientos[arena_bombeada_importada_tn]) + SUM(Yacimientos[arena_bombeada_nacional_tn]), 1000)
      
    - Fracturas promedio x Pozo = CALCULATE(AVERAGE(Yacimientos[cantidad_fracturas]), GROUPBY(Yacimientos, Yacimientos[id_pozo]))
      
- **M**: Para realizar queries más complejas en la manipulación de los datos.
- **Python**: Scripts para limpieza y combinación de datasets.
- **HTML**: Integración del dashboard en una página web estática.
- **GitHub Pages**: Publicación del sitio web.

---

## 🧠 Modelo Semántico (Power BI)

El archivo `.pbix` contiene un modelo semántico construido siguiendo buenas prácticas de modelado estrella (Star Schema), optimizado para performance, legibilidad y facilidad de navegación.

### 📂 Tablas del Modelo

| Tabla                 | Tipo         | Descripción breve                                                  |
|----------------------|--------------|---------------------------------------------------------------------|
| `Produccion`          | Fact table      | Contiene datos detallados de producción por año, yacimiento, tipo de recurso, etc. (3M+ filas) |
| `Yacimientos`     | Dimention table       | Información detallada de pozos y fracturas por año, empresa, recurso, etc. (~10k filas) |
| `DimYacimiento`            | Dimension table    | Tabla de fechas calendarizadas para análisis temporal. |
| `Calendario`            | Date Table    | Tabla de fechas calendarizadas para análisis temporal. |


---

### 🔗 Relaciones y Cardinalidades

| Desde                | Hacia               | Cardinalidad   | Crossfiltering      | Activa |
|---------------------|---------------------|----------------|-----------------------------|--------|
| `Calendario[Date]`   | `Produccion[indice_tiempo]` | 1 : *          | Single            | ✅     |
| `DimYacimiento[id_yac]`   | `Produccion[id_yac]` | * : 1    | Single           | ✅     |
| `Yacimientos[id_yac]` | `DimYacimiento[id_yac]` | * : 1   | Single            | ✅     |


> Todas las relaciones son activas, de uno a muchos, con filtrado unidireccional. No se utilizan relaciones bidireccionales para evitar ambigüedades en el modelo.

---
## 📊 Estructura del Proyecto

/Dashboard-Hidrocarburos-Arg

│

├── powerbi/ # Archivos de Power BI (.pbix)

├── datasets/ # Archivos de datos originales

├── imagenes/ # Imágenes utilizadas en el dashboard

├── index.html # Página principal del dashboard

└── favicon.ico # Icono del sitio web


---

## 🚀 Despliegue en GitHub Pages

El sitio web está alojado en GitHub Pages y es accesible públicamente en:

🔗 [https://AnalyticsLover.github.io/Dashboard-Hidrocarburos-Arg](https://AnalyticsLover.github.io/Dashboard-Hidrocarburos-Arg)

---


## 👤 Autor

👨‍💻 **Lucía Pardini**  

🔗 [LuciaPardini.com](https://analyticslover.github.io/Portafolio-LuciaPardini/)

🔗 [Perfil de Github](https://github.com/AnalyticsLover)

---


## 📬 Contacto

- 📧 [Email: luciajulianapardini@outlook.com](mailto:luciajulianapardini@outlook.com)
- 💼 [LinkedIn](https://www.linkedin.com/in/luciapardini)

---

Si tienes comentarios o sugerencias, no dudes en abrir un *issue* en este repositorio.
