# Challenge 2

## Resumen general

Colombia es un país caracterizado por su variada topografía y patrones climáticos variados. Esta variedad combinada con fenómenos climáticos como La Niña, incrementa significativamente la vulnerabilidad del país a crecidas de ríos e inundaciones, afectando gravemente a comunidades y generando daños a la infraestructura. Frente a este panorama, la detección temprana de crecidas se convierte en un pilar fundamental para la gestión del riesgo de desastres, enfoque que se decidió adoptar en el desarrollo de un proyecto para la clase de Internet de las Cosas.

Se considera favorable una solución tecnológica basada en Internet de las Cosas (IOT), debido a la capacidad que tiene de monitorear variables ambientales en tiempo real, permitiendo una detección oportuna de puedan desencadenar inundaciones. Además, el traspaso de información de sensores conectados y sistemas de alerta automáticos puede marcar la diferencia entre una evacuación exitosa y una tragedia.

## Estructura del documento

1. **Solución Propuesta**
   - Arquitectura propuesta
   - Restricciones de diseño identificadas
   - Criterios de diseño
   - Estándares de ingeniería aplicados
     
2. **Desarrollo y evaluación del prototipo**
   - Acta de trabajo
   - Configuración experimental 
   - Resultados y Análisis
   - Autoevaluación del protocolo
       
4. **Conclusiones**
   
5. **Referencias**
   
6. **Anexos**

---

