# OpenSpec: Diseño y Presupuesto de Workstation 3D con SDD & ODD

[![OpenSpec](https://img.shields.io/badge/OpenSpec-v2.1.0-blue.svg)](https://github.com/Fission-AI/OpenSpec)
[![Metodología](https://img.shields.io/badge/Metodolog%C3%ADa-SDD%20%2B%20ODD-green.svg)](#)
[![Git Branches](https://img.shields.io/badge/Git-Feature%20Branches-orange.svg)](#)
[![Hardware](https://img.shields.io/badge/Hardware-Ryzen%209%207950X%20%7C%20RTX%204080%20SUPER-purple.svg)](#)
[![Licencia](https://img.shields.io/badge/Entrega-Pr%C3%A1ctica%201%20APOF-success.svg)](#)

Repositorio oficial para la **Práctica 1 (sin código): Diseño y Presupuesto de un Servidor o PC de Ensamblaje con OpenSpec**, aplicando **Specification-Driven Development (SDD)** y **Organic Development (ODD)** en equipo asistido por **Subagentes de Inteligencia Artificial**.

> **Repositorio GitHub:** [https://github.com/aitorino2009/OpenSpec-1](https://github.com/aitorino2009/OpenSpec-1)  
> **Memoria Completa de Entrega:** [`ENTREGA_PRACTICA_1.md`](./ENTREGA_PRACTICA_1.md)

---

## 🎯 Objetivo de la Práctica

Diseñar, presupuestar y validar la arquitectura técnica y termomecánica de una estación de trabajo informática de alta gama para **edición de vídeo 4K/6K en tiempo real (DaVinci Resolve Studio) y renderizado 3D (Blender Cycles, Chaos V-Ray y Unreal Engine 5)**, garantizando la compatibilidad del 100% en:
- Dimensiones y tolerancias físicas (*Clearance* de memoria RAM vs radiador AIO, longitud de GPU vs chasis).
- Estabilidad eléctrica ante picos transitorios (estándar **ATX 3.0** y conector nativo blindado **12V-2x6** de 600W).
- Régimen térmico continuo de 230W en CPU y 320W en GPU con **presión estática positiva** en el chasis.

---

## 🖥️ Ficha Técnica del Equipo Ensamblado ("Workstation Pro 3D")

| Componente | Modelo Seleccionado | Especificaciones Principales |
| :--- | :--- | :--- |
| **Procesador (CPU)** | **AMD Ryzen 9 7950X** | 16 núcleos / 32 hilos, 5.7 GHz boost, 80 MB Caché, Socket AM5, PPT 230W |
| **Placa Base** | **ASUS TUF GAMING B650-PLUS WIFI** | Formato ATX, VRM 12+2 fases (60A DrMOS), PCIe 5.0 M.2, Wi-Fi 6, 2.5 GbE |
| **Memoria RAM** | **Corsair Vengeance DDR5 64 GB (2x32 GB)** | 6000 MT/s CL30-36-36-76 @ 1.35V con perfil AMD EXPO I, Altura 35 mm |
| **Tarjeta Gráfica (GPU)** | **ASUS TUF Gaming GeForce RTX 4080 SUPER 16GB** | 10.240 CUDA Cores, 320 Tensor Cores, 80 RT Cores, 736 GB/s, Dual AV1 NVENC |
| **Almacenamiento SO** | **Samsung 990 PRO 1 TB NVMe M.2** | PCIe 4.0 x4, Lectura 7.450 MB/s, Escritura 6.900 MB/s, 1 GB DRAM LPDDR4 |
| **Almacenamiento Datos**| **Kingston KC3000 2 TB NVMe M.2** | PCIe 4.0 x4, Phison E18, 7.000 MB/s, Durabilidad extrema de **1.600 TBW** |
| **Fuente de Alimentación**| **Corsair RM850x SHIFT 850W 80+ Gold** | Modular, ATX 3.0 / PCIe 5.0, Raíl único +12V (70.8A), Cable nativo 12V-2x6 |
| **Refrigeración Líquida**| **ARCTIC Liquid Freezer III 360** | Radiador de 38 mm grosor, 3x ventiladores P12 PWM PST, Ventilador para VRM |
| **Chasis / Gabinete** | **Fractal Design Meshify 2 Black Solid** | Semitorre ATX de alto flujo, Open Layout (GPU hasta 467 mm), Filtros de nailon |
| **Presupuesto Total** | **2.766,30 € (IVA 21% incluido)** | Montaje, certificación, actualización de BIOS y garantía incluidos |

---

## 📂 Estructura Documental del Repositorio

- **[`ENTREGA_PRACTICA_1.md`](./ENTREGA_PRACTICA_1.md)**: Documento unificado maestro de entrega con el desarrollo de todos los pasos y conclusiones.
- **[`docs/01-exploracion-requisitos.md`](./docs/01-exploracion-requisitos.md)**: Paso 2 — Exploración con `/opsx:explore`, comparativa de refrigeración líquida vs aire y análisis de riesgos eléctricos.
- **[`docs/subsistema-proceso-energia.md`](./docs/subsistema-proceso-energia.md)**: Paso 4 — Informe del Alumno A (Subagente A: CPU, Placa Base, RAM, Fuente y transitorios).
- **[`docs/subsistema-graficos-chasis.md`](./docs/subsistema-graficos-chasis.md)**: Paso 4 — Informe del Alumno B (Subagente B: GPU, Almacenamiento NVMe dual, Chasis, Flujo de aire y tolerancias).
- **[`docs/presupuesto-detallado.md`](./docs/presupuesto-detallado.md)**: Paso 5 — Lista de materiales (BOM), precios desglosados con IVA, garantías y retorno de inversión (ROI).
- **[`docs/validacion-prueba-fuego-benchmarks.md`](./docs/validacion-prueba-fuego-benchmarks.md)**: Paso 6 — Matriz de compatibilidad física al milímetro y estimación de benchmarks oficiales (Blender, Cinebench, DaVinci, V-Ray, 3DMark).
- **[`docs/respuestas-preguntas-finales.md`](./docs/respuestas-preguntas-finales.md)**: Respuestas rigurosas a las preguntas finales del ejercicio.
- **`openspec/`**: Estructura nativa de OpenSpec con las propuestas de cambio (`openspec/changes/diseno-workstation-render-3d/`) y especificaciones definitivas (`openspec/specs/workstation-hardware/spec.md`).

---

## 🌿 Flujo de Trabajo en Git y Ramas en GitHub

El proyecto se gestionó siguiendo el estándar industrial de ramas por funcionalidad (*Feature Branches*):

```text
main ───────────────────────────────────────────────────────────────────────────────► (Entrega)
  │                                    ▲                     ▲            ▲
  ├─ feature/paso-2-exploracion-hardware ┘                     │            │
  ├─ feature/paso-3-propuesta-tecnica-openspec ──────────────┘            │
  ├─ feature/odd-alumno-a-proceso-energia (Subagente A) ──────┐            │
  ├─ feature/odd-alumno-b-graficos-chasis (Subagente B) ──────┼────────────┤
  ├─ feature/paso-5-presupuesto-unificacion ──────────────────┘            │
  └─ feature/paso-6-prueba-de-fuego-benchmarks ────────────────────────────┘
```

1. **`chore(init): inicializar estructura openspec y repositorio git`**
2. **`feat(explore): documentar exploracion de requisitos de hardware, refrigeracion y analisis de potencia (paso 2)`**
3. **`feat(propose): definir artefactos tecnicos openspec proposal, design, spec y tasks (paso 3)`**
4. **`feat(odd): subsistema de proceso y energia desarrollado por alumno a (subagente a)`**
5. **`feat(odd): subsistema de graficos almacenamiento y chasis desarrollado por alumno b (subagente b)`**
6. **`feat(apply): consolidar especificaciones definitivas openspec y presupuesto detallado con iva (paso 5)`**
7. **`feat(verify): prueba de fuego tecnica, analisis de benchmarks y memoria final de entrega (paso 6)`**

---

## ⚡ Comandos Útiles de OpenSpec

Para verificar la integridad del proyecto en un entorno local:

```bash
# Validar el cambio propuesto
openspec validate diseno-workstation-render-3d

# Ver el estado de tareas y artefactos
openspec status --change diseno-workstation-render-3d

# Listar las especificaciones consolidadas
openspec list --specs
```
