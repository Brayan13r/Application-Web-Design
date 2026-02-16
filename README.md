# Proyecto Halcon - Sistema de Gestion de Pedidos

## Informacion del Estudiante
* **Nombre completo:** Brayan Eleazar Villegas Navarro
* **Matricula:** Al03091719
* **Carrera:** IDS (Ingenieria en Desarrollo de Software)
* **Semestre:** 6
* **Materia:** Aplicaciones web
* **Profesor:** Cristopher Gerardo Gaytan Diaz

---

## 1. Descripcion del Proyecto
Aplicacion web desarrollada para "Halcon", una distribuidora de materiales de construccion. Este sistema automatiza los procesos internos de venta, almacen y distribucion, permitiendo a los clientes rastrear el estatus de sus pedidos en tiempo real y visualizar evidencias fotograficas de sus entregas.

## 2. Metodologia de Trabajo
**Seleccion:** Metodologia Agil - Scrum.

**Justificacion:**
Para el desarrollo de la plataforma de Halcon, Scrum es la metodologia idonea debido a la naturaleza modular de los requerimientos. Al dividir el proyecto en Sprints (iteraciones cortas), se pueden entregar modulos funcionales de forma incremental. Esto permite estructurar el desarrollo separando la logica de autenticacion y roles, la gestion del inventario y el flujo logistico de las entregas. Esta adaptabilidad garantiza que el software siempre este alineado con la logica de negocio de la distribuidora y permite integrar cambios o mejoras sin afectar los modulos ya terminados.

## 3. Diagramas del Sistema

### 3.1 Diagrama BPMN (Logica de Negocio)
```mermaid
flowchart TD
    subgraph Cliente
        C1[Realiza pedido por telefono]
        C2[Consulta estatus en sistema]
    end

    subgraph Ventas
        V1[Registra pedido y datos fiscales]
        V2((Estatus: Ordenado))
    end

    subgraph Almacen
        A1[Revisa pedido]
        A2{¿Material en stock?}
        A3[Cambia estatus a: En proceso]
        A4[Prepara pedido para envio]
        A5[Notifica falta de stock]
    end

    subgraph Compras
        P1[Adquiere material con proveedor]
    end

    subgraph Ruta
        R1[Carga material a la unidad]
        R2[Sube fotografia de carga]
        R3((Estatus: En ruta))
        R4[Entrega en domicilio]
        R5[Sube fotografia de entrega]
        R6((Estatus: Entregado))
    end

    C1 --> V1
    V1 --> V2
    V2 --> A1
    A1 --> A2
    A2 -- Si --> A3
    A2 -- No --> A5
    A5 --> P1
    P1 --> A3
    A3 --> A4
    A4 --> R1
    R1 --> R2
    R2 --> R3
    R3 --> R4
    R4 --> R5
    R5 --> R6
    R6 -.-> C2
```

### 3.2 Diagrama de Actividades (Ciclo de vida del pedido)
```mermaid
stateDiagram-v2
    [*] --> RegistroPedido
    RegistroPedido --> Ordenado : Vendedor registra datos
    Ordenado --> ValidacionStock : Almacen revisa
    
    state ValidacionStock {
        [*] --> Revision
        Revision --> EnStock : Material disponible
        Revision --> SinStock : Material faltante
        SinStock --> CompraExterna : Compras adquiere
        CompraExterna --> EnStock
    }
    
    ValidacionStock --> EnProceso : Almacen prepara
    EnProceso --> CargaUnidad : Ruta asignada
    CargaUnidad --> FotografiaCarga : Ruta sube evidencia
    FotografiaCarga --> EnRuta
    EnRuta --> EntregaDomicilio
    EntregaDomicilio --> FotografiaDescarga : Ruta sube evidencia
    FotografiaDescarga --> Entregado
    Entregado --> [*]
```

