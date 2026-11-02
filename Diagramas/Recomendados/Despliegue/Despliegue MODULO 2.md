[[Despliegue MODULO 1]]
@startuml
skinparam shadowing false
skinparam linetype ortho
skinparam nodesep 50
skinparam ranksep 50

package "Smartphones de Prueba" {
  node "Smartphone 1\n(Usuario A)" as Phone1
  node "Smartphone 2\n(Usuario B - BLE)" as Phone2
  node "Smartphone 3\n(Usuario C)" as Phone3
}

package "Estaciones Web de Prueba" {
  node "PC Operador 1\n(Atencion SOS)" as PC1
  node "PC Operador 2\n(Atencion SOS)" as PC2
  node "PC Admin General\n(Monitoreo)" as PC3
}

node "Servidor de Despliegue" as ServerNode {
  component "API Node.js +\nSupabase DB" as Server
}

Phone1 --> Server : WSS / HTTPS
Phone2 .. Phone1 : Red Mesh BLE (Offline)
Phone3 --> Server : WSS / HTTPS

PC1 --> Server : WSS / HTTPS
PC2 --> Server : WSS / HTTPS
PC3 --> Server : HTTPS

Phone1 .. PC1 : Streaming WebRTC P2P

@enduml