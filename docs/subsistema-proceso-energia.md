# Informe de Ingeniería: Subsistema de Proceso y Energía
**Estación de Trabajo de Alto Rendimiento para Renderizado 3D y Producción Audiovisual**

---

| **Metadato de Proyecto** | **Detalle de Especificación** |
| :--- | :--- |
| **Documento Técnico:** | `docs/subsistema-proceso-energia.md` |
| **Responsable / Rol:** | Alumno A – Especialista en el Subsistema de Proceso y Energía |
| **Metodología:** | Specification-Driven Development (SDD) & Organic Development (ODD) |
| **Entorno:** | OpenSpec v2.1.0 |
| **Fecha de Emisión:** | 23 de Noviembre de 2026 |
| **Estado:** | Aprobado para Integración Técnica |

---

## 1. Introducción y Alcance del Subsistema

El **Subsistema de Proceso y Energía** constituye la columna vertebral computacional y eléctrica de la estación de trabajo. Su propósito de ingeniería es garantizar:
1. Una capacidad de cálculo bruto multihilo y monohilo de primer nivel para la compilación de shaders, cálculo de dinámicas, renderizado por CPU (CPU Buckets) y manipulación interactiva de geometrías 3D complejas en viewport.
2. Un suministro eléctrico ininterrumpido, estabilizado y de bajísimo rizado (ripple voltage < 30 mV) capaz de digerir picos transitorios severos sin activar disparos falsos de protección.
3. Un ancho de banda de memoria masivo y una latencia ultra baja manteniendo una estabilidad absoluta del 100% bajo cargas de trabajo continuadas de 8 a 24 horas ininterrumpidas.

A continuación se detalla el análisis exhaustivo de cada uno de los cuatro componentes clave que conforman este subsistema.

---

## 2. Unidad Central de Procesamiento (CPU): AMD Ryzen 9 7950X

```text
+-----------------------------------------------------------------------------------+
|                        AMD RYZEN 9 7950X (Zen 4 - 5nm TSMC)                       |
|                                                                                   |
|  +-------------------------------------+   +-------------------------------------+  |
|  |             CCD 0 (5nm)             |   |             CCD 1 (5nm)             |  |
|  |  8 Núcleos Físicos / 16 Hilos       |   |  8 Núcleos Físicos / 16 Hilos       |  |
|  |  L1 Cache: 512 KB (32K I + 32K D/c) |   |  L1 Cache: 512 KB (32K I + 32K D/c) |  |
|  |  L2 Cache: 8 MB (1 MB por núcleo)   |   |  L2 Cache: 8 MB (1 MB por núcleo)   |  |
|  |  L3 Cache: 32 MB (Unificada)        |   |  L3 Cache: 32 MB (Unificada)        |  |
|  +------------------+------------------+   +------------------+------------------+  |
|                     |                                         |                     |
|                     +--------------------+--------------------+                     |
|                                          | Infinity Fabric (FCLK @ 2000 MHz)         |
|                                          v                                          |
|  +-------------------------------------------------------------------------------+  |
|  |                              cIOD (6nm TSMC)                                  |  |
|  |   Controlador Dual-Channel DDR5 (UCLK @ 3000 MHz para DDR5-6000 MT/s)         |  |
|  |   Controlador PCIe 5.0 (28 líneas totales: 24 útiles + 4 chipset link)        |  |
|  |   Radeon Graphics integrada (RDNA 2, 2 CUs para diagnóstico y display)        |  |
|  +-------------------------------------------------------------------------------+  |
+-----------------------------------------------------------------------------------+
```

### 2.1. Arquitectura de Silicio y Topología de Caché
El procesador seleccionado es el **AMD Ryzen 9 7950X**, basado en la microarquitectura **Zen 4** fabricado en el nodo fotolitográfico de **5 nm FinFET de TSMC** para los complejos de computación y **6 nm** para la matriz de entrada/salida (cIOD).

* **Topología de núcleos:** Dispone de **16 núcleos físicos y 32 subprocesos lógicos** (SMT) distribuidos simétricamente en dos matrices de cálculo denominadas **Core Complex Dies (CCD0 y CCD1)**, cada uno con 8 núcleos activos.
* **Frecuencias de operación:** Frecuencia base de **4.5 GHz** en todos los núcleos y una frecuencia de reloj máxima turbo (Single-Core Boost) de hasta **5.7 GHz**, gobernada por el algoritmo autónomo *Precision Boost 2*.
* **Jerarquía de memoria caché:**
  * **Caché L1:** 1 MB total (32 KB de caché de instrucciones L1I + 32 KB de caché de datos L1D dedicada por núcleo de 8 vías de asociatividad).
  * **Caché L2:** 16 MB total (1 MB dedicado y privado por cada núcleo, lo que duplica los 512 KB de la generación Zen 3, reduciendo sustancialmente los fallos de caché en bucles de cálculo de sombreado).
  * **Caché L3:** 64 MB total (32 MB unificados compartidos de acceso cruzado en cada CCD).
  * **Caché total accesible:** **80 MB (L2 + L3)**. Esta densa reserva de memoria de ultra alta velocidad mitiga las penalizaciones de latencia causadas por el acceso a la RAM del sistema durante la renderización de rayos estocásticos en motores como *Chaos V-Ray* y *Blender Cycles*.

