# Recalculo
Link: [como llegar a este design doc](#)

Author(s): Humberto Santos

Status [Draft]

Ultima actualizacion: 2024-01-24

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

Es un proceso que recalcula todas las noches

Actualmente ya existe un sistema de recalculo pero no se implementa automaticamente

## Goals

- Es un proceso que corre todas las noches a las 00:00 donde busca viajes donde ninguna de sus paradas sean done y esten finalizadas
- Necesita generar Historicos

## Non-Goals

- No generar interfaz grafica
- No modificar codigo existente del recalculo

## Background 

Hace años se creo el recalculo basado en solicitud con el scrapping de traffilog

## Overview

Necesitamos una consulta que nos traiga todos los viajes a recalcular que no cumplan con la condicion de done

Por el momento obtenemos la informacion a traves del Car, fecha y horarios de inicio y fin con el scrapping

El scrapping de traffilog es [API](https://mx.traffilog.com/AppEngine_2_1/default.aspx) ahi obtenemos:
 - listado de puntos del auto en el tiempo que se solicita y la fecha
 - Duracion del token 24 horas y 12 horas si se reciclan
 - Solo 64 tokens se permiten al mismo tiempo

## Detail Design


## Solution 1
### Obtener listado de puntos de viaje en traffilog
A traves del endpoint *AppEngine_2_1/default.aspx* obtenemos los puntos del viaje con el cual vamos a comparar los puntos que
se agregaron en bustrax asi saber si el auto esta en la distancia y tiempo en el que se establecio

### Obtener listado de puntos de viaje en traffilog
A traves del endpoint *AppEngine_2_1/default.aspx* obtenemos los puntos del viaje con el cual vamos a comparar los puntos que
se agregaron en bustrax asi saber si el auto esta en la distancia y tiempo en el que se establecio


## Solution 2
### cambiar el api de traffilog
A traves del endpoint *https://api.traffilog.mx/clients/json* donde obtenemos los puntos pero dependiendo de los viajes de la unidad en el transcurso del dia, muchas veces el horario no coincide.

## Consideraciones
lenguaje php, verificar que el cron funcione correctamente y los correos se envien correctamente al inicio y fin

## Metricas
Es importante un log y tiempos para monitorear el proceso

