# Design: Arquitectura Técnica y Criterios de Selección para Workstation 3D

## Context

El diseño responde a la necesidad de disponer de una estación de trabajo de alta fiabilidad y rendimiento sostenido para modelado 3D, animación, simulaciones y renderizado por GPU/CPU. Véase `proposal.md` para los antecedentes y objetivos de negocio.

Las principales restricciones físicas, térmicas y eléctricas que moldean el diseño son:
- Mantener la temperatura de la CPU y GPU por debajo de los umbrales de estrangulamiento térmico (thermal throttling) durante cargas continuas de más de 8 horas.
- Garantizar total compatibilidad física entre los componentes (longitud de GPU, altura de memorias RAM, grosor del radiador de refrigeración líquida y espacio en chasis).
- Diseñar una infraestructura de alimentación eléctrica conforme al estándar ATX 3.0 / PCIe 5.0 con conector nativo 12V-2x6 para disipar picos transitorios de potencia sin inestabilidad.

## Goals / Non-Goals

**Goals:**
- Proporcionar un rendimiento de cómputo superior en Blender Cycles (OptiX), DaVinci Resolve y V-Ray manteniendo una estabilidad operativa del 100%.
- Asegurar una presión positiva de aire en el chasis para minimizar la acumulación interna de polvo y maximizar la refrigeración de los VRM y unidades NVMe.
- Implementar cableado nativo y modular sin adaptadores que comprometan la seguridad térmica de los conectores.
- Validar holguras físicas con un margen de tolerancia superior a 15 mm en todos los componentes clave.

**Non-Goals:**
- No se contempla el overclocking manual agresivo por encima de las especificaciones de fábrica (PBO controlado únicamente dentro de límites térmicos seguros).
- No se contempla refrigeración líquida custom de circuito abierto (custom loop), debido a su alto coste, mantenimiento complejo y riesgo de fugas en un entorno de producción/instituto.
- No se requiere configuración multi-GPU (SLI/NVLink está deprecado comercialmente para este segmento; una sola RTX 4080 SUPER ofrece el balance térmico y de coste ideal).

## Decisions

### 1. Plataforma de Procesamiento: AMD Socket AM5 y Ryzen 9 7950X
- **Decisión**: Se selecciona el procesador AMD Ryzen 9 7950X (16C/32T) con arquitectura Zen 4 en nodo de 5 nm sobre socket AM5.
- **Razón**: Brinda 32 hilos de cómputo nativo a 5.7 GHz boost para tareas de renderizado por CPU y cálculo de físicas en Blender/Unreal Engine, ofreciendo además soporte a largo plazo para futuras actualizaciones de CPU en el mismo socket AM5 hasta 2027+.
- **Alternativas consideradas**:
  - *Intel Core i9-14900K*: Descartado por su desmesurado consumo energético en carga plena (>300W-350W), severas inestabilidades de microcódigo reportadas en la arquitectura Raptor Lake y plataforma LGA1700 ya descontinuada al final de su ciclo de vida.
  - *AMD Ryzen 7 7800X3D*: Excelente para videojuegos gracias a su 3D V-Cache, pero inferior en un 45% en renderizado multihilo puro por disponer únicamente de 8 núcleos.

### 2. Placa Base y Chipset: ASUS TUF GAMING B650-PLUS WIFI (Formato ATX)
- **Decisión**: Formato estándar ATX con chipset AMD B650 y sistema VRM de 12+2 fases (DrMOS de 60A).
- **Razón**: El formato ATX estándar (305 x 244 mm) garantiza separación suficiente entre el zócalo de la CPU, las ranuras de memoria RAM y el primer puerto PCIe 4.0/5.0 x16. El chipset B650 proporciona todas las líneas PCIe necesarias sin el sobrecoste ni el consumo térmico pasivo innecesario de un chipset X670E de doble puente.
- **Alternativas consideradas**:
  - *Micro-ATX (B650M)*: Descartado porque la tarjeta gráfica de 3.2 slots bloquearía los puertos PCIe inferiores y situaría la GPU demasiado cerca del fondo del chasis, asfixiando el flujo de aire.