### 2.2. Plataforma y Zócalo de Conexión: Socket AM5 (LGA 1718)
La plataforma AM5 representa una ruptura tecnológica estratégica frente a AM4:
* **Matriz de contactos:** Adopta un encapsulado **LGA (Land Grid Array) de 1718 pines** de contacto elástico en la placa base con almohadillas en el sustrato del procesador. Esto erradica el riesgo clásico de doblar patillas en la CPU durante inserciones mecánicas o extracciones de disipador.
* **Ciclo de vida garantizado:** AMD ha formalizado soporte continuo para la plataforma AM5 hasta **2027+**. Esto asegura al cliente una ruta directa de actualización futura a arquitecturas Zen 5 y Zen 6 sin sustituir placa base, memoria RAM ni sistema de refrigeración, protegiendo la inversión de capital inicial.

### 2.3. Parámetros Eléctricos y Térmicos Oficiales de AMD
Para dimensionar con rigor el subsistema de entrega de energía y refrigeración, se desglosan las especificaciones eléctricas de fábrica de AMD:

$$\text{TDP Nominal} = 170\text{ W}$$

$$\text{PPT (Package Power Tracking)} = 230\text{ W}$$

$$\text{TDC (Thermal Design Current)} = 160\text{ A}$$

$$\text{EDC (Electrical Design Current)} = 215\text{ A}$$

$$\text{TJMax (Temperature Junction Maximum)} = 95\text{ }^\circ\text{C}$$

* **PPT (Package Power Tracking):** Es el umbral de potencia de entrada de CC continuo que el zócalo AM5 puede inyectar al encapsulado del procesador. En tareas de renderizado continuo multihilo, el procesador operará de forma fija a los **230 W**.
* **TDC (Thermal Design Current):** Límite de corriente sostenida a largo plazo (160 A) que el circuito VRM de la placa base debe proporcionar sin sobrecalentarse térmicamente.
* **EDC (Electrical Design Current):** Límite de corriente pico transitoria (215 A) demandado durante cambios rápidos de estados de energía (de reposo a carga máxima instantánea de cálculo AVX-512).
* **TJMax y Comportamiento PB2:** Zen 4 está programado deliberadamente para escalar su frecuencia de reloj y voltaje hasta topar con el límite térmico de **95 °C**. No se trata de un comportamiento erróneo ni de peligro de degradación de silicio, sino de la estrategia de optimización térmica de AMD: si la refrigeración es superior, el procesador mantendrá frecuencias de reloj más elevadas dentro de ese techo térmico.

### 2.4. Comportamiento en Renderizado Continuado de 8+ Horas (Blender / V-Ray)
Durante una jornada intensiva de renderizado de secuencias 3D:
* Los 32 hilos se saturan al 100% mediante instrucciones vectoriales avanzadas (incluyendo el soporte nativo de instrucciones **AVX-512** introducido en Zen 4 con canalización FADD/FMUL de 256 bits dobles).
* El consumo se clava en los **230 W de PPT**.
* Gracias a la refrigeración líquida AIO seleccionada (Arctic Liquid Freezer III 360), la temperatura de unión se estabiliza entre **76 °C y 82 °C** (muy por debajo de los 95 °C de TJMax).
* Esto permite mantener una frecuencia en todos los núcleos (**All-Core Boost**) estable de **5.10 GHz a 5.25 GHz**, evitando de forma absoluta cualquier estrangulamiento térmico (*thermal throttling*), que reduciría la velocidad a frecuencias base (4.5 GHz) y penalizaría los tiempos de entrega de proyectos en hasta un 15%.

---

## 3. Placa Base (Motherboard): ASUS TUF GAMING B650-PLUS WIFI

```text
+-----------------------------------------------------------------------------------+
|                        ASUS TUF GAMING B650-PLUS WIFI                             |
|                                                                                   |
|  [Panel Trasero I/O]          [Conectores EPS: 8+4 Pines ProCool]                |
|  - BIOS FlashBack Botón        +-----------------------------------------------+  |
|  - 2.5G Realtek LAN            |      VRM 12+2 Fases DrMOS 60A (Aluminio)      |  |
|  - Wi-Fi 6 + BT 5.2            +-----------------------------------------------+  |
|  - USB-C 20 Gbps (3.2 Gen 2x2)            |                                       |
|                                           v                                       |
|                             +---------------------------+    [4x DDR5 DIMMs]      |
|                             |      ZÓCALO AM5 LGA1718   |    Topología:           |
|                             |    (AMD Ryzen 9 7950X)    |    Daisy-Chain          |
|                             +---------------------------+    Canales: A2 y B2     |
|                                           |                  (Poblados 2x32GB)    |
|                             +-------------+-------------+                         |
|                             |                           |                         |
|   [Q-LED Diagnóstico]       v                           v                         |
|   (CPU, DRAM, VGA, BOOT)  +------------------------+  +-----------------------+   |
|                           | Slot M.2_1 (PCIe 5.0)  |  | SafeSlot PCIe x16     |   |
|                           | (Directo CPU + Heatsink|  | (Refuerzo estructural |   |
|                           +------------------------+  | para GPU Pesada)      |   |
|                                                       +-----------------------+   |
|                                                                                   |
|                           +------------------------+                              |
|                           | Chipset AMD B650       |                              |
|                           | (Disipador Pasivo)     |                              |
|                           +------------------------+                              |
|                           | 2x M.2 PCIe 4.0        |                              |
|                           | 4x SATA 6 Gbps         |                              |
+-----------------------------------------------------------------------------------+
```

