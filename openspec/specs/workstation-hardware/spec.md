# workstation-hardware Specification

## Purpose

Define formalmente los requisitos técnicos, eléctricos, dimensionales y de compatibilidad para el ensamblaje de una estación de trabajo de alta gama para edición y renderizado 3D.

## Requirements

### Requirement: Socket and Processor Compatibility
La placa base SHALL alojar el procesador de 16 núcleos en el zócalo AM5 con soporte eléctrico para un PPT sostenido de al menos 230W mediante VRM de 12+2 fases.

#### Scenario: Instalación del procesador en el zócalo
- **WHEN** el procesador AMD Ryzen 9 7950X es insertado en el zócalo AM5 de la placa base ASUS TUF Gaming B650-PLUS WIFI
- **THEN** el mecanismo de retención se cierra sin fuerza anormal y el sistema de alimentación suministra voltajes estables VDDCR_CPU en render continuo.

### Requirement: Memory Subsystem Clearance and EXPO Stability
El subsistema de memoria SHALL operar en modo dual channel a 6000 MT/s CL30 con perfil AMD EXPO sin colisionar mecánicamente con ningún componente del sistema de refrigeración.

#### Scenario: Activación de perfil AMD EXPO en BIOS
- **WHEN** el usuario activa el perfil AMD EXPO en la BIOS UEFI con 2 módulos DDR5 de 32 GB instalados en las ranuras A2 y B2
- **THEN** el sistema arranca con frecuencia efectiva de 6000 MHz y latencia CL30 manteniendo una holgura física superior a 25 mm respecto al radiador superior.

### Requirement: Graphics Card Physical Clearance and Power Interface
El chasis y la fuente de alimentación SHALL admitir una tarjeta gráfica de hasta 340 mm de longitud y 3.2 ranuras de grosor con conexión de potencia nativa PCIe 5.0 12V-2x6 de 600W.

#### Scenario: Montaje mecánico y conexión eléctrica de la GPU
- **WHEN** la tarjeta gráfica NVIDIA RTX 4080 SUPER es fijada a la primera ranura PCIe 4.0/5.0 x16 y conectada mediante el cable directo 12V-2x6
- **THEN** queda una distancia libre superior a 100 mm hacia el frontal del chasis y más de 45 mm hacia el panel lateral de cristal templado.

### Requirement: Thermal Dissipation and Cooling Pressure
El sistema de refrigeración líquida AIO de 360 mm y los ventiladores del chasis SHALL mantener la CPU por debajo de 85°C bajo carga máxima sostenida de renderizado preservando presión positiva.

#### Scenario: Carga sostenida de renderizado en Blender Cycles
- **WHEN** se ejecuta un render continuo de 60 minutos con la CPU al 100% de utilización y la GPU al 100% de utilización simultánea
- **THEN** la temperatura del procesador se mantiene inferior a 85°C y el flujo de aire entrante frontal supera al caudal de extracción para prevenir polvo.

### Requirement: Power Supply Headroom and Transient Spike Protection
La fuente de alimentación SHALL suministrar un mínimo de 850W con estándar ATX 3.0 y capacidad de absorción de transitorios del 200% para un raíl único de +12V de al menos 70A.

#### Scenario: Pico transitorio de consumo en viewport 3D en tiempo real
- **WHEN** la GPU experimenta un pico de consumo instantáneo de hasta 600W durante la carga de geometrías complejas
- **THEN** la fuente de alimentación entrega la corriente requerida en el raíl de +12V sin activar erróneamente las protecciones OCP/OPP ni provocar reinicios.

### Requirement: Dual High-Speed NVMe Storage Layout
El subsistema de almacenamiento SHALL disponer de dos unidades NVMe PCIe 4.0 x4 independientes refrigeradas por los disipadores pasivos de la placa base con anclaje M.2 Q-Latch.

#### Scenario: Escritura masiva simultánea de metraje RAW y caché
- **WHEN** se transfieren archivos de vídeo RAW al SSD principal mientras el motor de renderizado escribe secuencias EXR en el SSD secundario
- **THEN** ambas unidades mantienen tasas de transferencia superiores a 4000 MB/s sostenidas con temperaturas de controladora inferiores a 65°C.
