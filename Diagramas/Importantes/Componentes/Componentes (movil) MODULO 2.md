[[Componentes (movil) MODULO 1]]
@startuml
skinparam componentStyle uml2
skinparam shadowing false

package "App Android (Kotlin)" {
  [App Mobile Core] as App
}

package "Backend Core (Node.js API)" {
  [REST API Controller] as REST
  [WebSocket Controller] as WS
  [Modulo Senalizacion WebRTC] as Signaling
}

cloud "Servicios Externos" {
  node "Firebase Cloud Messaging (FCM)" as FCM
  node "Mapbox SDK" as Mapbox
  node "Servidor STUN / TURN" as WebRTC
}

App --> REST : HTTP / JSON (Auth, Perfil, Salidas)
App --> WS : Socket.io (GPS cada 30s)
App --> Signaling : Handshake Audio / Video
FCM --> App : Notificaciones Push
App --> Mapbox : Renderizado de mapas
Signaling --> WebRTC : Establecer conexion P2P

@enduml