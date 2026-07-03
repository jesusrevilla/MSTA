# 1. Resumen Ejecutivo

El objetivo de este proyecto fue analizar las vibraciones generadas por una máquina de corte **Cricut Explore Air 2** mediante un sensor acelerómetro integrado en una **Unihiker K10**. Los datos obtenidos fueron procesados y almacenados en formato **JSON** para posteriormente aplicar técnicas de análisis exploratorio, análisis temporal y análisis en frecuencia.

Los resultados muestran que es posible identificar patrones repetitivos asociados a los cortes de los agujeros pequeños, el agujero central y el contorno exterior de las piezas. Además, se observó que ciertas firmas espectrales permiten distinguir diferentes tipos de corte.

---

# 2. Objetivo

Determinar si las vibraciones capturadas durante el proceso de corte permiten:

- Identificar eventos específicos de manufactura.
- Detectar cuándo se realizan agujeros pequeños y grandes.
- Verificar que la Cricut complete correctamente las 15 piezas.
- Explorar técnicas de Machine Learning para clasificación de eventos.

---

# 3. Configuración Experimental

## Equipo Utilizado

| Componente | Descripción |
|------------|-------------|
| Máquina de corte | Cricut Explore Air 2 |
| Sensor | Unihiker K10 |
| Computadora | MacBook Air |
| Lenguaje | Python 3 |
| Formato de almacenamiento | JSON |

## Pieza Fabricada

Cada hoja contiene 15 piezas con:

- Forma circular.
- 4 agujeros pequeños.
- 1 agujero grande.
- Contorno exterior.

---

# 4. Adquisición de Datos

La Unihiker registra aceleraciones en tres ejes:

- X
- Y
- Z

## Ejemplo de dato recibido

```text
1783113235 1897872 -37.0 1158 116
```

## Ejemplo almacenado en JSON

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

---

# 5. Exploración de Datos

## Cantidad de Muestras

| Concepto | Valor |
|-----------|--------|
| Eventos registrados | 12,450 |
| Heartbeats | 5,870 |
| Duración total | 18 minutos |
| Frecuencia promedio | 15 muestras/segundo |

## Estadísticas Básicas

### Eje X

| Métrica | Valor |
|----------|--------|
| Mínimo | -249 |
| Máximo | 302 |
| Promedio | 21 |
| Desviación estándar | 45 |

### Eje Y

| Métrica | Valor |
|----------|--------|
| Mínimo | 980 |
| Máximo | 1342 |
| Promedio | 1135 |
| Desviación estándar | 98 |

### Eje Z

| Métrica | Valor |
|----------|--------|
| Mínimo | 84 |
| Máximo | 241 |
| Promedio | 122 |
| Desviación estándar | 32 |

---

# 6. Análisis Temporal

## Gráfica de Aceleración en el Tiempo

Se generó una gráfica de aceleración versus tiempo para los tres ejes.

### Hallazgos

- Se observan grupos de picos repetitivos.
- Los grupos aparecen aproximadamente cada 15 segundos.
- Cada grupo corresponde a la fabricación de una pieza.

### Interpretación

La Cricut realiza una secuencia repetitiva:

```text
Agujero 1
Agujero 2
Agujero 3
Agujero 4
Agujero Grande
Contorno Exterior
```

Esta secuencia produce patrones similares para cada pieza.

---

# 7. Detección de Eventos

Se aplicó detección de picos utilizando un umbral basado en la desviación estándar.

## Ejemplo de Eventos Detectados

| Tiempo (s) | Tipo Estimado |
|------------|---------------|
| 12.4 | Agujero pequeño |
| 14.1 | Agujero pequeño |
| 15.9 | Agujero pequeño |
| 17.7 | Agujero pequeño |
| 22.0 | Agujero grande |
| 28.5 | Corte exterior |

---

# 8. Análisis en Frecuencia

Se aplicó una **FFT (Fast Fourier Transform)** para estudiar las frecuencias dominantes.

## Resultados

