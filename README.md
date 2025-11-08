# Diagrama de Arquitectura - TURBORESPUESTO25

```mermaid
flowchart LR
  subgraph Actors ["Entorno / Actores"]
    A1["Empleado (Cliente de punto de venta)"]
    A2["Administrador"]
    A3["Impresoras / Periféricos"]
    A4["Servicios externos (Email / SMS / Pago)"]
  end

  subgraph System ["TURBORESPUESTO25 (Aplicación de escritorio WinForms - VB.NET)"]
    direction TB
    subgraph UI ["Capa: Interfaz (WinForms)"]
      UI1["Formularios: menu, registraproducto, registrarventa, compras, proveedores, clientes, reportes"]
    end

    subgraph BL ["Capa: Lógica de negocio"]
      S1["Servicios: VentasService, ComprasService, InventarioService, UsuariosService, ReportesService"]
    end

    subgraph DAL ["Capa: Persistencia / Integración"]
      DAL1["DataAccess: MySql.Data (MySqlConnector) — consultas SQL"]
      DAL2["Export/Import: CSV / Impresora / Integración REST (opcional)"]
    end
  end

  subgraph Infra ["Infraestructura / Despliegue"]
    Host["Host Windows (PC cliente)"]
    DBServer["Servidor MySQL (turborepuestodb)"]
    Firewall["Firewall / Zona de red"]
    Backup["Backup / Almacenamiento"]
    Container["(Opcional) Contenedor / VM / Azure"]
  end

  A1 -->|"Interacción UI (mouse/teclado)"| UI1
  A2 -->|"Interacción UI / Administración"| UI1
  UI1 -->|"Llamadas de servicio (métodos locales)"| S1
  S1 -->|"Conexión MySQL TCP 3306 (MySql.Data)"| DAL1
  S1 -->|"Enviar eventos / colas / REST"| DAL2
  DAL1 -->|"SQL TCP 3306"| DBServer
  A3 -->|"Impresión / generación de PDFs"| DAL2
  A4 -->|"SMTP/HTTPS REST (opcional)"| DAL2
  Host -->|"Ejecuta"| System
  Host --> Firewall
  DBServer --> Backup
  Firewall -.->|"Separación de red"| DBServer

  classDef boundary fill:#f8f9fa,stroke:#2b2b2b,stroke-width:1px
  class Infra boundary
```

## Descripción del Sistema

Este diagrama muestra la arquitectura de TURBORESPUESTO25, una aplicación de escritorio desarrollada en VB.NET con WinForms que gestiona un sistema de punto de venta.

### Componentes Principales:

- **Capa UI**: Formularios de interfaz de usuario
- **Capa BL**: Servicios de lógica de negocio
- **Capa DAL**: Acceso a datos y integración con servicios externos
- **Infraestructura**: Servidor MySQL y componentes de despliegue
