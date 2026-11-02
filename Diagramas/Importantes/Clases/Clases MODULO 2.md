[[Clases MODULO 1]]

@startuml
skinparam classAttributeIconSize 0
skinparam shadowing false
left to right direction

' --- ENUMERACIONES ---
enum EstadoAlerta {
  CREADA
  EN_ATENCION
  ESCALADA_911
  RESUELTA
  FALSA_ALARMA
}

enum PrioridadAlerta {
  BAJA
  MEDIA
  ALTA
  CRITICA
}

' --- REFERENCIA DE CONEXIÓN (VIENE DE PARTE 1) ---
class Usuario <<Viene de Parte 1>> {
  - idUsuario: UUID
  - nombre: String
  - rol: String
}

' --- ENTIDADES DE OPERACIÓN Y PREVENCIÓN ---
class AlertaSOS {
  - idAlerta: UUID
  - fechaHoraInicio: DateTime
  - fechaHoraCierre: DateTime
  - latitud: Double
  - longitud: Double
  - prioridadCalculada: PrioridadAlerta
  - estado: EstadoAlerta
  - modoOfflineBLE: Boolean
  + calcularPrioridad(edad: Int, puntajeSalud: Int): Int
  + cambiarEstado(nuevoEstado: EstadoAlerta): Void
}

class UbicacionGPS {
  - idUbicacion: UUID
  - latitud: Double
  - longitud: Double
  - precisionMetros: Float
  - nivelBateria: Int
  - timestamp: DateTime
}

class ExpedienteIncidente {
  - idExpediente: UUID
  - folioOficial: String
  - resumenOperador: String
  - clasificacionCategorizada: String
  - derivado911: Boolean
  - fechaRegistro: DateTime
  + generarPDF(): Byte[]
}

class ZonaSegura {
  - idZona: UUID
  - nombre: String
  - latitudCentro: Double
  - longitudCentro: Double
  - radioMetros: Double
  - activa: Boolean
  + validarGeocerca(lat: Double, lng: Double): Boolean
}

class SalidaPlaneada {
  - idSalida: UUID
  - destino: String
  - horaInicio: DateTime
  - horaEstimadaRegreso: DateTime
  - recordatorioEnviado: Boolean
  - llegadaConfirmada: Boolean
  + enviarNotificacionPreventiva(): Void
  + evaluarInactividad(): Boolean
}

class ZonaRoja {
  - idZonaRoja: UUID
  - poligonoGeoJSON: String
  - totalIncidentes: Int
  - aprobadaPorAdmin: Boolean
  - anioEvaluacion: Int
  + validarThresholdAlertas(): Boolean
  + actualizarConteoIncidentes(): Void
}

' --- RELACIONES ---
Usuario "1" -- "0..*" AlertaSOS : emite >
Usuario "0..1" -- "0..*" AlertaSOS : atiende (Operador) >
AlertaSOS "1" *-- "1..*" UbicacionGPS : registra >

AlertaSOS "1" -- "0..1" ExpedienteIncidente : genera >
Usuario "0..1" -- "0..*" ExpedienteIncidente : redacta (Operador) >

Usuario "1" -- "0..*" ZonaSegura : configura >
Usuario "1" -- "0..*" SalidaPlaneada : programa >
Usuario "0..1" -- "0..*" ZonaRoja : autoriza (Admin Gen) >

' --- NOTA DE CONEXIÓN DE ENTRADA ---
note left of Usuario
  <b>VIENE DE PARTE 1:</b>
  Reutiliza la entidad 'Usuario'
  para vincular la identidad y 
  salud con el flujo de emergencias.
end note

@enduml