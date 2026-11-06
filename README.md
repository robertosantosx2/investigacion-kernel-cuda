# Investigación: Megakernels y Hyperkernel

**Investigación estructurada sobre el ecosistema de generación automática de kernels GPU (megakernels / persistent kernels).**

---

## 📌 Descripción

Este repositorio contiene un informe completo sobre **Hyperkernel** (Hyperstition) y los principales proyectos del ecosistema de **megakernels**: sistemas que fusionan el forward pass completo (o grandes porciones) de un modelo en un único lanzamiento de kernel GPU.

El objetivo es eliminar el overhead de lanzamiento de kernels, reducir los viajes a HBM entre operadores, maximizar el solapamiento compute-comunicación y acercar el rendimiento al límite de ancho de banda de memoria (MBU).

---

## 📄 Informe Principal

**[INFORME_MEGAKERNELS_HYPERKERNEL.md](./INFORME_MEGAKERNELS_HYPERKERNEL.md)**

El informe incluye:

- Análisis profundo de **Hyperkernel**
- Fichas detalladas de todos los proyectos similares
- Ventajas e inconvenientes de cada uno
- Licencias y grado de apertura
- Limitaciones de explotación / uso comercial
- Tabla comparativa de madurez y capacidades
- Análisis de trascendencia y aplicabilidad a hardware más pequeño
- Conclusiones y recomendaciones

---

## 🔍 Proyectos Cubiertos

| Proyecto | Enfoque principal | Licencia | Estado |
|----------|-------------------|----------|--------|
| **Hyperkernel** | Generación multi-dominio (LLMs, robótica, genómica, world models...) | MIT | Alpha |
| **Mirage (MPK)** | Compilador + runtime de megakernels multi-GPU para LLMs | Apache-2.0 | Maduro |
| **AutoMegaKernel** | Agente + validación estática de schedules (Llama-family) | MIT | Investigación |
| **Hazy Megakernels** | Referencia de baja latencia (ThunderKittens) | MIT | Investigación |
| **Luminal** | Compilador de inferencia en Rust con e-graphs | Apache-2.0 / MIT | Activo |
| **ForgeMegakernel** | Generación con agentes + oráculo de estados intermedios | Solo paper | Investigación |
| **Ada-MK** | Megakernels en GPUs Ada + integración TensorRT-LLM | Solo paper | Producción interna (Baidu) |

---

## 🧠 Conceptos Clave

- **Megakernel / Persistent Kernel**: Un único kernel que permanece residente y ejecuta el forward pass completo (o un paso de decode) sin devolver el control al host.
- **Kernel Fusion**: Eliminación de lanzamientos intermedios y viajes a HBM.
- **SM-level scheduling**: Programación a nivel de Streaming Multiprocessor con roles especializados (loader, consumer, storer, etc.).
- **Software Pipelining**: Solapamiento de operadores dentro del mismo kernel.
- **Memory Bandwidth Utilization (MBU)**: Métrica clave de eficiencia en regímenes bound por memoria.

---

## 🎯 Utilidad del Repositorio

- Referencia rápida del estado del arte en megakernels (noviembre 2026).
- Comparativa clara de madurez, apertura y capacidades.
- Base para decidir qué proyectos seguir, contribuir o combinar.
- Análisis de viabilidad en hardware más pequeño / edge.

---

## 📅 Fecha de la Investigación

**6 de noviembre de 2026**

---

## 📜 Licencia del Repositorio

Este repositorio y el informe se publican bajo licencia **MIT**.

Los proyectos analizados mantienen sus propias licencias (Apache-2.0, MIT, o solo paper según el caso).

---

## 🔗 Enlaces Rápidos

- [Hyperkernel (Hyperstition)](https://github.com/Hyperstitioncc/Hyperkernel)
- [Mirage Persistent Kernel](https://github.com/mirage-project/mirage)
- [AutoMegaKernel](https://github.com/RightNow-AI/AutoMegaKernel)
- [Hazy Research Megakernels](https://github.com/HazyResearch/Megakernels)
- [Luminal](https://github.com/luminal-ai/luminal)

---

*Repositorio creado como parte de la investigación sobre el ecosistema de generación automática de kernels GPU.*
