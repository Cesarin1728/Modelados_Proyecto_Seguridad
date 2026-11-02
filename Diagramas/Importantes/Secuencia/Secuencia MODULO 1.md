[[Secuencia]]
@startuml
autonumber
skinparam responseMessageBelowArrow true
skinparam maxMessageSize 150

actor "Usuario (App Kotlin)" as App
participant "API / Node.js" as Backend
participant "WebSocket Server" as WS
database "PostgreSQL / PostGIS" as DB
actor "Operador (Panel Angular)" as Panel
participant "Servicio Push (FCM)" as FCM
actor "Contactos / 911" as RedAuxilio

== 1. Detonacion y Confirmacion SOS ==
App -> App: Usuario acciona gesto / boton / voz
App -> App: Inicia cuenta regresiva (3 s)

alt Usuario Cancela
  App -> App: Aborta envio (Requiere PIN / Biometria)
else SOS Confirmado (Sin cancelacion)
  App -> Backend: POST /api/sos/detonar (lat, lng, idUsuario)
  activate Backend
  Backend -> DB: Consultar PerfilSalud y Edad del Usuario
  DB --> Backend: Retorna datos de edad y factores de salud
  Backend -> Backend: Calcular Prioridad Acumulada (Edad + Salud)
  Backend -> DB: Insertar AlertaSOS (Estado: CREADA)
  Backend --> App: 201 Created (idAlerta)
  deactivate Backend

  Backend -> WS: Emitir evento NUEVA_ALERTA (Con nivel de prioridad)
  WS -> Panel: Renderizar tarjeta de alerta con prioridad cromatica
  
  Backend -> FCM: Solicitar envio de alerta a Grupo Familiar
  FCM -> RedAuxilio: Notificacion Push "Familiar en Emergencia"
end

== 2. Rastreo GPS y Audio/Video P2P ==
loop Cada 30 segundos durante SOS Activo
  App -> WS: Stream Coordenadas GPS (lat, lng, nivelBateria)
  WS -> Panel: Actualizar marcador en Mapa en tiempo real
end

Panel -> Backend: PUT /api/sos/id/atender (Operador acepta y bloquea la alerta)
activate Backend
Backend -> DB: Actualizar AlertaSOS (Estado: EN_ATENCION, idOperador)
Backend -> WS: Broadcast ALERTA_TOMADA
deactivate Backend

App -> Panel: Iniciar canal P2P WebRTC (Audio / Video en vivo)
Panel -> App: Confirmar conexion WebRTC

== 3. Reglas de Escalacion por Tiempo ==
alt Alerta NO atendida en 5 minutos
  Backend -> WS: Incrementar Prioridad (+1) y reemitir
else Alerta NO atendida en 15 minutos
  Backend -> RedAuxilio: Disparar llamada/enlace automatico a servicios 911
  Backend -> DB: Actualizar Estado: ESCALADA_911
end

@enduml