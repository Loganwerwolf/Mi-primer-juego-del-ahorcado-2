Juego para dos jugadores
Al iniciar la aplicación se solicitan los nombres de los dos participantes.
Cada jugador jugará por turnos.

Palabras aleatorias
El sistema selecciona una palabra al azar de una lista predefinida:
PALABRAS = [
    "abismo",
    "sangre",
    "infierno",
    "oscuridad",
    "demonio",
    "metal",
    "calavera",
    "tinieblas",
    "tormento",
    "sacrificio",
    "maldicion",
    "sepulcro"
]
Cada nueva partida genera automáticamente una palabra diferente.

Temporizador
Cada ronda tiene un límite de:
TIEMPO_LIMITE = 60
Si el tiempo llega a cero antes de adivinar la palabra:
El jugador pierde.
Se registra el resultado.
Se cambia el turno automáticamente.

Sistema de errores
El jugador dispone de:

MAX_ERRORES = 6
Cada letra incorrecta aumenta el contador de errores y se dibuja una nueva parte del ahorcado.
Cuando se alcanzan los 6 errores:
La partida termina.
Se revela la palabra.
Se cambia el turno.

Sistema de puntuación
Las puntuaciones se actualizan en tiempo real.
Aciertos
+10 puntos
Error
-5 puntos
Victoria
+50 puntos
Esto genera una competencia continua entre los dos jugadores.

Historial de partidas
Cada partida se guarda automáticamente en un archivo:

historial_ahorcado.json
La información almacenada incluye:
Jugador
Resultado
Palabra utilizada
Fecha y hora
Ejemplo:

{
    "jugador": "Carlos",
    "resultado": "GANÓ",
    "palabra": "abismo",
    "fecha": "05/06/2026 18:20:45"
}

Interfaz Gráfica
La interfaz está construida completamente con Tkinter.

Elementos principales:
Título del juego.
Indicador del turno actual.
Temporizador.
Palabra oculta.
Contador de errores.
Dibujo del ahorcado.
Marcador de puntos.
Teclado interactivo.
Botón para consultar el historial.

Funcionamiento General
1. Inicio
Al abrir la aplicación:

iniciar_jugadores()
Se solicitan los nombres de ambos jugadores.

Nueva partida
La función:
nueva_partida()
se encarga de:
Seleccionar una palabra aleatoria.
Reiniciar errores.
Reiniciar tiempo.
Habilitar todas las letras.
Actualizar la pantalla.

Selección de letras
Cuando el usuario pulsa una letra:
probar_letra(letra)

el programa verifica si la letra existe en la palabra.
Si existe
Se descubre en pantalla.
Se suman puntos.

Si no existe
Se suma un error.
Se descuentan puntos.
El botón cambia de color.

Verificación de victoria o derrota
La función:
verificar_estado()
comprueba:
Si la palabra fue completada.
Si se alcanzó el límite de errores.
Según el resultado, se muestra un mensaje y comienza una nueva ronda.

Temporizador
La función:
temporizador()
reduce un segundo cada vez mediante:
root.after(1000, temporizador)
Cuando el contador llega a cero:
La ronda termina.
Se registra el resultado.
Se cambia el turno.

Estructura de Archivos
Proyecto/
│
├── ahorcado.py
├── historial_ahorcado.json
└── README.md
Tecnologías Utilizadas
Python

Lenguaje principal del proyecto.

Tkinter
Utilizado para crear toda la interfaz gráfica.

JSON
Utilizado para almacenar el historial de partidas.

Módulos estándar
import tkinter as tk
import random
import json
import os
import time
No requiere librerías externas.

Conclusión
Ahorcado del Abismo es una adaptación del clásico juego del ahorcado, que combina una estética oscura con mecánicas adicionales como puntuaciones, temporizador e historial de partidas. El proyecto demuestra el uso práctico de interfaces gráficas, manejo de archivos y programación orientada a eventos en Python, ofreciendo una experiencia más dinámica y entretenida para los jugadores.
