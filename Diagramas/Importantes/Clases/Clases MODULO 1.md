[[Importantes]]
@startuml
skinparam classAttributeIconSize 0
skinparam shadowing false
left to right direction

' --- ENUMERACIONES ---
enum RolUsuario {
  ADMIN_GENERAL
  OPERADOR
  ADMIN_COMUNIDAD
  USUARIO
}

' --- ENTIDADES DE NÚCLEO ---
class Usuario {
  - idUsuario: UUID
  - nombre: String
  - email: String
  - telefono: String
  - fechaNacimiento: Date
  - rol: RolUsuario
  - activo: Boolean
  + autenticar2FA(codigo: String): Boolean
  + activarSOS(): Void
  + actualizarUbicacion(lat: Double, lng: Double): Void
}

class PerfilSalud {
  - idPerfil: UUID
  - tieneHipertension: Boolean
  - tieneDiabetes: Boolean
  - discapacidadMotora: Boolean
  - tipoSangre: String
  - notasMedicas: String
  + calcularPuntajeSalud(): Int
}

class PlanSuscripcion {
  - idPlan: UUID
  - stripeCustomerId: String
  - stripeSubscriptionId: String
  - tipoPlan: String
  - fechaVencimiento: DateTime
  - estadoActivo: Boolean
  + procesarPago(): Boolean
  + cancelarSuscripcion(): Boolean
}

class Comunidad {
  - idComunidad: UUID
  - nombreGrupo: String
  - codigoInvitacion: String
  - maxIntegrantes: Int = 10
  - fechaCreacion: DateTime
  + agregarIntegrante(idUsuario: UUID): Void
  + removerIntegrante(idUsuario: UUID): Void
  + generarCodigoInvitacion(): String
}

class IntegranteComunidad {
  - esAdminGrupo: Boolean
  - visibleParaGrupo: Boolean
  - fechaUnion: DateTime
  + alternarVisibilidadGPS(): Void
}

' --- RELACIONES ---
Usuario "1" -- "0..1" PerfilSalud : posee >
Usuario "1" -- "0..1" PlanSuscripcion : contrata >
Usuario "1" -- "0..*" IntegranteComunidad : participa en >
Comunidad "1" -- "1..*" IntegranteComunidad : contiene >

' --- NOTA DE CONEXIÓN CON LA PARTE 2 ---
note right of Usuario
  <b>CONTINÚA EN PARTE 2:</b>
  La entidad 'Usuario' se conecta en la Parte 2 con:
  • AlertaSOS (emite / atiende)
  • ExpedienteIncidente (redacta)
  • ZonaSegura (configura)
  • SalidaPlaneada (programa)
  • ZonaRoja (autoriza)
end note

@enduml