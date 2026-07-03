# MSTA

# Proyecto de Industria 4.0: Monitoreo y Análisis del Proceso de Corte en una Cricut mediante Sensores IoT y Machine Learning

## Descripción General

Este proyecto integra conceptos de **Industria 4.0**, **Internet de las Cosas (IoT)**, **Adquisición de Datos**, **Análisis de Series de Tiempo** y **Machine Learning**, utilizando una máquina de corte digital **Cricut Explore Air 2** y una plataforma embebida **Unihiker K10** para capturar y analizar las vibraciones generadas durante el proceso de manufactura.

El enfoque principal consiste en determinar si la información de vibración registrada durante el corte permite identificar eventos específicos del proceso, validar la correcta ejecución de las operaciones y generar una base para aplicaciones de monitoreo inteligente y mantenimiento predictivo.

---

# Infraestructura del Proyecto

## 1. Sistema de Corte

Se utiliza una **Cricut Explore Air 2** para cortar una hoja tamaño carta.

Cada hoja contiene **15 piezas idénticas** con las siguientes características:

- Forma circular.
- Cuatro orificios pequeños de sujeción.
- Un orificio circular de mayor tamaño para un ducto.
- Corte perimetral exterior de la pieza.

Estas geometrías generan distintos patrones mecánicos de movimiento y vibración que pueden reflejarse en los datos del acelerómetro.

---

## 2. Captura de Datos IoT

Para monitorear el proceso se emplea una **Unihiker K10**, la cual registra continuamente las aceleraciones producidas durante el corte mediante su acelerómetro integrado.

Las mediciones se transmiten por puerto serial con el siguiente formato:

```text
timestamp system_up_time_ms acc_x acc_y acc_z
```

### Ejemplo

```text
1783113235 1897872 -37.0 1158 116
```

### Descripción de los campos

| Campo | Descripción |
|---------|-----------|
| `timestamp` | Marca de tiempo |
| `system_up_time_ms` | Tiempo de actividad del dispositivo |
| `acc_x` | Aceleración en el eje X |
| `acc_y` | Aceleración en el eje Y |
| `acc_z` | Aceleración en el eje Z |

---

## 3. Procesamiento de Datos

La Unihiker se conecta a una **MacBook Air**, donde un script en **Python** recibe los datos seriales y genera un archivo JSON estructurado.

Durante este proceso se clasifican los registros en:

### Evento

Cuando ocurre un cambio significativo en las aceleraciones.

### Heartbeat

Cuando no existe una variación relevante y el sistema únicamente reporta actividad normal.

### Ejemplo de registro

```json
{
  "timestamp": 1783113235,
  "system_up_time_ms": 1897872,
  "tipo_envio": "evento",
  "accelerometer": {
    "x": -37.0,
    "y": 1158,
    "z": 116
  }
}
```

El resultado final es un arreglo de objetos JSON que representa la secuencia completa de vibraciones producidas durante el proceso de corte.

---

# Contexto Académico

Los estudiantes ya han completado el curso:

- **Analyzing IoT Data in Python (DataCamp)**

Actualmente están cursando:

- **Machine Learning for Time Series Data in Python (DataCamp)**

Además, revisarán material complementario relacionado con:

- Series de tiempo.
- Extracción de características.
- Transformada Rápida de Fourier (FFT).
- Análisis espectral.
- Clasificación de eventos.
- Detección de anomalías.

---

# Objetivo General

Aplicar técnicas de análisis de datos IoT y Machine Learning para estudiar las vibraciones generadas por una máquina de corte digital y determinar la relación entre los patrones de vibración observados y las operaciones físicas realizadas durante el proceso.

---

# Objetivos Específicos

## Exploración de Datos

Realizar un análisis exploratorio que permita:

- Comprender la estructura de los datos.
- Identificar valores atípicos.
- Analizar distribuciones.
- Detectar periodos de actividad e inactividad.
- Comparar el comportamiento entre los ejes X, Y y Z.

