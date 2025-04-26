# Challenge 3

## Integrantes
Samuel Espitia  
Miguel Ángel Feo  
Carlos Zuluaga

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
![](https://github.com/carlosazmora/Challenge3_IoT/blob/284b62822c99c2196eaace46ccfc98a98b517f48/SolucionPropuesta.jpg)
**Figura 1. Diagrama de bloques sobre la arquitectura de la solución IoT**

El presente diagrama permite visualizar la división de módulos en que se compone la solución IoT propuesta. Consiste, principalmente, en 3 capas dedicadas a la recolección de datos, procesamiento y actuación a partir de dicha información. La primera de ellas, la capa de recolección de información a partir del entorno se compone de un sensor **HC-SR04**, un ultrasónico que permite cuantificar distancias [1], empleado para medir la lejanía entre el nivel del agua y los límites definidos como permitidos, junto con un ‘raindrop sensor module’ [2], los cuales en este caso permiten determinar si la fuerza de la lluvia es algún signo de peligro de crecimiento en los niveles del agua o si la lluvia está siendo moderada. Los datos son recibidos y procesados mediante una **ESP32**, la cual envía los datos a la una Raspberry Pi que actúa como **MQTT Gateway** [3], y estos datos son enviados a **Ubidots**, la plataforma donde el dashboard que contiene la información está alojado. Por último, se tiene el **Buzzer**, el cual es la alarma que suena una vez el agua alcanza o sobrepasa los limites dispuestos con la medición. Este buzzer puede ser activado y desactivado desde el dashboard. 


![](https://github.com/carlosazmora/Challenge3_IoT/blob/addb06195c2efed232ba75f139e191e37b4b4685/UMLSolucion.jpg)
**Figura 2. Diagrama UML sobre la arquitectura de la solución IoT**

**Escala**
Para la demostración de la solución, se tomó como referencia el estrecho del río Magdalena, ubicado en el municipio de San Agustín, Huila, cuya profundidad aproximada es de 15 metros. En la maqueta, se planteó una escala de 1 cm por cada 2 metros reales. Bajo esta escala, se definió como nivel crítico aquel en el que el agua se encuentra a 3 cm de distancia del sensor.

## Restricciones de diseño identificadas

Una restricción técnica identificada en la fase de diseño consiste en la sensibilidad del cableado ante el flujo de líquidos en las condiciones actuales. Durante las pruebas físicas del montaje, hubo que manejar con especial cuidado el sensor de lluvia por su distintivo contacto con el agua, elemento eléctricamente riesgoso. Esto no sólo se limita a las pruebas de funcionamiento, sino que constituye toda una restricción de espacio si se considera la implementación a nivel masivo, teniendo en cuenta que proteger dispositivos eléctricos de condiciones climáticas extremas podría resultar costoso a nivel monetario y logístico. Otra restricción encontrada es la conectividad con el servidor web, ya que las redes universitarias, o las redes con algún espacio en el nombre presentaban problemas a la hora de intentar realizar la conexión. Sin la conexión no se pueden ver comunicar los datos y no se podrían mandar a Ubidots, haciendo que la solución no cumpla con su objetivo.

---

## Criterios de diseño

### Sensores

- **HC-SR04**: Este sensor fue seleccionado debido a la facilidad de uso y la forma en la que cumplía con lo planeado a la hora de buscar solución ante el problema, es decir permitía de manera sencilla medir la distancia entre el límite y el nivel del agua.
- **Módulo de sensor de lluvia**: Este sensor era el más adecuado a la hora de implementar algún modulo que permitiera saber si estaba lloviendo, ayudando a la resolución del problema y a la recepción de más datos.

### Microcontrolador

- **ESP32**: Se utilizo un ESP32 debido a su capacidad de conectarse con una red WiFi y de permitir conectar los sensores necesarios para la práctica, además de permitir alojar un servidor web que muestre los datos recolectados de los sensores.

### Gateway

- **Raspberry Pi**: Se utilizo una Raspberry Pi 5 como Gateway ya que permitía recibir los datos recolectados por la ESP32 y los mandaba a Ubidots, esto utilizando MQTT.

### Actuadores

- **LED de alerta**: Este LED fue utilizado con el propósito de mostrar una advertencia cuando se sienta que está lloviendo en cantidades medianas/fuertes. Permite ayudar al cliente a estar alerta una vez se encienda el LED.
- **Buzzer**: El buzzer es la alarma que suena una vez el nivel del agua llega al límite, lo que permite alertar a las personas del peligro que esto puede representar. El buzzer se seleccionó debido a esta función, siendo uno de los principales componentes de la solución.

### Dashboard

- **Ubidots**: Se utilizó la plataforma Ubidots, siguiendo las recomendaciones del docente y aprocevhando que permite su uso de manera gratuita usando las credenciales institucionales. Dicha plataforma permite alojar el dashboard en la nube, y su facilidad de uso ayuda a la hora de comunicar los datos y mostrarlos en pantalla. 

---

## Estándares de ingeniería aplicados

- **ISO 9001**: Asegura que el diseño del hardware cumple con procesos estructurados de calidad. [3].
- **ISO 4373**: Asegura la correcta medición del nivel del agua. [4].

---

## Desarrollo y evaluación del prototipo

### Acta de trabajo

Se definieron unos roles a la hora de realizar el trabajo, dependiendo las tareas asignadas, las cuales se pueden ver en la imagen  
![](https://github.com/carlosazmora/Challenge3_IoT/blob/5a11d4342af9decd64cc685f696b9dea135ef5a1/ListaDeTareas.jpg)

Para el desarrollo del proyecto, inicialmente se investigó cómo utilizar el protocolo MQTT, comprendiendo su funcionamiento y su importancia en la comunicación IoT. Posteriormente, se procedió a instalar y configurar la Raspberry Pi, preparando el entorno necesario para su operación como gateway. Con la infraestructura básica lista, se modificó el código de la ESP32 para permitir su conexión adecuada con la Raspberry Pi. Paralelamente, se codificó la Raspberry Pi para que pudiera actuar como intermediario entre la ESP32 y la nube, gestionando el tráfico de datos. Una vez establecida esta funcionalidad, se realizó la conexión entre la Raspberry Pi y la plataforma Ubidots, asegurando que los datos transmitidos fueran correctamente recibidos y almacenados. Para visualizar los datos de manera clara y en tiempo real, se creó un dashboard en Ubidots adaptado a las necesidades del proyecto. Finalmente, se llevaron a cabo las conexiones finales y las pruebas de integración, verificando el correcto funcionamiento de todo el sistema de comunicación de extremo a extremo.

### Configuración experimental

Basándose en el reto anterior, ya se tenían seleccionados los materiales necesarios. Luego era necesario realizar la fase de ideación donde se definió la mejor manera en la que se podrían enviar los datos recolectados con la ESP32, y se tuvo que buscar la forma para enviarlos a alguna plataforma en la nube que en este caso fue Ubidots. Principalmente se siguió la simulación del último desafío ya que tenía la misma funcionalidad, y en este caso se tuvo que realizar la configuración de la Raspberry Pi y la codificación de su funcionamiento para que esta actuara como Gateway de MQTT. 

Una vez se podían comunicar la ESP32 con la Raspberry Pi, se tuvo que conectar la Raspberry con Ubidots para que se mostraran los datos recolectados y los mismos pudiesen ser monitoreados. En Ubidots se mostraba la distancia entre el agua y el límite establecido, junto con el estado de la lluvia, además de permitir activar y desactivar la alarma de alerta. 

Para mostrar estos datos fue necesario crear una base de datos que almacenara esta información, permitiendo de esta manera guardarlos en caso de que se requiera un histórico de datos, además de permitir la visualización por medio de un gráfico las ultimas distancias de agua, ayudando a saber en qué estado esta y ha estado el nivel del agua. 

![](https://github.com/Espiti88/Challenge-2-IoT/blob/main/imagen_2025-03-25_190221235.png)

---

## Resultados

hola
---

## Autoevaluación del protocolo de pruebas

El desarrollo del prototipo IoT para la detección temprana de crecidas en ríos tuvo como base el cumplimiento de los requisitos mínimos establecidos en el reto. Estos consistían en la creación de un sistema funcional capaz de monitorear en tiempo real la presencia de crecidas debido a lluvias intensas y generar una notificación local cuando se detectara un evento de este tipo.

Para cumplir con estos requerimientos, se implementó un sensor ultrasónico **HC-SR04**, el cual permitió medir la distancia entre el nivel del agua y un límite crítico definido. Junto a este, se utilizó un sensor de lluvia que detectó la presencia e intensidad de precipitaciones, activando un LED de alerta cuando estas alcanzaban niveles moderados o fuertes. La información de ambos sensores fue procesada mediante una **ESP32**, que activaba un buzzer para emitir una alarma sonora en caso de niveles críticos de agua. Estos elementos permitieron la notificación in situ requerida en el planteamiento del problema.

Más allá de cumplir con los requisitos mínimos, se incorporaron mejoras adicionales que aumentaron la funcionalidad del sistema. La solución no solo detecta cuando el agua alcanza un nivel crítico, sino que también permite monitorear cómo evoluciona la crecida, proporcionando datos sobre cuánto ha aumentado el nivel del río en un período de tiempo. Esta capacidad amplía el alcance del sistema, permitiendo un análisis del comportamiento del río que puede ser utilizada para detectar posibles desbordamientos con una antelación aún mayor. Además, se implementó un dashboard alojado en nube que permite ver los valores registrados por los sensores, proporcionando información diferenciada sobre la situación del río y factores que pueden afectarlo, como lo son las precipitaciones.

Entre las oportunidades de mejora identificadas, se encuentra la necesidad de desarrollar una protección adecuada para los componentes electrónicos expuestos a condiciones climáticas adversas, garantizando su durabilidad en aplicaciones reales. También se plantea un monitoreo que permita mostrar los datos de manera más rápida y eficiente, que permita asimilarse a un monitoreo en tiempo real para el usuario, además de la implementación de algún dispositivo que permita dar la misma conexión a la raspberry y al circuito con el fin de que pueda funcionar en cualquier momento. 

En conclusión, el sistema desarrollado cumplió con los requisitos mínimos del reto al ofrecer un monitoreo en tiempo real de las crecidas y una notificación local efectiva en caso de riesgo, junto con un tablero que permite ver los datos recolectados por los sensores. Además, las mejoras implementadas y las oportunidades de optimización identificadas posicionan esta solución como un sistema prometedor para minimizar los daños causados por inundaciones en Colombia.

---

## Conclusiones

El proceso de prototipado de la presente solución presento varias dificultades al no tener conocimientos sobre como enviar datos de una ESP32 a una Raspberry Pi por medio de MQTT. Sin embargo, se logró realizar esta comunicación de datos de la manera correcta 

La incorporación de la **ESP32** como microcontrolador central permitió optimizar el diseño y ampliar la funcionalidad del sistema permitiendo su conexión a la red. De esta manera se transmitían los datos a l raspberry y posteriormente se permitía mostrar la información en el dashboard. Además, se permitió guardar los datos en una base de datos. 

La solución propuesta cumple con los requerimientos técnicos planteados por el docente y constituye una sólida propuesta de solución para la problemática del reto, permitiendo la medición del nivel de altura del agua, así como la detección de lluvias, con el fin de fortalecer los protocolos de prevención de desastres, además de permitir la visualización de los datos mediante un dashboard alojado en la nube lo que facilitara a las autoridades a realizar un monitoreo más eficiente. Lo que va a generar mejoras a la hora de tomar precauciones ante los desastres de la comunidad   

---

## Referencias
[1] Androbot TECH. Utiliza este Sensor para medir el nivel de agua! (28 de junio de 2022). Accedido el 17 de febrero de 2025. [Video en línea]. Disponible: https://www.youtube.com/watch?v=VobYZbzTXzc 

[2] “RAIN SENSOR Tutorial | RAIN SENSOR with Arduino Uno - SriTu Hobby”. SriTu Hobby. Accedido el 17 de febrero de 2025. [En línea]. Disponible: https://srituhobby.com/rain-sensor-tutorial-rain-sensor-with-arduino-uno/ 

[3[“Servidor web con ESP32”. Programarfacil Arduino y Home Assistant. Accedido el 26 de marzo de 2025. [En línea]. Disponible: https://programarfacil.com/esp32/servidor-web-con-esp32/ 

[4] “ISO 9001 Sistemas de Gestión de la Calidad | Normas ISO”. Normas ISO | Normas ISO Asesoría y Formación en Sistemas de Gestión. Accedido el 17 de febrero de 2025. [En línea]. Disponible: https://www.normas-iso.com/iso-9001/ 

[5] “UNE-EN ISO 4373:2022 Hidrometría. Dispositivos de medida del n...” UNE - Asociación Española de Normalización. Accedido el 17 de febrero de 2025. [En línea]. Disponible: https://www.une.org/encuentra-tu-norma/busca-tu-norma/norma/?c=N0070707 

---

## Anexos
### Código fuente - Raspberry

```cpp
import sqlite3
import json
import paho.mqtt.client as mqtt
import paho.mqtt.publish as publish
from datetime import datetime

# SQLite
DB_NAME = "sensores.db"

# MQTT Local
LOCAL_MQTT_BROKER = "localhost"
LOCAL_MQTT_TOPIC_NIVEL = "rio/nivel"
LOCAL_MQTT_TOPIC_LLUVIA = "rio/lluvia"
LOCAL_MQTT_TOPIC_LED = "esp32/control_alarma"  # Topic para controlar el LED en la ESP32

# MQTT Ubidots
UBIDOTS_BROKER = "industrial.api.ubidots.com"
UBIDOTS_PORT = 1883
UBIDOTS_TOKEN = "BBUS-gjDIrL1Jl5EoKpzlxloRt0qcgRaJTJ"  # <- ¡Reemplaza esto!
UBIDOTS_DEVICE_LABEL = "pepelepew"
UBIDOTS_TOPIC_LED_CONTROL = "/v1.6/devices/pepelepew/control_alarma"  # Topic desde Ubidots

# Función para insertar datos en SQLite
def insertar_datos(distancia=None, lluvia=None):
    conn = sqlite3.connect(DB_NAME)
    cursor = conn.cursor()
    if distancia is not None and lluvia is None:
        cursor.execute("INSERT INTO lecturas (distancia) VALUES (?)", (distancia,))
        print(f"[{datetime.now()}] Guardado: {distancia} cm")
    elif lluvia is not None and distancia is None:
        cursor.execute("INSERT INTO lecturas (lluvia) VALUES (?)", (lluvia,))
        print(f"[{datetime.now()}] Guardado: {lluvia}")
    conn.commit()
    conn.close()

# Función para enviar datos a Ubidots
def enviar_a_ubidots(distancia=None, lluvia=None):
    topic = f"/v1.6/devices/{UBIDOTS_DEVICE_LABEL}"
    payload = {}

    if distancia is not None:
        payload["distancia"] = {"value": distancia}

    if lluvia is not None:
        # Transformar la lluvia en un valor numérico basado en el texto
        lluvia_value = procesar_lluvia(lluvia)  # Obtiene el valor numérico
        if lluvia_value is not None:  # Solo si el valor numérico es válido
            payload["lluvia"] = {
                "value": lluvia_value,  # Valor numérico obligatorio
                "context": {"descripcion": lluvia}  # Texto adicional
            }

    try:
        publish.single(
            topic,
            json.dumps(payload),
            hostname=UBIDOTS_BROKER,
            port=UBIDOTS_PORT,
            auth={"username": UBIDOTS_TOKEN, "password": ""},
            retain=False
        )
        print(f"[{datetime.now()}] Enviado a Ubidots: {json.dumps(payload)}")
    except Exception as e:
        print("❌ Error al enviar a Ubidots:", e)

# Función para procesar valores de lluvia
def procesar_lluvia(lluvia):
    # Asigna valores numéricos según el texto recibido
    if lluvia == "No hay lluvia":
        return 0
    elif lluvia == "Lluvia mediana :|":
        return 1
    elif lluvia == "Lluvia re fuerte :(":
        return 2
    else:
        print(f"❌ Advertencia: Descripción de lluvia desconocida -> {lluvia}")
        return None  # Retorna None si el texto no coincide con los casos definidos

# Función para manejar el control del LED
def reenviar_comando_led(comando):
    try:
        publish.single(
            LOCAL_MQTT_TOPIC_LED,
            comando,
            hostname=LOCAL_MQTT_BROKER,
            retain=False
        )
        print(f"[{datetime.now()}] Comando LED reenviado a ESP32: {comando}")
    except Exception as e:
        print("❌ Error al reenviar el comando LED:", e)

# Función de conexión MQTT
def on_connect(client, userdata, flags, rc):
    print("Conectado al broker MQTT local con código", rc)
    client.subscribe([
        (LOCAL_MQTT_TOPIC_NIVEL, 0),
        (LOCAL_MQTT_TOPIC_LLUVIA, 0),
        (UBIDOTS_TOPIC_LED_CONTROL, 0)  # Suscribirse al control del LED desde Ubidots
    ])

# Función para manejar mensajes MQTT
def on_message(client, userdata, msg):
    try:
        payload = msg.payload.decode()

        if msg.topic == LOCAL_MQTT_TOPIC_NIVEL:
            # Procesar nivel (numérico)
            try:
                distancia = float(payload)  # Convertir a float
                insertar_datos(distancia=distancia)
                enviar_a_ubidots(distancia=distancia)
            except ValueError:
                print(f"❌ Error: Valor de nivel no numérico -> {payload}")

        elif msg.topic == LOCAL_MQTT_TOPIC_LLUVIA:
            # Procesar lluvia (texto)
            lluvia = payload.strip()  # Eliminar espacios
            insertar_datos(lluvia=lluvia)
            enviar_a_ubidots(lluvia=lluvia)

        elif msg.topic == UBIDOTS_TOPIC_LED_CONTROL:
            # Controlar LED (comando desde Ubidots)
            print(f"Comando de LED recibido desde Ubidots: {payload}")
            reenviar_comando_led(payload)  # Reenviar a la ESP32

    except json.JSONDecodeError:
        print(f"❌ Error: Payload no es JSON válido -> {msg.payload}")
    except Exception as e:
        print("❌ Error al procesar el mensaje:", e)

# Configuración y bucle MQTT
client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message

client.connect(LOCAL_MQTT_BROKER, 1883, 60)
client.loop_forever()
```

### Código fuente - ESP32

```cpp
#include <WiFi.h>
#include <WebServer.h>
#include <PubSubClient.h>

const char* ssid = "REALME";
const char* password = "IoTplsdeja";

// MQTT Broker
const char* mqtt_server = "192.168.38.84";

WiFiClient espClient;
PubSubClient client(espClient);

#define RAIN_SENSOR_PIN 34
#define LED_PIN 2
#define TRIG_PIN 5
#define ECHO_PIN 18
#define BUZZER_PIN 19

const float water_level = 10.0;
int isRaining = 100;
int value = 1000;
bool alarmaActiva = true;

WebServer server(80);

#define HISTORICO_SIZE 10
float historicoDistancias[HISTORICO_SIZE] = {0.0};
String historicoLluvia[HISTORICO_SIZE] = {"N/A"};
int historicoIndex = 0;

float distanciaActual = 0.0;
String lluviaActual = "No hay lluvia :-)";

float medirDistancia() {
    digitalWrite(TRIG_PIN, LOW);
    delayMicroseconds(2);
    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN, LOW);
    long duration = pulseIn(ECHO_PIN, HIGH);
    return (duration * 0.0343) / 2;
}

void agregarAlHistorico(float distancia, String lluvia) {
    historicoDistancias[historicoIndex] = distancia;
    historicoLluvia[historicoIndex] = lluvia;
    historicoIndex = (historicoIndex + 1) % HISTORICO_SIZE;
}

void verificarBuzzer(float distancia) {
    if (distancia <= water_level && alarmaActiva) {
        tone(BUZZER_PIN, 523, 500);
        delay(500);
        Serial.println("ALERTA: Nivel de agua bajo!");
    } else {
        noTone(BUZZER_PIN);
    }
}

void handleRoot() {
    // Página web (no modificado)
}

void handleApagar() {
    alarmaActiva = false;
    Serial.println("Alarma apagada desde la página web.");
    server.send(200, "text/plain", "Alarma desactivada.");
}

void handleEncender() {
    alarmaActiva = true;
    Serial.println("Alarma encendida desde la página web.");
    server.send(200, "text/plain", "Alarma activada.");
}

void reconnect() {
    while (!client.connected()) {
        Serial.print("Conectando a MQTT...");
        if (client.connect("ESP32Client")) {
            Serial.println("conectado!");
            client.subscribe("esp32/alarma_control"); // Suscribirse al topic de control de alarma
        } else {
            Serial.print("falló, rc=");
            Serial.print(client.state());
            Serial.println(" intentando de nuevo en 5 segundos");
            delay(5000);
        }
    }
}

void publicarDatos() {
    String distString = String(distanciaActual, 2);
    String lluviaString = lluviaActual;

    client.publish("fraterniu/sensor/distancia", distString.c_str());
    client.publish("rio/lluvia", lluviaString.c_str());
    client.publish("rio/nivel", distString.c_str()); // NUEVO TOPIC
}

void tareaMedirDistancia(void* parameter) {
    for (;;) {
        distanciaActual = medirDistancia();
        verificarBuzzer(distanciaActual);
        agregarAlHistorico(distanciaActual, lluviaActual);
        publicarDatos();
        delay(1000);
    }
}

void tareaCheckRain(void* parameter) {
    for (;;) {
        value = analogRead(RAIN_SENSOR_PIN);
        if (value < 300) isRaining = true;
        else isRaining = false;

        lluviaActual = (value < 1600) ? "Lluvia re fuerte :(" : (value < 2800) ? "Lluvia mediana :|" : "No hay lluvia";
        digitalWrite(LED_PIN, isRaining);
        delay(100);
    }
}

void callback(char* topic, byte* payload, unsigned int length) {
    String message;
    for (int i = 0; i < length; i++) {
        message += (char)payload[i];
    }
    Serial.println("Mensaje recibido en el topic: " + String(topic));
    Serial.println("Payload: " + message);

    if (String(topic) == "esp32/control_alarma") {
        if (message == "0") {
            alarmaActiva = false; // Apaga la alarma
            Serial.println("Alarma desactivada por MQTT.");
        } else if (message == "1") {
            alarmaActiva = true; // Enciende la alarma
            Serial.println("Alarma activada por MQTT.");
        }
    }
}

void loop() {
    if (!client.connected()) {
        reconnect();
    }
    client.loop();
    server.handleClient();
}

void setup() {
    Serial.begin(9600);
    pinMode(RAIN_SENSOR_PIN, INPUT);
    pinMode(TRIG_PIN, OUTPUT);
    pinMode(ECHO_PIN, INPUT);
    pinMode(BUZZER_PIN, OUTPUT);
    pinMode(LED_PIN, OUTPUT);

    WiFi.begin(ssid, password);
    Serial.print("Conectando a WiFi...");
    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }
    Serial.println("\nConectado a WiFi!");
    Serial.println(WiFi.localIP());

    client.setServer(mqtt_server, 1883);
    client.setCallback(callback); // Configuración del callback

    server.on("/", handleRoot);
    server.on("/apagar", handleApagar);
    server.on("/encender", handleEncender);
    server.begin();

    xTaskCreate(tareaMedirDistancia, "MedirDistancia", 4096, NULL, 1, NULL);
    xTaskCreate(tareaCheckRain, "CheckRain", 4096, NULL, 1, NULL);
}
```
