# Second Exam 2023 - 1st Semester

## Equipo de Trabajo

- [Sergio Andres Amortegui Ardila](https://www.linkedin.com/in/sergioamar/) - 0000203657
- [Juan David Forero Rodriguez](https://www.linkedin.com/in/juanforo/) - 0000204301
- [Nadiv Esteban Mejia Morales](https://www.linkedin.com/in/nadivmejia/) - 0000201946

## 1. Análisis de la Problemática

Fruver 1A S.A, una comercializadora de mandarinas busca aumentar su producción para desarrollarse y competir al nivel de comercializadoras internacionales. Por lo cual, propone la automatización de la línea de operación de segmentación, empaquetado y entrega de mandarinas, mediante un sistema que permita mantener control y calidad para asegurar la cantidad, calidad y entrega requerida.

## 2. Diseño de la Solución

Identificando los aspectos claves para plantear un sistema automatizado que cumpla con las necesidades y expectativas de comercializar mandarinas, se identificaron los siguientes aspectos:

1. El objetivo diario es entregar 100 paquetes de mandarinas.
2. Cada paquete debe contar con 50 mandarinas.
3. No se debe requerir de intervension de personal a la hora de llenar los paquetes de mandarinas.

Por lo cual, buscando mantener una correcta segmentación y empaquetado de las mandarinas, se divirá el sistema en 5 flujos, los cuales cumplen una única función básica, buscando control y calidad en la entrega de mandarinas, mediante su cuantificación y empaquetado, evitando desperdicios y/o empaquetado de mandarinas fuera de lo inicialmente pactado, los flujos son presentados a continuación.

### 2.1 Flujo del Sistema

Flujo dedicado a la recopilación del cargamento de mandarinas, desempacado, selección de mandarinas, transporte de mandarinas, detección y cuenta de mandarinas, transporte de cajas y entrega de paquetes.

#### 2.1.1 Flow #0 - Cargamento

Proceso encargado de recibir, almacenar y actualizar el estado del cargamento entrante de mandarinas. Un agente es el encargado de la recolección y almacenamiento de las mandarinas.

#### 2.1.2 Flow #1 - Descarga y Selección

Proceso delegado de descargar las mandarinas y llevarlas a la línea de transporte, seleccionando las mandarinas que cumplan con los estándares de calidad requeridos. Un agente es el encargado de la selección de las mandarinas.

#### 2.1.3 Flow #2 - Transporte de Mandarinas

Proceso encargado de transportar las mandarinas seleccionadas desde el punto de selección del agente hasta el punto de detección de mandarinas.

#### 2.1.4 Flow #3 - Detección y Cuenta de Mandarinas

Proceso encargado de identificar y contar las mandarinas que pasan por el punto de detección, actualizando el estado del sistema para detener o activar las bandas de transporte.

#### 2.1.5 Flow #4 - Transporte de Cajas

Proceso encargado de transportar las cajas pasando desde el punto de detección hasta el punto de entrega.

### 2.2 Diagrama de Flujo

El flujo resultante de los segmentos analizados y definidos, se presenta a continuación.

![Diagrama de Flujo](Assets/Diagrama%20de%20Flujo.png)

## 3. Modelo de Simulación

Bajo la plataforma Codesys se desarrolló un sistema automatizado y simulado bajo el lenguaje ladder, que suple las necesidades escenciales para Fruver 1A S.A, mediante la implementación de los diferentes flujos identificados en el análisis de la problemática, incluyendo en cada uno de ellos estados, variables y eventos que permiten el correcto funcionamiento del sistema. Además, mediante una HMI (Human Machine Interface) se permite la interaccion con el usuario, permitiendo el inicio, detención, reinicio del sistema y la visualizacion de los estados y variables del sistema por medio de indicadores y animaciones.

### 3.1 Programación Ladder

#### 3.1.1 Red #1

Esta red simula el tiempo requerido para la descarga de las mandarinas del provedor, mediante el uso de un timer que simula la espera requerida para concretar dicho procedimiento. Una vez se complete el tiempo de espera se activa la variable `.is_loaded`, que permite proceder con el flujo del sistema.

![Red 1](Assets/Red%201.png)

#### 3.1.2 Red #2

Esta red cuenta con dos _Inputs_, `.is_on` & `.is_selecting_tangerines`, las cuales una vez se activan, permiten el inicio del proceso de automatización del sistema.

![Red 2](Assets/Red%202.png)

#### 3.1.3 Red #3

En este punto, se utiliza un timer a través del cual se simula el tiempo requerido para que una mandarina avance desde el inicio de la cinta transportadora hasta el final de la misma, esto, con el fin de conocer después de cuanto tiempo se debe encender el sensor encargado de la detección de mandarinas. (Solo es efectivo bajo la primera ejecución ocurrida dentro de la sesión del sistema, gracias a la variable `.is_fresh_start`)

![Red 3](Assets/Red%203.png)

#### 3.1.4 Red #4

Llegados a esta red, el sensor infrarrojo se enciende, y cuenta con un tiempo de espera definido por la variable `.t_tangerines_fall_delay`, este tiempo referencia el espacio existente desde que pasa una mandarina por el sensor hasta que pasa la siguiente mandarina, y activa la variable `.is_tagerine_detected` para que se pueda proceder con el flujo del sistema.

![Red 4](Assets/Red%204.png)

#### 3.1.5 Red #5

Esta red tiene como función generar un tiempo de espera pequeño, determinado por `.t_restart_detection_delay`, y posteriormente reiniciar el detector de mandarinas, de esta forma nos asesoramos que el relay interno del sensor infrarojo se encuentre restablecido para la siguiente mandarina.

![Red 5](Assets/Red%205.png)

#### 3.1.6 Red #6

Al llegar a esta red nos encontramos con un contador ascendente (CTU), este será el encargado de contar la cantidad de mandarinas que han pasado por el sensor a lo largo del tiempo, y al llegar al valor definido por la variable `.n_tangerines_per_boxes` activará la segunda cinta transportadora con la siguiente caja vacía, y detendrá la primera cinta para que no caigan mandarinas durante el proceso de ubicar la nueva caja.

![Red 6](Assets/Red%206.png)

#### 3.1.7 Red #7

Similar a la red anterior, nos encontramos con un CTU, pero en lugar de contar mandarinas, este se encargará de llevar la cuenta de la totalidad de cajas llenadas durante la sesión de trabajo del sistema bajo la variable `.n_box_counter`. La cantidad de cajas objetivo, o "milestone", se almacena en la variable `.n_boxes_daily_milestone` y una vez alcanzado este objetivo, se activará la variable `.is_milestone`.

![Red 7](Assets/Red%207.png)

#### 3.1.8 Red #8

El timer mostrado en esta red es el encargado de bajar el flanco del sistema de conteo de las cajas, de forma que cuando se llene una nueva caja, esta se agregará correctamente al contador. Sin este timer, el contador no recibiría un cambio de flanco, y por tanto, no contaría una nueva entrada.

![Red 8](Assets/Red%208.png)

#### 3.1.9 Red #9

Esta red es la encargada de volver a iniciar la cinta transportadora de mandarinas y otros elementos, después de que las cajas se hayan terminado de mover y la nueva caja este a disposición para las mandarinas transportadas por la primera cinta transportadora.

![Red 9](Assets/Red%209.png)

#### 3.1.10 Red #10

Esta es una red auxiliar empleada para asignar el valor negado de la variable `.is_tangerine_detected` a la variable `.is_tangerine_not_detected`, esto con el fin de poder utilizar dicha variable como recurso para un mejor manejo de animaciones y estados del sistema.

![Red 10](Assets/Red%2010.png)

#### 3.1.11 Red #11

Red encargada de reiniciar todos los estados y variables del sistema, para que este pueda volver a funcionar de forma correcta en una nueva sesion de trabajo.

![Red 11](Assets/Red%2011.png)

#### 3.1.12 Red #12

Red encargada de reiniciar todos los estados y variables del sistema bajo una emergencia. Ademas, bloquea el funcionamiento general del sistema, para que este pueda volver a funcionar de forma correcta en una nueva sesión de trabajo una vez haya concluido dicha emergencia.

![Red 12](Assets/Red%2012.png)

### Human Machine Interface (HMI)

La HMI permite la interacción con el usuario, permitiendo el inicio, detención, reinicio del sistema y la visualización de los estados y variables del sistema por medio de indicadores y animaciones.

![HMI](Assets/HMI.jpg)

La información proporcionada por la HMI busca mantener una relación directa con respecto a los estados y operaciones del sistema, incluyendo ilulstraciones gráficas que indican el estado actual de diversos procesos, variables y puntos en el tiempo mediante indicadores luminicos, botones y barras de progreso. Ademas, expresa de manera simple el flujo definido en el diseño de la solucion mediante animaciones, iconos, y colores.

https://user-images.githubusercontent.com/62365030/230659743-c020f4ee-137b-43b9-b697-f08db5da9489.mp4

## 4. Implementación

El montaje del protipo consta de un segmento físico y una segmento lógico, la segmento físico se encarga de recrear el funcionamiento del prototipo en un ambiente funcional, mientras que el segmento lógico se encarga de la programación del sistema. El segmento físico se realizó mediante la construcción de un prototipo con base a los materiales mencionados anteriormente, mientras que el segmento lógico se realizó mediante la programación del sistema con base al lenguaje ladder en el software openPLC. A continuación se expone en mayor detalle lo mencionado anteriormente.

### 4.1 Construcción del Prototipo: Segmento Lógico

Mediante manejo de estados, entradas y salidas correspondientes a actuadores como sensores, botones, entre otros. Se gestionó el flujo de trabajo de la linea de operacion de tal forma que permitiera la pausa, reinicio de emergencia e inicio del sistema de forma correcta.

#### 4.1.1 Red #1

Define el inicio del sistema, mediante reinicio de contadores y activación de la banda de tranporte de mandarinas.

![Red 1](Assets/Red%201%20PLC.png)

#### 4.1.2 Red #2

Define el funcionamiento correspondiente al movimiento de la banda de transporte de cajas, mediante el uso de un timer TON, el cual permite la activación de la banda de transporte de cajas por un tiempo determinado, y posteriormente, la desactivación de la misma.

![Red 2](Assets/Red%202%20PLC.png)

#### 4.1.3 Red #3

Define el funcionamiento correspondiente al conteo de las mandarinas en la caja, mediante la activación por el sensor de proximidad (sensor infrarojo). Además, una vez completado el conteo, se activa la banda de transporte de cajas para que esta pueda transportar la caja llena a la estación de empaquetado.

![Red 3](Assets/Red%203%20PLC.png)

#### 4.1.4 Red #4

Define el funcionamiento correspondiente al conteo de las cajas llenas, mediante el registro de la cantidad de mandarinas registradas y el movimiento de la banda transportadora. Ademas, una vez completado el conteo, se activa la bandera del objetivo del dia (milestone) indicando que se ha cumplico con éxito la cantidad objetivo de cajas esperadas.

![Red 4](Assets/Red%204%20PLC.png)

#### 4.1.5 Red #5

Define el funcionamiento correspondiente al estado apagado del sistema, desactivando todos los módulos del sistema, así como sus respectivas variables y estados.

![Red 5](Assets/Red%205%20PLC.png)

#### 4.1.6 Red #6

Define el funcionamiento correspondiente al estado de emergencia del sistema, inabilitando todos los modulos y reiniciando completamente el sistema.

![Red 6](Assets/Red%206%20PLC.png)

#### 4.1.7 Red #7

Define el funcionamiento correspondiente al estado de pausa del sistema, inabilitando el funcionamiento de la banda de transporte de mandarinas y la banda de transporte de cajas.

![Red 7](Assets/Red%207%20PLC.png)

### 4.2 Construcción del Prototipo: Segmento Físico

El prototipo se construyó mediante la utilización de materiales como Arduino UNO, motorreductores, leds RGB, bateria de 9V, cableado y conectores, sensor infrarojo, entre otros. El prototipo se construyó de tal forma que permitiera la automatización de la línea de operación, segmentación, empaquetado y entrega de mandarinas. Además, permite la interacción con el usuario mediante un controlador, el cual permite el inicio, detención, reinicio del sistema y la visualización de los estados y variables del sistema por medio de indicadores lumínicos.

#### 4.2.1 Configuracion Funcional

![Configuracion Funcional](Assets/Montaje%20de%20Configuracion.jpg)

#### 4.2.2 Configuracion Fisica

![Configuracion Fisica](Assets/Montaje%20de%20Configuracion%20Fisica.jpg)

### 4.3 Diagrama de Circuito Eléctrico

## 5. Anexos

### 5.1 Tipos

| Tipo   | Convención |
| :----- | :--------- |
| `BOOL` | is\_       |
| `INT`  | n\_        |
| `TIME` | t\_        |
| `TON`  | ton\_      |
| `ET`   | et\_       |
| `CTU`  | ctu\_      |

### 5.2 Definicion de Variables

#### 5.2.1 Variables de Proceso

Define las variables empleadas en el proceso de automatización de la línea de operación de segmentación, empaquetado y entrega de mandarinas.

| Sección    | Variable                   | Tipo   | Valor Inicial | Operación | Descripción                                                              |
| :--------- | :------------------------- | :----- | :------------ | :-------- | :----------------------------------------------------------------------- |
| `Global`   | is_on                      | `BOOL` | 0             | `Input`   | Define el estado de inicio del sistema                                   |
| `Global`   | is_stop                    | `BOOL` | 0             | `Input`   | Define el estado de parada de emergencia del sistema                     |
| `Global`   | is_new_box                 | `BOOL` | 0             | `Output`  | Define el estado que indica si una nueva caja se ha completado           |
| `Global`   | is_milestone               | `BOOL` | 0             | `Output`  | Define el estado de si se ha cumplido el objetivo de cajas del dia       |
| `Global`   | is_fresh_start             | `BOOL` | 0             | `Output`  | Define el estado de si es el primer inicio del sistema                   |
| `Global`   | is_reset_box_counter       | `BOOL` | 0             | `Output`  | Define el estado de si se debe reiniciar la cuenta de cajas              |
| `Global`   | is_reset_tangerine_counter | `BOOL` | 0             | `Output`  | Define el estado de si se debe reiniciar la cuenta de mandarinas         |
| `Global`   | n_box_counter              | `INT`  | 0             | `Output`  | Define la cantidad actual de cajas completadas                           |
| `Global`   | n_tangerine_counter        | `INT`  | 0             | `Output`  | Define la cantidad actual de manadarinas dentro de una caja              |
| `Global`   | n_tangerines_per_boxes     | `INT`  | 50            | `Output`  | Define la cantidad de mandarinas maximas por caja                        |
| `Global`   | n_boxes_daily_milestone    | `INT`  | 100           | `Output`  | Define la cantidad de cajas minimas para completar el objetivo diario    |
| `Flow #0`  | is_loaded                  | `BOOL` | 0             | `Output`  | Define el estado de si completo la entrega de mandarinas                 |
| `Flow #1`  | is_selecting_tangerines    | `BOOL` | 0             | `Input`   | Define el estado de si el agente esta seleccionando mandarinas           |
| `Flow #2`  | is_moving_tangerines       | `BOOL` | 0             | `Output`  | Define el estado de si la banda transportadora de mandarinas esta activa |
| `Flow #3`  | is_sensor_active           | `BOOL` | 0             | `Output`  | Define el estado de si el sensor infrarojo esta activo                   |
| `Flow #3`  | is_tangerine_detected      | `BOOL` | 0             | `Output`  | Define el estado de si una mandarina fue detectada cayendo a la caja     |
| `Flow #4`  | is_moving_boxes            | `BOOL` | 0             | `Output`  | Define el estado de si la banda tranportadora de cajas esta activa       |
| `Counters` | ctu_tangerines             | `CTU`  | 0             | `Output`  | Define la cantidad actual de mandarinas dentro de la caja                |
| `Counters` | ctu_boxes                  | `CTU`  | 0             | `Output`  | Define la cantidad actual de cajas completadas                           |

#### 5.2.2 Variables de Simulación

Define las variables empleadas en el proceso de las simulación de automatización de la línea de operación de segmentación, empaquetado y entrega de mandarinas.

| Sección   | Variable                    | Tipo   | Valor Inicial | Operación | Descripción                                                                                                   |
| :-------- | :-------------------------- | :----- | :------------ | :-------- | :------------------------------------------------------------------------------------------------------------ |
| `Flow #0` | t_delivery_delay            | `TIME` | T#5S          | `Output`  | Define el tiempo de espera que simula la entrega de mandarinas                                                |
| `Flow #2` | t_tangerines_conveyor_delay | `TIME` | T#5S          | `Output`  | Define el tiempo de espera que simula el movimiento de la banda transportadora de naranjas                    |
| `Flow #3` | is_tangerine_not_detected   | `BOOL` | 0             | `Output`  | Define el estado de si una mandarina fue detectada cayaendo a la caja (negada)                                |
| `Flow #3` | t_tangerines_fall_delay     | `TIME` | T#5S          | `Output`  | Define el tiempo de espera que simula la caida de las mandarinas                                              |
| `Flow #3` | t_restart_detection_delay   | `TIME` | T#5S          | `Output`  | Define el tiempo de espera que simula el tiempo entre caida de mandarinas                                     |
| `Flow #4` | t_boxes_conveyor_delay      | `TIME` | T#5S          | `Output`  | Define el tiempo de espera que simula el movimiento de la banda transportadora de cajas                       |
| `Flow #4` | t_restart_new_box_delay     | `TIME` | T#5S          | `Output`  | Define el tiempo de espera que simula el tiempo entre cajas completadas                                       |
| `Timers`  | ton_delivery                | `TON`  | 0             | `Output`  | Define el timer que actua como delay para simular la entrega de las mandarinas                                |
| `Timers`  | et_delivery                 | `TIME` | 0             | `Output`  | Define el tiempo actual para el timer `ton_delivery`                                                          |
| `Timers`  | ton_conveyor_tangerines     | `TON`  | 0             | `Output`  | Define el timer que actua como delay para simular el movimiento de la banda transportadora de mandarinas      |
| `Timers`  | et_conveyor_tangerines      | `TIME` | 0             | `Output`  | Define el tiempo actual para el timer `ton_conveyor_tangerines`                                               |
| `Timers`  | ton_tangerine_detector      | `TON`  | 0             | `Output`  | Define el timer que actua como delay para simular el tiempo de deteccion entre mandarinas                     |
| `Timers`  | et_tangerine_detector       | `TIME` | 0             | `Output`  | Define el tiempo actual para el timer `ton_tangerine_detector`                                                |
| `Timers`  | ton_conveyor_boxes          | `TON`  | 0             | `Output`  | Define el timer que actua como delay para simular el tiempo de movimiento de la banda transportadora de cajas |
| `Timers`  | et_conveyor_boxes           | `TIME` | 0             | `Output`  | Define el tiempo actual para el timer `ton_conveyor_boxes`                                                    |
| `Timers`  | ton_restart_detection       | `TON`  | 0             | `Output`  | Define el timer que actua como delay para reiniciar el timer `ton_conveyor_tangerines`                        |
| `Timers`  | et_restart_detection        | `TIME` | 0             | `Output`  | Define el tiempo actual para el timer `ton_restart_detection`                                                 |
| `Timers`  | ton_restart_new_box         | `TON`  | 0             | `Output`  | Define el timer que actua como delay para reiniciar el timer `ton_conveyor_boxes`                             |
| `Timers`  | et_restart_new_box          | `TIME` | 0             | `Output`  | Define el tiempo actual para el timer `ton_restar_new_box`                                                    |
