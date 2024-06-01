# ironhack-lab-11
Repositorio de Laboratorio 11 de IronHack

---

# Informe del Laboratorio: Diseño Teórico de una Arquitectura Web Segura y Escalable en AWS

## Parte 1: Diseño de Infraestructura en la Nube

### Diseño Propuesto

1. **Amazon EC2 (Elastic Compute Cloud)**
   - **Instancias de Cómputo**: Utilizaremos instancias EC2 para alojar la aplicación web. Las instancias estarán distribuidas en múltiples zonas de disponibilidad para asegurar alta disponibilidad y tolerancia a fallos.
   - **Grupos de Autoescalado**: Configuraremos grupos de autoescalado para aumentar o disminuir el número de instancias EC2 según la carga de tráfico.

2. **Amazon S3 (Simple Storage Service)**
   - **Almacenamiento de Archivos Estáticos**: S3 se utilizará para almacenar archivos estáticos como imágenes, videos y archivos de configuración. S3 ofrece alta durabilidad y disponibilidad, además de ser escalable automáticamente.

3. **Amazon VPC (Virtual Private Cloud)**
   - **Red Privada**: Configuraremos una VPC para definir una red privada segura. Dentro de la VPC, crearemos subredes públicas y privadas.
   - **Subredes**: Las subredes públicas alojarán las instancias de balanceo de carga (ELB), mientras que las subredes privadas alojarán las instancias de aplicación y bases de datos.

### Diagrama de Arquitectura

![AWS Architecture Diagram](link_to_diagram) *(El diagrama debe incluir EC2, S3, VPC con subredes públicas y privadas, y otros componentes relevantes)*

## Parte 2: Configuración de IAM

### Configuración Propuesta

1. **Roles de IAM**:
   - **Desarrolladores**: Acceso a los recursos necesarios para desarrollo y pruebas (por ejemplo, instancias EC2 de desarrollo, S3 buckets de desarrollo).
   - **Administradores**: Acceso completo para gestionar todos los recursos de AWS, incluida la configuración de seguridad y políticas.
   - **Servidores de Aplicaciones**: Acceso específico a S3 para lectura/escritura de archivos estáticos, acceso limitado a bases de datos y otros recursos necesarios para el funcionamiento de la aplicación.

2. **Políticas de IAM**:
   - **Política de Desarrolladores**:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "ec2:DescribeInstances",
             "s3:ListBucket",
             "s3:GetObject"
           ],
           "Resource": "*"
         }
       ]
     }
     ```
   - **Política de Administradores**:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": "*",
           "Resource": "*"
         }
       ]
     }
     ```
   - **Política de Servidores de Aplicaciones**:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "s3:PutObject",
             "s3:GetObject",
             "rds:DescribeDBInstances"
           ],
           "Resource": "*"
         }
       ]
     }
     ```

## Parte 3: Estrategia de Gestión de Recursos

### Estrategia Propuesta

1. **Autoescalado (AWS Auto Scaling)**:
   - Configuraremos grupos de autoescalado para las instancias EC2 con políticas de escalado basadas en métricas de uso de CPU y tráfico de red.
   - Definiremos políticas de escalado predictivo para anticipar aumentos en la carga y escalar proactivamente.

2. **Balanceo de Carga (Elastic Load Balancer - ELB)**:
   - Utilizaremos ELB para distribuir el tráfico de entrada entre múltiples instancias EC2. Esto asegurará alta disponibilidad y redundancia.
   - Configuraremos ELB en múltiples zonas de disponibilidad para aumentar la resiliencia.

3. **Optimización de Costos (AWS Budgets)**:
   - Estableceremos presupuestos en AWS Budgets para monitorear y controlar los gastos.
   - Configuraremos alertas para notificar cuando el uso de recursos se acerque a los límites presupuestarios.

## Parte 4: Implementación Teórica

### Descripción de la Arquitectura

1. **Instancias EC2**:
   - Las instancias EC2 alojarán la aplicación web, distribuidas en múltiples zonas de disponibilidad para alta disponibilidad.
   - Los grupos de autoescalado ajustarán el número de instancias basándose en la demanda.

2. **S3**:
   - S3 se utilizará para almacenar contenido estático accesible por las instancias EC2.
   - S3 proporcionará una solución de almacenamiento duradera y escalable.

3. **VPC**:
   - La VPC proporcionará una red segura para todos los componentes de la aplicación.
   - Las subredes públicas alojarán ELB y las subredes privadas alojarán las instancias de aplicación y bases de datos.

4. **ELB**:
   - ELB distribuirá el tráfico entre las instancias EC2, asegurando balanceo de carga y alta disponibilidad.

### Flujo de Datos y Control

1. Los usuarios acceden a la aplicación web a través de ELB.
2. ELB distribuye las solicitudes entre las instancias EC2.
3. Las instancias EC2 interactúan con S3 para obtener/almacenar contenido estático.
4. Las instancias EC2 acceden a la base de datos alojada en una subred privada para operaciones de lectura/escritura.
5. Los grupos de autoescalado ajustan el número de instancias EC2 según la demanda.

## Parte 5: Discusión y Evaluación

### Elección de Servicios y su Interacción

- **EC2**: Proporciona capacidad de cómputo escalable.
- **S3**: Ofrece almacenamiento duradero y escalable para contenido estático.
- **VPC**: Asegura una red privada y segura.
- **ELB**: Distribuye el tráfico, asegurando alta disponibilidad.

### Contribución de Políticas IAM a la Seguridad

- **Principio de Privilegio Mínimo**: Asegura que cada rol tiene solo los permisos necesarios para realizar sus tareas.
- **Roles Específicos**: Diferentes roles y políticas aseguran que los accesos estén bien definidos y controlados, minimizando riesgos de seguridad.

### Estrategia de Gestión de Recursos

- **Autoescalado**: Garantiza que la aplicación pueda manejar picos de tráfico sin problemas.
- **Balanceo de Carga**: Asegura que el tráfico se distribuya uniformemente, mejorando la disponibilidad y rendimiento.
- **Optimización de Costos**: Uso de AWS Budgets para monitorear y controlar gastos, asegurando un uso eficiente de recursos.

El diseño propuesto para una arquitectura web segura y escalable en AWS combina servicios clave como EC2, S3, VPC y ELB para proporcionar una solución robusta y eficiente. Las configuraciones de IAM aseguran que los accesos estén controlados y seguros, mientras que la estrategia de gestión de recursos optimiza el rendimiento y los costos. Esta aproximación teórica sirve como guía para implementar una infraestructura en la nube que pueda manejar las necesidades dinámicas de una aplicación web moderna.
