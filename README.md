# Serverless Inteligente — Arquitectura Cloud para E-Commerce

Arquitectura **100 % serverless**, creada para demostrar que se puede tener escalabilidad infinita, cero servidores, y aún así conservar la cordura (casi). 
El objetivo fue **eliminar costos fijos**, **aumentar la resiliencia** y **mejorar la latencia global**, aprovechando los servicios administrados de **AWS**.

---

## Descripción General

Migración desde una infraestructura monolítica hacia una arquitectura **event-driven y serverless**.  
Menos costos fijos, más agilidad, y ni un solo cron job que se queje los domingos.

El sistema integra los principales componentes serverless de AWS:

| Capa | Servicio AWS | Rol |
|------|---------------|-----|
| **Edge & Static Content** | CloudFront + S3 (OAC) | CDN global seguro con hosting estático |
| **API Layer** | API Gateway + Cognito | Puerta de entrada REST con autenticación JWT |
| **Compute** | AWS Lambda (Users, Orders, Processor) | Lógica de negocio y facturación |
| **Data & Storage** | DynamoDB (On-Demand) + S3 Intelligent-Tiering | Persistencia optimizada por demanda |
| **Mensajería** | SNS + SQS | Procesamiento asíncrono y desacoplado |
| **Observabilidad** | CloudWatch + X-Ray | Monitoreo y trazas (sin dramas) |

---

## Arquitectura

```text

+-----------------------------------------------------------------------------------+
|                                 AWS CLOUD (Global)                                |
|                                                                                   |
|   +-----------------------------------------------------------------------------+ |
|   |                                Edge Layer                                   | |
|   |                                                                             | |
|   |   [Usuario/Cliente] ---> CloudFront (OAC) ---> S3 Static Hosting            | |
|   |                                                                             | |
|   |   [Cognito] ---> API Gateway (JWT, Cache, Throttling)                       | |
|   +------------------------------------|----------------------------------------+ |
|                                        V                                          |
|   +-----------------------------------------------------------------------------+ |
|   |                             AWS Region (us-east-1)                          | |
|   |                                                                             | |
|   |   +-----------------------------------------------------------------------+ | |
|   |   | [Lambda Users]   --> DynamoDB (CRUD Usuarios)                         | | |
|   |   | [Lambda Orders]  --> DynamoDB + SNS Topic (Event Driven)              | | |
|   |   | [SNS Topic] --> [SQS Queue] --> [Lambda Processor] --> S3 (Facturas)  | | |
|   |   +-----------------------------------------------------------------------+ | |
|   |                                                                             | |
|   +-----------------------------------------------------------------------------+ |
|                                                                                   |
+-----------------------------------------------------------------------------------+
```

---

**Flujo:**  
1. El cliente realiza peticiones a la API autenticadas por Cognito.  
2. Las Lambdas gestionan operaciones CRUD sobre DynamoDB.  
3. Los eventos de pedidos se publican en SNS → procesados en SQS → ejecutados por Lambda Processor.  
4. Los archivos generados se guardan en S3 con URLs firmadas temporales.

---

## Resultados Técnicos

| Métrica                 | Resultado                                      |
| ----------------------- | ---------------------------------------------- |
| Cobertura de pruebas    | **84 %**                                       |
| Ciclos TDD completados  | **12 (Red-Green-Refactor)**                    |
| Refactorizaciones clave | **4 (rendimiento, modularidad, testabilidad)** |
| Cold Start optimizado   | **InitDuration reducido por scope global**     |
| Latencia global         | **60 ms P95 (CloudFront Edge)**                |
| Costo estimado mensual  | **≈ 246 USD (pico simulado)**                  |

---

## Estrategias de Optimización

* **Provisioned Concurrency:** activada solo en horario laboral (porque los Lambdas también merecen dormir).
* **S3 Intelligent-Tiering:** ahorro automático de almacenamiento, sin perder disponibilidad.
* **Single-Table Design en DynamoDB:** consultas rápidas y cero “WHERE infernales”.
* **Monitoreo proactivo:** alarmas 5XX, throttling y métricas de latencia con CloudWatch, que avisa antes de que el caos aparezca (como todo buen aliado).

---

## Tecnologías Clave

* **Lenguaje:** Python 3.12
* **Framework:** AWS SAM (Serverless Application Model)
* **Testing:** pytest, unittest.mock
* **Infraestructura como Código:** YAML templates (IaC) 
* **Seguridad:** Cognito JWT, políticas OAC, KMS
* **Integración:** API Gateway + Lambda + DynamoDB + SNS/SQS

---

## Aprendizajes Principales

* Serverless no es “sin servidores”: es “servidores 'de otro' que ahora pagas por segundo”.
* La automatización de costos es el camino zen del desarrollador cloud.
* TDD funciona… cuando recuerdas correr los tests antes de refactorizar (conocimiento empírico).
* Una buena arquitectura no necesita magia: solo buen diseño y una pizca de caos controlado.

---

## Contacto Profesional

¿Interesado en colaboraciones técnicas, proyectos cloud o simplemente en debatir sobre por qué el café es infraestructura crítica?
Puedes contactarme al correo **[tu_correo_profesional]**.

---

### © 2025 — Proyecto académico-profesional inspirado en AWS Serverless Lens y el Well-Architected Framework.



---
> Este proyecto corresponde a una simulación completa de arquitectura y desarrollo serverless. No se implementó el despliegue real, pero todos los componentes y flujos se definen conforme a prácticas reales de AWS.
>