### 3.1. Factor de Forma y Arquitectura de Pistas
* **Formato:** ATX estándar (**305 mm x 244 mm** / 12.0" x 9.6").
* **PCB:** Construcción de **6 capas de cobre de grado servidor (2 oz de cobre)**, esencial para aislar las trazas electromagnéticas de alta frecuencia requeridas por el bus DDR5 y las líneas de transmisión PCIe 5.0, minimizando la diafonía (*crosstalk*).

### 3.2. Módulo Regulador de Voltaje (VRM): Topología y Disipación Térmica
El Ryzen 9 7950X demanda una entrega de corriente impecable. La ASUS TUF B650-PLUS WIFI implementa un circuito VRM industrial:

```text
Entrada EPS 12V (8+4 Pines) 
    │
    ▼
Controlador PWM Digital ASUS Digi+ (ASP2208G)
    │
    ├─► 12 Fases Vcore  ──► 12x DrMOS 60A (Alpha & Omega / Vishay) ──► Bobinas Choke TUF + Cap 5K
    │                       (Capacidad total sostenida: 720A)             │
    │                                                                     ▼
    └─► 2 Fases Vsoc   ──► 2x DrMOS 60A                             Vcore al Socket AM5
                            (Alimentación del cIOD / Memoria)         (0.95V - 1.35V)
```

* **Etapas de potencia (Power Stages):** Configuración de **12 + 2 fases** de potencia en diseño paralelo (teaming). Cada etapa utiliza chips integrados **DrMOS de 60 A** que combinan el circuito excitador (*gate driver*) y los MOSFETs de canal alto (*high-side*) y canal bajo (*low-side*) en un único encapsulado térmicamente eficiente.
* **Capacidad teórica:** El raíl Vcore dispone de $12 \times 60\text{ A} = 720\text{ A}$. Frente a la demanda máxima de TDC del 7950X (160 A continuos), las fases operan con un coeficiente de carga de apenas el **22.2%**, ubicándolas en el punto de mínima generación térmica y máxima eficiencia de conmutación.
* **Disipadores térmicos pasivos:** Dos bloques masivos de aluminio extrusionado sobredimensionados cubren el área MOSFET y las bobinas de choque, comunicados térmicamente con pads de alto rendimiento de **5 W/mK**.
* **Conectores de alimentación ProCool:** Conector primario **EPS de 8 pines** y secundario de **4 pines**. A diferencia de los conectores estándar de pines huecos de chapa estampada, la tecnología *ProCool* utiliza pines sólidos de aleación de cobre que garantizan una resistencia de contacto inferior a $0.03\text{ }\Omega$, impidiendo calentamientos en el cableado bajo 230 W continuos.

### 3.3. Enrutamiento de Memoria: Topología Daisy-Chain
La placa base ASUS implementa una topología de pistas en **Daisy-Chain** (cadena margarita) para las pistas que interconectan el cIOD de la CPU con los 4 zócalos DIMM:

```text
Controlador CPU ───► DIMM Canal A Slot 1 (A1) ───► DIMM Canal A Slot 2 (A2 - TERMINADO)
                ───► DIMM Canal B Slot 1 (B1) ───► DIMM Canal B Slot 2 (B2 - TERMINADO)
```

* **Principio físico:** En la topología Daisy-Chain, la pista llega al primer slot y continúa hasta el segundo, donde se produce la terminación física de señal. 
* Si se instalan solo 2 módulos en las ranuras **A2 y B2**, no existen ramificaciones de señal ("stubs") vacías colgando del extremo. Esto elimina por completo el rebote de ondas de radiofrecuencia (reflexión de señal) en el bus de 3000 MHz (6000 MT/s), permitiendo alcanzar latencias bajas y alta integridad de bus que serían imposibles en placas antiguas con topología en "T".

### 3.4. Conectividad, Expansión y Mantenimiento Técnico
* **Ranura PCIe Principal Reforzada (SafeSlot Core+):** 1x PCIe 4.0/5.0 x16 anclada con una carcasa metálica insertada y soldada mediante puntos de fijación mecánica reforzados en el PCB. Soporta tarjetas gráficas de triple ranura con pesos superiores a 2.1 kg sin deformación del zócalo (*GPU sag*). Cuenta además con el pulsador ergonómico *Q-Release* para extracción sencilla de GPUs de gran volumen.
* **Bahías M.2:**
  * 1x M.2 PCIe 5.0 x4 (2280) conectada a la CPU para futuros SSDs ultrarrápidos (>10 GB/s) con disipador dedicado.
  * 2x M.2 PCIe 4.0 x4 (2280/22110) con disipadores de aluminio integrados para las unidades del sistema y almacenamiento masivo.
* **Herramientas de Mantenimiento:**
  * **BIOS FlashBack:** Botón exterior en el panel de I/O trasero y puerto USB dedicado que permite reprogramar la BIOS/UEFI mediante un microcontrolador secundario utilizando únicamente la fuente de alimentación, sin necesidad de tener montados la CPU, la tarjeta gráfica o la memoria RAM.
  * **ASUS Q-LED:** 4 diodos LED en placa (CPU, DRAM, VGA, BOOT) que ofrecen un código visual de diagnóstico inmediato durante la fase POST (Power-On Self-Test), reduciendo drásticamente el tiempo de resolución de averías ante fallos de hardware.

