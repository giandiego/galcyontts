# Galcyon TTS script Asterisk - Loquendo/Edge

Este script permite convertir texto a voz y reproducirlo para el usuario desde asterisk.  
Este servicio TTS se conecta al API de Galcyon-TTS que es un proyecto independiente y privado.  

## Dependencias

Desarrollado utilizando:

* PHP > 7.4  
* phpagi 
* Soporte Json para PHP


## Instalación
Primero clonar el repositorio en su PBX y moverlo al directorio agi-bin.
Por lo general, esto es ```/var/lib/asterisk/agi-bin/``` Para asegurarse que es la ruta correcta, verificar su archivo ```/etc/asterisk/asterisk.conf```.

```bash
  git clone https://github.com/giandiego/galcyontts.git
  cd galcyontts
  cp -R agi_asterisk/* /var/lib/asterisk/agi-bin/
```

Dar permisos de ejecución:
```bash
  chmod -R +x /var/lib/asterisk/agi-bin/phpagi/
  chmod -R +x /var/lib/asterisk/agi-bin/galcyon-tts.agi
```

## USO
Puedes copiar este plan de marcado a tu extensions.conf y marcar la extensión 1234 y realizar la prueba.


```bash
exten => 1234,1,Answer()
same=>n,agi(galcyon-tts.agi,"¡Bienvenido! mi nombre es Carlos, y soy la voz sintetizada de Loquendo funcionando desde Asterisk. Con este sistema de T T S puedes usar no solo mi voz.",Carlos)
same=>n,agi(galcyon-tts.agi,"Por ejemplo, mi nombre es Ximena, una voz de loquendo. Podrás usar este AGI para reproducir diferentes locuciones o crear cualquier mensaje de bienvenida..",Ximena)
same=>n,agi(galcyon-tts.agi,"Adicionalmente, puedes usar la voz del navegador Microsoft Edge, el cual a diferencia del Loquendo usa internet para funcionar, por lo que podría ser un poco más lento.",es-PE-Camila)
same=>n,agi(galcyon-tts.agi,"También tienes disponible un API que te ayudará a ver todas las voces soportadas, así como algunas funciones extras.",Carlos)
same=>n,agi(galcyon-tts.agi,"Este AGI es solo experimental y es un ejemplo de lo que se puede hacer con Asterisk y un poco de paciencia. Puedes descargar este ejemplo y probarlo en tu plataforma desde hoy!.",es-PE-Camila)
same=>n,hangup()
```

También puedes realizar un IVR simple como el siguiente ejemplo.


```bash
[ivr-tts]
exten => s,1,Answer()
same=>n,Set(TIMEOUT(digit)=5)
same=>n,agi(galcyon-tts.agi,"Bienvenido a mi pequeño menú interactivo de respuesta de voz.",Carlos)

;Esperar dígito:
same=>n(start),agi(galcyon-tts.agi,"Por favor marque un dígito.",Ximena,any)
same=>n,WaitExten()

exten=>_X,1,agi(galcyon-tts.agi,"Acabas de presionar ${EXTEN}. Prueba con otro por favor.",es-PE-Camila,any)
exten=>_X,n,WaitExten()

exten=>i,1,agi(galcyon-tts.agi,"Extensión inválida.",Carlos)
exten=>i,n,goto(s,start)

exten=>t,1,agi(galcyon-tts.agi,"Tiempo de espera agotado.",Carlos)
exten=>t,n,goto(s,start)
```

## Endpoints

#### TTS Status

```http
  GET https://tts.galcyon.com:8001/api/v1/tts/ttsStatus
```

#### Voice List

```http
  GET https://tts.galcyon.com:8001/api/v1/tts/voiceList
```

## Documentación de API en POSTMAN

[API TTS GALCYON - POSTMAN](https://github.com/giandiego/galcyontts/blob/main/API%20TTS%20GALCYON.json)

## Info

> Gian Diego Javes (mailto:gjaves.tnegocios@gmail.com)