### 3. Memoria RAM: Kit Dual-Channel 64 GB DDR5-6000 MHz CL30 (AMD EXPO)
- **Decisión**: 2 módulos de 32 GB Corsair Vengeance DDR5 a 6000 MT/s, latencia CL30-36-36-76 con perfil AMD EXPO y altura de 35 mm.
- **Razón**: 64 GB permiten cargar proyectos con texturas en alta resolución (4K/8K) y cachés de renderizado sin recurrir al archivo de paginación del disco. Usar 2 módulos en lugar de 4 preserva la topología de memoria y permite mantener la frecuencia óptima de 6000 MHz con el controlador de memoria trabajando en relación 1:1 (UCLK = MCLK = 3000 MHz).
- **Alternativas consideradas**:
  - *Configuración 4x16 GB*: Inestable a 6000 MHz en arquitecturas DDR5 modernas, forzando a bajar a 4800 MHz o 5200 MHz por degradación de señal en la placa base.

### 4. Tarjeta Gráfica: NVIDIA GeForce RTX 4080 SUPER 16 GB GDDR6X
- **Decisión**: GPU de arquitectura Ada Lovelace con 10.240 CUDA Cores, 320 Tensor Cores y 16 GB GDDR6X a 736 GB/s.
- **Razón**: Compatibilidad total con las APIs OptiX y CUDA utilizadas universalmente por Blender, V-Ray, OctaneRender y DaVinci Resolve Studio. Los 16 GB de VRAM son obligatorios para renderizado de escenas complejas.
- **Alternativas consideradas**:
  - *AMD Radeon RX 7900 XTX (24 GB)*: A pesar de tener 24 GB de VRAM, en motores de renderizado 3D estándar como Blender Cycles rinde entre un 50% y un 70% menos que la RTX 4080 SUPER debido a la inmadurez de HIP frente al ecosistema OptiX.

### 5. Refrigeración: ARCTIC Liquid Freezer III 360 en Montaje Superior (Top Exhaust)
- **Decisión**: Sistema AIO de 360 mm montado en el panel superior de la caja expulsando aire hacia fuera.
- **Razón**: Su radiador de 38 mm de grosor con ventiladores de alta presión estática (P12 PWM) disipa más de 250W térmicos con niveles acústicos muy bajos (<30 dBA). El bloque de CPU incluye un ventilador auxiliar orientado hacia los disipadores del VRM de la placa base, reduciendo su temperatura en hasta 12°C.
- **Alternativas consideradas**:
  - *Disipador de aire doble torre (Thermalright Peerless Assassin)*: Aunque fiable, operaría al límite térmico en cargas de 230W continuadas y generaría interferencias de espacio con los módulos de RAM.

### 6. Sistema de Alimentación: Corsair RM850x SHIFT 850W ATX 3.0 / PCIe 5.0
- **Decisión**: Fuente 80 Plus Gold con raíl único de +12V (70.8A) y conector nativo 12V-2x6 (PCIe 5.0 600W).
- **Razón**: Soporta picos transitorios de potencia de hasta el 200% del consumo nominal. El cable nativo de 16 pines suprime adaptadores multipuerto propensos a falsos contactos mecánicos y fallos por temperatura.

### 7. Chasis: Fractal Design Meshify 2
- **Decisión**: Caja semitorre ATX de alto flujo de aire con frontal de malla, filtros magnéticos y soporte para GPUs de hasta 467 mm y radiadores de 360 mm.
- **Razón**: Facilita un montaje limpio, holgura de más de 100 mm entre la GPU y el frontal, y 185 mm de espacio lateral para evitar tensiones o dobleces bruscos en el cable de alimentación de la GPU.

## Risks / Trade-offs

- **[Riesgo] Doblez forzado del cable 12V-2x6 de la GPU**: El cable de 16 pines requiere un radio mínimo de 35 mm sin doblar desde el conector para evitar fatiga en los terminales.  
  *Mitigación*: La caja Fractal Meshify 2 cuenta con un ancho interior de 185 mm libres desde el socket/ranura PCIe hasta el panel lateral, garantizando más de 50 mm de espacio holgado para el recorrido del cable.
- **[Riesgo] Grosor del radiador de 38 mm del AIO Liquid Freezer III**: Con los ventiladores (27 mm), el grosor total alcanza los 65 mm, lo que en cajas compactas colisiona con la placa base o la memoria RAM.  
  *Mitigación*: La Fractal Meshify 2 desplaza el anclaje superior de 360 mm hacia el panel exterior, dejando un despeje vertical de más de 70 mm sobre los conectores superiores de la placa base y los módulos de memoria RAM de 35 mm.
- **[Riesgo] Inestabilidad de frecuencias en DDR5**:  
  *Mitigación*: Uso estricto de memorias en la lista QVL (Qualified Vendor List) de ASUS, configuración 2x DIMM y activación del perfil optimizado AMD EXPO I.
