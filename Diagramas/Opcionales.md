Son opcionales no mas porque se me antojo la verdad
@startuml
skinparam shadowing false
skinparam linetype ortho
skinparam nodesep 70
skinparam ranksep 70

package "Capa de Presentacion (Clientes)" {
  package "com.seguridad.mobile" as MobilePkg {
    [UI & Composables]
    [Local DB & Mesh BLE]
  }

  package "com.seguridad.web" as WebPkg {
    [Dashboard & Mapas UI]
    [RxJS Streaming Client]
  }
}

package "Capa Backend Core (Arquitectura Hexagonal)" {
  package "com.seguridad.backend.primary" as PrimaryPkg {
    [REST Controllers]
    [WebSocket Gateway]
  }

  package "com.seguridad.backend.application" as AppPkg {
    [SOS Service]
    [Auth & 2FA Service]
    [Geofencing Service]
  }

  package "com.seguridad.backend.domain" as DomainPkg {
    [Entities & Aggregates]
    [Domain Events]
  }

  package "com.seguridad.backend.secondary" as SecondaryPkg {
    [PostGIS DB Adapter]
    [FCM Push Adapter]
    [WebRTC Signaling Adapter]
  }
}

package "Servicios Externos y Persistencia" {
  database "Supabase / PostgreSQL" as DB
  node "Firebase Cloud Messaging" as FCM
  node "Servicios Cloud (SES, Stripe, Mapbox)" as ExternalNode
}

' Dependencias de Paquetes (Import/Use)
MobilePkg ..> PrimaryPkg : <<use>> (REST / WS)
WebPkg ..> PrimaryPkg : <<use>> (REST / WS)

PrimaryPkg ..> AppPkg : <<import>>
AppPkg ..> DomainPkg : <<import>>
SecondaryPkg ..> DomainPkg : <<import>>
AppPkg ..> SecondaryPkg : <<use>>

SecondaryPkg ..> DB : <<use>>
SecondaryPkg ..> FCM : <<use>>
SecondaryPkg ..> ExternalNode : <<use>>

@enduml