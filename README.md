# ConverSense - Sistema de Gestión Automotriz con IA

## Descripción del Proyecto

ConverSense es un sistema avanzado de gestión y automatización desarrollado como proyecto de tesis. Aunque el desarrollo utilizó una base de datos de ejemplo del sector automotriz, el sistema no está limitado a ese rubro. Combina inteligencia artificial con flujos de trabajo automatizados, y puede adaptarse fácilmente a distintos tipos de empresas según sus necesidades.

## Características Principales

### 🤖 Asistente Virtual Inteligente
- **Agente de Ventas**: Chatbot especializado en ventas y soporte
- **Procesamiento Multimodal**: Soporte para texto y audio (transcripción automática)
- **Gestión de Conversaciones**: Memoria contextual con PostgreSQL
- **Protección Contra Manipulación**: Sistema de seguridad integrado

### 📊 Dashboard de Analytics
- **Métricas en Tiempo Real**: Ventas, conversiones, inventario
- **Gráficos Interactivos**: Ventas mensuales, estado de cotizaciones, test drives
- **Reportes Automatizados**: Generación de reportes HTML con visualizaciones
- **Top Vendedores**: Ranking y comisiones automáticas

### 🔧 Gestión de Clientes
- **Base de Datos Centralizada**: Información unificada de clientes
- **Sistema de Etiquetas**: Clasificación automática por intereses y comportamiento
- **Cotizaciones Automáticas**: Generación automática de presupuestoss

### 🗃️ Gestión de Inventario
- **Consulta Avanzada**: Búsqueda por categoría, atributos del producto, rango de precios y disponibilidad.
- **Estadísticas de Stock**: Conteos por ubicación, tipo de producto y estado.
- **Análisis de Mercado**: Promedios de precios, variaciones de demanda y tendencias generales.

## Arquitectura Técnica

### Stack Tecnológico
- **n8n**: Automatización de flujos de trabajo
- **PostgreSQL**: Base de datos principal
- **Ollama**: Modelos de lenguaje local
- **Qdrant**: Vector database para búsquedas semánticas
- **Docker**: Contenerización y orquestación

### Componentes del Sistema

```
ConverSense/
├── n8n/                 # Flujos de trabajo automatizados
├── shared/              # Archivos compartidos y dashboard
├── docker-compose.yml   # Orquestación de servicios
└── .env                 # Configuración de entorno
```

## Instalación y Configuración

### Prerrequisitos
- Docker y Docker Compose
- Git
- Ngrok (para acceso externo)

### Instalación Rápida

1. **Clonar el repositorio:**
```bash
git clone https://github.com/CristobalSalas04/ConverSense.git
cd ConverSense
```

2. **Configurar variables de entorno:**
```bash
cp .env.example .env
# Editar .env
```

3. **Ejecutar con GPU (recomendado):**
```bash
docker compose --profile gpu-nvidia up -d
# Soporte para GPUS sobre la serie 1000
```

4. **Ejecutar solo con CPU:**
```bash
docker compose --profile cpu up -d
```

### Levantar el Dashboard (Nginx)

Dentro de la carpeta `shared` existe un segundo `docker-compose.yml` usado para el Dashboard.  
Para ejecutarlo, usa los siguientes comandos:

```bash
cd shared
docker compose up -d
```

### Configuración de Base de Datos

Si encuentras problemas con la base de datos, puedes inicializarla manualmente:

```bash
docker run -d \
  --name postgres_init \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=tu_contraseña \
  -e POSTGRES_DB=n8n \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  postgres:16-alpine
```

### Configuración de Túneles utilizando ngrok

Para acceso externo, configura túneles con ngrok:

```bash
# Terminal 1: Tunnel para n8n
ngrok http 5678 - --pooling-enabled
# O utilizar: 
ngrok http --url=tu_dominio_gratuito 5678 --pooling-enabled

# Terminal 2: Tunnel para dashboard  
ngrok http 8080 --pooling-enabled
# O utilizar: 
ngrok http --url=tu_dominio_gratuito 8080 --pooling-enabled


# si cuentas con un plan de paga y quieres tener ambos a la vez:
# Terminal 1: Tunnel para n8n
ngrok http 5678 --subdomain=tu_dominio_gratuito-n8n --pooling-enabled

# Terminal 2: Tunnel para dashboard  
ngrok http 8080 --subdomain=tu_dominio_gratuito-dashboard --pooling-enabled


```

## Estructura de Servicios

### Servicios Principales
- **n8n**: Interfaz de automatización (puerto 5678)
- **postgres**: Base de datos (puerto 5432)
- **ollama**: Modelos de IA local
- **qdrant**: Base de datos vectorial
- **dashboard**: Panel de control (puerto 8080)

### Volúmenes Persistentes
- `n8n_storage`: Configuraciones de n8n
- `postgres_test`: Datos de PostgreSQL
- `ollama_storage`: Modelos de IA
- `qdrant_storage`: Vectores e embeddings

## Uso del Sistema

### Acceso a las Interfaces

1. **n8n Dashboard**: `http://localhost:5678`
2. **Dashboard ConverSense**: `http://localhost:8080`
3. **API Webhooks**: Configurados automáticamente

### Flujos de Trabajo Principales

1. **Atención al Cliente**: 
   - Recepción de mensajes vía webhook
   - Procesamiento con IA
   - Gestión de conversaciones
   - Asignación de etiquetas

2. **Gestión de Ventas**:
   - Cotizaciones automáticas
   - Programación de test drives
   - Seguimiento de leads
   - Reportes de desempeño

