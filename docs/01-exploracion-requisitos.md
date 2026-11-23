# Paso 2: Exploración de Requisitos de Hardware con `/opsx:explore`

**Proyecto:** Estación de Trabajo de Alto Rendimiento para Edición de Vídeo y Renderizado 3D  
**Metodología:** Specification-Driven Development (SDD) & Organic Development (ODD)  
**Entorno de Trabajo:** OpenSpec v2.1.0  
**Fecha:** 23 de Noviembre de 2026  

---

## 1. Contexto y Prompt de Exploración

En esta primera fase de la metodología **SDD (Specification-Driven Development)**, antes de redactar código o comprometer presupuesto en compras de componentes, se realiza una sesión interactiva de exploración mediante el comando `/opsx:explore` para definir la arquitectura del sistema, descubrir cuellos de botella y mapear las restricciones físicas, térmicas y eléctricas.

### Prompt de Exploración ejecutado:
> *"Quiero diseñar la configuración de hardware para un PC de estación de trabajo enfocado a edición 3D. Necesito definir la CPU, placa base, refrigeración, memoria RAM, tarjeta gráfica, almacenamiento NVMe y fuente de alimentación, asegurando que no haya incompatibilidades de zócalo, espacio en caja o potencia eléctrica."*

---

## 2. Análisis Crítico de la Respuesta de la IA y Componentes Propuestos

A partir de la exploración técnica, se evalúan las cargas de trabajo objetivo:
- **Modelado y Renderizado 3D:** Blender (Cycles con backend OptiX), Autodesk Maya, 3ds Max, Chaos V-Ray y Unreal Engine 5.4 (Lumen y Nanite en tiempo real).
- **Postproducción y Edición de Vídeo:** DaVinci Resolve Studio (graduación de color acelerada por GPU, pistas Fusion), Adobe Premiere Pro y After Effects.

### Propuesta Base de Componentes:

