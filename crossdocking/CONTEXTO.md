# Contexto: asignación de personal en conveyor de crossdocking

Para continuar en otra cuenta o con otra IA: pega este texto y adjunta `index.html`.

## Problema
Asignar colaboradores a posiciones de una línea de crossdocking, al inicio del turno y reajustando cada hora según la demanda.

## Roles
- **Inductor:** induce la paquetería a la cinta. Hay uno o más por lado (FBM y Colecta).
- **Pescador:** está junto a la cinta, toma de ella los paquetes de su código y los deja en la tolva.
- **Bipeador:** está al final de cada brazo de la tolva, con handheld. Escanea el paquete y arma el HU/pallet.
- **Repesca:** está al final de la cinta y recoge los paquetes que no fueron pescados.

## Estructura física
- Cinta vertical al centro. A la izquierda Inducción FBM, a la derecha Inducción Colecta (espejo).
- Cada lado tiene códigos (subcanales), hoy 9, que se pueden agregar o quitar.
- Cada código tiene una isla PRINCIPAL. El ESPEJO es la misma isla de ese código reforzada cuando sube el volumen.
- Orden desde la cinta hacia afuera: código con Pescadores, etiqueta ISLA, Bipeadores.

## Reglas de negocio acordadas
- Se ingresa el total del turno y las horas. El plan inicial reparte el total parejo por hora.
- El volumen por hora se puede corregir hora a hora. Si se deja vacío, usa el promedio.
- El % de demanda por código varía por hora. Hay un % base y se puede sobrescribir por hora.
- Si un código se satura, hay que mover gente a reforzarlo. La app muestra los movimientos respecto a la hora anterior.
- El espejo se activa solo cuando el volumen por hora de un código pasa un umbral. Ese umbral aún no está medido y es editable. Hoy es 600 paq/h, un número de ejemplo.
- Fuera de alcance por ahora: los modelos de dotación que ya tienen.

## Algoritmo (greedy)
1. Cubre primero un mínimo por prioridad: Inductores, Repesca, luego cada código (Pescador y Bipeador) del más al menos demandado.
2. Cada persona extra va a la posición con mayor uso (carga dividida por la capacidad de quienes ya están).
3. Si no alcanza el personal, marca qué posiciones quedan sin cubrir. Si el uso pasa de 100%, marca sobrecarga.
4. No minimiza movimientos entre horas ni considera distancias.

## Datos de ejemplo, NO reales
- Nombres de códigos tomados de sus fotos: ERMX, SRM1, SRM2, RMX1, SXI1, SPM1/SLT1, EPM2, HIBRIDO, SRC1.
- % de demanda por código, 24.000 paq de total, 30 colaboradores, capacidades por persona (Inductor 1200, Pescador 600, Bipeador 450, Repesca 400 paq/h), 3% a repesca y umbral de espejo 600: todos inventados.

## Estado
- MVP funcionando: `crossdocking/index.html` (un solo archivo, sin internet). Rama git `claude/crossdocking-staff-allocation-5pwwqu`.
- Layout estilo hoja de cálculo, una celda por colaborador con su rol. Cuando no se escriben nombres aparecen C01, C02...

## Pendiente
1. Cargar volumen y % reales por código de un turno reciente.
2. Medir capacidades reales por persona de cada rol.
3. Medir el umbral de espejo, o cambiarlo a un botón manual por código y hora.
4. Decidir si se quiere minimizar los movimientos entre horas, aunque el reparto sea menos equilibrado.
5. Decidir si los códigos extra (las celdas rojas de sus fotos, por ejemplo SLS1 y SNU1) deben modelarse.

## Cómo continuar con otra IA
Pega este archivo y adjunta `index.html`. Pide cambios sobre ese archivo. Pide que no entre en detalle técnico.