3. **Dashboard Analytics**:
   - Métricas en tiempo real
   - Visualización de datos
   - Reportes ejecutivos

## Configuración de IA

### Modelos Soportados
- GPT-4.1-mini (OpenAI)
- Modelos locales via Ollama
- Procesamiento de audio con Whisper

### Memoria de Conversaciones
- Almacenamiento en PostgreSQL 
- Contexto de 10 mensajes
- Sesiones por ID de chat

## Desarrollo y Personalización

### Estructura de Flujos
Los flujos de trabajo están organizados en:
- **Gestión de Clientes**: Registro y seguimiento
- **Ventas y Cotizaciones**: Proceso comercial
- **Inventario**: Consultas y estadísticas
- **Analytics**: Reportes y dashboards

### Variables de Entorno Importantes de cambiar
```env
POSTGRES_USER=tu_usuario
POSTGRES_PASSWORD=tu_contraseña
N8N_ENCRYPTION_KEY=clave_segura
OLLAMA_HOST=ollama:11434
```
### Base de Datos de Ejemplo

ConverSense incluye una base de datos de ejemplo basada en el sector automotriz.  
Esta base de datos se utiliza únicamente con fines demostrativos para mostrar el funcionamiento del sistema, sus flujos automatizados y las capacidades de integración con la inteligencia artificial.  

El proyecto **no está limitado** al rubro automotriz: la estructura del sistema permite reemplazar fácilmente la base de datos por información de cualquier otro sector o empresa según las necesidades del usuario.

# Backup y Restauración de la Base de Datos

Este proyecto incluye una base de datos PostgreSQL con datos de ejemplo del rubro automotriz.  
Si deseas exportar o restaurar la base de datos completa, sigue estas instrucciones.

```bash
# Opción 1: Usando Get-Content
Get-Content backups/automovilistica_backup.sql | docker exec -i self-hosted-ai-starter-kit-postgres-1 psql -U root -d automovilistica

# Opción 2: Usando el alias cat 
cat backups/automovilistica_backup.sql | docker exec -i self-hosted-ai-starter-kit-postgres-1 psql -U root -d automovilistica
```
root puede ser cambiado por el usuario que indicaste al crear tus variables de entorno. Lo mas probable es que tengas que mover 


# Restaurar Flujos y Credenciales de n8n

Este proyecto incluye flujos y credenciales de n8n dentro de la carpeta:

```
./n8n/demo-data/
```

El sistema está configurado para **importar automáticamente** estos archivos al iniciar los contenedores gracias al servicio `n8n-import` definido en el `docker-compose.yml`.

---

## Sistema de Importación Automática

### Arquitectura del Proceso
Cuando ejecutas `docker compose up`, se activan dos servicios secuenciales:

1. **n8n-import** (servicio de una sola ejecución)
   - Ejecuta: `n8n import:credentials --separate --input=/demo-data/credentials`
   - Ejecuta: `n8n import:workflow --separate --input=/demo-data/workflows`
   - Monta la carpeta `./n8n/demo-data/` en `/demo-data/`

2. **n8n** (servicio principal)
   - Solo inicia después que `n8n-import` termine exitosamente
   - Usa la configuración ya importada

### Estructura Requerida
```
./n8n/demo-data/
├── credentials/
│   ├── openAiApi.json     # Credenciales de OpenAI
│   ├── postgres.json      # Credenciales de PostgreSQL
│   └── ... otros .json
└── workflows/
    ├── workflow1.json     # Flujos de trabajo
    ├── workflow2.json
    └── ... otros .json
```
### Importación manual de flujos en n8n

Si por alguna razón el contenedor `n8n-import` no logra importar el flujo de ConverSense automáticamente, puedes realizar la importación manual directamente desde la interfaz de n8n.

Sigue estos pasos:

1. Ingresa al **Editor UI de n8n** en tu navegador (por defecto: `http://localhost:5678`).
2. Crea un **nuevo flujo**.
3. En la esquina superior derecha, haz clic en los **tres puntos del menú**.
4. Selecciona **“Import from File”**.
5. Busca y elige el archivo `conversense.json` ubicado en:
```
./n8n/demo-data/workflows/conversense.json
```
Una vez importado, podrás editarlo, ejecutarlo o ajustarlo según lo necesites.

## Solución de Problemas

### Problemas Comunes

1. **Base de datos no inicializada**:
   - Verificar variables de entorno
   - Ejecutar contenedor de inicialización

2. **Modelos de IA no cargan**:
   - Verificar perfil de GPU/CPU
   - Comprobar almacenamiento de Ollama

3. **Webhooks no funcionan**:
   - Verificar configuración de Ngrok
   - Comprobar URLs en n8n

### Logs y Monitoreo
```bash
# Ver logs de todos los servicios
docker compose logs -f

# Logs específicos de n8n
docker logs n8n -f
```

## Contribución

Este proyecto es parte de una tesis universitaria. Para contribuciones o consultas, contactar al autor del proyecto.

## Licencia


Este proyecto es un fork de **n8n Self-Hosted AI Starter Kit**, y mantiene la licencia original del proyecto:

Este proyecto está licenciado bajo la Licencia Apache 2.0. Consulta el archivo LICENSE para más detalles.

Este fork fue desarrollado como parte de un proyecto académico llamado **ConverSense**.

---

**Nota**: Este sistema está diseñado para entornos de producción con las debidas consideraciones de seguridad y escalabilidad.