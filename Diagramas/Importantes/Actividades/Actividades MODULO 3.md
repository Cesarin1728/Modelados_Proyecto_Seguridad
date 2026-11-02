[[Actividades MODULO 2]]
@startuml
skinparam shadowing false

start
:Usuario o Admin define Zona Segura (Radio 50m a 3km);
:Guardar centro y radio en base de datos;

while (App activa y monitoreo GPS en segundo plano)
  :Recibir coordenadas GPS del dispositivo;
  :Calcular distancia respecto al centro de la Zona Segura;
  
  if (¿Dispositivo esta fuera del radio configurado?) then (si)
    if (¿Salida estaba previamente programada?) then (si)
      :Registrar salida autorizada sin emitir alerta;
    else (no)
      :Disparar notificacion de Salida No Autorizada;
      :Notificar a Admin de Comunidad / Grupo Familiar;
    endif
  else (no)
    :Mantener estado Dentro de Zona Segura;
  endif
endwhile
stop

@enduml