---

## 4. Subsistema de Memoria de Acceso Aleatorio (RAM)

```text
+-----------------------------------------------------------------------------------+
|              CORSAIR VENGEANCE DDR5 64 GB (2x32 GB) - AMD EXPO                    |
|                         Código: CMK64GX5M2B6000Z30                                |
|                                                                                   |
|  Especificaciones Eléctricas y Timings:                                           |
|  - Ancho de Banda: DDR5-6000 MT/s (PC5-48000)                                     |
|  - Frecuencia Real del Bus de Datos (MCLK): 3000 MHz                              |
|  - Latencias Primarias: CL30-36-36-76 @ 1.35V                                     |
|  - PMIC Integrado en el módulo (Gestión de Voltaje Localizada en PCB)             |
|                                                                                   |
|  Perfil Físico y Despeje (Clearance):                                             |
|  <--------------------------- 133.35 mm ---------------------------->            |
|  +-------------------------------------------------------------------+  ^         |
|  | Disipador de Aluminio Anodizado Negro de Perfil Bajo              |  | 35 mm   |
|  +-------------------------------------------------------------------+  v (CLEAR) |
|  |||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||            |
+-----------------------------------------------------------------------------------+
```

### 4.1. Ficha Técnica y Códigos de Fabricante
* **Modelo comercial:** Corsair Vengeance DDR5 64 GB (Kit 2x32 GB) 6000 MHz CL30 AMD EXPO.
* **Número de pieza exacto (Part Number):** `CMK64GX5M2B6000Z30`.
* **Circuito Integrado (IC DRAM):** Módulos construidos con chips **SK Hynix A-die / M-die** de alta densidad (16 Gb / 24 Gb por chip), reconocidos en la industria como los de mayor estabilidad, tolerancia a voltaje y menor tasa de errores temporales en arquitecturas DDR5.
* **Perfil de Overclocking:** Certificación nativa **AMD EXPO (Extended Profiles for Overclocking)** que carga automáticamente en la UEFI los voltajes VDD (1.35V), VDDQ (1.35V), VDDIO (1.25V) y la matriz completa de latencias primarias, secundarias y terciarias calibradas para procesadores Ryzen 7000.

### 4.2. Justificación de Capacidad (64 GB) para Flujos de Render y Postproducción
El dimensionamiento en **64 GB** responde a análisis empíricos en los programas que utilizará la estación de trabajo:

| Software / Carga de Trabajo | Consumo de RAM Típico | Impacto de Disponer de 64 GB frente a 32 GB |
| :--- | :--- | :--- |
| **DaVinci Resolve Studio 18/19** (Edición 4K/6K RAW, Prores 422 HQ) | **42 GB – 54 GB** | Permite alojar el búfer de reproducción de vídeo sin comprimir (*RAM Playback Cache*) en memoria para scrubbing a 60 fps sin saltos de fotogramas. Con 32 GB, el sistema entra en paginación a disco NVMe, provocando tirones severos. |
| **DaVinci Fusion** (Efectos de composición nodal, partículas y tracking 3D) | **36 GB – 48 GB** | Los árboles de nodos complejos apilan capas de búfer en memoria antes de rasterizar; 64 GB previenen el error clásico *"Out of Memory"* durante el pre-render. |
| **Blender 4.x / V-Ray** (Escenas de 15 a 30 millones de polígonos con texturas 8K UDIM) | **38 GB – 52 GB** | La construcción de la jerarquía de volumen envolvente (**BVH - Bounding Volume Hierarchy**) para trazado de rayos requiere descomprimir geometrías completas en RAM del sistema antes de transmitirlas a la GPU. |
| **Multitarea Concurrente Profesional** (Blender + Photoshop + After Effects + Navegador) | **48 GB – 58 GB** | Permite alternar instantáneamente entre aplicaciones sin congelamientos causados por el swappeo del sistema operativo. |

### 4.3. Topología de Memoria: ¿Por qué 2x32 GB es muy Superior a 4x16 GB?
Existe una concepción errónea heredada de DDR4 según la cual poblar las 4 ranuras DIMM es beneficioso. En la arquitectura **DDR5 y Zen 4**, la instalación de 4 módulos (configuración 2 DPC - 2 DIMMs Per Channel) es contraproducente por razones de electromagnetismo y diseño de silicio:

```text
CONFIGURACIÓN 2x32 GB (1 DPC - 1 DIMM Por Canal) [SELECCIONADA]:
- Topología limpia en ranuras A2 y B2.
- Frecuencia alcanzable: 6000 MT/s (CL30).
- Relación de relojes: 1:1 síncrona (UCLK = MCLK = 3000 MHz).
- Integridad de señal: Señal con bordes de subida limpios, cero diafonía.

CONFIGURACIÓN 4x16 GB (2 DPC - 2 DIMMs Por Canal) [RECHAZADA]:
- Ambas ramas del bus Daisy-Chain pobladas.
- Carga capacitiva duplicada en el bus de direcciones/comandos.
- Frecuencia máxima estable: 4800 MT/s (o 3600 MT/s según directriz oficial JEDEC/AMD).
- Relación de relojes forzada: Modo 1:2 asíncrono (UCLK = 1/2 MCLK).
- Penalización de latencia: Incremento de +18 a +25 ns en tiempo de acceso.
```

