[[Recomendados]]
@startuml
skinparam shadowing false
skinparam linetype ortho
skinparam nodesep 80
skinparam ranksep 70

[*] --> Creada : Detonacion SOS

state Creada {
  [*] --> CuentaRegresiva
  CuentaRegresiva --> Cancelada : Cancelar con PIN\no Biometria (3 s)
  CuentaRegresiva --> Emitida : Expira tiempo (3 s)
}

Cancelada --> [*]

Emitida --> EnAtencion : Operador toma\ny bloquea
Emitida --> Escalada911 : Sin atencion\n(> 15 min)

EnAtencion --> Resuelta : Cierre formal\ncon expediente
EnAtencion --> FalsaAlarma : Descarte por\noperador

Escalada911 --> Resuelta : Atencion 911\nfinalizada

Resuelta --> [*]
FalsaAlarma --> [*]

@enduml