[[Componentes]]
@startuml
skinparam componentStyle uml2
skinparam shadowing false

package "App Android (Kotlin)" {
  [UI y ViewModels] as UI
  [Base de Datos Local (Room)] as RoomDB
  [Servicio de GPS y Sensores] as GPSService
  [Modulo BLE Mesh (Offline)] as BLEModule
}

UI --> RoomDB : Lectura / Escritura de datos
UI --> GPSService : Captura latitud y longitud
UI --> BLEModule : Dispara auxilio sin conexion

@enduml