[[Componentes (Panel) MODULO 1]]
@startuml
skinparam componentStyle uml2
skinparam shadowing false

package "Panel Web (Angular)" {
  [Panel Operadores] as Web
}

package "Backend Core (Node.js API)" {
  [REST Controller] as RESTWeb
  [WebSocket Gateway] as WSWeb
}

cloud "Infraestructura y Servicios Web" {
  database "PostgreSQL / Supabase" as DB
  node "Stripe API" as Stripe
  node "Amazon SES" as SES
  node "Mapbox Services" as MapboxWeb
}

Web --> RESTWeb : HTTP / JSON (Expedientes y Config)
Web --> WSWeb : Socket.io (Recepcion instantanea SOS)
RESTWeb --> DB : Consultas GIS y Base de datos
RESTWeb --> Stripe : Gestion comercial y pagos
RESTWeb --> SES : Notificaciones por email
Web --> MapboxWeb : Mapa de calor (Zonas Rojas >400)

@enduml