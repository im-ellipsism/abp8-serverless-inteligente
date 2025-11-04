# Descripción de la Arquitectura

El sistema se organiza en cinco capas principales:

1. **Edge Layer**
   - CloudFront + S3 (OAC)
   - Distribución de contenido estático y caching global.

2. **API Layer**
   - API Gateway + Cognito
   - Autenticación y ruteo de peticiones.

3. **Compute Layer**
   - AWS Lambda: Users, Orders, Processor
   - Lógica de negocio y orquestación de eventos.

4. **Data & Storage Layer**
   - DynamoDB (On-Demand) + S3 Intelligent-Tiering
   - Persistencia y optimización de costos.

5. **Messaging & Monitoring Layer**
   - SNS → SQS → Lambda Processor + CloudWatch / X-Ray
   - Procesamiento asíncrono y observabilidad.

Diagrama complementario: `diagram_ascii.txt`.
