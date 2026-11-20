# Analizador de espectro por bandas de octava en Python

Proyecto final de **Tratamiento Digital de Señales (TDS115)**, Escuela de Ingeniería Eléctrica, FIA, Universidad de El Salvador, ciclo II-2026.
Proyecto 8 de la guía: *Analizador de espectro por bandas*.

| Integrante | Carné | Usuario de GitHub |
| --- | --- | --- |
| José Roberto Martínez Pérez | MP21075 | @robertomartínez-code |
| Gerson Gustavo Escalante García | EG21017 | @[usuario] |
| Alex Francisco Deleon Reyes | DR20018 | @[usuario] |

## Qué hace

Divide una señal de audio en bloques de N = 4096 muestras (85,3 ms a 48 kHz) con traslape de 50 %, aplica una ventana (rectangular o Hann), calcula la FFT real de cada bloque y suma la energía de sus componentes en las **8 bandas de octava** de IEC 61260-1 (63 Hz a 8 kHz). El resultado son 8 niveles en dBFS por bloque, que se muestran como barras que suben y bajan al ritmo del sonido.

```
audio WAV (48 kHz, 16 bits, mono) → bloques N = 4096, 50 % → ventana → rfft → |X[k]|² por banda → L_b [dBFS] → barras
```

## Objetivos

1. Simular la FFT por bloques y la agrupación en 8 bandas: al menos 95 % de 40 tonos de prueba (5 por banda) asignados a la banda correcta.
2. Demostrar que la ventana de Hann reduce la fuga espectral en al menos 15 dB respecto a la rectangular.
3. Procesar al menos una grabación real y validarla con dos mediciones:
   - niveles por banda frente a un banco de filtros Butterworth de octava de scipy (diferencia menor a 1,5 dB);
   - frecuencia del pico dominante frente al analizador de espectro de Audacity (±1 componente, ±11,7 Hz).

## Especificaciones

| Parámetro | Valor |
| --- | --- |
| Frecuencia de muestreo | 48 kHz (Nyquist 24 kHz) |
| Cuantificación | 16 bits PCM, WAV mono sin compresión |
| Tamaño de bloque | N = 4096 (85,3 ms), traslape 50 % |
| Resolución | Δf = 11,72 Hz |
| Bandas | 63, 125, 250, 500, 1000, 2000, 4000 y 8000 Hz (44,2 Hz a 11,3 kHz) |
| Ventanas | rectangular y Hann |

## Estructura del repositorio

```
tds115-analizador-bandas/
├── README.md
├── requirements.txt
├── data/          grabaciones WAV y señales sintéticas
├── src/           módulo del analizador (bandas, FFT por bloques, referencias)
├── notebooks/     simulación y mediciones
├── figures/       gráficas para el artículo
└── docs/          propuesta, artículo y diapositivas en PDF
```

## Instalación

```bash
git clone https://github.com/[usuario]/tds115-analizador-bandas.git
cd tds115-analizador-bandas
python -m venv .venv
source .venv/bin/activate        # en Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

## Estado

- [x] Semana 1: propuesta, pitch y repositorio
- [ ] Semana 2: marco teórico, estado del arte y simulación en Python
- [ ] Semana 3: señales reales, dos mediciones, artículo final, video y defensa

## Cómo grabar las señales

- Grabar en WAV, 48 kHz, 16 bits, mono, sin compresión ni procesamiento (sin control automático de ganancia ni reducción de ruido).
- Grabar 5 s de silencio en el mismo lugar para medir el piso de ruido.
- Mantener los picos por debajo de −3 dBFS para evitar saturación.
- Si se graba con la laptop, desconectar el cargador para no captar el zumbido de 60 Hz.
- No grabar conversaciones de otras personas sin su consentimiento. Los equipos eléctricos se graban a distancia, sin tocarlos ni abrirlos.

## Referencias principales

- A. V. Oppenheim y R. W. Schafer, *Discrete-Time Signal Processing*, 3.ª ed., Pearson, 2010.
- F. J. Harris, "On the use of windows for harmonic analysis with the discrete Fourier transform," *Proc. IEEE*, vol. 66, no. 1, pp. 51–83, 1978, doi: 10.1109/PROC.1978.10837.
- IEC 61260-1:2014, *Electroacoustics – Octave-band and fractional-octave-band filters – Part 1: Specifications*.
- Documentación de [numpy.fft.rfft](https://numpy.org/doc/stable/reference/generated/numpy.fft.rfft.html), [scipy.signal](https://docs.scipy.org/doc/scipy/reference/signal.html) y [matplotlib.animation](https://matplotlib.org/stable/api/animation_api.html).