| Componente | Modelo Propuesto | Especificaciones Clave | Justificación Técnica |
| :--- | :--- | :--- | :--- |
| **CPU (Procesador)** | **AMD Ryzen 9 7950X** | 16 núcleos / 32 hilos, 4.5 GHz base / 5.7 GHz boost, 64 MB L3 Cache, Socket AM5, TDP 170W (PPT 230W) | Máximo rendimiento multihilo en renderizado de CPU (V-Ray / Cinebench) combinado con alta frecuencia mononúcleo para viewport y manipulación geométrica en 3D interactivo. Plataforma AM5 con soporte PCIe 5.0 y DDR5 de largo ciclo de vida. |
| **Placa Base** | **ASUS TUF GAMING B650-PLUS WIFI** | Chipset AMD B650, Socket AM5, VRM 12+2 fases de potencia (60A), 1x PCIe 5.0 M.2 + 2x PCIe 4.0 M.2, 2.5 GbE LAN, Wi-Fi 6, USB-C 20 Gbps | VRM robusto refrigerado por disipadores de aluminio sobredimensionados para soportar los 230W continuados del Ryzen 9 sin estrangulamiento térmico (thermal throttling). Excelente relación calidad/precio frente a X670E sin sacrificar estabilidad. |
| **Memoria RAM** | **Corsair Vengeance DDR5 64 GB (2x32 GB) 6000 MHz CL30 AMD EXPO** | DDR5, Kit de 2 módulos de 32 GB (Dual Channel), 6000 MT/s, Latencia CL30-36-36-76 @ 1.35V, Perfil AMD EXPO | 64 GB es el estándar mínimo recomendado para escenas 3D densas y búferes de previsualización 4K en RAM. La configuración 2x32 GB preserva el modo 1:1 del controlador de memoria (UCLK=MCLK) en Ryzen a 6000 MHz, garantizando máxima estabilidad y mínima latencia. Módulos de perfil bajo (35 mm) para evitar colisiones. |
| **Tarjeta Gráfica (GPU)** | **NVIDIA GeForce RTX 4080 SUPER 16 GB GDDR6X** (Ej. ASUS TUF Gaming OC Edition) | 10.240 CUDA Cores, 320 Tensor Cores (4ª Gen), 80 RT Cores (3ª Gen), 16 GB GDDR6X (bus 256 bits, 736 GB/s), TGP 320W | Los núcleos Tensor y RT aceleran de forma exponencial el renderizado con NVIDIA OptiX en Blender Cycles (reducción de tiempos de hasta un 600% vs CPU) y aceleran los efectos neuronales (Magic Mask, Super Scale) en DaVinci Resolve. 16 GB de VRAM son indispensables para texturas 4K/8K sin desbordar a la RAM del sistema (evitando caída drástica de FPS). |
| **Almacenamiento NVMe (Sistema y Apps)** | **Samsung 990 PRO 1 TB NVMe M.2** | PCIe Gen 4.0 x4, Controlador Pascal, DRAM Cache LPDDR4 1 GB, Lectura hasta 7.450 MB/s, Escritura hasta 6.900 MB/s, 600 TBW | Unidad dedicada exclusivamente al Sistema Operativo (Windows 11 Pro / Linux) y software de trabajo, con caché DRAM para lecturas de arranque instantáneas y multitarea fluida. |
| **Almacenamiento NVMe (Scratch / Proyectos)** | **Kingston KC3000 2 TB NVMe M.2** | PCIe Gen 4.0 x4, Controlador Phison E18, DRAM Cache DDR4 2 GB, TLC 3D 176 capas, Lectura 7.000 MB/s, Escritura 7.000 MB/s, 1.600 TBW | Disco de trabajo activo para archivos de proyecto, caché de renderizado y metraje RAW. Destaca por su alta durabilidad (1.600 TBW), crucial para escrituras masivas diarias en renderizado de secuencias de frames. |
| **Fuente de Alimentación (PSU)** | **Corsair RM850x SHIFT 850W 80 Plus Gold Modular (ATX 3.0 / PCIe 5.0)** | 850 Vatios, Certificación 80 Plus Gold y Cybenetics Gold, Condensadores 100% japoneses 105°C, Estándar ATX 3.0 con cable nativo 12V-2x6 (12VHPWR), Raíl único +12V de 70.8A | Estándar ATX 3.0 obligatorio para absorber transitorios de carga de hasta el 200% del TGP de la GPU. Raíl único de 12V capaz de entregar 849.6W en continua. Sistema de conectores laterales SHIFT para simplificar la gestión de cableado. |
| **Refrigeración CPU** | **Liquid Freezer III 360 de ARCTIC** | Refrigeración líquida todo en uno (AIO) de 360 mm, radiador de 38 mm de grosor, 3x ventiladores P12 PWM PST, bomba PWM con ventilador VRM integrado | Rendimiento térmico líder del mercado para disipar cargas continuas de más de 250W. Su radiador extra grueso (38 mm frente a los 27 mm estándar) y el ventilador dedicado a la zona del VRM garantizan que la placa y la CPU operen por debajo de los 75°C en render sostenido. |
| **Chasis / Caja** | **Fractal Design Meshify 2 Black Solid / Dark Tint** | Formato Semi-Torre ATX espaciosa, frontal de malla de alta permeabilidad, soporte para radiadores superiores hasta 360 mm o frontales hasta 420 mm, longitud máxima de GPU hasta 467 mm (o 315 mm con rack de discos, 451 mm en layout abierto) | Garantiza presión positiva de aire, excelente insonorización, filtros antipolvo de fácil extracción y espacio holgado para el grosor de 65 mm (radiador 38mm + ventilador 27mm) del AIO sin colisionar con la placa base ni con la RAM. |

---

## 3. Análisis de Refrigeración: ¿Líquida (AIO) o por Aire?

### Advertencia Técnica:
La CPU seleccionada (**AMD Ryzen 9 7950X**) tiene un comportamiento térmico diseñado para exprimir todo el margen térmico disponible hasta los **95°C (TJMax)**. En tareas de renderizado 3D por CPU (como renderizado por buckets en Blender o V-Ray), los 16 núcleos operan al 100% de carga de forma ininterrumpida durante horas, con un consumo de potencia en paquete (**PPT**) constante de **230 W**.

