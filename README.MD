# Second Exam 2023 - 1st Semester

## Equipo de Trabajo

- [Sergio Andres Amortegui Ardila](https://www.linkedin.com/in/sergioamar/) - 0000203657
- [Juan David Forero Rodriguez](https://www.linkedin.com/in/juanforo/) - 0000201946
- [Nadiv Esteban Mejia Morales](https://www.linkedin.com/in/nadivmejia/) - 0000204301

## 1. Análisis de la Problemática

Fruver 1A S.A, una comercializadora de mandarinas busca aumentar su produccion para desarrollarse y competir al nivel de comercializadoras internacionales. Por lo cual, propone la automatización de la línea de operación de segmentación, empaquetado y entrega de mandarinas, mediante un sistema que permita mantener control y calidad para asegurar la cantidad, calidad y entrega requerida.

## 2. Diseño de la Solución

Identificando los aspectos claves para plantear un sistema automatizado que cumpla con las necesidades y expectativas de comercializar mandarinas, se identificaron los siguientes aspectos:

1. El objetivo diario es entregar 100 paquetes de mandarinas.
2. Cada paquete debe contar con 50 mandarinas.
3. No se debe requerir de intervension de personal a la hora de llenar los paquetes de mandarinas.

Por lo cual, buscando mantener una correcta segmentacion y empaquetado de las mandarinas, se divira el sistema en 5 flujos, los cuales cumplen una unica funcion basica, buscando control y calidad en la entrega de mandarinas, mediante su cuantificacion y empaquetado, evitando desperdicios y/o empaquetado de mandarinas fuera de lo inicialmente pactado, los flujo son presentados a continuacion.

### 2.1 Flujo del Sistema

Flujo dedicado a la recopilacion del cargamento de mandarinas, desempacado, seleccion de mandarinas, transporte de mandarinas, deteccion y cuenta de mandarinas, transporte de cajas y entrega de paquetes.

#### 2.1.1 Flow #0 - Cargamento

Proceso encargado de recibir, almacenar y actualizar el estado del cargamento entrante de mandarinas. Un agente es el encargado de la recoleccion y almacenamiento de las mandarinas.

#### 2.1.2 Flow #1 - Descarga y Seleccion

Proceso delegado de descargar las mandarinas y llevarlas a la linea de transporte, seleccionando las mandarinas que cumplan con los estandares de calidad requeridos. Un agente es el encargado de la seleccion de las mandarinas.

#### 2.1.3 Flow #2 - Transporte de Mandarinas

Proceso encargado de transportar las mandarinas seleccionadas desde el punto de seleccion del agente hasta el punto de deteccion de mandarinas.

#### 2.1.4 Flow #3 - Deteccion y Cuenta de Mandarinas

Proceso encargado de identificar y contar las mandarinas que pasan por el punto de deteccion, actualizando el estado del sistema para detener o activar las bandas de transporte.

#### 2.1.5 Flow #4 - Transporte de Cajas

Proceso encargado de transportar las cajas pasando desde el punto de deteccion hasta el punto de entrega.

### 2.2 Diagrama de Flujo

El flujo resultante de los segmentos analizados y definidos, se presenta a continuacion.

![Diagrama de Flujo](Assets/Diagrama%20de%20Flujo.png)

## 3. Modelo de Simulación

Bajo la plataforma Codesys se desarrollo un sistema automatizado y simulado bajo el lenguaje ladder, que suple las necesidades escenciales para Fruver 1A S.A, mediante la implementacion de los diferentes flujos identificados en el analisis de la problematica, incluyendo en cada uno de ellos estados, variables y eventos que permiten el correcto funcionamiento del sistema. Ademas, mediante una HMI (Human Machine Interface) se permite la interaccion con el usuario, permitiendo el inicio, paro, reinicio del sistema y la visualizacion de los estados y variables del sistema por medio de indicadores y animaciones.

### 3.1 Programación Ladder

#### 3.1.1 Red #1

![Red 1](Assets/Red%201.png)

Esta red simula el tiempo requerido para la descarga de las mandarinas, mediante el uso de un timer que simula la espera requerida para concretar dicho procedimiento. Una vez se complete el tiempo de espera se activa la variables `.is_loaded`, que permite proceder con el flujo del sistema.

#### 3.1.2 Red #2

Esta red cuenta con dos _Inputs_, `.is_on` & `.is_selecting`, las cuales una vez se activan, permiten el inicio del proceso de seleccion de mandarinas.

![Red 2](Assets/Red%202.png)

#### 3.1.3 Red #3

algo

![Red 3](Assets/Red%203.png)

#### 3.1.4 Red #?

El proceso de seleccion de mandarinas se realiza mediante el uso de un sensor infrarrojo, el cual cuenta con un tiempo de espera de 1 segundo, el cual se activa mediante el uso de un timer. Una vez se complete el tiempo de espera, se activa la variable `.is_selected`, que permite proceder con el flujo del sistema.

![Red 4](Assets/Red%204.png)

#### 3.1.5 Red #5

algo

![Red 5](Assets/Red%205.png)

#### 3.1.6 Red #6

algo

![Red 6](Assets/Red%206.png)

#### 3.1.7 Red #7

algo

![Red 7](Assets/Red%207.png)

#### 3.1.8 Red #8

algo

![Red 8](Assets/Red%208.png)

#### 3.1.9 Red #9

algo

![Red 9](Assets/Red%209.png)

### Human Machine Interface (HMI)

La HMI permite la interaccion con el usuario, permitiendo el inicio, paro, reinicio del sistema y la visualizacion de los estados y variables del sistema por medio de indicadores y animaciones.

![HMI](Assets/HMI.jpg)

## 4. Implementación

Bajo la aproximacion trabajada en el modelo de simulacion, la construccion e implementacion de la solucion a un protipo fisico implico el uso de diverso materiales como:

1. Arduino UNO
2. Servomotores
3. Bateria
4. Cableado y Conectores
   1. Sensor Infrarrojo

### 4.1 Diagrama de Circuito Eléctrico

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

Define las variables empleadas en el proceso de automatizacion de la linea de operacion de segmentacion, empaquetao y entrega de mandarinas.

| Sección    | Variable                   | Tipo   | Valor Inicial | Operación | Descripción                                               |
| :--------- | :------------------------- | :----- | :------------ | :-------- | :-------------------------------------------------------- |
| `Global`   | is_on                      | `BOOL` | 0             | `Input`   | Define el inicio del sistema                              |
| `Global`   | is_stop                    | `BOOL` | 0             | `Input`   | Define la parada del sistema                              |
| `Global`   | is_new_box                 | `BOOL` | 0             | `Output`  | Define la si una nueva caja se ha completado              |
| `Global`   | is_milestone               | `BOOL` | 0             | `Output`  | Define el objetivo de cajas del dia                       |
| `Global`   | is_fresh_start             | `BOOL` | 0             | `Output`  | Define el estado inicial del sistema                      |
| `Global`   | is_reset_box_counter       | `BOOL` | 0             | `Output`  | Define cuando el contador deba reiniciarse                |
| `Global`   | is_reset_tangerine_counter | `BOOL` | 0             | `Output`  | Define cuando el contador deba reiniciarse                |
| `Global`   | n_box_counter              | `INT`  | 0             | `Output`  | Mantiene la cuenta de la cantidad de cajas despachadas    |
| `Global`   | n_tangerine_counter        | `INT`  | 0             | `Output`  | Mantiene la cuenta de la cantidad de manadarinas          |
| `Global`   | n_tangerines_per_boxes     | `INT`  | 0             | `Output`  | Define el estado inicial del sistema                      |
| `Global`   | n_boxes_daily_milestone    | `INT`  | 0             | `Output`  | Define el estado inicial del sistema                      |
| `Flow #0`  | is_loaded                  | `BOOL` | 0             | `Output`  | Define cuando el cargamento de mandarinas se recibio      |
| `Flow #1`  | is_selecting_tangerines    | `BOOL` | 0             | `Input`   | Define el inicio de la automatizacion                     |
| `Flow #2`  | is_moving_tangerines       | `BOOL` | 0             | `Output`  | Define el estado de la banda transportadora de mandarinas |
| `Flow #3`  | is_sensor_active           | `BOOL` | 0             | `Output`  | Define el el tiempo que toma la primera mandarina         |
| `Flow #3`  | is_tangerine_detected      | `BOOL` | 0             | `Output`  | Define el el tiempo que toma la primera mandarina         |
| `Flow #4`  | is_moving_boxes            | `BOOL` | 0             | `Output`  | Define el el tiempo que toma la primera mandarina         |
| `Counters` | ctu_tangerines             | `CTU`  | 0             | `Output`  | Define cuando el contador deba reiniciarse                |
| `Counters` | ctu_boxes                  | `CTU`  | 0             | `Output`  | Define cuando el contador deba reiniciarse                |

#### 5.2.2 Variables de Simulación

Define las variables empleadas en el proceso de las simulacion de automatizacion de la linea de operacion de segmentacion, empaquetao y entrega de mandarinas.

| Sección   | Variable                    | Tipo   | Valor Inicial | Operación | Descripción                                       |
| :-------- | :-------------------------- | :----- | :------------ | :-------- | :------------------------------------------------ |
| `Flow #0` | t_delivery_delay            | `TIME` | T#5S          | `Output`  | Define el tiempo que toma recibir las mandarinas  |
| `Flow #2` | t_tangerines_conveyor_delay | `TIME` | T#5S          | `Output`  | Define el el tiempo que toma la primera mandarina |
| `Flow #3` | is_tangerine_not_detected   | `BOOL` | 0             | `Output`  | Define el el tiempo que toma la primera mandarina |
| `Flow #3` | t_tangerines_fall_delay     | `TIME` | T#5S          | `Output`  | Define el el tiempo que toma la primera mandarina |
| `Flow #3` | t_restart_detection_delay   | `TIME` | T#5S          | `Output`  | Define el el tiempo que toma la primera mandarina |
| `Flow #4` | t_boxes_conveyor_delay      | `TIME` | T#5S          | `Output`  | Define el el tiempo que toma la primera mandarina |
| `Flow #4` | t_restart_new_box_delay     | `TIME` | T#5S          | `Output`  | Define el el tiempo que toma la primera mandarina |
| `Timers`  | ton_delivery                | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_delivery                 | `TIME` | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_conveyor_tangerines     | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_conveyor_tangerines      | `TIME` | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_tangerine_detector      | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_tangerine_detector       | `TIME` | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_conveyor_boxes          | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_conveyor_boxes           | `TIME` | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_restart_detection       | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_restart_detection        | `TIME` | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_restart_new_box         | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_restart_new_box          | `TIME` | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