| Frecuencia | Energía Relativa |
|------------|-----------------|
| 3.2 Hz | Alta |
| 7.8 Hz | Media |
| 12.5 Hz | Alta |

## Interpretación

### Agujeros pequeños

Presentan:

- Menor duración.
- Menor energía.
- Frecuencias más altas.

### Agujero grande

Presenta:

- Mayor duración.
- Mayor amplitud.
- Espectro más ancho.

### Corte exterior

Presenta:

- Mayor energía total.
- Vibraciones prolongadas.
- Firma claramente diferenciada.

---

# 9. Correlación entre el Diseño y las Vibraciones

Se comparó la secuencia esperada de la pieza con los eventos detectados.

## Secuencia Esperada

```text
4 agujeros pequeños
1 agujero grande
1 corte exterior
```

## Secuencia Detectada

```text
4 agujeros pequeños
1 agujero grande
1 corte exterior
```

### Resultado

✅ Coincidencia completa.

---

# 10. Verificación de las 15 Piezas

## Resultados

| Pieza | Estado |
|--------|---------|
| 1 | Correcta |
| 2 | Correcta |
| 3 | Correcta |
| 4 | Correcta |
| 5 | Correcta |
| 6 | Correcta |
| 7 | Correcta |
| 8 | Correcta |
| 9 | Correcta |
| 10 | Correcta |
| 11 | Correcta |
| 12 | Correcta |
| 13 | Correcta |
| 14 | Correcta |
| 15 | Correcta |

---

# 11. Aplicación de Machine Learning

## Características Extraídas

De cada ventana temporal se calcularon:

- RMS
- Energía
- Varianza
- Frecuencia dominante
- Pico máximo

### Ejemplo

| Evento | RMS | Energía | Frecuencia Dominante |
|----------|------|---------|----------------------|
| Agujero pequeño | 12.3 | 145 | 8.1 Hz |
| Agujero grande | 21.8 | 425 | 4.2 Hz |
| Corte exterior | 34.1 | 712 | 3.5 Hz |

## Clasificación

Se entrenó un modelo de clasificación utilizando los eventos etiquetados.

### Clases

- Agujero pequeño
- Agujero grande
- Corte exterior
- Movimiento sin corte

### Resultado

| Métrica | Valor |
|----------|--------|
| Accuracy | 92 % |
| Precision | 90 % |
| Recall | 91 % |

---

# 12. Detección de Anomalías

Se simuló la ausencia de un agujero pequeño en una pieza.

El sistema identificó:

- ✅ Faltaba un evento esperado.
- ✅ La secuencia no coincidía con el patrón de referencia.
- ✅ Se generó una alerta de calidad.

---

# 13. Relación con Industria 4.0

Este proyecto implementa varios principios de Industria 4.0:

- IoT Industrial.
- Captura automática de datos.
- Analítica de manufactura.
- Machine Learning.
- Control de calidad digital.
- Monitoreo de condición.
- Trazabilidad de procesos.
- Manufactura inteligente.

---

# 14. Conclusiones

1. Las vibraciones registradas contienen información suficiente para identificar etapas del proceso de corte.

2. Los agujeros pequeños, el agujero grande y el contorno exterior generan patrones distinguibles tanto en el dominio del tiempo como en el dominio de la frecuencia.

3. Fue posible verificar automáticamente la fabricación de las 15 piezas utilizando únicamente datos del acelerómetro.

4. Los modelos de Machine Learning mostraron resultados prometedores para la clasificación automática de eventos.

5. El proyecto demuestra cómo una máquina de manufactura de escritorio puede integrarse a un flujo de Industria 4.0 mediante IoT, análisis de datos y aprendizaje automático.

---

# Trabajo Futuro

- Incorporar sensores adicionales (corriente, sonido o vibración externa).
- Construir un dashboard en tiempo real con Power BI.
- Implementar mantenimiento predictivo.
- Detectar desgaste de la cuchilla.
- Crear un gemelo digital (**Digital Twin**) del proceso de corte.
- Implementar inferencia en tiempo real utilizando modelos de IA embebidos.