### Comparativa:

| Criterio | Refrigeración Líquida AIO (360 mm) | Refrigeración por Aire Doble Torre (ej. Noctua NH-D15 / Peerless Assassin 120) |
| :--- | :--- | :--- |
| **Capacidad de Disipación Térmica** | Excelente (>280W-300W de disipación térmica continua). Radiador de 360 mm con gran superficie de aletas. | Muy buena (hasta 220W-240W continuos), pero se aproxima a su límite térmico en cargas de 230W continuas. |
| **Temperatura en Render Sostenido** | Mantiene la CPU entre 72°C y 82°C, evitando el thermal throttling y permitiendo frecuencias sostenidas de 5.1-5.3 GHz en todos los núcleos. | La CPU alcanzará rápidamente los 90°C-95°C, activando la reducción automática de frecuencia boost para proteger el silicio (pérdida de un 4-7% de rendimiento). |
| **Clearance / Compatibilidad Física con RAM** | **100% libre en la zona del zócalo.** El bloque de agua sobre la CPU tiene un perfil muy bajo (<65 mm). No obstaculiza las ranuras DIMM de la memoria RAM ni dificulta la extracción de módulos. | Los disipadores de doble torre tienen un ancho de ~140-150 mm y el ventilador frontal cuelga directamente sobre los zócalos de memoria RAM. Si los módulos de RAM superan los 32-35 mm de altura, el ventilador debe elevarse, aumentando la altura total del cooler y chocando contra el panel lateral de la caja. |
| **Espacio y Tensión Mecánica en Placa Base** | El peso se distribuye en el chasis (el radiador va atornillado al techo de la caja). No ejerce palanca ni tensión sobre el PCB de la placa base. | Pesa entre 1.000 g y 1.400 g suspendidos en voladizo sobre el socket AM5, lo que puede causar torsión mecánica en transportes o vibraciones. |
| **Mantenimiento y Durabilidad** | Vida útil típica de la bomba: 5-7 años. Riesgo residual mínimo de fallo de bomba o microporosidad del líquido a muy largo plazo. | Fiabilidad casi infinita (sólo requiere cambiar ventiladores si fallan). Mantenimiento limitado a soplado de polvo. |

### Conclusión del Asesoramiento:
Para un perfil profesional de renderizado 3D donde la CPU estará al 100% durante horas, **la refrigeración líquida AIO de 360 mm montada en el techo (Top Exhaust) es la opción técnicamente recomendada**. Proporciona el margen térmico necesario para mantener las frecuencias turbo máximas, elimina completamente el problema de interferencia con los módulos de RAM y evacúa el calor del procesador directamente fuera del chasis sin calentar la tarjeta gráfica.

---

## 4. Riesgos Técnicos en el Consumo Energético y Selección de la Fuente de Alimentación

### A. Excursiones de Potencia Transitoria (Power Spikes)
Las tarjetas gráficas modernas basadas en la arquitectura NVIDIA Ada Lovelace y Hopper presentan transitorios de potencia ultrarrápidos (picos de milisegundos) donde el consumo instantáneo puede dispararse entre un **150% y un 200% del TGP nominal** (picos de hasta 480W-640W sólo en la GPU).
- Si se utiliza una fuente de alimentación antigua con estándar ATX 2.4 o inferior, el circuito de protección contra sobrecorriente (**OCP - Over Current Protection**) o sobrepotencia (**OPP - Over Power Protection**) interpreta este pico como un cortocircuito, disparando el apagado repentino del equipo (apagado de seguridad) a mitad de un renderizado crítico.
- **Solución implementada:** La fuente seleccionada cumple con el estándar **ATX 3.0 / ATX 3.1**, que exige por diseño que la fuente sea capaz de tolerar excursiones de potencia transitorias de hasta el 200% de su potencia nominal durante 100 microsegundos sin desconectarse.