#### El "Sweet Spot" de Zen 4 (Relación 1:1):
El procesador opera con tres dominios de reloj:
1. **FCLK (Fabric Clock):** Frecuencia del bus Infinity Fabric que comunica los CCDs con el cIOD. Frecuencia óptima fija: **2000 MHz**.
2. **MCLK (Memory Clock):** Frecuencia de reloj real del bus de memoria. Para DDR5-6000 MT/s: $6000 / 2 = \mathbf{3000\text{ MHz}}$.
3. **UCLK (Unified Memory Controller Clock):** Frecuencia del controlador de memoria interno dentro del cIOD.

Al instalar **2 módulos a 6000 MT/s**, el controlador opera en relación síncrona:

$$\text{UCLK} = \text{MCLK} = 3000\text{ MHz} \quad (1:1)$$

$$\text{Latencia Absoluta de Acceso (First Word)} = \frac{\text{CL} \times 2000}{\text{Data Rate}} = \frac{30 \times 2000}{6000} = \mathbf{10.0\text{ ns}}$$

Si se hubieran instalado 4 módulos de 16 GB, el controlador se ve forzado a conmutar a la relación asíncrona divisora $1:2$ ($\text{UCLK} = \frac{1}{2} \text{MCLK} = 1500\text{ MHz}$) o degradar la frecuencia a 4800 MT/s o menos para no generar errores de paridad de bus, destruyendo el rendimiento en renderizado.

### 4.4. Despeje Físico (Clearance) y Verificación en Lista QVL
* **Altura de los módulos:** Los módulos Corsair Vengeance DDR5 tienen una cota de altura de **35.0 mm**, perteneciendo a la categoría de perfil bajo (*Low Profile*).
* **Ausencia de colisiones:** El radiador superior de refrigeración líquida (Arctic Liquid Freezer III 360) tiene un grosor conjunto de 65 mm (radiador 38 mm + ventiladores 27 mm). La caja Fractal Design Meshify 2 desplaza los raíles de fijación superior 35 mm hacia el panel lateral izquierdo, dejando un espacio libre vertical de más de 70 mm sobre la placa. No obstante, al medir únicamente 35 mm, los módulos de RAM se sitúan totalmente por debajo del plano horizontal de ventilación, facilitando su sustitución o ampliación sin necesidad de desmontar el radiador.
* **Validación QVL (Qualified Vendor List):** El código `CMK64GX5M2B6000Z30` está explícitamente certificado en la lista de soporte calificado oficial de ASUS para la placa TUF GAMING B650-PLUS WIFI en su última revisión de BIOS AGESA ComboAM5PI 1.2.0.2+, garantizando arranque a la primera mediante el perfil EXPO sin necesidad de ajustes manuales de voltajes secundarios.

---

## 5. Fuente de Alimentación (PSU) y Gestión Eléctrica

```text
+-----------------------------------------------------------------------------------+
|               CORSAIR RM850x SHIFT (850W - 80 PLUS GOLD / ATX 3.0)                |
|                                                                                   |
|  [TOPOLOGÍA INTERNA DE ALTA FRECUENCIA]                                           |
|  Entrada AC (100-240V) ──► Filtro EMI ──► Puente Rectificador PFC Activo          |
|                             (FP > 0.99)         │                                 |
|                                                 ▼                                 |
|  Condensadores Japoneses 105°C ◄── Circuito Resonante LLC                         |
|  (Nippon Chemi-Con / Nichicon)                  │                                 |
|                                                 ▼                                 |
|  Transformador Principal ──────► Rectificación Síncrona Secundaria                |
|                                                 │                                 |
|                                                 ├─► RAÍL +12V PRINCIPAL (70.8A)   |
|                                                 │   (849.6 W en continua)         |
|                                                 ▼                                 |
|                                         Módulos DC-DC Buck                        |
|                                                 ├─► Raíl +5V  (20A)               |
|                                                 └─► Raíl +3.3V (20A)              |
|                                                                                   |
|  [INTERFAZ LATERAL DE CONEXIONES SHIFT PATENTADA]                                 |
|  - Conector Nativo 12V-2x6 (PCIe 5.0) blindado 600W directo hacia GPU             |
|  - 2x Conectores EPS 8-pines dedicados e independientes hacia CPU                 |
|  - Absorción de transitorios: 200% de la potencia nominal (1700 W / 100 µs)        |
+-----------------------------------------------------------------------------------+
```

### 5.1. Ficha Técnica y Certificaciones de Rendimiento
* **Modelo:** Corsair RM850x SHIFT (Referencia: `CP-9020252-EU`).
* **Potencia Nominal Sostenida:** **850 Vatios continuos** a temperatura ambiente de trabajo de hasta 50 °C.
* **Certificaciones de Eficiencia y Ruido:**
  * **80 PLUS Gold:** Eficiencia superior al 90% a media carga en red europea de 230V.
  * **Cybenetics Gold:** Certificación independiente de eficiencia global superior al 89%.
  * **Cybenetics A-:** Certificación acústica ultra silenciosa, manteniendo niveles de presión sonora entre 25 y 30 dBA en régimen de trabajo.
* **Tecnología de Montaje Lateral SHIFT:** Los puertos modulares están situados en el panel lateral de la fuente en lugar de en la parte trasera. Esto aprovecha la bahía lateral del chasis Fractal Meshify 2, permitiendo conectar y desconectar cables de forma directa sin tener que forcejear con el espacio reducido de la cubierta de la fuente (*PSU shroud*).

