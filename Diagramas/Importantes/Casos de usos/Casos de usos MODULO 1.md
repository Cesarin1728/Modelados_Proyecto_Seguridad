[[Casos de usos]]
@startuml
left to right direction
skinparam packageStyle rectangle
skinparam shadowing false

actor "Usuario (App Móvil)" as user
actor "Operador (Panel Web)" as operator

rectangle "Módulo 1: Emergencias, SOS y Monitoreo en Tiempo Real" {
  ' Acciones del Usuario
  usecase "Activar Alerta SOS (Botón / Gesto / Voz)" as UC_SOS
  usecase "Iniciar Cuenta Regresiva (3 s)" as UC_Countdown
  usecase "Cancelar SOS (PIN / Biometría)" as UC_CancelSOS
  usecase "Transmitir Ubicación GPS (Frecuencia 30 s)" as UC_GPS_SOS
  usecase "Emitir Alerta por Red Mesh BLE (Modo Offline)" as UC_BLE
  usecase "Transmitir Audio/Video P2P (WebRTC)" as UC_WebRTC_App

  ' Acciones del Operador
  usecase "Visualizar Alerta SOS en Cola" as UC_ViewAlert
  usecase "Calcular y Mostrar Prioridad (Edad + Salud)" as UC_Priority
  usecase "Tomar / Bloquear Alerta SOS" as UC_LockAlert
  usecase "Establecer Canal WebRTC (Voz / Video / Chat)" as UC_WebRTC_Op
  usecase "Reenviar Alerta No Atendida (5 min)" as UC_RetryAlert
  usecase "Escalar Automáticamente al 911 (15 min)" as UC_911
  usecase "Clasificar Incidente y Cerrar Alerta" as UC_CloseAlert
  usecase "Generar Expediente Formativo" as UC_Expediente
}

' Relaciones de Usuario
user --> UC_SOS
user --> UC_CancelSOS
user --> UC_BLE

' Relaciones de Operador
operator --> UC_ViewAlert
operator --> UC_LockAlert
operator --> UC_WebRTC_Op
operator --> UC_CloseAlert

' Inclusiones y Extensiones
UC_SOS .> UC_Countdown : <<include>>
UC_SOS .> UC_GPS_SOS : <<include>>
UC_SOS .> UC_WebRTC_App : <<include>>
UC_SOS <.. UC_CancelSOS : <<extend>>

UC_ViewAlert .> UC_Priority : <<include>>
UC_ViewAlert <.. UC_RetryAlert : <<extend>>
UC_ViewAlert <.. UC_911 : <<extend>>

UC_LockAlert .> UC_WebRTC_Op : <<include>>
UC_CloseAlert .> UC_Expediente : <<include>>

@enduml