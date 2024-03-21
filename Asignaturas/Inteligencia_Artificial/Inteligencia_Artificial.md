# Apuntes
[[Clase1-Motivación2024.pdf]]
[[Clase2IA_2024.pdf]]


# Ayudantía

## Ayudantía 1
[[ayudantia1_2024.pdf]]
### Problema de la aspiradora robot
Supuestos (simplificación de la vida real):
1. Robot sabe donde esta y lo que ha visitado.
2. Problema de navegación esta resuelto
	1. Conoce la habitación.
	2. Ruta óptima.
3. Robot conoce sus dimensiones.
4. No hay casillas con derrame y suciedad.
5. Limpia 100% en una habitación.
6. Trapea.
7. Movimientos son siempre efectivos.
8. Discretizado

| 12 ? | 13 <- | 14 <- | 15 <- |
| ---- | ----- | ----- | ----- |
| 8 -> | 9 ->  | 10 -> | 11 ^  |
| 4 ^  | 5 <-  | 6 <-  | 7 <-  |
| 0 -> | 1 ->  | 2 ->  | 3 ^   |
Modelo de interacción entre agente y ambiente
![[img1.png]]

No es problema de camino mas corto. Va decidiendo a medida que se va moviendo. (agente inteligente)

> [!f(x)]
> Si esta sucio, entonces limpia.
> Si esta mojado, entonces trapea.
> Si esta limpio, entonces ve hacia donde puede moverse, escogemos dirección segun algoritmo de navegacion y luego mover.

| Percepción  | Acción    |
| ----------- | --------- |
| [0,Limpio]  | Mover Der |
| [1,Limpio]  | Mover Der |
| [2, Limpio] | Mover Der |
| [3, Sucio]  | Aspirar   |
| [3, Limpio] | Mover Ar  |
| [7, Mojado] | Trapear   |
### Otro problema

|              | Ajedrez | Starcraft              |
| ------------ | ------- | ---------------------- |
| Observable   | si      | parcialmente           |
| Determinista | si      | no (hay cosas al azar) |
| Episódico    | si      | no (es continuo)       |
| Estático     | no      | no                     |
| Discreto     | ?       | continuo               |
| Multiagente  | no      | ?                      |
![[img2.png]]