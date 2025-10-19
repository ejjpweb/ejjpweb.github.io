---
layout: post
title: "La Estrella de la Muerte que se Alimenta del Sol: Fotobaterías en la Noche de los Investigadores 2025"
date: '2025-10-17T10:00:00+02:00'
tags: [fotobaterías, divulgación, Noche de los Investigadores, Star Wars, INMA, energía solar, SOLiBAT, VOLTA]
---

## La Estrella de la Muerte que Necesita el Sol: Enseñando Fotobaterías con Star Wars

El pasado 26-27 de septiembre, los grupos HYMAT y NFP del **Instituto de Nanociencia y Materiales de Aragón (INMA)** llevaron a la Plaza del Pilar de Zaragoza una experiencia única que combinó ciencia con la épica de Star Wars. Nuestra misión: enseñar al público cómo funcionan las fotobaterías usando una réplica en miniatura de la Estrella de la Muerte.

![Equipo INMA en la Noche de los Investigadores](/imgs/collage-staff.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
*El equipo de voluntarios del INMA listo para la batalla (científica)*
{: style="color:gray; font-size: 70%; text-align: center;"}

## El Desafío del Gran Moff Tarkin: Mantener las Defensas Activas

La premisa era simple pero educativa: **¿Puede el Gran Moff Tarkin mantener operativos los sistemas de defensa de la Estrella de la Muerte mientras orbita un planeta?** Para ahorrar cristales kyber (y enseñar conceptos de energía renovable), nuestro sistema de defensas (LEDs) debe mantenerse activo usando una pequeña estrella con ciclos de iluminación de 2 minutos.

### Los 4 Sistemas de Alimentación Propuestos

Planteamos a los visitantes cuatro posibles soluciones energéticas:

1. **Solo Celda Solar** ❌
   - *Problema*: Energía intermitente y sin defensas durante la noche orbital

2. **Solo Batería de Li-Ion** ❌
   - *Problema*: Necesita recarga externa (¿dónde está el puerto USB en el espacio?)

3. **Celda Solar + Batería (conectadas solo en luz)** ✅
   - *¡Solución correcta!* La fotobatería ideal

4. **Celda Solar + Batería (siempre conectadas)** ❌
   - *Problema*: La celda solar actúa como LED infrarrojo en oscuridad, drenando energía




![La fotobatería en tres situaciones diferentes](/imgs/esquema-luz-oscuridad-superdiodo.png){:style="display:block; margin-left:auto; margin-right:auto"}
*Esquema de la Estrella de la Muerte con fotobatería mostrando las tres situaciones posibles en luz/oscuridad con y sin superdiodo*
{: style="color:gray; font-size: 70%; text-align: center;"}



## La Ciencia Detrás de la Fuerza (Fotovoltaica)

### ¿Qué es una Fotobatería?

Una fotobatería es un dispositivo que **combina la captación de energía solar y el almacenamiento en una única unidad integrada**. En los sistemas tradicionales, los paneles solares y las baterías son componentes separados conectados por cables. En una fotobatería verdadera, ambas funciones ocurren en el mismo dispositivo.
Nuestra Estrella de la Muerte es un modelo educativo que demuestra el principio de funcionamiento: aunque la celda solar y la batería todavía están físicamente separadas (como en los sistemas actuales), ilustra perfectamente por qué necesitamos la gestión inteligente de la conexión entre ambos componentes. Es un paso intermedio hacia las fotobaterías completamente integradas que estamos desarrollando en el laboratorio, donde un único apilamiento de capas de material podrá captar luz y almacenar energía simultáneamente.

### El Problema del "LED Vampiro"

Durante la fase oscura de la órbita, descubrimos un fenómeno fascinante: **las celdas solares se comportan como LEDs infrarrojos cuando no reciben luz**, emitiendo radiación y drenando la batería. Por eso necesitamos nuestro "superdiodo" - un componente que desconecta inteligentemente la celda solar durante la oscuridad además de evitar las pérdidas de energía que produce un diodo convencional.



## Tecnología Real para un Imperio Imaginario

### El Simulador Solar Orbital

Utilizamos nuestro [simulador solar programable](/2025/05/04/Programmable-Light-Dimmer/) desarrollado específicamente para replicar los ciclos luz/oscuridad que experimenta un satélite en órbita. Con una lámpara halógena dicroica controlada por Raspberry Pi Pico, podemos simular:

- **Órbitas LEO**: ~90 minutos con 60 minutos de luz y 30 de oscuridad.
- **Órbitas personalizadas**: Ajustables para cualquier escenario. En la noche de los investigadores usamos un ciclo de 2 minutos de luz y oscuridad.
- **Transiciones suaves**: Simulando amaneceres y atardeceres orbitales.

### Monitorización en Tiempo Real

El sistema incorpora nuestro [*tracker MPPT* "Perovskino"](/2024/01/30/Perovskino/)  para monitorizar el estado de carga de la fotobatería, específicamente:
- **Sensor INA219**: Mide voltaje (SOC, estado de carga) en tiempo real.
- **Arduino con conectividad**: Recolecta y transmite datos al chromebook.
- **Interfaz web temática**: Gráficos al estilo Star Wars mostrando la carga/descarga
- **Pantalla en vivo**: Los visitantes podían ver cómo la batería se cargaba con luz y se descargaba en oscuridad llevando la imagen del chromebook a una pantalla más grande via HDMI.


Mira este video con el setup en acción:
<div style="display:block; margin-left:auto; margin-right:auto; max-width:100%; width:560px;">
    <a href="https://www.youtube.com/watch?v=dIh66YtGS60" target="_blank">
        <img src="https://img.youtube.com/vi/dIh66YtGS60/maxresdefault.jpg" 
             alt="Video demostración Estrella de la Muerte" 
             style="width:100%; border: 2px solid #ccc;">
    </a>
</div>
<p style="color:gray; font-size: 70%; text-align: center;">
    <em>⬆️ Click en la imagen para ver el video: La Estrella de la Muerte con fotobatería en acción</em>
</p>


## Making Of: Construyendo la Estrella de la Muerte

La construcción de nuestra Estrella de la Muerte educativa fue todo un proyecto de ingeniería con mucha prueba-error, encontrar el número adecuado de LEDs y unas resistencias adecuadas que no drenaran toda la batería en menos de dos minutos:

![Proceso de construcción](/imgs/collage-mk-of-estrella.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
*Del modelo 3D a la realidad: integrando electrónica en una esfera de 12 cm*
{: style="color:gray; font-size: 70%; text-align: center;"}

![Esquema de la fotobatería](/imgs/esquema-estrella-muerte.png){:style="display:block; margin-left:auto; margin-right:auto; max-width:30%; height:auto;"}
*Esquema interno de la Estrella de la Muerte mostrando la integración de la fotobatería*
{: style="color:gray; font-size: 70%; text-align: center;"}





### Componentes Clave:
- **Esfera de 12 cm**: Impresa en 3D y pintada para parecer la Estrella de la Muerte
- **Modulo solar de silicio**: constituido por 6 celdas de Si monocristalino soldadas en serie con alambre que permitía adaptarse con facilidad a la superficie curva de la esfera y a máxima iluminación producía > 3V de Voc necesarios para cargar la batería.
- **Batería LiFePO4**: recargable y comercial, de 3.4V nominal.
- **LEDs**: situados en diferentes posiciones de la esfera y divididos en tres segmentos que representan los "sistemas de defensa" visibles
- **Electrónica de control**: Microcontrolador extraido de un juguete que permitía encender/apagar todos los segmentos a la vez o por separado a distintas frecuencias de intermitencia que afectan al consumo.
- **Maletín de transporte**: Todo organizado y listo para el evento

![Maletín de transporte](/imgs/collage-maletin.jpg){:style="display:block; margin-left:auto; margin-right:auto"}
*El dispositivo preparado para su transporte: todo organizado y listo para el evento*
{: style="color:gray; font-size: 70%; text-align: center;"}


## El Impacto: Ciencia que Conecta

### Visitantes Entusiasmados

Durante los dos días del evento, según datos oficiales de Ayuntamiento y Policia de Zaragoza por la carpa de la Ciencia pasaron [38.000 personas](https://www.unizar.es/actualidad/vernoticia_ng.php?id=92664&idh=13140), en los dos días de apertura y 3 sesiones, cientos de personas de todas las edades participaron en nuestra actividad. Los más pequeños quedaban fascinados viendo cómo los LEDs se apagaban cuando "el planeta bloqueaba el sol", mientras que los adultos hacían preguntas técnicas sobre aplicaciones reales y el misterioso *superdiodo*, muchos desconocían que las celdas solares se comportan como un LED en oscuridad si tienen una fuente de energía que los alimente.


![Publico general](/imgs/collage-i-publico.jpg){:style="display:block; margin-left:auto; margin-right:auto;max-width:30%"}
*Visitantes en nuestro stand*
{: style="color:gray; font-size: 70%; text-align: center;"}


![Visitantes disfrazados](/imgs/Narkina5-Willrow-Hood-Camtono.jpg){:style="display:block; margin-left:auto; margin-right:auto;max-width:30%"}
*Tres Willrow Hood con su Camtono llenito de Beskar y un residente de Narkina 5*
{: style="color:gray; font-size: 70%; text-align: center;"}

### Conceptos Aprendidos:
- **Energía renovable**: La importancia del almacenamiento para fuentes intermitentes
- **Electrónica básica**: Cómo funcionan los LEDs, baterías y celdas solares
- **Ingeniería espacial**: Los desafíos energéticos de los satélites reales
- **Innovación**: Las fotobaterías como tecnología emergente

## Aplicaciones del Mundo Real

Mientras los niños se divertían salvando la Estrella de la Muerte, también explicábamos aplicaciones reales de las fotobaterías:

### En el Espacio:
- **Satélites CubeSat**: Optimización del peso con sistemas integrados
- **Misiones a Marte**: Rovers que necesitan sobrevivir largas noches marcianas
- **Estación Espacial Internacional**: Gestión eficiente de ciclos de 90 minutos

### En la Tierra:
- **IoT remoto**: Sensores ambientales autónomos
- **Dispositivos médicos**: Implantes que se recargan con luz a través de la piel
- **Electrónica portable**: El futuro de dispositivos sin cables
- **Almacenamiento en red**: Soluciones para energía solar doméstica

## La Ciencia Continúa: Proyectos de Investigación

Esta actividad divulgativa está respaldada por investigación puntera en el INMA:


### Proyecto SOLiBAT (CNS2023-145197)
Desarrollo de nuevas arquitecturas de fotobaterías basadas en materiales emergentes, financiado por MICIU/AEI y la Unión Europea NextGenerationEU/PRTR.
![Logo Solibat](/imgs/Logo-Solibat.png){:style="display:block; margin-left:auto; margin-right:auto"}
*Proyectos que hacen posible esta investigación: Solibat*
{: style="color:gray; font-size: 70%; text-align: center;"}


### Proyecto VOLTA (PID2022-140516OB-I00)
[Investigación en sistemas de almacenamiento energético híbridos, cofinanciado por FEDER, UE.](https://volta.unizar.es)
![Logo VOLTA](/imgs/Logo-VOLTA.png){:style="display:block; margin-left:auto; margin-right:auto"}
*Proyectos que hacen posible esta investigación: VOLTA*
{: style="color:gray; font-size: 70%; text-align: center;"}

## Recursos Educativos

### Para Educadores:
Si quieres replicar esta actividad en tu centro educativo, hemos preparado:
- [Guía de construcción del simulador solar](/2025/05/04/Programmable-Light-Dimmer/)
- Esquemas electrónicos de la fotobatería (disponibles por solicitud)
- Presentación didáctica adaptable a diferentes niveles

### Para Makers:
- Código del Perovskino para monitorización [disponible en GitHub](https://github.com/ej-jp/perovskino)
- Lista de componentes y proveedores
- Modelo 3D de la esfera 

## Reflexión Final: Cuando la Ciencia Ficción Inspira Ciencia Real

Lo más gratificante de esta experiencia fue ver cómo una narrativa de ciencia ficción puede hacer accesibles conceptos científicos complejos. Desde el niño de 6 años que entendió que "la Estrella de la Muerte necesita batería para cuando no hay sol" hasta el ingeniero jubilado que nos preguntó sobre la eficiencia de conversión de nuestras celdas, todos se llevaron algo valioso.

Las fotobaterías representan el futuro del almacenamiento energético integrado, y qué mejor manera de explicarlo que con una galaxia muy, muy lejana que todos conocemos y amamos.

**Que la fuerza (fotovoltaica) os acompañe.** ⚡🌟

---

## Agradecimientos

Esta actividad fue posible gracias a:
- Todo el equipo de voluntarios del INMA que dedicaron su fin de semana a la divulgación
- El Gobierno de Aragón y Aragón Investiga por organizar la Noche Europea de los Investigadores
- Los proyectos SOLiBAT y VOLTA por financiar esta investigación
- Lucasfilm (indirectamente) por crear un universo que sigue inspirando a nuevas generaciones de científicos

---

*¿Interesado en nuestras investigaciones sobre fotobaterías? Síguenos en nuestro [canal de Telegram](https://t.me/oss_lab) y mantente al día con los últimos avances del OSSLab del INMA.*

---

**Autor:** Emilio J. Juarez-Perez, Marta Haro Remón  
**Grupo de investigación:** NFP - Nanostructured Films & Particles y HYMAT - Hybrid Materials for Optoelectronics  
**Instituto:** INMA (CSIC-Universidad de Zaragoza)
