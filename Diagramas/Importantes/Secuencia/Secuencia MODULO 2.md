[[Secuencia MODULO 1]]
@startuml
autonumber
skinparam responseMessageBelowArrow true
skinparam maxMessageSize 150

actor "Usuario / Operador" as User
participant "App Movil / Web" as Client
participant "API / Node.js" as Backend
database "PostgreSQL / Supabase" as DB
participant "Servicio Email / SMS (SES)" as EmailSms

== 1. Autenticación de Primer Factor (Credenciales) ==
User -> Client: Ingresa email y contrasena
Client -> Backend: POST /api/auth/login (email, password)
activate Backend
Backend -> DB: Consultar usuario por email
DB --> Backend: Retorna datos de usuario y hash de contrasena
Backend -> Backend: Validar hash de contrasena (Bcrypt)

alt Credenciales Invalidas
  Backend --> Client: 401 Unauthorized (Credenciales incorrectas)
  Client --> User: Mostrar mensaje de error
else Credenciales Validas
  Backend -> Backend: Generar codigo OTP de 6 digitos (Expiracion 5 min)
  Backend -> DB: Guardar OTP temporal y timestamp
  Backend -> EmailSms: Enviar codigo OTP al correo o telefono
  EmailSms --> User: Recibe mensaje con codigo 2FA
  Backend --> Client: 200 OK (Requiere 2FA, token temporal)
end
deactivate Backend

== 2. Verificación de Segundo Factor (2FA) ==
User -> Client: Ingresa codigo OTP de 6 digitos
Client -> Backend: POST /api/auth/verify-2fa (idUsuario, codigoOTP)
activate Backend
Backend -> DB: Consultar OTP activo del usuario
DB --> Backend: Retorna OTP y fecha de expiracion

alt Codigo Incorrecto o Expirado
  Backend --> Client: 400 Bad Request (Codigo invalido o expirado)
  Client --> User: Mostrar error y opcion de reintentar
else Codigo Valido
  Backend -> Backend: Generar JWT (Access Token y Refresh Token)
  Backend -> DB: Registrar inicio de sesion exitoso
  Backend --> Client: 200 OK (Tokens JWT y Datos del Rol)
  deactivate Backend
  Client -> Client: Almacenar Token de forma segura
  Client --> User: Redirigir a Pantalla Principal / Dashboard
end

@enduml