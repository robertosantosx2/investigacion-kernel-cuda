# Informe de Investigación: Hyperkernel y el Ecosistema de Megakernels

**Fecha de elaboración:** 6 de noviembre de 2026  
**Fuente principal de análisis:** Repositorio [Hyperstitioncc/Hyperkernel](https://github.com/Hyperstitioncc/Hyperkernel) y proyectos comparables del ecosistema de generación automática / fusión de kernels GPU.

---

## 1. Introducción y Contexto

Este informe consolida toda la investigación realizada sobre **Hyperkernel** (proyecto de Hyperstition) y los proyectos similares del ecosistema de **megakernels** (kernels persistentes que fusionan el forward pass completo o grandes porciones de un modelo en un único lanzamiento de kernel GPU).

Los megakernels buscan eliminar el overhead de lanzamiento de kernels, reducir los viajes de ida y vuelta a HBM entre operadores, mejorar el solapamiento compute-comunicación y acercar el rendimiento al límite de ancho de banda de memoria (Memory Bandwidth Utilization - MBU).

---

## 2. Hyperkernel (Proyecto Principal)

### 2.1 Descripción
Hyperkernel es un **generador recursivo de megakernels** desarrollado por Hyperstition. Se presenta como la solución más completa actual para la generación automática de megakernels de alto rendimiento en múltiples dominios.

A diferencia de la mayoría de sistemas centrados casi exclusivamente en LLMs, Hyperkernel genera kernels optimizados para:
- Inferencia de LLMs (DeepSeek-V4.1-Flash, Kimi, GLM)
- Preentrenamiento (arquitecturas Qwen MoE)
- Robótica (π₀-FAST, ACT)
- Modelos de mundo / difusión en tiempo real (Matrix-Game)
- Conducción autónoma (OpenPilot)
- Modelos de voz (Moshi)
- Genómica (Evo 2)
- Plegamiento de proteínas (ESMFold)

Todo se ejecuta en un **único lanzamiento de kernel**. El sistema es capaz de generar kernels optimizados en menos de 50-60 minutos para varios dominios, utilizando relativamente poco código fuente por modelo (≈60-100 líneas).

### 2.2 Actores clave y perfiles sociales
- **Hyperstition** — X: [@hyperstition_cc](https://x.com/hyperstition_cc) (cuenta oficial del laboratorio)
- **Deven Pietrzak** — X: [@DevenPzak](https://x.com/DevenPzak)

**LinkedIn / Facebook / Instagram:** No se localizaron perfiles públicos verificados de LinkedIn, Facebook o Instagram asociados a Hyperstition o a Deven Pietrzak en el momento de esta investigación. La presencia pública se concentra principalmente en X.

### 2.3 Estado actual del repositorio
- **URL:** https://github.com/Hyperstitioncc/Hyperkernel
- **Estado:** Alpha (noviembre 2026)
- **Contenido actual:** Implementaciones públicas de los megakernels generados (proyecto llamado Hyperserve) + servidor OpenAI-compatible + demos.
- **Anuncio oficial:** Se liberará el generador completo (el “Hyperkernel” propiamente dicho) junto con stacks de serving para Kimi y GLM en las próximas semanas.
- **Hardware principal de referencia:** NVIDIA B300 (extensible a B200, AMD y TPUs según los autores).

### 2.4 Ventajas
- Enfoque verdaderamente **multi-dominio** (uno de los más amplios del ecosistema).
- Generación muy rápida y con alto rendimiento reportado (supera stacks de NVIDIA en varios casos y ofrece 2–3× en dominios no-LLM).
- Código generado de alta calidad y bajo volumen de código fuente.
- Potencial de ser un “one-stop shop” de infraestructura para investigación heterogénea.
- Licencia permisiva.

### 2.5 Inconvenientes
- Todavía en fase **Alpha**.
- Actualmente solo están abiertos los megakernels generados; el generador completo aún no está público.
- Principalmente optimizado y probado en NVIDIA B300.
- Requiere checkpoints de modelos externos (no incluidos) y recursos de hardware elevados en algunos casos (ej. mucha RAM host para LLMs).
- Comunidad y documentación todavía emergentes.

### 2.6 Licencia y apertura
- **Licencia:** MIT
- El código publicado (megakernels + Hyperserve) está bajo MIT.
- El generador completo se ha anunciado como open-source próximo.
- Uso comercial, modificación y redistribución permitidos bajo MIT.

### 2.7 Limitaciones de explotación
- De momento solo se pueden usar/reutilizar los kernels ya generados y publicados.
- Dependencia de hardware específico y de pesos de modelos externos.
- Estado Alpha implica posibles cambios de API y falta de madurez en producción.

---

## 3. Proyectos Similares (Fichas Detalladas)

### 3.1 Mirage Persistent Kernel (MPK)
**Enlaces**  
- Repositorio: https://github.com/mirage-project/mirage  
- Documentación: https://mirage-project.readthedocs.io/  
- Papers: OSDI 2025 (Mirage) y OSDI 2026 (MPK)

**Actores clave y perfiles sociales**  
- **Zhihao Jia** (profesor CMU, líder de Mirage / MPK)  
  - X: [@JiaZhihao](https://x.com/JiaZhihao)  
  - LinkedIn: [linkedin.com/in/jiazhihao](https://www.linkedin.com/in/jiazhihao)  
  - Facebook / Instagram: No se localizaron perfiles públicos verificados.

**Descripción**  
Compilador y runtime que transforma automáticamente la inferencia de LLMs (incluyendo multi-GPU) en un único megakernel. Introduce representación a nivel de SM, software pipelining entre operadores, solapamiento fino de compute y comunicación, y scheduling descentralizado dentro del kernel persistente.

**Ventajas**  
- Madurez relativamente alta y fuerte respaldo académico (CMU + NVIDIA).  
- Reducciones de latencia reportadas de 1.2× a 6.7×.  
- Soporte multi-GPU real.  
- Código completo abierto y bien documentado.

**Inconvenientes**  
- Enfocado principalmente en LLMs.  
- Curva de aprendizaje del stack de compilación.

**Licencia:** Apache-2.0 (stack completo abierto).  
**Limitaciones:** Libre para uso comercial. Algunas demos dependen de modelos gated de Hugging Face.

---

### 3.2 AutoMegaKernel (AMK)
**Enlaces**  
- Repositorio: https://github.com/RightNow-AI/AutoMegaKernel  
- Paper: https://arxiv.org/abs/2606.09682

**Actores clave y perfiles sociales**  
- **Jaber Jaber** (fundador de RightNow AI, autor principal)  
  - X: [@Akashi203](https://x.com/Akashi203)  
  - LinkedIn: [linkedin.com/in/jaber-j-b65246234](https://jo.linkedin.com/in/jaber-j-b65246234)  
  - Facebook / Instagram: No se localizaron perfiles públicos verificados.
- **RightNow AI** — X: [@rightnowai_co](https://x.com/rightnowai_co)  
  - LinkedIn de empresa: [linkedin.com/company/rightnowai](https://www.linkedin.com/company/rightnowai)

**Descripción**  
Harness de agente (compatible con Claude Code / Codex) que compila modelos de la familia Llama de Hugging Face en un único kernel cooperativo persistente. Incluye validación estática de schedules (deadlock-free y race-free), retargeting automático entre arquitecturas (sm_80 / sm_90 / sm_120) y un bucle de autoresearch que mejora el kernel de forma no supervisada.

**Ventajas**  
- Enfoque en corrección formal del schedule.  
- Retargeting multi-arquitectura desde una sola base de código.  
- Capacidad de auto-mejora continua.

**Inconvenientes**  
- Centrado en familia Llama y régimen de batch-1 decode.  
- Rendimiento absoluto todavía por debajo de los mejores kernels manuales en algunos escenarios.

**Licencia:** MIT (versión de investigación abierta). Existe versión enterprise cerrada llamada Forge.  
**Limitaciones:** La versión más potente es comercial. Dependiente de agentes de código externos.

---

### 3.3 Hazy Research Megakernels
**Enlaces**  
- Repositorio: https://github.com/HazyResearch/Megakernels  
- Blog: Look Ma, No Bubbles! (Hazy Research)

**Actores clave y perfiles sociales**  
- **Hazy Research** — X: [@HazyResearch](https://x.com/HazyResearch)  
- **Stuart Sul** (contribuidor clave en ThunderKittens y megakernels)  
  - X: [@stuart_sul](https://x.com/stuart_sul)  
  - Sitio personal: [stuartsul.com](https://stuartsul.com) (enlaza a LinkedIn)  
  - Facebook / Instagram: No se localizaron perfiles públicos verificados.

**Descripción**  
Implementaciones de referencia de megakernels de baja latencia (especialmente para Llama) usando el paradigma de “instruction interpreter” y ThunderKittens. Enfocados en eliminar burbujas de ejecución y maximizar residencia de CTAs.

**Ventajas**  
- Código limpio y educativo.  
- Fuertes resultados de latencia en decode.  
- Buena base para investigación y experimentación.

**Inconvenientes**  
- Más orientado a referencia de investigación que a producto soportado.  
- Menor automatización que Hyperkernel o AMK.

**Licencia:** MIT.  
**Limitaciones:** Libre para uso comercial. Mantenimiento más orientado a investigación.

---

### 3.4 Luminal
**Enlaces**  
- Repositorio: https://github.com/luminal-ai/luminal  
- Documentación: https://docs.luminalai.com  
- Blog: https://blog.luminal.com/p/compiling-models-to-megakernels

**Actores clave y perfiles sociales**  
- **Luminal** — X: [@luminal_ai](https://x.com/luminal_ai)  
- **Joe Fioti** (CEO y co-fundador)  
  - X: [@joefioti](https://x.com/joefioti)  
  - LinkedIn: [linkedin.com/in/joe-fioti-24a986189](https://www.linkedin.com/in/joe-fioti-24a986189)  
  - Facebook / Instagram: No se localizaron perfiles públicos verificados.

**Descripción**  
Compilador de inferencia de alto rendimiento escrito en Rust. Utiliza e-graphs (egglog), búsqueda genética de extracciones y generación de kernels fusionados / megakernels. Enfoque generalista orientado a maximizar rendimiento y minimizar costos de inferencia.

**Ventajas**  
- Stack moderno en Rust.  
- Licencia dual muy permisiva.  
- Búsqueda automática de optimizaciones complejas.

**Inconvenientes**  
- Todavía en evolución activa (reescrituras importantes en la rama principal).  
- Menos enfocado exclusivamente en el paradigma “un solo lanzamiento” que otros proyectos.

**Licencia:** Apache-2.0 o MIT (a elección del usuario).  
**Limitaciones:** Muy permisivo. Algunos ejemplos pueden estar en transición.

---

### 3.5 ForgeMegakernel
**Enlaces**  
- Paper: https://arxiv.org/abs/2609.12379  
- Proyecto relacionado: https://github.com/OpenBMB/ForgeKernel

**Actores clave**  
- Autores principales del paper (Lushan Li, Zhui Zhu, Xianglong Deng y colaboradores de Tsinghua University / ModelBest).  
- Presencia pública limitada en X, LinkedIn, Facebook e Instagram al momento de esta investigación.

**Descripción**  
Framework que usa agentes de código + base de conocimiento de 10 hitos progresivos + oráculo de estados intermedios para generar megakernels de decode específicos por modelo. Reporta altos niveles de MBU (50.5–85.9 %) y speedups geométricos significativos sobre SGLang y compiladores de megakernels.

**Ventajas**  
- Buena generalización a través de familias de modelos.  
- Fuerte énfasis en verificación numérica y de rendimiento durante la generación.

**Inconvenientes**  
- El framework principal no está completamente open-source (solo paper).  
- Coste elevado de generación (horas de agente + tokens).

**Licencia:** Paper bajo licencia arXiv; código del framework principal no público.  
**Limitaciones:** Actualmente no explotable de forma abierta. Requiere contactar a los autores o esperar liberación.

---

### 3.6 Ada-MK
**Enlaces**  
- Paper: https://arxiv.org/abs/2605.11581

**Actores clave**  
- Equipo de Baidu (Wenxin Dong, Mingqing Hu, Guanghui Yu y colaboradores). Proyecto interno de producción.  
- Presencia pública limitada en X, LinkedIn, Facebook e Instagram.

**Descripción**  
Sistema de Baidu orientado a GPUs Ada (L20). Usa modelo de restricciones de shared memory en 3D + splitting de dimensión K, búsqueda offline de DAGs con MLIR y un motor híbrido que integra el megakernel como plugin de TensorRT-LLM. Primer despliegue industrial conocido de megakernels en un sistema comercial de publicidad online.

**Ventajas**  
- Despliegue real en producción.  
- Optimizaciones específicas para hardware con shared memory limitada.  
- Combinación inteligente de prefill (alto throughput) y decode (baja latencia).

**Inconvenientes**  
- Completamente cerrado (solo paper).  
- Orientado a un hardware y caso de uso muy específicos.

**Licencia:** Solo paper (CC BY 4.0 en arXiv).  
**Limitaciones:** No es open-source. No se puede usar ni modificar el código.

---

## 4. Tabla Comparativa de Apertura y Madurez

| Proyecto              | Licencia principal     | Código completo abierto | Uso comercial libre | Madurez          | Dominios principales      | Generación automática |
|-----------------------|------------------------|--------------------------|---------------------|------------------|---------------------------|-----------------------|
| **Hyperkernel**      | MIT                   | Parcial (kernels sí)    | Sí                 | Alpha           | Multi-dominio (11)       | Muy alta             |
| **Mirage (MPK)**     | Apache-2.0            | Sí                      | Sí                 | Alta            | LLMs (multi-GPU)         | Alta                 |
| **Luminal**          | Apache-2.0 / MIT      | Sí                      | Sí                 | Media-Alta      | Inferencia general       | Alta                 |
| **Hazy Megakernels** | MIT                   | Sí                      | Sí                 | Media           | LLMs (baja latencia)     | Baja-Media           |
| **AutoMegaKernel**   | MIT (research)        | Parcial                 | Sí (versión abierta)| Media           | Llama-family decode      | Muy alta             |
| **ForgeMegakernel**  | Paper only            | No                      | No                 | Investigación   | Decode multi-familia     | Alta                 |
| **Ada-MK**           | Paper only            | No                      | No                 | Producción interna | Decode en Ada GPUs     | Media                |

---

## 5. Análisis de Trascendencia y Aplicabilidad a Hardware Más Pequeño

### 5.1 ¿Es trascendente Hyperkernel?
Sí, en el sentido de que representa uno de los esfuerzos más ambiciosos hasta la fecha por generalizar la generación automática de megakernels más allá de los LLMs. Su capacidad de producir kernels competitivos en dominios tan diversos (robótica, genómica, world models, voz, etc.) en tiempos muy cortos lo posiciona como un posible catalizador para que la investigación en áreas no-LLM deje de estar limitada por la falta de equipos de optimización de kernels.

### 5.2 Aplicabilidad a hardware más pequeño
Los megakernels son especialmente valiosos en regímenes de **baja batch size / baja latencia**, donde el overhead de lanzamiento de kernels y los viajes a HBM dominan. En hardware más pequeño (GPUs de consumo, edge, o GPUs con menor ancho de banda):

- La eliminación de kernel launches y la maximización de residencia de datos en shared memory / registros pueden dar ganancias relativamente mayores.
- Proyectos como Ada-MK demuestran que se pueden adaptar las técnicas a arquitecturas con shared memory más limitada mediante splitting y búsqueda offline de schedules.
- El reto principal es el retargeting automático (Hyperkernel y AMK ya trabajan en esta dirección) y la reducción del footprint de memoria de los kernels generados.

En resumen: las técnicas de megakernel son **más relevantes**, no menos, cuando el hardware es más pequeño o el régimen es más latency-bound.

---

## 6. Conclusiones y Recomendaciones

1. **Hyperkernel** destaca por su amplitud de dominios y velocidad de generación. Es el proyecto más interesante si se busca un generador generalista.
2. **Mirage (MPK)** es actualmente la opción más madura y abierta para producción / investigación seria en LLMs multi-GPU.
3. **AutoMegaKernel** es excelente si se valora la verificación estática de schedules y el retargeting multi-arquitectura.
4. Para investigación educativa o experimentación ligera, **Hazy Research Megakernels** y **Luminal** son muy recomendables.
5. Los proyectos cerrados (ForgeMegakernel y Ada-MK) muestran el potencial industrial, pero limitan la reproducción y extensión por la comunidad.

**Recomendación práctica:**  
- Seguir de cerca la liberación del generador completo de Hyperkernel.  
- Combinar ideas de Mirage (runtime robusto) + AMK (validación estática) + Hyperkernel (amplitud de dominios) como base para futuros sistemas.

---

## 7. Referencias Materias

- Hyperkernel: https://github.com/Hyperstitioncc/Hyperkernel
- Mirage: https://github.com/mirage-project/mirage
- AutoMegaKernel: https://github.com/RightNow-AI/AutoMegaKernel + arXiv:2606.09682
- Hazy Megakernels: https://github.com/HazyResearch/Megakernels
- Luminal: https://github.com/luminal-ai/luminal
- ForgeMegakernel: arXiv:2609.12379
- Ada-MK: arXiv:2605.11581

---

*Informe generado y estructurado a partir de la investigación completa realizada en noviembre de 2026. Todos los detalles de licencias, enlaces, ventajas, inconvenientes, estado de apertura y actores clave (incluyendo perfiles sociales disponibles) se han preservado.*

**Nota metodológica sobre redes sociales:** Se buscaron perfiles públicos de LinkedIn, Facebook e Instagram de todos los actores clave. La mayoría de estos investigadores y founders tienen presencia profesional principalmente en X (Twitter) y LinkedIn. No se localizaron perfiles públicos verificados de Facebook o Instagram asociados a su trabajo profesional en el momento de la investigación.
