# MEMORIA DE ENTREGA DE LA PRÁCTICA 1 (SIN CÓDIGO)
## OPENSPEC PARA EMPRESA HARDWARE: DISEÑO Y PRESUPUESTO DE UN SERVIDOR O PC DE ENSAMBLAJE

---

### Ficha Técnica de la Entrega
- **Materia / Módulo:** Arquitectura y Planificación de Hardware / Specification-Driven Development (SDD)
- **Metodologías Aplicadas:** 
  - **SDD (Specification-Driven Development)** mediante **OpenSpec v2.1.0**
  - **ODD (Organic Development)** mediante trabajo en paralelo por subsistemas con **Subagentes de IA**
  - **Control de Versiones:** Git & GitHub con flujo de ramas por funcionalidad (*Feature Branches*) y confirmaciones atómicas (*Conventional Commits*)
- **Repositorio Oficial de Entrega:** [https://github.com/aitorino2009/OpenSpec-1](https://github.com/aitorino2009/OpenSpec-1)
- **Fecha de Realización:** 23 de Noviembre de 2026

---

## 1. Resumen Ejecutivo y Objetivos

El objetivo de esta práctica es planificar, diseñar y validar técnicamente una estación de trabajo informática de altas prestaciones para tareas profesionales de **edición de vídeo 4K/6K en tiempo real (DaVinci Resolve Studio) y renderizado 3D fotorrealista (Blender Cycles, Chaos V-Ray, Unreal Engine 5)**.

Mediante la metodología **SDD (Specification-Driven Development)**, toda la arquitectura de hardware se define y documenta en especificaciones formales (`proposal.md`, `design.md`, `spec.md`, `tasks.md`) antes de emitir cualquier propuesta comercial o realizar compras físicas. Esto permite:
1. Erradicar incompatibilidades físicas y colisiones de espacio (altura de memorias RAM, dimensiones de tarjeta gráfica, grosor del radiador de refrigeración líquida).
2. Evitar estrangulamiento térmico (*thermal throttling*) y dimensionar correctamente el flujo de aire con presión estática positiva.
3. Asegurar la estabilidad eléctrica ante picos transitorios de demanda mediante el estándar **ATX 3.0** y conectividad directa **12V-2x6 (PCIe 5.0)**.
4. Distribuir el trabajo técnico en equipo mediante **ODD (Organic Development)** con subagentes independientes para los subsistemas de **Proceso/Energía** y **Gráficos/Almacenamiento/Chasis**.

---

## 2. Estructura y Navegación del Proyecto

El repositorio está estructurado de acuerdo a las directrices de OpenSpec y las buenas prácticas de ingeniería de proyectos:

```text
OpenSpec-1/
├── .gitignore                                      # Filtro de archivos no deseados
├── README.md                                       # Portada principal del repositorio GitHub
├── ENTREGA_PRACTICA_1.md                           # Memoria técnica completa de la entrega
│
├── docs/                                           # Informes de ingeniería y análisis detallados
│   ├── 01-exploracion-requisitos.md                # Paso 2: Exploración interactiva (/opsx:explore)
│   ├── subsistema-proceso-energia.md               # Paso 4: Informe del Alumno A (Subagente A)
│   ├── subsistema-graficos-chasis.md               # Paso 4: Informe del Alumno B (Subagente B)
│   ├── presupuesto-detallado.md                    # Paso 5: Propuesta económica desglosada con IVA
│   ├── validacion-prueba-fuego-benchmarks.md       # Paso 6: Prueba de fuego técnica y benchmarks
│   └── respuestas-preguntas-finales.md             # Preguntas finales justificadas para evaluación
│
└── openspec/                                       # Entorno de trabajo OpenSpec
    ├── config.yaml                                 # Configuración global OpenSpec (schema: spec-driven)
    ├── changes/                                    # Registro de cambios y propuestas técnicas
    │   └── diseno-workstation-render-3d/
    │       ├── .openspec.yaml                      # Metadatos del cambio
    │       ├── README.md                           # Descripción del cambio
    │       ├── proposal.md                         # Propuesta técnica formal (Why, What, Capabilities)
    │       ├── design.md                           # Decisiones de diseño, arquitectura y mitigaciones
    │       ├── spec.md                             # Especificación técnica con criterios SHALL/MUST
    │       ├── tasks.md                            # Checklist de tareas 100% completadas (12/12)
    │       └── specs/workstation-hardware/spec.md  # Delta formal de la capacidad
    └── specs/                                      # Especificaciones consolidadas del sistema
        └── workstation-hardware/spec.md            # Especificación final aprobada y válida
```

---

## 3. Desarrollo Paso a Paso de la Práctica

### Paso 1: Inicialización del Entorno OpenSpec y Repositorio Git
- **Comandos ejecutados:**
  ```powershell
  git init -b main
  git remote add origin https://github.com/aitorino2009/OpenSpec-1
  openspec init --tools "antigravity,github-copilot" --language es --no-animation
  ```
- **Resultado:** Estructura de carpetas OpenSpec creada con soporte para español y herramientas de IA. Repositorio inicializado y sincronizado con GitHub.

---

### Paso 2: Exploración de Requisitos de Hardware (`/opsx:explore`)
- **Prompt ejecutado:**
  > *"Quiero diseñar la configuración de hardware para un PC de estación de trabajo enfocado a edición 3D. Necesito definir la CPU, placa base, refrigeración, memoria RAM, tarjeta gráfica, almacenamiento NVMe y fuente de alimentación, asegurando que no haya incompatibilidades de zócalo, espacio en caja o potencia eléctrica."*
- **Análisis de Componentes Propuestos:**
  - **CPU:** AMD Ryzen 9 7950X (16 núcleos / 32 hilos en Socket AM5). Frecuencias de hasta 5.7 GHz boost y 80 MB de caché L2+L3.
  - **Placa Base:** ASUS TUF GAMING B650-PLUS WIFI (Formato ATX, VRM 12+2 fases DrMOS a 60A, soporte PCIe 5.0 M.2).
  - **Memoria RAM:** Corsair Vengeance 64 GB DDR5 (2x32 GB) a 6000 MHz CL30 con perfil AMD EXPO I.
  - **Tarjeta Gráfica:** ASUS TUF Gaming GeForce RTX 4080 SUPER 16 GB GDDR6X (Arquitectura Ada Lovelace, aceleración OptiX y dual NVENC AV1).
  - **Almacenamiento NVMe dual:** Samsung 990 Pro 1 TB (SO) + Kingston KC3000 2 TB (Scratch disk y proyectos, 1.600 TBW).
  - **Fuente de Alimentación:** Corsair RM850x SHIFT 850W 80 Plus Gold / ATX 3.0 con cable nativo 12V-2x6.
  - **Refrigeración:** ARCTIC Liquid Freezer III 360 (radiador de 38 mm con ventilador VRM integrado).
  - **Caja:** Fractal Design Meshify 2 (semitorre ATX de flujo abierto con frontal mallado y filtros antipolvo).
- **Análisis de Refrigeración (Líquida vs Aire):**
  - La CPU presenta un consumo sostenido en render de **230W PPT** alcanzando hasta 95°C si no se disipa adecuadamente.
  - La refrigeración líquida AIO de 360 mm montada en el techo (Top Exhaust) garantiza temperaturas de **76°C a 82°C** continuas a 5.2 GHz, superando la limitación de disipadores de aire doble torre que colisionan con las ranuras de RAM y sobrecargan mecánicamente la placa base.
- **Riesgos Técnicos Eléctricos Detectados:**
  - Picos transitorios de demanda (hasta un 200% del TGP de la GPU en microsegundos) absorbidos por el estándar ATX 3.0.
  - Eliminación del adaptador de 8 pines gracias al cable nativo blindado 12V-2x6 de 600W.
  - Raíl único de +12V de 70.8A con un consumo máximo estimado de 693W, operando en la franja de eficiencia óptima del 60%-75%.
- **Documento generado:** [`docs/01-exploracion-requisitos.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/docs/01-exploracion-requisitos.md).
- **Rama Git:** `feature/paso-2-exploracion-hardware` (fusionada en `main`).

---

### Paso 3: Generación de la Propuesta Técnica (`/opsx:propose`)
- **Comando ejecutado:**
  ```powershell
  openspec new change diseno-workstation-render-3d --description "Diseño y especificación técnica de Workstation para edición y renderizado 3D"
  ```
- **Artefactos clave generados y validados:**
  1. [`proposal.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/openspec/changes/diseno-workstation-render-3d/proposal.md): Justificación del problema, objetivos y declaración de la nueva capacidad `workstation-hardware`.
  2. [`design.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/openspec/changes/diseno-workstation-render-3d/design.md): Criterios arquitectónicos, comparativa frente a alternativas (Intel Core i9-14900K, AMD RX 7900 XTX) y mitigación de riesgos de montaje.
  3. [`spec.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/openspec/changes/diseno-workstation-render-3d/spec.md): Requisitos normativos *SHALL/MUST* con 6 escenarios de verificación *WHEN/THEN*.
  4. [`tasks.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/openspec/changes/diseno-workstation-render-3d/tasks.md): Desglose estructurado de 12 tareas operativas divididas por responsabilidades.
- **Validación OpenSpec:** `openspec validate diseno-workstation-render-3d` -> **Válido (100%)**.
- **Rama Git:** `feature/paso-3-propuesta-tecnica-openspec` (fusionada en `main`).

---

### Paso 4: Trabajo Colaborativo y Responsabilidad por Subsistemas (ODD) con Subagentes
Siguiendo la metodología ODD (Organic Development), se crearon dos subagentes especializados que investigaron y redactaron en paralelo sus respectivos subsistemas:

#### Alumno A — Subagente de Proceso y Energía:
- **Responsabilidad:** CPU AMD Ryzen 9 7950X, Placa Base ASUS TUF B650-PLUS WIFI, Memoria Corsair 64GB DDR5 6000MHz CL30 EXPO y Fuente Corsair RM850x SHIFT ATX 3.0.
- **Entregable:** [`docs/subsistema-proceso-energia.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/docs/subsistema-proceso-energia.md) (456 líneas de análisis técnico).
- **Rama Git:** `feature/odd-alumno-a-proceso-energia` (fusionada en `main`).

#### Alumno B — Subagente de Gráficos, Almacenamiento y Chasis:
- **Responsabilidad:** GPU ASUS TUF RTX 4080 SUPER 16GB, Almacenamiento dual Samsung 990 Pro + Kingston KC3000, Chasis Fractal Meshify 2 y Refrigeración AIO Arctic Liquid Freezer III 360 con dinámica de fluidos y presión positiva.
- **Entregable:** [`docs/subsistema-graficos-chasis.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/docs/subsistema-graficos-chasis.md) (409 líneas de análisis técnico).
- **Rama Git:** `feature/odd-alumno-b-graficos-chasis` (fusionada en `main`).

---

### Paso 5: Aplicación, Consolidación de Especificaciones y Presupuesto Comercial (`/opsx:apply`)
- **Consolidación en OpenSpec:** Se generó y validó la especificación definitiva en [`openspec/specs/workstation-hardware/spec.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/openspec/specs/workstation-hardware/spec.md) con 6 requisitos técnicos aprobados.
- **Propuesta Económica:** Se elaboró el presupuesto comercial con precios de mercado oficiales en España, desglosando base imponible, 21% de IVA, garantías y retorno de inversión en [`docs/presupuesto-detallado.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/docs/presupuesto-detallado.md):
  - **Inversión Total Llave en Mano:** **2.766,30 € (IVA incluido)**.
- **Rama Git:** `feature/paso-5-presupuesto-unificacion` (fusionada en `main`).

---

### Paso 6: Comprobación y "Prueba de Fuego" Técnica + Benchmarks
- **Validación Física y Eléctrica:**
  - *Clearance RAM:* Módulos de 35 mm con despeje total bajo radiador desplazado hacia el lateral en chasis Meshify 2.
  - *GPU Clearance:* 348.2 mm instalados en 442 mm disponibles (**93.8 mm libres** de flujo directo hacia ventiladores frontales).
  - *Cable 12V-2x6:* 52 mm de holgura lateral recta frente a los 35 mm mínimos exigidos.
  - *Conectores de Placa:* Doble EPS 8 pines independiente desde la fuente para mitigar calor en los terminales.
- **Benchmarks Contrastados en Bases de Datos Oficiales:**
  - **Blender 4.2 LTS Cycles OptiX:** *Monster* a 4.685 samples/min, *Junkshop* a 2.215 samples/min, *Classroom* a 2.340 samples/min (**reducción de tiempos de hasta un 85% frente a CPU**).
  - **Cinebench R23:** **38.850 pts Multi-Core** (5.2 GHz sostenidos en los 16 núcleos sin throttling) y **2.055 pts Single-Core**.
  - **DaVinci Resolve Studio 19:** Puntuación Puget de **3.280 pts**; exportación dual AV1 a 180 fps en 4K.
  - **V-Ray 6:** 29.600 vsamples (CPU) y 5.450 vrpaths (GPU RTX).
- **Documento generado:** [`docs/validacion-prueba-fuego-benchmarks.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/docs/validacion-prueba-fuego-benchmarks.md).
- **Rama Git:** `feature/paso-6-prueba-de-fuego-benchmarks`.

---

## 4. Respuestas a las Preguntas Finales de Evaluación

### Pregunta 1: ¿Por qué es fundamental documentar las especificaciones físicas y eléctricas (`spec.md`) antes de proponer la compra de componentes de hardware?
> **Respuesta Resumida:** *(Véase el desarrollo completo en [`docs/respuestas-preguntas-finales.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/docs/respuestas-preguntas-finales.md))*.
> Es fundamental porque evita pérdidas económicas y retrasos causados por incompatibilidades mecánicas invisibles a simple vista (como colisiones entre radiadores de 65 mm y memorias RAM altas, o tarjetas gráficas de 35 cm que no caben en cajas estándar). Además, protege la integridad de los equipos al garantizar que la fuente de alimentación soporte los picos transitorios de potencia mediante el estándar ATX 3.0 y cuente con conectividad nativa 12V-2x6, erradicando el peligro de sobrecalentamiento y fusión de conectores. Finalmente, fija por contrato las condiciones térmicas que impiden el estrangulamiento (*thermal throttling*), asegurando que el cliente reciba el 100% del rendimiento por el que ha pagado.

### Pregunta 2: ¿Cómo os habéis repartido los subsistemas de hardware en el equipo y qué problemas de compatibilidad física (espacio, conectores) habéis tenido que resolver conjuntamente?
> **Respuesta Resumida:** *(Véase el desarrollo completo en [`docs/respuestas-preguntas-finales.md`](file:///c:/Users/AITORPORTALESCRESPI/OneDrive%20-%20CC%20Sant%20Josep%20Obrer%20I/Documentos/APOF/OpenSpec%20nuevo/docs/respuestas-preguntas-finales.md))*.
> El equipo se dividió mediante **ODD** asignando al **Alumno A** la CPU, placa base, RAM y fuente de alimentación (garantizando compatibilidad de socket AM5, VRM y potencia eléctrica), y al **Alumno B** la tarjeta gráfica, SSDs NVMe, chasis y refrigeración (garantizando dimensiones físicas y flujos de aire).
> Conjuntamente resolvieron 4 problemas críticos:
> 1. *Grosor del AIO (65 mm) vs RAM:* Se seleccionaron módulos de bajo perfil (35 mm) y una caja con raíles superiores desplazados para evitar cualquier colisión.
> 2. *Longitud de la GPU (348 mm):* Se configuró el chasis en *Open Layout* al suprimir bandejas HDD mecánicas, liberando 442 mm de espacio y 93.8 mm de margen para flujo de aire directo.
> 3. *Radio del cable 12V-2x6:* Se comprobó que el ancho interior de 185 mm de la caja deja 52 mm libres, superando con creces los 35 mm de trayectoria recta obligatorios para no deformar los pines.
> 4. *Disipación de SSDs M.2:* Se distribuyeron los discos en las ranuras superior e inferior para evitar que el calor de la GPU asfixiara las unidades de almacenamiento.

---

## 5. Historial de Ramas y Commits en Git / GitHub

El flujo de trabajo se implementó íntegramente utilizando ramas por funcionalidad y confirmaciones estructuradas con el repositorio remoto [https://github.com/aitorino2009/OpenSpec-1](https://github.com/aitorino2009/OpenSpec-1):

| Rama (*Branch*) | Propósito / Hito | Hash Commit | Mensaje de Confirmación |
| :--- | :--- | :---: | :--- |
| `main` | Inicialización de repositorio y OpenSpec | `0bd5d7a` | `chore(init): inicializar estructura openspec y repositorio git` |
| `feature/paso-2-exploracion-hardware` | Paso 2: Análisis de requisitos e IA | `3c37b66` | `feat(explore): documentar exploracion de requisitos de hardware, refrigeracion y analisis de potencia (paso 2)` |
| `feature/paso-3-propuesta-tecnica-openspec` | Paso 3: Artefactos formalizados | `77c08d1` | `feat(propose): definir artefactos tecnicos openspec proposal, design, spec y tasks (paso 3)` |
| `feature/odd-alumno-a-proceso-energia` | Paso 4: Subagente Alumno A | `85639c6` | `feat(odd): subsistema de proceso y energia desarrollado por alumno a (subagente a)` |
| `feature/odd-alumno-b-graficos-chasis` | Paso 4: Subagente Alumno B | `e8bead1` | `feat(odd): subsistema de graficos almacenamiento y chasis desarrollado por alumno b (subagente b)` |
| `feature/paso-5-presupuesto-unificacion` | Paso 5: Presupuesto y spec consolidada | `db68ac1` | `feat(apply): consolidar especificaciones definitivas openspec y presupuesto detallado con iva (paso 5)` |
| `feature/paso-6-prueba-de-fuego-benchmarks` | Paso 6: Prueba de fuego y entrega final | *Actual* | `feat(verify): prueba de fuego tecnica, analisis de benchmarks y memoria final de entrega` |

---
*Fin del documento de entrega oficial.*
