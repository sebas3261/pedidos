# Sweetmatch -- Parcial I (Patrones Arquitectónicos Avanzados)

### Autores: Sebastian Sanchez Sandoval y Santiago Soler Prado

Este proyecto implementa el **despliegue completo** de una aplicación de
**gestión de pedidos** compuesta por **base de datos, backend y
frontend**.\
Se utilizan **Helm** para empaquetar la aplicación y **ArgoCD** para la
**entrega continua** en Kubernetes.

------------------------------------------------------------------------

## 📌 Arquitectura

La aplicación se compone de **tres servicios principales**:

-   **PostgreSQL** → Base de datos persistente (**subchart de
    Bitnami**).\
-   **Backend** *(Spring Boot / Node.js según implementación)* → API
    REST para gestionar pedidos.\
-   **Frontend** *(React / Angular / Vue)* → Interfaz de usuario que
    consume la API.

Cada servicio está desplegado en Kubernetes como **Deployment +
Service** y expuesto mediante un **Ingress**:

  Ruta       Servicio
  ---------- ----------
  `/api/*`   Backend
  `/`        Frontend

-   La base de datos utiliza un **PersistentVolumeClaim (PVC)** para
    garantizar la **persistencia de datos**.
-   Las **credenciales** y configuraciones se manejan mediante
    **Secrets** y **ConfigMaps**.

------------------------------------------------------------------------

## 📂 Estructura del repositorio

``` bash
charts/
  pedido-app/
    Chart.yaml
    values.yaml
    templates/
      backend-deployment.yaml
      backend-service.yaml
      ingres.yaml
      frontend-deployment.yaml
      frontend-service.yaml
      intdb-configmap.yaml
```

------------------------------------------------------------------------

## 🚀 Instalación manual con Helm

### 1. Clonar el repositorio

``` bash
git clone https://github.com/sebas3261/pedidos.git
cd charts/pedido-app
```

### 2. Instalar en el namespace **sweetmatch**

``` bash
kubectl create namespace sweetmatch
helm upgrade --install sweetmatch . -n sweetmatch -f values.yaml
```

### 3. Verificar recursos desplegados

``` bash
kubectl get pods -n sweetmatch
kubectl get svc -n sweetmatch
kubectl get ingress -n sweetmatch
```

------------------------------------------------------------------------

## 🔄 Integración con ArgoCD

La integración con **ArgoCD** se realiza mediante un **repositorio de
Helm** publicado en **GitHub Pages**.\
ArgoCD consume directamente el **chart empaquetado** (`.tgz`) desde la
URL del repositorio.

Cada vez que se publica una nueva versión del chart, **ArgoCD sincroniza
automáticamente** el clúster.

------------------------------------------------------------------------

## 🌐 Endpoints de acceso

  -----------------------------------------------------------------------
  Servicio            URL
  ------------------- ---------------------------------------------------
  **Frontend**        <http://134.199.176.72/>

  **Backend**         <http://134.199.176.72/api/>
  -----------------------------------------------------------------------

### Test de coneccion
- Se puede verificar la coneccion entre el frontend y el backend registrandote o iniciando sesion, de igual manera se puede revisar que fue exitoso con el token jwt y los datos del usuario que quedan almacenados en el local storage del navegador

### Endpoints de autenticación

-   **POST** `/api/auth/login` → Iniciar sesión\
-   **POST** `/api/auth/signup` → Registro de usuarios

**Cuerpo de la petición:**

``` json
{
    "user": "usuario",
    "password": "contraseña"
}
```

### Endpoints adicionales

-   **GET** `/api/users` → Lista de usuarios *(solo para fines de
    prueba)*

------------------------------------------------------------------------

## 🛠️ Tecnologías principales

-   **Kubernetes** → Orquestación de contenedores
-   **Helm** → Empaquetado y despliegue de la aplicación
-   **ArgoCD** → Entrega continua automatizada
-   **PostgreSQL** → Base de datos persistente
-   **Node.js / Spring Boot** → Backend
-   **React / Angular / Vue** → Frontend

------------------------------------------------------------------------

## Links a los repositorios adicionales

-   **Frontend** -> https://github.com/sebas3261/patrones-front
-   **Backend** -> https://github.com/sebas3261/patrons-back

------------------------------------------------------------------------

## 👨‍💻 Autores

-   **Sebastian Sanchez Sandoval**
-   **Santiago Soler Prado**


### Informacion adicional

El frontend fue reutilizado de un proyecto personal de repostreria, el link de la pagina en consstruccion y prueba https://sweetmatchbakery.com/. y si quieres comprar en ig @sweetmatchco
