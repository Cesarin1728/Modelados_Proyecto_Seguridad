[[Recomendados]]
@startuml
skinparam shadowing false
skinparam linetype ortho
skinparam nodesep 60
skinparam ranksep 60

node "Cliente Movil" as MobileNode {
  artifact "App Android\n(Kotlin)" as MobileApp
}

node "Cliente Web" as WebNode {
  artifact "Panel Angular\n(Navegador Web)" as WebApp
}

node "Servidor Backend" as BackendNode {
  component "API REST &\nWebSockets (Node.js)" as Backend
}

node "Servidor Media" as RTCNode {
  component "Servidor STUN / TURN\n(WebRTC Relay)" as TurnServer
}

cloud "Supabase & Servicios Cloud" as Cloud {
  database "PostgreSQL /\nPostGIS" as DB
  component "Firebase (FCM)" as FCM
  component "Amazon SES" as SES
  component "Stripe / Mapbox" as ExternalAPIs
}

MobileApp --> Backend : HTTPS / WSS
WebApp --> Backend : HTTPS / WSS
MobileApp --> TurnServer : WebRTC Relay
MobileApp .. WebApp : Audio / Video P2P

Backend --> DB : SQL / PostGIS
Backend --> FCM : Push Alerts
Backend --> SES : Emails
Backend --> ExternalAPIs : Pagos y Mapas

@enduml