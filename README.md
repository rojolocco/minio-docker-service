# Servicio Minio

Este directorio contiene la configuración necesaria para levantar un servicio de Minio utilizando Docker Compose. Está preparado tanto para entornos de desarrollo como de producción, con persistencia de datos y seguridad básica.

---

## **Estructura de Archivos**

```plaintext
minio/
├── .env.dev                     # Variables de entorno para desarrollo
├── .env.prod                    # Variables de entorno para producción
├── docker-compose.dev.yml       # Docker Compose para desarrollo
├── docker-compose.yml           # Docker Compose para producción
├── data/                        # Carpeta para persistencia de datos (creada automáticamente)
├── config/                      # Carpeta para configuración de Minio (creada automáticamente)
```

---

## **Requisitos Previos**

1. Docker y Docker Compose instalados.
2. Una red externa llamada `caddy_network` creada previamente:

   ```bash
   docker network create caddy_network
   ```

---

## **Configuración de Variables de Entorno**

Define las credenciales y ajustes del servicio en los archivos `.env.dev` y `.env.prod`.

### `.env.dev` (Desarrollo)

```plaintext
MINIO_ROOT_USER=dev_minio_user
MINIO_ROOT_PASSWORD=dev_minio_password
```

### `.env.prod` (Producción)

```plaintext
MINIO_ROOT_USER=prod_minio_user
MINIO_ROOT_PASSWORD=prod_minio_password
```

> **Nota:** Asegúrate de que estos archivos estén protegidos y excluidos del control de versiones mediante `.gitignore`.

---

## **Uso**

### **Levantar el servicio en desarrollo**

```bash
docker-compose -f docker-compose.dev.yml up -d
```

### **Levantar el servicio en producción**

```bash
docker-compose -f docker-compose.yml up -d
```

### **Detener el servicio**

```bash
docker-compose down
```

### **Verificar el estado de los contenedores**

```bash
docker ps
```

---

## **Acceso a la Consola de Minio**

Puedes acceder a la consola de administración de Minio en `http://<tu_ip>:9001`. Utiliza las credenciales definidas en los archivos `.env.dev` o `.env.prod`.

---

## **Persistencia de Datos**

Los datos de Minio se almacenan en dos volúmenes:

- **minio_data**: Almacena los objetos subidos a Minio.
- **minio_config**: Almacena la configuración de Minio.

Estos volúmenes aseguran que los datos y la configuración no se pierdan al detener o reiniciar los contenedores.

### **Ubicación del volumen:**

Los volúmenes son gestionados por Docker y no están directamente accesibles desde el sistema de archivos.

Para eliminar los datos almacenados:

```bash
docker volume rm minio_minio_data minio_minio_config
```

---

## **Solución de Problemas**

### Error: `Failed to authenticate`

- Asegúrate de que las credenciales en los archivos `.env` coincidan con las utilizadas para acceder a la consola de Minio.
- Verifica que los contenedores estén conectados a la red `caddy_network` correctamente.

### Error: `Address already in use`

- Asegúrate de que los puertos `9000` y `9001` no estén siendo utilizados por otro servicio. Puedes cambiar los puertos en el archivo `docker-compose` si es necesario.

---

## **Contribución**

Si deseas mejorar este servicio o agregar nuevas funcionalidades, sigue estos pasos:

1. Haz un fork de este repositorio.
2. Realiza tus cambios en una rama nueva.
3. Envía un pull request con tus mejoras.

---

## **Licencia**

Este proyecto está bajo la licencia MIT. Consulta el archivo `LICENSE` para más detalles.
