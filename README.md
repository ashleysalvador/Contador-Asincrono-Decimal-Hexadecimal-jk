# Contador asíncrono con selector de modo (decimal / hexadecimal)

Simulación en Proteus de un contador asíncrono de 4 bits con flip-flops JK
(74LS76). Con un selector puede contar de 0 a 9 (decimal) o de 0 a F
(hexadecimal).

![Circuito](captura-circuito.PNG)

## Componentes
- 4 flip-flops JK (74LS76) en modo toggle (J = K = 1)
- 2 compuertas AND (U1, U2) y 1 compuerta NOT (U3)
- Generador de pulsos como reloj
- Display de 7 segmentos (hexadecimal)
- 2 logic states como selectores de modo

## Funcionamiento
1. El reloj entra al primer flip-flop. La salida Q de cada uno es el reloj
   del siguiente, y cada etapa divide la frecuencia entre 2.
2. Las salidas D0 a D3 van al display.
3. U1 (AND) recibe D1 y D3. Estas dos salidas valen 1 a la vez por primera
   vez en el número 10 (1010).
4. U2 (AND) combina esa señal con el selector de modo.
5. U3 (NOT) invierte la salida de U2 y la envía al CLR (activo en bajo) de
   los cuatro flip-flops.
6. Con el selector de U2 en 1, al llegar al 10 el contador se reinicia a 0.
   Con el selector en 0, esa señal queda bloqueada y el contador llega hasta F.

## Modos de funcionamiento
| Selector izquierdo | Selector U2 | Comportamiento |
|---|---|---|
| 1 | 1 | Cuenta 0 → 9 y reinicia a 0 (decimal) |
| 1 | 0 | Cuenta 0 → F y reinicia a 0 (hexadecimal) |
| 0 | 0 | Se detiene en F (el preset está activo y no cuenta) |

El selector izquierdo alimenta J, K y los pines de preset (S) de los
flip-flops. Con 0 se anula el toggle y se fuerza Q = 1, por eso el valor
queda fijo en F.

## Secuencia (modo decimal)
| Pulso | D3 D2 D1 D0 | Display |
|-------|-------------|---------|
| 0 | 0 0 0 0 | 0 |
| 1 | 0 0 0 1 | 1 |
| 2 | 0 0 1 0 | 2 |
| 3 | 0 0 1 1 | 3 |
| 4 | 0 1 0 0 | 4 |
| 5 | 0 1 0 1 | 5 |
| 6 | 0 1 1 0 | 6 |
| 7 | 0 1 1 1 | 7 |
| 8 | 1 0 0 0 | 8 |
| 9 | 1 0 0 1 | 9 |
| 10 | 1 0 1 0 | Reinicio a 0 |

El estado 10 dura solo nanosegundos (el tiempo de propagación de las
compuertas), por lo que el display muestra de 0 a 9.
