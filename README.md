
# Serverless Inteligente — Arquitectura Cloud para E-Commerce

Arquitectura **100 % serverless** diseñada para una plataforma de comercio electrónico de alta demanda.  
El objetivo fue **eliminar costos fijos**, **aumentar la resiliencia** y **mejorar la latencia global**, aprovechando los servicios administrados de **AWS**.

---

## Descripción General

Migración desde una infraestructura monolítica a una **arquitectura basada en eventos (event-driven)**, sin servidores administrados.

El sistema integra los principales componentes serverless de AWS:

| Capa | Servicio AWS | Rol |
|------|---------------|-----|
| **Edge & Static Content** | CloudFront + S3 (OAC) | CDN global seguro con hosting estático |
| **API Layer** | API Gateway + Cognito | Puerta de entrada REST con autenticación JWT |
| **Compute** | AWS Lambda (Users, Orders, Processor) | Lógica de negocio y facturación |
| **Data & Storage** | DynamoDB (On-Demand) + S3 Intelligent-Tiering | Persistencia optimizada por demanda |
| **Mensajería** | SNS + SQS | Procesamiento asíncrono desacoplado |
| **Observabilidad** | CloudWatch + X-Ray | Métricas, trazas y alarmas automáticas |

---


> Este proyecto corresponde a una simulación completa de arquitectura y desarrollo serverless. No se implementó el despliegue real, pero todos los componentes y flujos se definen conforme a prácticas reales de AWS.
>
