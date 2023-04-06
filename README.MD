# Second Exam 2023 - 1st Semester

## Equipo de Trabajo

- [Sergio Andres Amortegui Ardila](https://www.linkedin.com/in/sergioamar/) - 0000203657
- [Juan David Forero Rodriguez](https://www.linkedin.com/in/juanforo/) - 0000201946
- [Nadiv Esteban Mejia Morales](https://www.linkedin.com/in/nadivmejia/) - 0000204301

## 1. Análisis de la Problemática

Explicar el problema y que se propone.

## 2. Diseño de la Solución

Etapas y actividades de cada una del ellas.

## 2.1 Diagrama de Flujo

flujo

## 3. Modelo de Simulación

Explicamos y referenciamos la tabla de la deficion de variables

## 4. Implementación

Parte fisica

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
| `Global`   | n_tangerine_counter        | `INT`  | 0             | `Output`  | Mantiene la cuenta de la cantidad de manadarinas          |
| `Global`   | n_box_counter              | `INT`  | 0             | `Output`  | Mantiene la cuenta de la cantidad de cajas despachadas    |
| `Global`   | is_new_box                 | `BOOL` | 0             | `Output`  | Define la si una nueva caja se ha completado              |
| `Global`   | is_milestone               | `BOOL` | 0             | `Output`  | Define el objetivo de cajas del dia                       |
| `Global`   | is_reset_tangerine_counter | `BOOL` | 0             | `Output`  | Define cuando el contador deba reiniciarse                |
| `Global`   | is_reset_box_counter       | `BOOL` | 0             | `Output`  | Define cuando el contador deba reiniciarse                |
| `Global`   | is_fresh_start             | `BOOL` | 0             | `Output`  | Define el estado inicial del sistema                      |
| `Flow #0`  | is_Loaded                  | `BOOL` | 0             | `Output`  | Define cuando el cargamento de mandarinas se recibio      |
| `Flow #1`  | is_selecting               | `BOOL` | 0             | `Input`   | Define el inicio de la automatizacion                     |
| `Flow #2`  | is_moving_tangerines       | `BOOL` | 0             | `Ouput`   | Define el estado de la banda transportadora de mandarinas |
| `Flow #3`  | is_sensor_active           | `BOOL` | 0             | `Ouput`   | Define el el tiempo que toma la primera mandarina         |
| `Flow #3`  | is_tangerine_detected      | `BOOL` | 0             | `Ouput`   | Define el el tiempo que toma la primera mandarina         |
| `Flow #4`  | is_moving_boxes            | `BOOL` | 0             | `Ouput`   | Define el el tiempo que toma la primera mandarina         |
| `Counters` | ctu_tangerines             | `CTU`  | 0             | `Output`  | Define cuando el contador deba reiniciarse                |
| `Counters` | ctu_boxes                  | `CTU`  | 0             | `Output`  | Define cuando el contador deba reiniciarse                |

#### 5.2.2 Variables de Simulación

Define las variables empleadas en el proceso de las simulacion de automatizacion de la linea de operacion de segmentacion, empaquetao y entrega de mandarinas.

| Sección   | Variable                    | Tipo   | Valor Inicial | Operación | Descripción                                       |
| :-------- | :-------------------------- | :----- | :------------ | :-------- | :------------------------------------------------ |
| `Flow #0` | t_delivery_delay            | `TIME` | T#5S          | `Output`  | Define el tiempo que toma recibir las mandarinas  |
| `Flow #2` | t_tangerines_conveyor_delay | `TIME` | T#5S          | `Ouput`   | Define el el tiempo que toma la primera mandarina |
| `Flow #3` | t_tangerines_fall_delay     | `TIME` | T#5S          | `Ouput`   | Define el el tiempo que toma la primera mandarina |
| `Flow #4` | t_boxes_conveyor_delay      | `TIME` | T#5S          | `Ouput`   | Define el el tiempo que toma la primera mandarina |
| `Timers`  | ton_delivery                | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_delivery                 | `ET`   | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_selection               | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_selection                | `ET`   | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_conveyor_tangerines     | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_conveyor_tangerines      | `ET`   | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_tangerine_detector      | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_tangerine_detector       | `ET`   | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_conveyor_boxes          | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_conveyor_boxes           | `ET`   | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_restart_detection       | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_restart_detection        | `ET`   | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | ton_restart_new_box         | `TON`  | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
| `Timers`  | et_restart_new_box          | `ET`   | ?             | `Output`  | Define cuando el contador deba reiniciarse        |