### 5.2. Topología Electrónica Interna
1. **Topología Primaria Resonante LLC con Conmutación en Voltaje Cero (ZVS):**
   A diferencia de las fuentes tradicionales con conmutación dura (*hard-switching*), la topología de medio puente LLC conmuta los transistores MOSFET cuando el voltaje a través de ellos es cero. Esto reduce drásticamente las pérdidas térmicas por conmutación y las interferencias electromagnéticas (EMI), incrementando la vida útil del equipo.
2. **Rectificación Síncrona y Módulos Conversores DC-DC:**
   En el secundario, todo el voltaje de CA se convierte en un único raíl masivo de **+12V de corriente continua**. Los raíles secundarios menores (+5V y +3.3V) no se derivan del transformador principal mediante devanados adicionales, sino que se generan a partir del propio raíl de +12V mediante placas reductoras conmutadas independientes **DC-DC (Buck Converters)**. Esto garantiza una regulación de voltaje independiente donde una carga masiva en el raíl de +12V jamás desestabilizará los voltajes de los discos de almacenamiento o buses de control.
3. **Condensadores 100% Japoneses clasificados a 105 °C:**
   Filtrado integral encomendado a condensadores electrolíticos y de polímero sólido de primeras marcas japonesas (*Nippon Chemi-Con, Nichicon, Rubycon*) certificados para **105 °C**. Su tasa de vida útil estimada supera las **100.000 horas de operación continua (MTBF)**, garantizando un voltaje limpio con rizado inferior a 20 mV en el raíl de 12V (muy por debajo de la norma Intel ATX de 120 mV).

### 5.3. Tabla de Distribución de Raíles de Alimentación

| Raíl de Voltaje | Corriente Máxima (Amperaje) | Potencia Máxima Entregable | Carga Típica Asignada |
| :--- | :--- | :--- | :--- |
| **+12 V (Raíl Único)** | **70.8 A** | **849.6 W** | CPU (Ryzen 9 7950X), GPU (RTX 4080 S), Ventiladores, Bomba AIO |
| **+5 V** | **20.0 A** | **150 W** (Combinado +5V y +3.3V) | Electrónica de SSDs NVMe, iluminación, circuitos lógicos USB |
| **+3.3 V** | **20.0 A** | (Compartido con +5V) | Circuitos de control de placa, PCIe bus logic, audio |
| **-12 V** | **0.3 A** | **3.6 W** | Puertos de comunicación serie y legado |
| **+5 Vsb (Standby)** | **3.0 A** | **15.0 W** | Alimentación de reposo, estados de suspensión S3/S5, USB Power-Off |
| **POTENCIA TOTAL CONTINUA** | — | **850.0 W** | — |

*El raíl único de +12V entrega el **99.95% de la potencia total** de la fuente en continua ($70.8\text{ A} \times 12\text{ V} = 849.6\text{ W}$), eliminando los típicos cortes de energía de fuentes multiraíl donde un raíl de 30A se saturaba mientras los demás quedaban vacíos.*

### 5.4. Estándar ATX 3.0 / PCIe 5.0 y Mitigación de Excursiones de Potencia (Power Spikes)
Las GPU modernas basadas en silicio Ada Lovelace cambian de estados de energía en microsegundos, pasando de consumir 30W en reposo a más de 550W durante transitorios de sombreado de trazado de rayos.

```text
POTENCIA INSTANTÁNEA (W)
  ▲
  │                       PICOS TRANSITORIOS (POWER EXCURSION)
1700 W ─────────────────►  ┌─┐  (Capacidad ATX 3.0: 200% durante 100 µs)
  │                        │ │
  │                        │ │
  │                        │ │
850 W ───────────────────┼─┼─┼─────────────────── Potencia Nominal Continua (100%)
  │                      │ │ │
  │                      │ │ │
693 W ───────────┐       │ │ │       ┌─────────── Carga Sostenida Máxima en Render
  │              └───────┘   └───────┘
  └───────────────────────────────────────────────────► TIEMPO (µs)
```

* **Exigencia ATX 3.0:** La norma ATX 3.0 de Intel obliga a que la fuente sea capaz de soportar picos transitorios del **200% de su capacidad nominal (1.700 W)** durante **100 microsegundos**, del **180% (1.530 W)** durante 1 milisegundo y del **160% (1.360 W)** durante 10 milisegundos.
* **Prevención de disparos OCP / OPP:** Las fuentes ATX 2.4 de generaciones anteriores poseían circuitos de protección analógicos sensibles que interpretaban un pico transitorio de 100 µs como un cortocircuito grave o sobrecarga, disparando el circuito **OPP (Over Power Protection)** y apagando el PC en plena sesión de renderizado. La Corsair RM850x SHIFT implementa retardos digitales calibrados que permiten absorber estas excursiones sin inestabilidad ni caídas de tensión en el raíl de +12V (manteniéndose siempre por encima de 11.4V).
* **Conector Nativo 12V-2x6 / 12VHPWR (PCIe 5.0) de 600W:**
  * La fuente incluye un cable nativo directo de 16 pines con la especificación de seguridad revisada por PCI-SIG (**12V-2x6**).
  * Los 4 pines de señal (*Sideband Signals: SENSE0, SENSE1*) son 1.7 mm más cortos que los 12 pines de alimentación. Si el conector no está introducido al 100% con un enclavamiento mecánico perfecto, los pines sensores no hacen contacto, indicándole a la GPU que restrinja su consumo o impidiendo el arranque, erradicando el peligro histórico de sobrecalentamiento o fundido del conector por desajuste mecánico.
  * Suprime el uso del arriesgado adaptador pasivo en "pulpo" (adaptador de 3 cables de 8 pines a 1 conector de 16 pines suministrado con las GPUs), eliminando 3 puntos de resistencia de contacto y reduciendo el volumen de cables en el chasis.