## Solucion propuesta
![](https://github.com/Espiti88/Challenge-2-IoT/blob/main/SolucionPropuesta.png)
**Figura 1. Diagrama de bloques sobre la arquitectura de la solución IoT**

El presente diagrama permite visualizar la división de módulos en que se compone la solución IoT propuesta. Consiste, principalmente, en 3 capas dedicadas a la recolección de datos, procesamiento y actuación a partir de dicha información. La primera de ellas, la capa de recolección de información a partir del entorno se compone de un sensor **HC-SR04**, un ultrasónico que permite cuantificar distancias [1], empleado para medir la lejanía entre el nivel del agua y los límites definidos como permitidos, junto con un ‘raindrop sensor module’ [2], los cuales en este caso permiten determinar si la fuerza de la lluvia es algún signo de peligro de crecimiento en los niveles del agua o si la lluvia está siendo moderada. Los datos son recibidos y procesados mediante una **ESP32**, la cual tiene un servidor web embebido, el cual permite mostrar en tiempo real la distancia que tiene el nivel del agua con los limites definidos, y permite conocer la fuerza de la lluvia. Por último, se tiene el **Buzzer**, el cual es la alarma que suena una vez el agua alcanza o sobrepasa los limites dispuestos con la medición. Este buzzer puede ser activado y desactivado con el servidor web.

## Restricciones de diseño identificadas

Una restricción técnica identificada en la fase de diseño consiste en la sensibilidad del cableado ante el flujo de líquidos en las condiciones actuales. Durante las pruebas físicas del montaje, hubo que manejar con especial cuidado el sensor de lluvia por su distintivo contacto con el agua, elemento eléctricamente riesgoso. Esto no sólo se limita a las pruebas de funcionamiento, sino que constituye toda una restricción de espacio si se considera la implementación a nivel masivo, teniendo en cuenta que proteger dispositivos eléctricos de condiciones climáticas extremas podría resultar costoso a nivel monetario y logístico. Otra restricción encontrada es la conectividad con el servidor web, ya que las redes universitarias, o las redes con algún espacio en el nombre presentaban problemas a la hora de intentar realizar la conexión. Sin la conexión no se pueden ver los datos en el servidor web, haciendo que el circuito no cumpla con su objetivo.

---

## Criterios de diseño

### Sensores

- **HC-SR04**: Este sensor fue seleccionado debido a la facilidad de uso y la forma en la que cumplía con lo planeado a la hora de buscar solución ante el problema, es decir permitía de manera sencilla medir la distancia entre el límite y el nivel del agua.
- **Módulo de sensor de lluvia**: Este sensor era el más adecuado a la hora de implementar algún modulo que permitiera saber si estaba lloviendo, ayudando a la resolución del problema y a la recepción de más datos.

### Microcontrolador

- **ESP32**: Se utilizo un ESP32 debido a su capacidad de conectarse con una red WiFi y de permitir conectar los sensores necesarios para la práctica, además de permitir alojar un servidor web que muestre los datos recolectados de los sensores.

### Actuadores

- **LED de alerta**: Este LED fue utilizado con el propósito de mostrar una advertencia cuando se sienta que está lloviendo en cantidades medianas/fuertes. Permite ayudar al cliente a estar alerta una vez se encienda el LED.
- **Buzzer**: El buzzer es la alarma que suena una vez el nivel del agua llega al límite, lo que permite alertar a las personas del peligro que esto puede representar. El buzzer se seleccionó debido a esta función, siendo uno de los principales componentes de la solución.

### Dashboard

- **Servidor web embebido**: Se utilizo un servidor web embebido que se implementó mediante la ESP32 permitiendo la gestión de una interfaz web que muestra datos recolectados por los sensores en tiempo real.

---

## Estándares de ingeniería aplicados

- **ISO 9001**: Asegura que el diseño del hardware cumple con procesos estructurados de calidad. [3].
- **ISO 4373**: Asegura la correcta medición del nivel del agua. [4].

---

## Desarrollo y evaluación del prototipo

### Acta de trabajo

Se definieron unos roles a la hora de realizar el trabajo, dependiendo las tareas asignadas, las cuales se pueden ver en la imagen  
![](https://github.com/Espiti88/Challenge-2-IoT/blob/main/SolucionPropuesta.png)
Las primeras 3 actividades se realizaron durante la semana del 12 al 19 de marzo, ya que eran las bases para poder realizar correctamente el desafío. Durante los días del 19 al 21 de marzo, se realizaron las ultimas 2 actividades, donde el día 21 de marzo, el desafío presento el correcto funcionamiento, dando por finalizado el proceso.  
### Configuración experimental

En base al desafío anterior, ya se tenían seleccionados los materiales necesarios. Luego era necesario realizar la fase de ideación donde se definió la mejor manera en la que se podrían mostrar los datos obtenidos por los sensores mediante los dashboards, y la mejor manera de realizarlos. Se llego a la conclusión de que la mejor manera de realizar esto era utilizando una ESP32, la cual permite la conexión wifi y la creación de un servidor web embebido. Al estar trabajando anteriormente con un Arduino, era necesario realizar el cambio en la simulación de un Arduino a una ESP32, la cual se realizó en Wowki. Esto permitía saber la manera en que los componentes se podían conectar a la ESP32 y la verificación de su funcionamiento. Una vez se verifico la simulación, se realizaron las conexiones de los componentes de manera física, y se utilizó una maqueta que simula el crecimiento de un rio

Junto con las conexiones, se realizó simultáneamente la programación para el servidor web embebido que permitía mostrar los datos obtenidos del hardware, mediante un dashboard. Este dashboard mostraba la distancia entre el agua y el límite establecido, y también mostraba la fuerza de la lluvia, además de mostrar un histórico de los últimos 10 datos capturados. Además de permitir la activación y desactivación de la alarma.

---

## Resultados

Comprobar el funcionamiento del sistema era de vital importancia. Para lo cual, se posicionó el sensor de lluvia sobre un recipiente, y se le regaba agua para funcionar como una simulación de lluvia, verificando su funcionamiento prendiendo un led de emergencia y mostrando el estado de la lluvia en el dashboard. Utilizando este recipiente también se verifico el funcionamiento del sensor, una vez el nivel del agua estaba muy cerca al límite definido, el sensor detectaba que tan cerca estaba el agua del límite, permitiendo enviar el dato a la ESP32, la cual mandaba los datos también al servidor web. Además, se encendía el buzzer el cual avisaba, y se podía manipular su estado de encendido y apagado mediante el servidor web.

Se obtuvo que todos los componentes funcionaron de manera correcta y conforme a lo esperado. El sensor ultrasónico **HC-SR04** permitió detectar de manera precisa el nivel del agua, cumpliendo con la función principal de la solución. El sensor de lluvia también mostró un rendimiento eficiente, logrando captar de forma efectiva la presencia de lluvia y contribuyendo a la implementación de medidas preventivas en el proyecto. La **ESP32** fue utilizada con éxito como microcontrolador principal, logrando la comunicación adecuada entre todos los componentes y permitiendo el envío de datos en tiempo real al servidor web embebido. A través del dashboard del servidor, se visualizaron correctamente los datos relevantes sobre el nivel del agua y la presencia de lluvia, lo que facilitó la interpretación y monitoreo del sistema. Finalmente, el buzzer y el LED funcionaron de manera óptima, generando alertas sonoras cuando se detectaba un nivel de agua crítico o lluvias moderadas y fuertes.

---

## Autoevaluación del protocolo de pruebas

El desarrollo del prototipo IoT para la detección temprana de crecidas en ríos tuvo como base el cumplimiento de los requisitos mínimos establecidos en el reto. Estos consistían en la creación de un sistema funcional capaz de monitorear en tiempo real la presencia de crecidas debido a lluvias intensas y generar una notificación local cuando se detectara un evento de este tipo.

Para cumplir con estos requerimientos, se implementó un sensor ultrasónico **HC-SR04**, el cual permitió medir la distancia entre el nivel del agua y un límite crítico definido. Junto a este, se utilizó un sensor de lluvia que detectó la presencia e intensidad de precipitaciones, activando un LED de alerta cuando estas alcanzaban niveles moderados o fuertes. La información de ambos sensores fue procesada mediante una **ESP32**, que activaba un buzzer para emitir una alarma sonora en caso de niveles críticos de agua. Estos elementos permitieron la notificación in situ requerida en el planteamiento del problema.

Más allá de cumplir con los requisitos mínimos, se incorporaron mejoras adicionales que aumentaron la funcionalidad del sistema. La solución no solo detecta cuando el agua alcanza un nivel crítico, sino que también permite monitorear cómo evoluciona la crecida, proporcionando datos sobre cuánto ha aumentado el nivel del río en un período de tiempo. Esta capacidad amplía el alcance del sistema, permitiendo un análisis del comportamiento del río que puede ser utilizada para detectar posibles desbordamientos con una antelación aún mayor. Además, se implementó un dashboard el cual permite ver los datos obtenidos por los sensores, proporcionando información diferenciada sobre la situación del río y factores que pueden afectarlo, como lo son las precipitaciones.

Entre las oportunidades de mejora identificadas, se encuentra la necesidad de desarrollar una protección adecuada para los componentes electrónicos expuestos a condiciones climáticas adversas, garantizando su durabilidad en aplicaciones reales. También se plantea un monitoreo que permita mostrar los datos de manera más rápida y eficiente, que permita asimilarse a un monitoreo en tiempo real para el usuario.

En conclusión, el sistema desarrollado cumplió con los requisitos mínimos del reto al ofrecer un monitoreo en tiempo real de las crecidas y una notificación local efectiva en caso de riesgo, junto con un tablero que permitía ver los datos recolectados por los sensores. Además, las mejoras implementadas y las oportunidades de optimización identificadas posicionan esta solución como un sistema prometedor para minimizar los daños causados por inundaciones en Colombia.

---

## Conclusiones

El proceso de prototipado de la presente solución presento varias dificultades al no tener conocimientos sobre cómo se utilizan los hilos en un contexto practico de IoT. Sin embargo, se logró aplicar este término en el proyecto

La incorporación de la **ESP32** como microcontrolador central, junto con un servidor web embebido, permitió optimizar el diseño y ampliar la funcionalidad del sistema mediante un dashboard accesible desde cualquier dispositivo conectado a la red. Este dashboard mejora significativamente la experiencia del usuario al presentar de forma clara y visual los datos obtenidos por los sensores

La solución propuesta cumple con los requerimientos técnicos planteados por el docente y constituye una sólida propuesta de solución para la problemática del reto, permitiendo la medición del nivel de altura del agua, así como la detección de lluvias, con el fin de fortalecer los protocolos de prevención de desastres, además de permitir la visualización de los datos mediante un dashboard alojado en un servidor embebido. Lo que va a generar mejoras a la hora de tomar precauciones ante los desastres de la comunidad
