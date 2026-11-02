[[Casos de usos MODULO 1]]
@startuml
left to right direction
skinparam packageStyle rectangle
skinparam shadowing false

actor "Usuario (App Móvil)" as user
actor "Admin de Comunidad" as adminCom

rectangle "Módulo 2: Grupos, Geocercas, Perfil y Salidas Planeadas" {
  ' Funciones de Usuario
  usecase "Gestionar Perfil de Salud (Enfermedades, Edad)" as UC_Health
  usecase "Configurar Zona Segura (Geocerca 50m - 3km)" as UC_Geofence
  usecase "Recibir Alerta por Salida de Geocerca" as UC_GeoAlert
  usecase "Registrar Salida Planeada" as UC_PlanExit
  usecase "Configurar Recordatorio Preventivo (10 min antes)" as UC_Preventive
  usecase "Confirmar Llegada Segura" as UC_SafeArrival
  usecase "Disparar Alerta por Inactividad en Salida" as UC_InactivityAlert

  ' Funciones del Admin de Comunidad
  usecase "Crear / Administrar Grupo (Hasta 3 grupos)" as UC_CreateGroup
  usecase "Invitar / Remover Integrantes del Grupo" as UC_Members
  usecase "Configurar Permisos de Visibilidad del Grupo" as UC_Perms
}

' Relaciones de Usuario
user --> UC_Health
user --> UC_Geofence
user --> UC_PlanExit
user --> UC_SafeArrival

' Admin de Comunidad hereda de Usuario y agrega administración
adminCom --|> user
adminCom --> UC_CreateGroup
adminCom --> UC_Members
adminCom --> UC_Perms

' Inclusiones y Extensiones
UC_Geofence <.. UC_GeoAlert : <<extend>>
UC_PlanExit .> UC_Preventive : <<include>>
UC_PlanExit <.. UC_InactivityAlert : <<extend>>
UC_PlanExit <.. UC_SafeArrival : <<extend>>

@enduml