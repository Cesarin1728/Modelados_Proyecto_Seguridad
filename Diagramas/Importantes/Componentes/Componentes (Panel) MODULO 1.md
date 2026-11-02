[[Componentes]]
@startuml
skinparam componentStyle uml2
skinparam shadowing false

package "Panel Web (Angular)" {
  [Dashboard UI Operadores] as UIWeb
  [RxJS Event Stream] as Stream
  [Cliente Receptor WebRTC] as RTCClient
}

UIWeb --> Stream : Escucha cola de alertas
UIWeb --> RTCClient : Reproduce streaming en vivo Audio/Video

@enduml