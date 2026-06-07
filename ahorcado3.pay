import tkinter as tk
from tkinter import messagebox, simpledialog
import random
import json
import os
import time

# =====================================================
# CONFIGURACIÓN
# =====================================================

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

COLOR_FONDO = "#0d0d0d"
COLOR_PANEL = "#1a1a1a"
COLOR_TEXTO = "#d0d0d0"
COLOR_ROJO = "#8b0000"
COLOR_VERDE = "#1f8b4c"
COLOR_BOTON = "#2a2a2a"
COLOR_AMARILLO = "#d4af37"

MAX_ERRORES = 6
TIEMPO_LIMITE = 60
ARCHIVO_HISTORIAL = "historial_ahorcado.json"

# =====================================================
# VARIABLES
# =====================================================

palabra = ""
letras_correctas = []
errores = 0
tiempo_restante = TIEMPO_LIMITE
juego_terminado = False

jugador1 = ""
jugador2 = ""
turno = 1

puntuaciones = {}

# =====================================================
# FUNCIONES
# =====================================================

def iniciar_jugadores():

    global jugador1, jugador2, puntuaciones

    jugador1 = simpledialog.askstring(
        "Jugador 1",
        "Nombre del Jugador 1:"
    )

    jugador2 = simpledialog.askstring(
        "Jugador 2",
        "Nombre del Jugador 2:"
    )

    if not jugador1:
        jugador1 = "Jugador1"

    if not jugador2:
        jugador2 = "Jugador2"

    puntuaciones = {
        jugador1: 0,
        jugador2: 0
    }

    actualizar_score()


def jugador_actual():

    if turno == 1:
        return jugador1
    else:
        return jugador2


def cambiar_turno():

    global turno

    if turno == 1:
        turno = 2
    else:
        turno = 1


def nueva_partida():

    global palabra
    global letras_correctas
    global errores
    global tiempo_restante
    global juego_terminado

    letras_correctas = []
    errores = 0
    tiempo_restante = TIEMPO_LIMITE
    juego_terminado = False

    palabra = random.choice(PALABRAS)

    for boton in botones.values():

        boton.config(
            state="normal",
            bg=COLOR_BOTON,
            fg=COLOR_TEXTO
        )

    actualizar_pantalla()
    temporizador()


def mostrar_palabra():

    texto = ""

    for letra in palabra:

        if letra in letras_correctas:
            texto += letra.upper() + " "
        else:
            texto += "_ "

    return texto


def actualizar_pantalla():

    label_palabra.config(
        text=mostrar_palabra()
    )

    label_errores.config(
        text=f"Errores: {errores}/{MAX_ERRORES}"
    )

    label_turno.config(
        text=f"Turno: {jugador_actual()}"
    )

    label_tiempo.config(
        text=f"Tiempo: {tiempo_restante}s"
    )

    dibujar_ahorcado()


def probar_letra(letra):

    global errores

    if juego_terminado:
        return

    botones[letra]["state"] = "disabled"

    if letra in palabra:

        letras_correctas.append(letra)

        puntuaciones[jugador_actual()] += 10

    else:

        errores += 1

        puntuaciones[jugador_actual()] -= 5

        botones[letra].config(
            bg=COLOR_ROJO
        )

    actualizar_score()
    actualizar_pantalla()

    verificar_estado()


def verificar_estado():

    global juego_terminado

    # GANAR
    if all(letra in letras_correctas for letra in palabra):

        juego_terminado = True

        ganador = jugador_actual()

        puntuaciones[ganador] += 50

        guardar_historial(
            ganador,
            "GANÓ"
        )

        actualizar_score()

        messagebox.showinfo(
            "☠ VICTORIA ☠",
            f"{ganador} adivinó la palabra:\n\n{palabra.upper()}"
        )

        cambiar_turno()

        root.after(
            1500,
            nueva_partida
        )

    # PERDER
    elif errores >= MAX_ERRORES:

        juego_terminado = True

        guardar_historial(
            jugador_actual(),
            "PERDIÓ"
        )

        messagebox.showerror(
            "☠ DERROTA ☠",
            f"La palabra era:\n\n{palabra.upper()}"
        )

        cambiar_turno()

        root.after(
            1500,
            nueva_partida
        )


def dibujar_ahorcado():

    canvas.delete("all")

    canvas.create_line(
        40, 200, 180, 200,
        fill="white",
        width=4
    )

    canvas.create_line(
        70, 200, 70, 40,
        fill="white",
        width=4
    )

    canvas.create_line(
        70, 40, 130, 40,
        fill="white",
        width=4
    )

    canvas.create_line(
        130, 40, 130, 60,
        fill="white",
        width=4
    )

    if errores >= 1:
        canvas.create_oval(
            110, 60, 150, 100,
            outline="red",
            width=3
        )

    if errores >= 2:
        canvas.create_line(
            130, 100, 130, 140,
            fill="red",
            width=3
        )

    if errores >= 3:
        canvas.create_line(
            130, 110, 110, 125,
            fill="red",
            width=3
        )

    if errores >= 4:
        canvas.create_line(
            130, 110, 150, 125,
            fill="red",
            width=3
        )

    if errores >= 5:
        canvas.create_line(
            130, 140, 110, 165,
            fill="red",
            width=3
        )

    if errores >= 6:
        canvas.create_line(
            130, 140, 150, 165,
            fill="red",
            width=3
        )


