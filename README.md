# DashboardV1_MGA
Dashboard Inventario TI

# MAOG18 | Dashboard_TI
### Versión 1.2

<div align="center">

![PowerBI](https://img.shields.io/badge/Power_Bi-F2C811?style=flat-square&logo=codeforces&logoColor=black)
![Security](https://img.shields.io/badge/Security-CIS_Benchmark-red?style=flat)
![License](https://img.shields.io/badge/License-MIT-green?style=flat)

**Documentación, DataSet y DashBoard PowerBI Inventario TI CUR-Carazo**  

</div>

## Estructura actual del repositorio

```
DashboardV1_MGA/
│
├── Data/
│   ├── DashboardV1_MGA.pbip
│   ├── DashboardV1_MGA.Report/
│   │   ├── definition.pbir
│   │   ├── .pbi/
│   │   │   └── localSettings.json
│   │   ├── definition/
│   │   │   ├── report.json
│   │   │   ├── version.json
│   │   │   └── pages/
│   │   │       ├── pages.json
│   │   │       └── .../page.json + visuals/
│   │   └── StaticResources/
│   │       ├── RegisteredResources/
│   │       └── SharedResources/
│   └── DashboardV1_MGA.SemanticModel/
│       ├── definition.pbism
│       ├── diagramLayout.json
│       └── definition/
│           ├── database.tmdl
│           ├── model.tmdl
│           ├── relationships.tmdl
│           ├── cultures/
│           │   └── es-NI.tmdl
│           └── tables/
│               ├── DateTableTemplate_*.tmdl
│               ├── Edificios.tmdl
│               ├── Inventario_CurCarazo.tmdl
│               └── LocalDateTable_*.tmdl
├── Docs/
│   ├── 01-List_Dataset.md
│   ├── 02-Conexion_PowerBi.md
│   ├── 03-Controles_Agregados.md
│   ├── 04-Add_UbicacionesTable.md
│   └── 05-Convencion_Datos_Vacios.md
├── Report/
│   └── DashboardV1_MGA.pbix
└── theme/
    ├── CurCarazo_Theme.json
    ├── CurCarazo_Theme_UNAN.json
    └── assets/
```

---

### MAOG18 | UNAN Managua-CUR Carazo