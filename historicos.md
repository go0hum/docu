# Obtencion de Historicos
Link: [como llegar a este design doc](#)

Author(s): Humberto Santos

Status [Draft]

Ultima actualizacion: 2024-01-29

## Contenido

- Goals
- Non-Goals
- Background 
- Overview
- Detail Design 
    - Solucion 1
        - Frontend 
        - Backend 
    - Solucion 2
        - Frontend
        - Backend
- Consideraciones
- Métricas

## Objetivo

Obtención de historicos de los viajes que no se obtienen

## Goals

- verificar como se guardan los historicos en tiempo real 
- verificamos como se guardan los historicos cuando se hace un recalculo

## Non-Goals

- No generar interfaz grafica
- No modificar codigo existente del recalculo

## Background 

Hace años se creo el historico por una empresa externa

## Overview

Se hace un llamado para recalculo en el metodo

### tracker/eta/eta_audit.php

calculateTripAuditETA -> este se realiza cada minuto en el cron.php y llama la clase para recalculo y a la vez llena el historico

### tracker/eta/updates/routes.php


processRoute($route_id) -> este proceso se corre en un archivo de test


### tracker/eta/updates/trips.php

getTripsHistory($limit = 120, $maxTimeInSeconds = 500) -> genera las historias de los viajes y se corre een un archivo de test

getBulkTripsHistory($limit = 120, $maxTimeInSeconds = 500, $batchSize = 50) -> genera las historias de los viajes como bulk y se corre en las noches en el cron_night

### tracker/eta/updates/updates.php

Este se utiliza en como servicio en el archivo put_json.php en la opcion "updateTripHistory"

updateTripHistory($json, $user = '', $level = '') -> actualiza las historias de los viajes

### Otra forma de ejecucion de Historicos

Diario a cada minuto se esta ejecutando el cron_night.php 
La configuracion es a partir de las 12:00 AM todos los Lunes, Martes, Miercoles, Jueves y Viernes por un lapso de 5 horas.
Este archivo lo que hace es guardar la información de traffilog directamente a la bd 

``` sql
SELECT th.trip, t.gps_tracker
				FROM tracker.trips_history AS th
				LEFT JOIN tracker.trips AS t ON t.id = th.trip
				WHERE th.status = 0 AND TIMESTAMPADD(MINUTE, -666, NOW()) > TIMESTAMP(t.end_date, t.end_time)
				ORDER BY t.start_date ASC, th.trip DESC LIMIT 300
```
Posteriormente se pasan los parametros a la funcion 

``` php
bulkTripsHistoryTFL($batch, false, "", false);
```

Este lo que hace es:

``` php
$paramsArray = [];
for ($tripIndex=0; $tripIndex < count($trips); $tripIndex++) { 
    $trip = $trips[$tripIndex];
    list($params, $origin, $destiny, $route, $tsft, $tripData) = getHistoryParameters($trip, $group_car, 'tfl');
    array_push($paramsArray, $params);
}

bulkTrackingRequests('history', $paramsArray);
```
Cuando se crea un historico es importante saber que se valida que este a 150 metros del punto de salida y 50 metros del punto final de la ruta

El retorno de un punto la estructura que nos regresa el stop es el siguiente


``` json
[
    {
        "id": "1", // numero de punto
        "des": "VALLE DE AMATIZTA / VALLE AMATIZTA FRACC. VALLE ESMERALDA", // descripcion
        "ads": "EN CONTRA ESQUINA DE HERRERIA", // adicionales
        "lat": "20.773386", // latitud
        "lng": "-105.258442", // longitud
        "d": "0", // distancia al punto
        "times": [
            "05:40" // horario
        ],
        "tr": -537705335, // tiempo registrado en segundos desde departure
        "dsj": 176, // distancia a la que se encontro del punto
        "sj": 27, // punto donde se encontro
        "f": 1, // si fue encontrado
        "dr": 28002 // distancia de la ruta
    }
]
```

## Consideraciones
Se debe analizar bien los calculos que se llevan a cabo al momento de generar un punto y las distancias que actualmente tienen los historicos

## Metricas
Se pueden agregar phpunit para tener test que nos vayan generando y dando informacion mas confiable y datos que se esperan 
