# Preguntas Finales para la Entrega de la Práctica

**Proyecto:** Estación de Trabajo de Alto Rendimiento para Edición 3D y Renderizado  
**Materia:** Diseño y Ensamblaje de Hardware con OpenSpec (SDD & ODD)  
**Fecha:** 23 de Noviembre de 2026  

---

## Pregunta 1: ¿Por qué es fundamental documentar las especificaciones físicas y eléctricas (`spec.md`) antes de proponer la compra de componentes de hardware?

### Respuesta Justificada:

En la ingeniería de hardware tradicional, es un error habitual proceder a la adquisición de componentes basándose únicamente en especificaciones lógicas o nominales (como la cantidad de núcleos de la CPU o los gigabytes de VRAM de la GPU) sin verificar minuciosamente sus restricciones de **interfaz física, envolvente térmica y demanda eléctrica transitoria**.

La aplicación de la metodología **Specification-Driven Development (SDD)** mediante el archivo formal `spec.md` es fundamental por las siguientes razones críticas:

1. **Prevención de Sobrecostes y Devoluciones por Incompatibilidad Mecánica (Clearance):**
   - En estaciones de trabajo de alto rendimiento, los disipadores de refrigeración líquida de 360 mm y las tarjetas gráficas modernas (como la RTX 4080 SUPER de 348 mm y 3.65 slots de grosor) poseen dimensiones colosales. Si no se documentan y cruzan las holguras en el `spec.md`, el técnico suele descubrir durante el montaje que el radiador colisiona con los disipadores térmicos de la memoria RAM, que la tarjeta gráfica no entra por topar con los ventiladores frontales, o que el cristal lateral no puede cerrarse sin doblar en exceso el conector de alimentación. Documentar estas restricciones evita paradas de producción, retrasos logísticos y costes por devolución de componentes abiertos.

2. **Garantía de Integridad Eléctrica y Prevención de Fallos Catastróficos (Power Spikes & 12V-2x6):**
   - Las tarjetas gráficas de última generación presentan picos transitorios de demanda (excursiones de potencia) de hasta el 200% de su TGP en intervalos de microsegundos. Una fuente de alimentación genérica o no conforme con el estándar **ATX 3.0** interpretará erróneamente estos picos como cortocircuitos mediante su circuito de protección contra sobrecorriente (OCP), provocando apagados repentinos del sistema a mitad de un renderizado de 10 horas.
   - Asimismo, el conector de 16 pines (**12V-2x6 / 12VHPWR**) requiere por especificación formal al menos **35 mm de recorrido recto** antes de cualquier curvatura y pines sensores activos para prevenir micro-desconexiones. Si no se exige y valida en el `spec.md` una anchura interior de chasis suficiente y un cable nativo sin adaptadores en pulpo, existe un riesgo real de recalentamiento del terminal y fusión por efecto Joule.

3. **Prevención del Estrangulamiento Térmico (Thermal Throttling) y Garantía del Rendimiento Contratado:**
   - Un procesador como el AMD Ryzen 9 7950X tiene un consumo sostenido en paquete de **230W (PPT)** cuando procesa renderizado 3D por buckets. Si la especificación no vincula de forma contractual el TDP/PPT con la capacidad de disipación del sistema de refrigeración y el caudal volumétrico (CFM) del chasis con presión positiva, el equipo alcanzará de inmediato los 95°C (TJMax), reduciendo automáticamente sus frecuencias turbo de 5.2 GHz a 4.6 GHz (pérdida directa de un 15-20% de rendimiento por el que el cliente ha pagado).

4. **El Documento `spec.md` como Contrato Técnico entre Cliente, Proveedor y Ensamblador:**
   - Al igual que en el desarrollo de software el código se valida frente a pruebas unitarias basadas en especificaciones, en el ensamblaje de hardware el documento `spec.md` define los criterios objetivos de aceptación (criterios *SHALL/MUST* y escenarios *WHEN/THEN*). Cualquier discrepancia entre lo presupuestado y lo entregado queda respaldada por un estándar técnico verificable.

---

## Pregunta 2: ¿Cómo os habéis repartido los subsistemas de hardware en el equipo y qué problemas de compatibilidad física (espacio, conectores) habéis tenido que resolver conjuntamente?

### Respuesta Justificada:

### A. Reparto de Subsistemas mediante Organic Development (ODD):

Para optimizar el tiempo de desarrollo e investigar en profundidad cada componente sin duplicar esfuerzos, el equipo dividió el proyecto en dos subsistemas principales claramente delimitados mediante la metodología **ODD (Organic Development)**:

- **Alumno A — Especialista en el Subsistema de Proceso y Energía:**
  - *Área de responsabilidad:* Unidad Central de Procesamiento (**CPU: AMD Ryzen 9 7950X**), Placa Base (**Motherboard: ASUS TUF GAMING B650-PLUS WIFI**), Memoria del Sistema (**RAM: Corsair Vengeance 64GB DDR5 6000MHz CL30 EXPO**) y Sistema de Alimentación Eléctrica (**PSU: Corsair RM850x SHIFT ATX 3.0**).
  - *Foco de investigación:* Compatibilidad del zócalo AM5, robustez térmica del VRM de 12+2 fases a 60A para suministrar 160A continuos, integridad de señal en topología de memoria Daisy-Chain (2x32GB vs 4x16GB) y cálculo riguroso del raíl único de +12V (70.8A) con margen del 22.6% frente a transitorios.

