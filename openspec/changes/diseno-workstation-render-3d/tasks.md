# Tasks

## 1. Subsistema de Proceso y Energía (Responsabilidad: Alumno A)

- [x] 1.1 Investigar y validar la compatibilidad física y de socket de la CPU AMD Ryzen 9 7950X con la placa ASUS TUF Gaming B650-PLUS WIFI, verificando las fases VRM (12+2 @ 60A) para soportar 230W continuos
- [x] 1.2 Definir la configuración de memoria RAM (Corsair Vengeance 64 GB DDR5 6000MHz CL30 EXPO), comprobando compatibilidad en la lista QVL de la placa y altura de módulos (35 mm) frente al espacio disponible
- [x] 1.3 Calcular los requerimientos eléctricos detallados, balance de raíles de +12V (70.8A) y seleccionar la fuente Corsair RM850x SHIFT ATX 3.0 con conector nativo 12V-2x6 PCIe 5.0, verificando la absorción de transitorios
- [x] 1.4 Documentar el informe técnico del Subsistema de Proceso y Energía en `docs/subsistema-proceso-energia.md`

## 2. Subsistema de Gráficos, Almacenamiento y Chasis (Responsabilidad: Alumno B)

- [x] 2.1 Seleccionar y validar las dimensiones físicas de la GPU NVIDIA GeForce RTX 4080 SUPER (longitud 304-340 mm, grosor 3.2 slots) frente a las bahías y clearances de la caja
- [x] 2.2 Diseñar la arquitectura de almacenamiento dual NVMe (Samsung 990 Pro 1 TB para SO y Kingston KC3000 2 TB para proyectos/render scratch), comprobando disipación térmica M.2 y líneas PCIe asignadas
- [x] 2.3 Validar el chasis Fractal Design Meshify 2 y el sistema de refrigeración líquida AIO Arctic Liquid Freezer III 360 mm, comprobando clearances en panel superior (despeje >65 mm) y flujo de aire con presión positiva
- [x] 2.4 Documentar el informe técnico del Subsistema de Gráficos, Almacenamiento y Chasis en `docs/subsistema-graficos-chasis.md`

## 3. Integración, Presupuesto y Prueba de Fuego Técnica

- [x] 3.1 Fusionar las especificaciones de ambos subsistemas, verificando la ausencia de conflictos de espacio (RAM vs Radiador, GPU vs Chasis/Cables) y congruencia de conectores eléctricos
- [x] 3.2 Elaborar la propuesta económica y presupuesto desglosado con precios de mercado con IVA en `docs/presupuesto-detallado.md`
- [ ] 3.3 Realizar la validación técnica "Prueba de Fuego" y análisis de benchmarks (Blender, Cinebench, DaVinci, V-Ray, 3DMark) en `docs/validacion-prueba-fuego-benchmarks.md`
- [ ] 3.4 Redactar las respuestas justificadas a las preguntas finales del ejercicio en `docs/respuestas-preguntas-finales.md`

## Workflow follow-up

- Aplicar el cambio con openspec apply para consolidar la especificación en openspec/specs/.
- Crear el informe técnico final y README para entrega en GitHub.