---

## 6. Presupuesto de Potencia Eléctrica y Curva de Eficiencia

### 6.1. Desglose de Consumo por Componente y Escenarios Operativos

A continuación se calcula el consumo en corriente continua demandado por cada componente en tres escenarios: Reposo (Idle), Carga Media de Trabajo (Edición 4K, modelado en viewport) y Carga Máxima Sostenida (Renderizado híbrido CPU + GPU):

| Componente del Sistema | Reposo (Idle) | Carga Media Típica | Carga Máxima Sostenida |
| :--- | :---: | :---: | :---: |
| **CPU AMD Ryzen 9 7950X** | 22 W | 85 W | **230 W** (PPT Límite Sostenido) |
| **GPU NVIDIA RTX 4080 SUPER 16GB** | 15 W | 140 W | **320 W** (TGP Nominal en Render) |
| **Placa Base ASUS TUF B650-PLUS WIFI** | 18 W | 30 W | **50 W** (Chipset, Audio, LAN 2.5G, VRM loss) |
| **Memoria RAM (2x32 GB DDR5-6000 @ 1.35V)** | 4 W | 8 W | **15 W** (Ráfagas continuas de lectura/escritura) |
| **Almacenamiento NVMe (Samsung 990 Pro + KC3000)**| 3 W | 8 W | **18 W** (Lectura y volcado simultáneo a disco) |
| **Refrigeración (Bomba AIO + 3x P12 + 3x Ventiladores Caja)** | 6 W | 16 W | **35 W** (Bomba al 100% y 6 ventiladores a 1800 rpm) |
| **Periféricos USB, Controladoras y Pérdidas** | 8 W | 15 W | **25 W** |
| **TOTAL CONSUMO DE POTENCIA (CC):** | **76 W** | **302 W** | **693 W** |

### 6.2. Fórmulas de Cálculo de Potencia y Margen de Seguridad

El consumo máximo en el peor escenario posible (renderizado combinado de CPU mediante buckets y GPU mediante OptiX simultáneamente) se calcula mediante la sumatoria:

$$P_{\text{sostenida}} = \sum_{i=1}^{n} P_i = P_{\text{CPU}} + P_{\text{GPU}} + P_{\text{Placa}} + P_{\text{RAM}} + P_{\text{NVMe}} + P_{\text{Refrig}} + P_{\text{Otros}}$$

$$P_{\text{sostenida}} = 230\text{ W} + 320\text{ W} + 50\text{ W} + 15\text{ W} + 18\text{ W} + 35\text{ W} + 25\text{ W} = \mathbf{693\text{ W}}$$

#### Margen de Seguridad respecto a la Capacidad de la Fuente (850 W):
Calculado de forma directa respecto al consumo máximo demandado:

$$\text{Margen de Seguridad} = \frac{P_{\text{PSU}} - P_{\text{sostenida}}}{P_{\text{sostenida}}} \times 100 = \frac{850 - 693}{693} \times 100 = \mathbf{22.65\%}$$

Calculado como porcentaje de reserva respecto a la capacidad nominal total de la fuente:

$$\text{Reserva de Potencia Total} = \frac{850 - 693}{850} \times 100 = \mathbf{18.47\%}$$

Este margen del **22.65%** es el estándar de oro en ingeniería de hardware para estaciones de trabajo profesionales:
* Previene el envejecimiento prematuro de los condensadores electrolíticos, que pierden capacidad de filtrado con los años debido al calor continuo si operan al 100% de carga.
* Permite la instalación futura de discos duros SATA adicionales, tarjetas capturadoras PCIe o dispositivos de expansión sin sustituir la fuente de alimentación.

### 6.3. Análisis de la Curva de Eficiencia Energética (80 PLUS Gold)

```text
EFICIENCIA (%)
  ▲
100%│
    │                    PUNTO ÓPTIMO DE EFICIENCIA
 92%│                    (92.5% @ 50%-60% Carga)
 90%│                  .---*---.
 88%│               .-'         `-.
 86%│            .-'               `-.
 80%│         .-'                     `-.
    │       .'                           `.
    └───────┴──────────┴──────────┴───────┴───────► % DE CARGA DE LA PSU
           10%        20%        50%     75%    100%
         (85W)      (170W)     (425W)   (638W)  (850W)
