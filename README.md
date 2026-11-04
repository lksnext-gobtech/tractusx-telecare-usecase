# tractusx-telecare-usecase

Espacio de datos de **teleasistencia** inspirado en [Eclipse Tractus-X](https://eclipse-tractusx.github.io/).

El proyecto simula el intercambio de datos de eventos de teleasistencia (caídas, niveles de oxígeno, etc.) entre dos organizaciones a través de un espacio de datos soberano real: los datos se generan, se enriquecen semánticamente (HL7 FHIR), se publican y se consumen mediante un **Eclipse Dataspace Connector (EDC)** desplegado sobre **Tractus-X Umbrella**, y finalmente se analizan — igual que ocurriría en un caso de uso real de intercambio de datos de salud entre proveedores.

## Arquitectura

Cinco servicios FastAPI independientes:

| Servicio | Puerto | Descripción |
|---|---|---|
| `generator` | 8000 | Generador sintético de eventos de teleasistencia |
| `semantic-adapter` | 8001 | Enriquece los eventos y los serializa a HL7 FHIR |
| `analytics` | 8002 | Genera activos derivados a partir de los eventos analizados |
| `dataspace-connector` | 8004 | Cliente EDC: publica el dato como Entidad A y lo consume como Entidad B a través del espacio de datos real |
| `demo-ui` | 8003 | Interfaz web que enlaza todo el flujo |

El flujo es: `generator` → `semantic-adapter` → `dataspace-connector` (publica en el EDC de Entidad A, consume desde el EDC de Entidad B) → `analytics`. La `demo-ui` presenta cada paso.

## Puesta en marcha

- **Entorno local (Python):** ver [DEV.md](DEV.md).
- **Despliegue en VPS (Docker):** ver [DEPLOY.md](DEPLOY.md).
- **Despliegue de Tractus-X Umbrella y del espacio de datos (EDC) sobre el que corre la demo:** ver [infra-notes/](infra-notes/).
- **Colección Bruno con el flujo EDC probado a mano:** ver [brunoExamples/](brunoExamples/README.md).

## Documentación adicional

- [infra-notes/UMBRELLA-SETUP.md](infra-notes/UMBRELLA-SETUP.md) — instalación de Tractus-X Umbrella (Minikube + Helm) en el VPS.
- [infra-notes/PORTAL-DATA-EXCHANGE.md](infra-notes/PORTAL-DATA-EXCHANGE.md) — ampliación del despliegue con el subset de Data Exchange (los dos EDC usados por `dataspace-connector`).
- [infra-notes/GUACAMOLE.md](infra-notes/GUACAMOLE.md) — acceso remoto de escritorio al VPS vía navegador.

## Funding / Financiación

<p align="center">
  <img src="docs/img/logos-prtr.png" alt="Financiado por la Unión Europea – NextGenerationEU · Gobierno de España – Ministerio para la Transformación Digital y de la Función Pública · Plan de Recuperación, Transformación y Resiliencia" width="800">
</p>

This repository is a result of the project **ZAINDATA: data bridge for the unified care of people** (file no. TSI-100121-2024-20). The project is funded by the **Ministry for Digital Transformation and Civil Service** of the Government of Spain, within the **Recovery, Transformation and Resilience Plan**, under the call for demonstrators and use cases of sectoral data spaces. It is **funded by the European Union – NextGenerationEU**.

Este repositorio es un resultado del proyecto **ZAINDATA: puente de datos para el cuidado unificado de las personas** (expediente TSI-100121-2024-20). El proyecto está financiado por el **Ministerio para la Transformación Digital y de la Función Pública** del Gobierno de España, en el marco del **Plan de Recuperación, Transformación y Resiliencia**, dentro de la convocatoria de demostradores y casos de uso de espacios de datos sectoriales. Está **financiado por la Unión Europea – NextGenerationEU**.

Beneficiary / Entidad beneficiaria: **LKS Next GobTech Euskadi S.Coop.** · Project website / Web del proyecto: <https://www.zaindata.es>

## Licencia

[Apache License 2.0](LICENSE).
