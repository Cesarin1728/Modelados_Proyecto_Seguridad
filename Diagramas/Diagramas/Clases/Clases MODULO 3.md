@startuml
left to right direction
skinparam packageStyle rectangle
skinparam shadowing false

actor "Admin General" as adminGen
actor "Operador (Panel Web)" as operator

rectangle "Módulo 3: Administración General, Zonas Rojas y Plataforma" {
  ' Acciones del Admin General
  usecase "Gestionar Cuentas de Usuarios" as UC_ManageUsers
  usecase "Alta / Baja / Asignación de Operadores" as UC_ManageOps
  usecase "Gestionar Planes y Suscripciones (Stripe)" as UC_Stripe
  usecase "Visualizar Mapa de Calor de Zonas Rojas" as UC_Heatmap
  usecase "Aprobar Zonas Rojas Automáticas (>400 incidentes)" as UC_ApproveRedZone
  usecase "Consultar Historial Global de Expedientes" as UC_GlobalHistory
  usecase "Generar Reportes y Estadísticas de Incidentes" as UC_Reports
  usecase "Configurar Tiempos Globale de Sistema (5/15 min)" as UC_SysConfig
}

' Relaciones
adminGen --> UC_ManageUsers
adminGen --> UC_ManageOps
adminGen --> UC_Stripe
adminGen --> UC_ApproveRedZone
adminGen --> UC_GlobalHistory
adminGen --> UC_Reports
adminGen --> UC_SysConfig

operator --> UC_Heatmap
operator --> UC_GlobalHistory

UC_ApproveRedZone .> UC_Heatmap : <<include>>

@enduml