### 3.3 Diagrama de Casos de Uso
```mermaid
flowchart LR
    %% Actores
    Cliente[Cliente]
    Admin[Administrador]
    Ventas[Ventas]
    Almacen[Almacen]
    Compras[Compras]
    Ruta[Ruta]

    %% Casos de Uso dentro del Sistema Halcon
    subgraph Sistema Halcon
        UC1([Consultar estatus de pedido])
        UC2([Gestionar usuarios y roles])
        UC3([Registrar nuevo pedido])
        UC4([Gestionar stock e inventario])
        UC5([Registrar compras externas])
        UC6([Actualizar estatus de envio])
        UC7([Subir evidencia fotografica])
        UC8([Gestion general de pedidos CRUD])
    end

    %% Relaciones
    Cliente --> UC1
    Admin --> UC2
    Admin --> UC8
    Ventas --> UC3
    Ventas --> UC8
    Almacen --> UC4
    Almacen --> UC8
    Compras --> UC5
    Compras --> UC8
    Ruta --> UC6
    Ruta --> UC7
    Ruta --> UC8
```

### 3.4 Diagrama de Clases
```mermaid
classDiagram
    class Usuario {
        +int idUsuario
        +String nombre
        +String correo
        +String contrasena
        +autenticar()
    }
    class Rol {
        +int idRol
        +String nombreDepartamento
    }
    class Cliente {
        +String numeroCliente
        +String razonSocial
        +String rfc
        +String direccionFiscal
    }
    class Pedido {
        +String numeroFactura
        +DateTime fechaHora
        +String direccionEntrega
        +String observaciones
        +String estatusActual
        +boolean estaEliminado
        +actualizarEstatus()
        +eliminarLogico()
        +restaurar()
    }
    class Evidencia {
        +int idEvidencia
        +String urlImagen
        +String etapa
        +DateTime fechaCaptura
    }

    Usuario "1" -- "1" Rol : pertenece a
    Cliente "1" -- "0..*" Pedido : solicita
    Pedido "1" -- "0..2" Evidencia : documenta
    Usuario "1" -- "0..*" Pedido : administra
```

### 3.5 Diagrama Entidad-Relacion (ER)
```mermaid
erDiagram
    ROL {
        int id_rol PK
        varchar nombre_rol
    }
    USUARIO {
        int id_usuario PK
        int id_rol FK
        varchar nombre_completo
        varchar correo_electronico
        varchar hash_contrasena
    }
    CLIENTE {
        varchar numero_cliente PK
        varchar razon_social
        varchar rfc
        varchar direccion_fiscal
    }
    PEDIDO {
        varchar numero_factura PK
        varchar numero_cliente FK
        int id_usuario_registro FK
        datetime fecha_pedido
        varchar direccion_entrega
        text observaciones
        varchar estatus
        boolean eliminado_logico
    }
    EVIDENCIA_FOTOGRAFICA {
        int id_evidencia PK
        varchar numero_factura FK
        int id_usuario_ruta FK
        varchar tipo_evidencia
        varchar url_archivo
        datetime fecha_registro
    }

    ROL ||--o{ USUARIO : tiene
    CLIENTE ||--o{ PEDIDO : genera
    USUARIO ||--o{ PEDIDO : gestiona
    PEDIDO ||--o| EVIDENCIA_FOTOGRAFICA : adjunta
    USUARIO ||--o{ EVIDENCIA_FOTOGRAFICA : sube
```

## 4. Reflexion Personal
El analisis e interpretacion del caso de la distribuidora Halcon me permitio comprender la importancia de traducir las reglas de negocio en un diseño arquitectonico solido antes de escribir codigo. Identificar correctamente los actores y limitar sus permisos a traves de un control de acceso basado en roles garantiza la seguridad y la integridad de las operaciones diarias de la empresa.

Adicionalmente, el diseño de la base de datos aplicando eliminacion logica para los pedidos protege el historial de la informacion y evita errores criticos en auditorias o consultas futuras. La elaboracion de los diagramas (BPMN, Clases, Casos de Uso, Actividades y ER) me facilito visualizar como interactuan las diferentes partes del sistema, desde que el cliente llama hasta que se entrega el material con su respectiva evidencia, asegurando que se cubran al cien por ciento los requerimientos establecidos en el planteamiento inicial.