- **Alumno B — Especialista en el Subsistema de Gráficos, Almacenamiento y Chasis:**
  - *Área de responsabilidad:* Tarjeta Gráfica (**GPU: ASUS TUF Gaming GeForce RTX 4080 SUPER 16GB**), Almacenamiento de Estado Sólido (**SSDs NVMe: Samsung 990 Pro 1TB + Kingston KC3000 2TB**), Gabinete (**Chasis: Fractal Design Meshify 2**) y Refrigeración Integral (**AIO Liquid Freezer III 360 + Ventiladores de Chasis**).
  - *Foco de investigación:* Dimensiones y momento flector de la GPU (348 mm / 1.97 kg), disipación pasiva bilateral en las bahías M.2, tolerancias milimétricas en el montaje del radiador superior de 65 mm y balance de caudal de aire (116 CFM) para mantener presión estática positiva.

---

### B. Problemas de Compatibilidad Física y Eléctrica Resueltos Conjuntamente:

Durante la fase de integración (Paso 4 y Paso 5), el equipo detectó cuatro puntos de fricción técnica y potenciales incompatibilidades que fueron resueltas mediante consenso de ingeniería:

#### 1. Conflicto entre el Grosor del Radiador AIO (65 mm) y la Altura de la Memoria RAM:
- **Problema detectado:** El disipador seleccionado por el Alumno B (ARCTIC Liquid Freezer III 360) cuenta con un radiador inusualmente grueso de 38 mm más 27 mm de ventiladores (65 mm en total). En la mayoría de cajas semitorre estándar, un grosor de 65 mm colisiona frontalmente con el borde superior de la placa base y los módulos de memoria RAM seleccionados por el Alumno A.
- **Resolución conjunta:** El Alumno A verificó que la memoria RAM seleccionada (Corsair Vengeance DDR5) tuviera un perfil bajo (**35 mm de altura**, en contraste con módulos RGB de 44-52 mm). Paralelamente, el Alumno B verificó en los planos técnicos del chasis Fractal Meshify 2 que los raíles de anclaje de 360 mm en el techo están desplazados hacia el panel lateral exterior (offset mounting), dejando más de 70 mm de despeje vertical sobre el PCB de la placa base. La incompatibilidad quedó 100% resuelta con holgura.

#### 2. Holgura de la GPU (348 mm) frente al Flujo de Aire Frontal:
- **Problema detectado:** La GPU seleccionada por el Alumno B (ASUS TUF RTX 4080 SUPER) tiene una longitud de casi 35 cm. Si el chasis se montara con bandejas para discos duros mecánicos de 3.5" (Storage Layout), el espacio máximo se reducía a 315 mm, haciendo imposible la instalación física de la GPU.
- **Resolución conjunta:** Ambos alumnos acordaron prescindir de discos mecánicos HDD lentos y adoptar una arquitectura 100% NVMe en placa. Esto permitió configurar el chasis Fractal Meshify 2 en **Open Layout**, ampliando la longitud libre para la GPU hasta **442 mm** (dejando **93.8 mm de espacio libre** entre la punta de la GPU y los ventiladores frontales), permitiendo además la instalación del brazo telescópico antipandeo (Anti-Sag Bracket) provisto en la caja de la GPU.

#### 3. Radio de Curvatura y Despeje Lateral del Cable de Potencia 12V-2x6:
- **Problema detectado:** La tarjeta gráfica tiene una anchura de 150 mm y sitúa su conector de 16 pines en el lomo superior. El Alumno A advirtió que el estándar PCIe exige al menos 35 mm de cable recto sin deflexión para evitar tensiones en los terminales metálicos.
- **Resolución conjunta:** El Alumno B comprobó la cota de anchura interior del chasis Fractal Meshify 2 (admite disipadores de hasta 185 mm de altura). Al restar los 150 mm de la GPU desde la placa, quedan **52 mm de holgura libre** hasta el cristal templado lateral. Además, el Alumno A seleccionó la fuente Corsair RM850x SHIFT, que incluye un cable nativo 12V-2x6 blindado con funda mallada ultraflexible y pines sensores de longitud reducida (evitando el uso de adaptadores rígidos en pulpo).

#### 4. Asignación de Ranuras PCIe y Disipadores M.2 sin Asfixia Térmica:
- **Problema detectado:** La GPU de 3.65 slots (72.6 mm de grosor) cubre prácticamente tres ranuras y media en la placa base, generando una zona de calor intenso justo encima de los puertos M.2 secundarios.
- **Resolución conjunta:** El Alumno A ubicó el SSD primario (Samsung 990 Pro) en la ranura superior M.2_1 (conectada directamente a las líneas de la CPU, por encima de la ranura PCIe x16 de la GPU y alejada de su calor residual). El SSD secundario (Kingston KC3000) se ubicó en la ranura inferior M.2_3, donde aprovecha tanto el disipador macizo de la placa base como la corriente de aire frío inyectada por el ventilador frontal inferior del chasis.

Gracias a este trabajo colaborativo y la validación cruzada mediante **OpenSpec**, todas las posibles colisiones físicas y eléctricas se detectaron y subsanaron antes de emitir la propuesta económica definitiva.
