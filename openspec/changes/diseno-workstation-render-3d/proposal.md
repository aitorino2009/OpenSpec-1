# Proposal: Diseño y Presupuesto de Workstation de Ensamblaje para Edición y Renderizado 3D

## Why

El departamento de producción audiovisual y modelado 3D (o centro educativo de formación técnica) requiere una estación de trabajo de alto rendimiento capaz de reducir drásticamente los tiempos de cálculo en renders fotorrealistas (Blender Cycles, V-Ray, Unreal Engine 5) y permitir la manipulación fluida de metraje 4K/6K en DaVinci Resolve Studio.

Actualmente, los equipos existentes sufren bloqueos por falta de memoria VRAM/RAM, cuellos de botella térmicos (thermal throttling) y tiempos de entrega inaceptables. Esta propuesta define formalmente la arquitectura de hardware previa a cualquier compra, aplicando la metodología Specification-Driven Development (SDD) para erradicar incompatibilidades mecánicas, eléctricas y térmicas antes del montaje físico.

## What Changes

- **Implementación de plataforma de cómputo de alta gama AM5**: Integración del procesador AMD Ryzen 9 7950X (16 núcleos / 32 hilos) sobre placa base ASUS TUF Gaming B650-PLUS WIFI.
- **Aceleración gráfica profesional**: Incorporación de GPU NVIDIA GeForce RTX 4080 SUPER 16 GB GDDR6X con núcleos RT (Ray Tracing) y Tensor para aceleración OptiX y DLSS.
- **Subsistema de memoria de alta velocidad**: Adopción de 64 GB DDR5 (2x32 GB) a 6000 MHz CL30 con certificación AMD EXPO para preservar la sincronización de reloj 1:1.
- **Almacenamiento NVMe dual segregado**: Unidad primaria Samsung 990 Pro de 1 TB para SO y aplicaciones, y unidad secundaria Kingston KC3000 de 2 TB con alto TBW para caché y proyectos.
- **Alimentación y gestión energética ATX 3.0**: Fuente modular Corsair RM850x SHIFT de 850W con certificación 80 Plus Gold y conector nativo 12V-2x6 (PCIe 5.0).
- **Control térmico por refrigeración líquida AIO 360 mm y chasis mesh**: Disipador Arctic Liquid Freezer III 360 en caja Fractal Design Meshify 2 con flujo de presión positiva.

## Capabilities

### New Capabilities

- `workstation-hardware`: Especificación técnica, eléctrica, dimensional y de compatibilidad para el ensamblaje de la estación de trabajo de edición y renderizado 3D.

### Modified Capabilities

*(Ninguna capacidad preexistente se modifica, se trata de una especificación de nueva creación).*

## Impact

- **Presupuesto y Adquisición**: Estimación de inversión controlada entre 2.600 € y 3.000 € con componentes estandarizados de máxima durabilidad y garantía oficial.
- **Espacio y Ergonomía**: Equipo de formato semitorre ATX silencioso (<38 dBA a plena carga) apto para entornos de estudio y aulas de desarrollo.
- **Mantenimiento**: Ensamblaje modular sin herramientas complejas, filtros antipolvo extraíbles y canalizaciones de cableado seguras.
