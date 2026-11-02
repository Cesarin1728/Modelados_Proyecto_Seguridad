[[Actividades MODULO 1]]
@startuml
skinparam shadowing false

start
:Usuario programa Salida Planeada;
:Sistema guarda la programacion;

repeat
  :Cron Job evalua tiempo restante de la salida;
backward:Esperar ciclo de verificacion;
repeat while (¿Faltan 10 minutos para la hora estimada?) is (no)

:Enviar notificacion push de recordatorio preventivo;

while (¿Llegada confirmada o tolerancia vencida?) is (no)
  :Esperar confirmacion del usuario o vencimiento de tiempo;
endwhile

if (¿Usuario confirmo llegada segura con PIN?) then (si)
  :Marcar Salida Planeada como COMPLETADA;
  stop
else (no / Tolerancia vencida)
  :Generar Alerta Automatica por Inactividad;
  :Notificar a Grupo Familiar y Panel de Operadores;
  :Iniciar rastreo GPS de emergencia;
  stop
endif

@enduml