---

## Análisis Temporal

Investigar el comportamiento de las señales a lo largo del tiempo mediante:

- Gráficas de aceleración vs. tiempo.
- Magnitud total de aceleración.
- Ventanas móviles.
- Detección de picos.
- Segmentación automática de eventos.

Se espera identificar momentos específicos asociados a:

- Inicio del corte.
- Corte de agujeros pequeños.
- Corte del agujero grande.
- Corte perimetral.
- Finalización de una pieza.

---

## Análisis en Frecuencia

Transformar las señales al dominio de la frecuencia utilizando técnicas como **FFT** para:

- Identificar frecuencias dominantes.
- Comparar diferentes eventos de corte.
- Detectar patrones repetitivos.
- Analizar el comportamiento mecánico de la Cricut.

Los estudiantes deberán determinar si diferentes tipos de corte producen firmas espectrales distinguibles.

---

## Correlación del Proceso de Corte

Uno de los objetivos más importantes consiste en correlacionar los datos de vibración con la secuencia de fabricación.

Los equipos deberán investigar si es posible identificar:

### Agujeros pequeños

Determinar si los cuatro agujeros de sujeción generan patrones repetitivos observables en los datos.

### Agujero grande

Verificar si el corte circular del ducto produce una firma distinta de vibración.

### Corte exterior

Identificar el patrón asociado al contorno final de la pieza.

---

# Verificación Automática del Proceso

A partir de los patrones encontrados, los estudiantes deberán desarrollar un mecanismo que permita validar automáticamente que:

- Se realizaron los cuatro agujeros pequeños.
- Se realizó el agujero grande.
- Se completó el corte exterior.
- Las 15 piezas fueron procesadas correctamente.

Esta actividad simula un sistema de **Control de Calidad Inteligente**, típico de una implementación de Industria 4.0.

---

# Aplicación de Machine Learning

Utilizando los conocimientos adquiridos en el curso de Series de Tiempo, los estudiantes podrán explorar técnicas como:

## Extracción de Características

- RMS (Root Mean Square)
- Energía
- Varianza
- Frecuencia dominante
- Entropía
- Amplitud máxima

## Clasificación de Eventos

Clasificar automáticamente segmentos de señal como:

- Agujero pequeño.
- Agujero grande.
- Corte exterior.
- Movimiento sin corte.

## Detección de Anomalías

Identificar situaciones donde:

- Falte un corte.
- Exista una vibración inusual.
- Ocurra una desviación respecto al patrón esperado.

---

# Relación con Industria 4.0

Este proyecto integra múltiples pilares de Industria 4.0:

- IoT Industrial mediante la captura continua de datos sensoriales.
- Digitalización de procesos de manufactura.
- Análisis de Big Data a pequeña escala.
- Machine Learning aplicado a manufactura.
- Monitoreo de condición de maquinaria.
- Control de calidad automatizado.
- Trazabilidad de procesos.
- Mantenimiento predictivo basado en datos.

---

# Entregables Esperados

Cada equipo deberá presentar:

1. Descripción del proceso experimental.
2. Análisis exploratorio de datos.
3. Visualizaciones temporales.
4. Análisis en frecuencia.
5. Identificación y segmentación de eventos.
6. Correlación entre el diseño de corte y las vibraciones registradas.
7. Evidencia de la detección de los cortes realizados.
8. Propuesta de modelo de Machine Learning.
9. Conclusiones y oportunidades de mejora.

---

# Resultado Esperado

Al finalizar el proyecto, los estudiantes deberán demostrar que es posible utilizar sensores IoT y técnicas de análisis de series de tiempo para construir un sistema inteligente capaz de monitorear y validar automáticamente el proceso de fabricación realizado por la **Cricut Explore Air 2**, reproduciendo principios fundamentales de una solución real de **Industria 4.0**.