def temporizador():

    global tiempo_restante
    global juego_terminado

    if juego_terminado:
        return

    if tiempo_restante > 0:

        tiempo_restante -= 1

        label_tiempo.config(
            text=f"Tiempo: {tiempo_restante}s"
        )

        root.after(
            1000,
            temporizador
        )

    else:

        juego_terminado = True

        guardar_historial(
            jugador_actual(),
            "TIEMPO AGOTADO"
        )

        messagebox.showerror(
            "☠ TIEMPO AGOTADO ☠",
            "Las sombras consumieron la partida."
        )

        cambiar_turno()

        root.after(
            1500,
            nueva_partida
        )


def actualizar_score():

    texto = ""

    for jugador, puntos in puntuaciones.items():

        texto += f"{jugador}: {puntos} pts\n"

    label_score.config(
        text=texto
    )


def guardar_historial(ganador, resultado):

    partida = {
        "jugador": ganador,
        "resultado": resultado,
        "palabra": palabra,
        "fecha": time.strftime("%d/%m/%Y %H:%M:%S")
    }

    historial = []

    if os.path.exists(ARCHIVO_HISTORIAL):

        with open(
            ARCHIVO_HISTORIAL,
            "r"
        ) as archivo:

            try:
                historial = json.load(archivo)
            except:
                historial = []

    historial.append(partida)

    with open(
        ARCHIVO_HISTORIAL,
        "w"
    ) as archivo:

        json.dump(
            historial,
            archivo,
            indent=4
        )


def mostrar_historial():

    if not os.path.exists(ARCHIVO_HISTORIAL):

        messagebox.showinfo(
            "Historial",
            "No existen partidas guardadas."
        )

        return

    with open(
        ARCHIVO_HISTORIAL,
        "r"
    ) as archivo:

        historial = json.load(archivo)

    texto = ""

    for partida in historial[-10:]:

        texto += (
            f"{partida['fecha']}\n"
            f"Jugador: {partida['jugador']}\n"
            f"Resultado: {partida['resultado']}\n"
            f"Palabra: {partida['palabra']}\n"
            f"----------------------\n"
        )

    ventana = tk.Toplevel(root)

    ventana.title("Historial")
    ventana.geometry("400x400")
    ventana.config(bg=COLOR_FONDO)

    texto_historial = tk.Text(
        ventana,
        bg="black",
        fg="red",
        font=("Consolas", 10)
    )

    texto_historial.pack(
        fill="both",
        expand=True
    )

    texto_historial.insert(
        "1.0",
        texto
    )

# =====================================================
# INTERFAZ
# =====================================================

root = tk.Tk()

root.title("☠ AHORCADO DEL ABISMO ☠")
root.geometry("700x720")
root.config(bg=COLOR_FONDO)

titulo = tk.Label(
    root,
    text="☠ AHORCADO DEL ABISMO ☠",
    font=("Old English Text MT", 20, "bold"),
    bg=COLOR_FONDO,
    fg="red"
)

titulo.pack(pady=10)

label_turno = tk.Label(
    root,
    text="",
    font=("Consolas", 14),
    bg=COLOR_FONDO,
    fg=COLOR_AMARILLO
)

label_turno.pack()

label_tiempo = tk.Label(
    root,
    text="",
    font=("Consolas", 14),
    bg=COLOR_FONDO,
    fg="cyan"
)

label_tiempo.pack()

label_palabra = tk.Label(
    root,
    text="",
    font=("Consolas", 26, "bold"),
    bg=COLOR_FONDO,
    fg="white"
)

label_palabra.pack(pady=15)

label_errores = tk.Label(
    root,
    text="",
    font=("Consolas", 14),
    bg=COLOR_FONDO,
    fg="orange"
)

label_errores.pack()

canvas = tk.Canvas(
    root,
    width=220,
    height=220,
    bg="black",
    highlightbackground="red",
    highlightthickness=3
)

canvas.pack(pady=15)

label_score = tk.Label(
    root,
    text="",
    font=("Consolas", 12),
    bg=COLOR_FONDO,
    fg="#00ff99"
)

label_score.pack()

frame_botones = tk.Frame(
    root,
    bg=COLOR_FONDO
)

frame_botones.pack(pady=15)

botones = {}

fila = 0
columna = 0

for letra in "ABCDEFGHIJKLMNOPQRSTUVWXYZ":

    boton = tk.Button(
        frame_botones,
        text=letra,
        width=3,
        height=1,
        font=("Consolas", 10, "bold"),
        bg=COLOR_BOTON,
        fg=COLOR_TEXTO,
        activebackground=COLOR_ROJO,
        relief="raised",
        bd=3,
        command=lambda l=letra.lower(): probar_letra(l)
    )

    boton.grid(
        row=fila,
        column=columna,
        padx=2,
        pady=2
    )

    botones[letra.lower()] = boton

    columna += 1

    if columna > 6:
        columna = 0
        fila += 1

frame_inferior = tk.Frame(
    root,
    bg=COLOR_FONDO
)

frame_inferior.pack(pady=15)

btn_historial = tk.Button(
    frame_inferior,
    text="Historial",
    font=("Consolas", 12, "bold"),
    bg=COLOR_ROJO,
    fg="white",
    padx=15,
    command=mostrar_historial
)

btn_historial.grid(
    row=0,
    column=0,
    padx=10
)

# =====================================================
# INICIO
# =====================================================

iniciar_jugadores()
nueva_partida()

root.mainloop()