### B. El Conector 12V-2x6 / 12VHPWR (PCIe 5.0) y Riesgo de Fusión Térmica
Uno de los mayores riesgos técnicos documentados en estaciones de trabajo con GPUs de gama alta (RTX 4080 / 4090) ha sido el calentamiento excesivo y fundido del conector de 16 pines debido a:
1. Mal asentamiento mecánico del conector (inserción parcial con espacio libre inferior a 1 mm).
2. Curvatura excesiva del cable a menos de 35 mm del conector contra el cristal de la caja.
3. Uso de adaptadores pasivos tipo "pulpo" (adaptadores de 3x 8-pines a 16-pines suministrados con la GPU), los cuales introducen múltiples puntos de resistencia de contacto y desequilibrio de amperaje entre cables.
- **Solución implementada:** La fuente seleccionada incluye el cable nativo directo **12V-2x6** (revisión de seguridad de PCI-SIG con pines sensores acortados para impedir la entrega de potencia completa si el conector no está introducido al 100%). Además, la caja seleccionada (Meshify 2) cuenta con 185 mm de anchura libre para la CPU/GPU, permitiendo un radio de curvatura seguro (>40 mm) para el cable de la GPU.

### C. Cálculo Riguroso del Vataje y Distribución de Raíles de 12V:

```text
[Presupuesto de Potencia Eléctrica en Carga Máxima]
- CPU (AMD Ryzen 9 7950X a PPT máximo sostenido):         230 W
- GPU (NVIDIA RTX 4080 SUPER a TGP nominal):              320 W
- Placa Base (Chipset B650, Audio, LAN 2.5G, VRM loss):    50 W
- Memoria RAM (2 módulos DDR5 @ 1.35V en ráfaga):          15 W
- Almacenamiento (2x NVMe PCIe 4.0 a lectura/escritura):   18 W
- Bomba AIO + 3x Ventiladores radiador + 3x Chasis:        35 W
- Periféricos USB y pérdidas del sistema:                  25 W
-----------------------------------------------------------------
TOTAL ESTIMADO EN CONSUMO MÁXIMO COMBINADO:               693 W
```

### Justificación de los 850W - 1000W:
- **Consumo real máximo simultáneo:** ~693 W en el peor escenario posible (render mixto CPU + GPU en Cycles).
- **Potencia recomendada:** **850W** proporciona un margen de seguridad del **22.6%**, mientras que **1000W** proporciona un margen del **44.3%**.
- **Curva de eficiencia:** Las fuentes con certificación **80 Plus Gold** alcanzan su máxima eficiencia energética (aproximadamente un 92%) cuando operan entre el **50% y el 70% de su capacidad total**. Con un consumo típico de trabajo de 500W-650W sobre una fuente de 850W-1000W, el sistema trabaja exactamente en el punto óptimo de eficiencia, reduciendo el calor residual y el ruido del ventilador de la fuente a cero (modo Zero-RPM fan).
- **Raíl único de +12V:** La fuente seleccionada entrega 70.8A en el raíl de +12V (849.6W en total), garantizando que tanto los dos conectores EPS de 8 pines de la CPU como el conector de la GPU dispongan de potencia ilimitada y estabilizada.

---

## 5. Conclusiones de la Exploración

La exploración de hardware ha validado que el diseño cumple con todos los requisitos de ingeniería:
1. **Compatibilidad física 100%:** La caja Fractal Design Meshify 2 aloja perfectamente el radiador AIO de 360 mm en el techo sin rozar los módulos DDR5 ni las fases VRM de la placa base, y dispone de más de 400 mm para la tarjeta gráfica de 304-340 mm.
2. **Sinergia térmica:** Entrada de aire fresco frontal mallado con presión positiva y extracción superior/trasera.
3. **Seguridad eléctrica:** Fuente ATX 3.0 con raíl único de 12V y cable 12V-2x6 nativo, erradicando los riesgos de cuelgues por picos de carga y fusión de conectores.

Con esta base técnica sólidamente analizada, el proyecto avanza al **Paso 3: Generación de la Propuesta Formal en OpenSpec (`/opsx:propose`)**.