```

* **Punto Óptimo:** La curva de campana de eficiencia de las fuentes con topología LLC resonante alcanza su vértice máximo (hasta un **92.5% de eficiencia**) en el intervalo situado entre el **50% y el 75% de la carga nominal**.
* **Comportamiento en nuestro sistema:**
  * En **carga media de trabajo (302 W)**, el sistema demanda el **35.5%** de la fuente, operando con una eficiencia del **91.2%**.
  * En **carga máxima de renderizado (693 W)**, el sistema demanda el **81.5%** de la capacidad, situándose en una zona de alta eficiencia del **90.4%**.
  * **Modo Zero-RPM Fan:** Cuando el consumo cae por debajo de los 340 W (40% de carga), el ventilador de la fuente se apaga completamente gracias al modo pasivo Zero-RPM, reduciendo el desgaste del rodamiento y garantizando un funcionamiento 100% inaudible durante las fases de modelado, guionizado o edición ligera.

---

## 7. Diagramas de Conectividad Eléctrica e Interfaz del Sistema

### 7.1. Esquema de Asignación de Líneas y Cableado Modular

```text
+-----------------------------------------------------------------------------------+
|               DISTRIBUCIÓN DEL CABLEADO MODULAR DE ALIMENTACIÓN                   |
+-----------------------------------------------------------------------------------+
| FUENTE DE ALIMENTACIÓN CORSAIR RM850x SHIFT                                       |
|                                                                                   |
| [Puerto ATX 24 Pines]  ===============================> [Placa Base ATX 24 Pines] |
|                                                                                   |
| [Puerto EPS 8 Pines 1] -------------------------------> [Placa Base EPS 8 Pines]  |
|                                                         (Vcore Primario 7950X)    |
| [Puerto EPS 8 Pines 2] -------------------------------> [Placa Base EPS 4 Pines]  |
|                                                         (Vcore Auxiliar ProCool)  |
|                                                                                   |
| [Puerto Nativo 12V-2x6] ==============================> [GPU RTX 4080 SUPER]      |
| (Cable blindado 600W / 16 AWG + 4x Sense Lines)         (Conector 16 Pines)       |
|                                                                                   |
| [Puerto SATA/PATA] -----------------------------------> [Controlador Fans/Bomba]  |
+-----------------------------------------------------------------------------------+
```

---

## 8. Matriz de Compatibilidad y Checklist de Validación de Ingeniería

| Parámetro de Verificación | Requisito de Diseño | Estado | Justificación Técnica |
| :--- | :--- | :---: | :--- |
| **Compatibilidad Zócalo** | CPU AM5 LGA1718 en Placa AM5 | **CONFORME** | Instalación nativa sin adaptadores. Soporte BIOS garantizado por ASUS. |
| **Capacidad de Corriente VRM** | Soporte continuo de 160A TDC y 230W PPT | **CONFORME** | VRM 12+2 fases DrMOS 60A = 720A teóricos; carga real por fase < 25%. |
| **Enrutamiento de Memoria** | Integridad a 6000 MT/s en topología Daisy-Chain | **CONFORME** | Kit 2x DIMM instalado en ranuras A2 y B2, eliminando reflexiones en bus. |
| **Relación de Relojes CPU** | Modo 1:1 síncrono (UCLK=MCLK a 3000 MHz) | **CONFORME** | Certificación AMD EXPO I con kit Corsair CMK64GX5M2B6000Z30 verificado. |
| **Clearance Físico de RAM** | Altura < 45 mm bajo radiador superior | **CONFORME** | Módulos de perfil bajo de 35 mm; despeje vertical superior > 70 mm. |
| **Límite Transitorio Eléctrico**| Absorción de picos transitorios de hasta el 200% | **CONFORME** | Estándar ATX 3.0 tolera 1700 W / 100 µs sin disparo involuntario de OPP. |
| **Conector de Alimentación GPU**| Prevención de fallos térmicos en conector 16-pin | **CONFORME** | Cable nativo 12V-2x6 de 600W con pines sensores integrados. Cero adaptadores. |
| **Margen de Potencia Sostenida**| Margen de seguridad > 15% sobre carga máxima | **CONFORME** | Consumo máximo: 693 W; Capacidad: 850 W; Margen: 22.65%. |

---

## 9. Conclusión y Dictamen para Integración en el Proyecto

El **Subsistema de Proceso y Energía** diseñado por el **Alumno A** cumple rigurosamente con todos los estándares industriales de ingeniería informática y especificaciones marcadas en el plan de trabajo OpenSpec:

1. **Rendimiento Sostenido:** El procesador **AMD Ryzen 9 7950X** dispone del soporte de entrega eléctrica (VRM 12+2 DrMOS 60A) y holgura térmica para operar a su máximo potencial de 230 W sin caídas de frecuencia en jornadas de renderizado de 8+ horas.
2. **Estabilidad de Datos:** La memoria **Corsair Vengeance DDR5 64 GB** en configuración 2x32 GB a 6000 MHz CL30 garantiza el punto dulce de latencia síncrona 1:1 sin sobrecargar el bus de memoria, ofreciendo el búfer necesario para vídeo 4K/6K en DaVinci Resolve y escenas complejas en Blender.
3. **Seguridad y Fiabilidad Eléctrica:** La fuente **Corsair RM850x SHIFT** con estándar ATX 3.0, raíl único de +12V de 70.8A y cable nativo 12V-2x6 de 600W elimina cualquier riesgo de inestabilidad por transitorios o fallos de conexión térmica en la GPU, operando en su curva de máxima eficiencia energética (81.5% de carga máxima en render y 35% en carga típica).

Este informe queda consolidado y listo para ser enlazado en la propuesta global y fusionado con el informe del **Alumno B** (Subsistema de Gráficos, Almacenamiento y Chasis).
