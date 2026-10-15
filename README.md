# Servidor de pruebas para una plataforma universitaria con Proxmox

Este repositorio reúne el diagnóstico del servidor del laboratorio 302 y una propuesta para organizarlo como plataforma universitaria de virtualización. El informe está dirigido a apoyar decisiones sobre capacidad, ampliaciones, separación de entornos, operación y recuperación.

## Documentos

- [Informe técnico de la plataforma Proxmox VE](Informe%20t%C3%A9cnico%20plataforma%20Proxmox%20VE,%20Laboratorio%20302.docx): diagnóstico, arquitectura propuesta, distribución de recursos, ampliaciones, operación, recuperación, evolución y guía de estudio del Hito 3.
- [Diagnóstico del proceso de aprendizaje](Pront%20Espejo.txt): reflexión sobre fortalezas, aspectos por reforzar y acciones de preparación para el Hito 3.

## Estado observado

La auditoría identifica un **HP Z820** con dos procesadores **Intel Xeon E5-2630 v2**, 24 hilos lógicos en total y aproximadamente **15 GiB de RAM detectados**. La ficha de HP establece hasta **512 GB de RAM** con dos procesadores, sujeto a usar módulos compatibles.

Las capturas actuales muestran ocho VM regulares y una plantilla. La configuración común reportada es 512 MiB de RAM y 6 vCPU. Los discos confirmados son 24 GB para la VM 104, 22 GB para la VM 111 y 20 GB para la plantilla 107.

El pool `local-lvm` tiene **198,61 GB**, con **184,69 GB ocupados** y unos **13,92 GB libres** (**92,99 % de uso**). Este almacenamiento casi lleno es la limitación inmediata. El inventario físico de discos y los módulos de RAM instalados todavía deben verificarse antes de comprar componentes.

## Propuesta resumida

| Entorno | RAM objetivo | vCPU objetivo | Almacenamiento inicial |
|---|---:|---:|---:|
| Producción | 48 GiB | 8 | 400 GB |
| Laboratorios docentes | 96 GiB total; 24 GiB por docente | 16 total; 4 por docente | 600 GB total; 150 GB por docente |
| Desarrollo | 32 GiB | 6 | 250 GB |
| Pruebas | 16 GiB | 4 | 150 GB |

La recomendación inicial es llevar la memoria a **256 GiB** y añadir almacenamiento redundante para las VM, sujeto a confirmar bahías, controlador y compatibilidad. Los respaldos deben guardarse en un destino independiente del HP Z820 y verificarse mediante pruebas periódicas de restauración.

## Alcance y limitaciones

Esta propuesta describe un diseño; **no significa que las ampliaciones o configuraciones ya se hayan implementado**. El servidor actual es un único nodo, por lo que puede usar VM, LXC, pools, permisos, plantillas, snapshots, backups externos y API, pero no ofrece alta disponibilidad ni migración entre hosts. Para esas funciones se requieren nodos adicionales, quorum, redes adecuadas y almacenamiento disponible para los nodos de recuperación.

Las cuotas propuestas son un punto de partida. Deben revisarse con métricas de carga tomadas durante clases y con el número real de estudiantes concurrentes. Los precios del informe son referencias de mercados extranjeros; no incluyen importación, impuestos ni disponibilidad local.
