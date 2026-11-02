[[Importantes]]
@startuml
autonumber
skinparam responseMessageBelowArrow true
skinparam maxMessageSize 150

actor "Usuario (App Kotlin)" as User
participant "API / Node.js" as Backend
database "PostgreSQL / Supabase" as DB
participant "Cron Worker (Node)" as Cron
participant "Servicio Push (FCM)" as FCM

== 1. Registro de Salida Planeada ==
User -> Backend: POST /api/salida-planeada (destino, horaInicio, horaRegreso)
activate Backend
Backend -> DB: Guardar SalidaPlaneada (Estado: PROGRAMADA)
Backend --> User: 201 Created (idSalida)
deactivate Backend

== 2. Notificacion Preventiva (10 min antes) ==
Cron -> DB: Consultar salidas proximas a vencer (10 min antes)
DB --> Cron: Retorna lista de salidas activas
Cron -> FCM: Enviar notificacion push de recordatorio
FCM -> User: Recordatorio: Confirma tu llegada segura antes de vencer el plazo

== 3. Confirmacion de Llegada o Inactividad ==
alt Usuario Confirma Llegada Segura
  User -> Backend: PUT /api/salida-planeada/id/confirmar (PIN / Biometria)
  activate Backend
  Backend -> DB: Actualizar SalidaPlaneada (Estado: COMPLETADA)
  Backend --> User: 200 OK (Llegada registrada)
  deactivate Backend
else Plazo Vencido sin Confirmacion (Inactividad)
  Cron -> DB: Detectar salida vencida no confirmada
  Cron -> Backend: Detonar Alerta de Inactividad Automatizadamente
  Backend -> DB: Registrar AlertaSOS por Inactividad
  Backend -> FCM: Notificar a Grupo Familiar y Operador
end

@enduml