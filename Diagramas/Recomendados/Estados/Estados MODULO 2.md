[[Estados MODULO 1]]
@startuml
skinparam shadowing false
skinparam linetype ortho
skinparam nodesep 80
skinparam ranksep 70

[*] --> Programada : Usuario define\nruta y horario

Programada --> Cancelada : Usuario cancela\nsalida
Programada --> RecordatorioEnviado : Faltan 10 min\npara limite

RecordatorioEnviado --> Completada : Confirma llegada\nsegura (PIN)
RecordatorioEnviado --> AlertaInactividad : Vence tiempo\nlimite

AlertaInactividad --> EscaladaSOS : Genera alerta\nSOS automatica

Completada --> [*]
Cancelada --> [*]
EscaladaSOS --> [*]

@enduml