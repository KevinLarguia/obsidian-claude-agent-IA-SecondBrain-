---
estado: activo
fecha-inicio: 2026-04-23
fecha-objetivo: 2026-05-15
tags:
  - proyecto
  - estado/activo
  - prioridad/alta
---

# Proyecto Principal — Sistema de Análisis de Datos

## 📝 Descripción

> Dataset real de 20 años de un negocio familiar: registros manuales en Excel migrados a un sistema de gestión con dashboards interactivos en Power BI.

El proyecto convierte datos históricos fragmentados y sin estructura en un sistema de reporting que permite tomar decisiones con información real. El cliente es real, el problema es real, los datos son reales.

## 🎯 Objetivos

1. Limpiar y normalizar 20 años de datos históricos
2. Construir modelo de datos relacional en SQL
3. Desarrollar dashboards con KPIs principales en Power BI

## 📌 Decisiones clave

| Fecha | Decisión | Razón |
|-------|----------|-------|
| 2026-04-23 | Usar Power BI en lugar de Tableau | Licencia disponible, integración con Excel directa |
| 2026-04-24 | Anonimizar datos antes de publicar | Proteger información del negocio |
| 2026-04-25 | Agregar capa SQL intermedia | Facilita queries futuras y demuestra stack completo |

## ✅ Próximos pasos

- [x] Análisis exploratorio inicial
- [x] Limpieza de datos (outliers, formatos inconsistentes)
- [ ] Modelo de datos relacional
- [ ] Dashboard — vista mensual
- [ ] Dashboard — top performers
- [ ] Documentación técnica para GitHub

## 📊 Estado actual

EDA completado. Dataset limpio. Próximo paso: diseño del modelo de datos antes de pasar a visualizaciones.

## 🔗 Relacionado

- Personas: [[personas/cliente]]
- Research: [[research/power-bi-dax]]

---
#proyecto #estado/activo #prioridad/alta
