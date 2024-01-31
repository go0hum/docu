# Configuración de wordpress
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

Configuracion de staging apuntando a [api.dev.bustrax.io](https://api.bustrax.io/)

## Goals

- Funcionamiento del wordpress apuntando a api.dev.bustrax.io

## Non-Goals

- No es necesario configurar todos los servicios solo el wordpress

## Background 

Hace años se creo un wordpress que se instalo en un godaddy y posteriormente fue migrado a traxi y despues de esto es necesario tener instalado esto en una maquina 
de desarrollo

## Overview

Los pasos para que apunten a la dirección correcta es la siguiente:

### 1 Identificar el archivo ejemplo app.js

![identificar archivo app.js](/images/edicion.png)


### 2 Compilamos con el comando:

``` bash
webpack
```

### copiamos el archivo generado y lo pegamos en la direccion

``` bash

/var/www/bustrax/bt/dist

```

## Consideraciones
Verificar que al cargar tenga las unidades de negocio y al guardar se guarda en la bd especifica.

## Metricas
Aun no tenemos herramientas de metricas

