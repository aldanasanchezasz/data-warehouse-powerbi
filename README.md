# Data Warehouse y dashboards de gestión: The Drinking Company

Diseño e implementación de un **Data Warehouse completo** para una empresa de bebidas ficticia, integrando cinco tipos de fuentes operacionales en un modelo dimensional y explotándolo con **dashboards interactivos en Power BI** para las gerencias.

**Stack:** SQL Server · T-SQL · SSIS (Integration Services) · MySQL · Power BI · modelado dimensional (Kimball)

## Arquitectura

```text
Fuentes operacionales          ETL (SSIS)                       Consumo
──────────────────────         ──────────────────────────       ──────────────
SQL Server  (ventas hist.)  ┐
MySQL       (ventas act.)   │   Staging DB   (carga cruda)
XML         (clientes)      ├─► Intermediate (limpieza y      ─► Data Warehouse ─► Power BI
TXT         (productos,     │                 transformación)    (esquema estrella)
             stock)         │
Excel       (empleados,     ┘
             feriados)
```

- **5 dimensiones:** fecha, cliente, empleado, grupo etario y producto.
- **3 tablas de hechos:** facturación, detalle de facturación y stock.
- **Orquestación:** un paquete maestro de SSIS ejecuta los paquetes de extracción y transformación en orden.

![Modelo dimensional](Modelo_dimensional/Modelado_Dimensional.png)

## Cómo reproducirlo

1. Restaurar los backups de `Entregables/` (Staging, Intermediate y Data Warehouse) en SQL Server, o crear las bases con los scripts de [`Database_Creation_Queries/`](Database_Creation_Queries/).
2. Abrir `TDC_Project/TDC_Project.sln` en Visual Studio con SSDT y la extensión de Integration Services, y ejecutar `Master_Pkg.dtsx`. La configuración de conexiones (incluido el driver ODBC de MySQL) está en [`Documentacion_Ingesta_SSIS.md`](Database_Creation_Queries/Documentacion_Ingesta_SSIS.md).
3. Abrir `PowerBI_DW_TDC_grupo20_.pbix` en Power BI Desktop.

## Equipo

Lucas Díaz Celauro · Lisandro Odisio Martinelli · Aldana Desiré Sánchez
Trabajo práctico final de Bases de Datos 2 (TUIA – UNR).

---

## Estructura del Repositorio

El proyecto está organizado en las siguientes carpetas principales, separando las fuentes crudas, los scripts de bases de datos, el proyecto de integración y los entregables finales:

```text
├── Trabajo Práctico Final - Enunciado.pdf  # Requerimientos y consignas del TP
├── Trabajo Práctico Final - Software.pdf   # Requisitos de instalación
├── .gitignore                              # Archivos ignorados por Git
├── PowerBI_DW_TDC_grupo20_.pbix            # Dashboards interactivos en Power BI
├── PowerBI_Queries.sql                     # Consultas sobre el DW usadas por los dashboards
│
├── data/                                   # Fuentes de datos crudas (Staging Area)
│   ├── Area_Comercializacion/              # Clientes (XML), regiones y ventas históricas/actuales
│   ├── Area_Produccion/                    # Catálogo de productos y variaciones de stock (TXT)
│   └── Area_Recursos_Humanos/              # Nómina de empleados y feriados (Excel)
│
├── Database_Creation_Queries/              # Scripts SQL para la arquitectura de datos
│   ├── SQL_Staging_DB_Creation.sql         # Creación de la base de datos de Staging
│   ├── SQL_Intermediate_DB_Creation.sql    # Creación de la base de datos Intermedia
│   ├── SQL_DW_DB_Creation.sql              # Creación del Data Warehouse final
│   └── Documentacion_Ingesta_SSIS.md       # Notas y documentación sobre el proceso ETL para la ingesta a staging
│
├── Modelo_dimensional/                     # Diseño Conceptual y Lógico
│   ├── Modelado_Dimensional.png            # Diagrama del esquema estrella
│   └── Modelado_dimensional.sql            # Script complementario del modelado
│
├── TDC_Project/                            # Proyecto de SSIS (Visual Studio)
│   ├── Master_Pkg.dtsx                     # Paquete Maestro que orquesta el flujo general
│   ├── SSIS_Pkg_*.dtsx                     # Paquetes individuales de extracción y transformación
│   └── TDC_Project.sln                     # Solución principal de Visual Studio
│
└── Entregables/                            # Archivos finales para la corrección del TP
    ├── DataBase_TDC_Staging_DB.zip         # Backup comprimido de la BD Staging
    ├── DataBase_TDC_Intermediate.zip       # Backup comprimido de la BD Intermedia
    ├── DataBase_TDC_DataWarehouse.zip      # Backup comprimido del DW final
    └── SSIS_Project.zip                    # Proyecto ETL comprimido completo
```
