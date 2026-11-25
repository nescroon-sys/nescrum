# NESCRUN — Vocal Suite All-in-One

Plugin **VST3 para Windows** (probado en el flujo de FL Studio) que junta en una sola ventana la cadena vocal completa para dejar una voz lista para un tema comercial.

## Descargar
Andá a **Releases** (columna derecha del repo) y bajá `NESCRUN-Windows-x64.zip`. Las instrucciones están en `INSTALAR.txt`.

Cada vez que se sube un cambio a `main`, GitHub Actions compila el plugin en Windows, lo valida con *pluginval* y publica un release nuevo.

## Qué hace (orden de la cadena)

| Módulo | Qué hace | Inspirado en el flujo de… |
|---|---|---|
| **TUNE** | Afinador en tiempo real (YIN + PSOLA, conserva formantes). **Natural**: corrige el centro de cada nota y conserva vibrato e inflexiones. **Hard**: autotune duro, efecto robot. Tonalidad + 8 escalas, Speed, Amount, Humanize. | Melodyne (afinación natural) / Auto-Tune |
| **CLEAN / AI** | Corte de graves 24 dB/oct, puerta de ruido suave, **Resonance AI** (supresor espectral dinámico: detecta y baja solo las resonancias que sobresalen, sobre todo la dureza de 1,5 a 6,5 kHz), de-esser de banda partida. | Vocal EQ "AI" / limpiadores de resonancias |
| **DYNAMICS** | Compresor con 4 caracteres (**Clean**, **FET** rápido y al frente, **Opto** suave de dos etapas, **Vari-Mu** cálido) + nivelador opto + compresión paralela. | Cadenas tipo CLA / Butch Vig |
| **TONE / SILK** | EQ de 5 bandas (Body, Mud, Mid, Presence, Air) con curva en pantalla + **Silk** (armónicos sedosos en agudos *Red* o calidez *Blue*). | Silk Vocal |
| **COLOR / BOOM** | Saturación Tape / Tube / Clip, y **BOOM**: pecho y pegada (compresión ascendente de 90–220 Hz + realce de ataque). | |
| **MOTION** | Doubler (dos tomas falsas L/R), Chorus 80s, Flanger, Phaser + Width. | Sonidos "Soda / Bocanada" |
| **DELAY** | Sincronizado al BPM de FL Studio, ping-pong, tono de cinta en el loop, *ducking* (los ecos bajan mientras cantás). | |
| **SPACE** | Reverb con pre-delay, filtro de graves y ducking. | |
| **MASTER** | Loudness (empuje) + limitador con anticipación y techo (por defecto −1 dB): suena fuerte sin pasar de 0 dB. Input, Output, Dry/Wet, medidores. | |
| **AUTO MIX** | Escucha ~7 s de voz y ajusta ganancia, puerta, EQ (contra un perfil de voz comercial), de-esser y compresión. | Mix assistants (Nectar, MixPilot) |

**41 presets de fábrica** por estilo: Básico, Trap, Latino, Pop, Rock (6 estilo Cerati: Soda, Bocanada, Ahí Vamos, Fuerza Natural, Siempre es Hoy, Dynamo; 3 estilo Charly García), Brit (Liam Gallagher solista, al frente y fino con dobles voces) y Productores (estilo Héctor Castillo y Tweety González). También aparecen en el menú de presets de FL Studio. Podés guardar los tuyos con **SAVE** (`Documentos\NESCRUN Presets`).

> Los presets "estilo X" son aproximaciones hechas de oído; NESCRUN no tiene relación con esos artistas ni con las marcas de plugins nombradas. Ningún algoritmo es una copia de esos productos.

## Compilar a mano (opcional)
```
cmake -B build -G "Visual Studio 17 2022" -A x64
cmake --build build --config Release --target NESCRUN_VST3
```
Requiere CMake 3.22+ y Visual Studio 2022. JUCE 8.0.4 se descarga solo.

## Licencia
Código propio sobre [JUCE](https://juce.com) (licencia AGPLv3 / comercial de JUCE). Para vender el plugin cerrado necesitás una licencia comercial de JUCE.
