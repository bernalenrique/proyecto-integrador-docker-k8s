# Proyecto Final - Docker & Kubernetes

**Alumno:** Felix Enrique Bernal Conde
**Fecha:** 25/11/2025
**Curso:** Docker & Kubernetes - i-Quattro

## Links de Docker Hub
- Backend v2.1: https://hub.docker.com/repository/docker/bernalconde/springboot-api/tags
- Frontend v2.2: https://hub.docker.com/repository/docker/bernalconde/angular-frontend/tags

## Parte 1: Setup del Ambiente

**Ambiente utilizado:**
- [VirtualBox]
- Nombre de VM/Instancia: bernalcondefelix
- Sistema operativo: Ubuntu 24.04 LTS
- Recursos: 4GB RAM, 2 CPU cores
- Red configurada: Bridge
- Rango MetalLB: 192.168.1.240-192.168.1.250

### Screenshots
*microk8s instalado con addons habilitados*
![microk8s status](screenshots/parte1-1-microk8s_status.png)
*Proyecto v2.0 funcionando en el cluster*
![cluster status](screenshots/parte1-2-status_cluster.png)
*frontend Angular*
![Frontend via MetalLB](screenshots/parte1-3-frontend.png)
*JSON con el saludo*
![JSON con el saludo](screenshots/parte1-4-greeting.png)
*lista de usuarios*
![users](screenshots/parte1-5-users.png)
*Heatlh*
![health](screenshots/parte1-6-health.png)
*Hostname*
![hostname](screenshots/parte1-7-ambiente.png)

## Parte 2: Backend v2.1
Ademas del codigo proporcionado tambien se hizo la importacion de la libreria
```bash
import org.springframework.http.ResponseEntity;
```

### Código Agregado
```bash
@GetMapping("/api/info")
public ResponseEntity<Map<String, Object>> getInfo() {
    Map<String, Object> info = new HashMap<>();
    info.put("alumno", "FELIX ENRIQUE BERNAL CONDE");
    info.put("version", "v2.1");
    info.put("curso", "Docker & Kubernetes - i-Quattro");
    info.put("timestamp", LocalDateTime.now().toString());
    info.put("hostname", System.getenv("HOSTNAME"));
    return ResponseEntity.ok(info);
}
```

### Screenshots
*Código del endpoint agregado*
![code endpoint](screenshots/parte2-1-add_endpoint.png)
*docker images*
![docker images](screenshots/parte2-2-docker_images.png)
*Docker Hub*
![docker hub](screenshots/parte2-3-docker_hub.png)
*kubectl rollout status durante la actualización*
![Rollout](screenshots/parte2-4-rollout.png)
![Rollout](screenshots/parte2-5-rollout.png)
*kubectl get pods*
![pods](screenshots/parte2-6-pods.png)
*output*
![API Info](screenshots/parte2-7-curl_info.png)

**Nota:** Se presento un error durante el rollout debido a una mala configuracion en la adicion de un nuevo endpoint, se adiciono el nuevo endpoint a la misma ruta. Una vez corregido el error se procedio a repetir el ciclo de construcion y despliegue.

## Parte 3: Frontend v2.2
Se procedio con la adicion de un boton que permite desplegar informacion del sistema.

### Screenshots
*appcomponent.html*
![appcomponent html](screenshots/parte3-1-mod_html.png)
*appcomponent.ts*
![appcomponent ts](screenshots/parte3-2-mod_ts.png)
*Docker Hub*
![docker hub](screenshots/parte3-3-docker_hub.png)
*kubectl get pods*
![pods](screenshots/parte3-4-rollout.png)
*botón "Ver Info del Sistema"*
![boton](screenshots/parte3-6-boton.png)
*información*
![informacion](screenshots/parte3-7-inf_system.png)

## Parte 4: Gestión de Versiones

### ¿Qué hace kubectl rollout undo?
Permite la vuelta atras teniendo como puntos de referencias cada deploy realizado con exito.

## Parte 5: Ingress + MetalLB

**IP del Ingress:** [192.168.1.240]

### Screenshots
*MetalLB*
![metallb](screenshots/parte5-3-metallb.png)
*frontend*
![frontend](screenshots/parte5-4-frontend.png)
*curl a /api/users y /api/greeting*
![users greeting](screenshots/parte5-5-users_greeting.png)
*curl a /api/info*
![info](screenshots/parte5-6--info.png)
*curl a /actuator/health*
![health](screenshots/parte6-7-health.png)

## Conclusiones
Se logro realizar de manera satisfactoria las tareas planteas en el desarrollo del proyecto, si bien se presentaron dificultades estas fueron subsanadas en su mayoria. Se observo que el uso de docker efectivamente reduce el uso de recursos. Se pudo entender que a pesar de su sencilles es posible desplegar sistemas complejos que se comunican entre si permitiendonos tambien el control de versiones (despliegue y reversion).


### Aprendizajes principales
- [Punto 1] Se aprendio las configuraciones basicas para desplegar un entorno distribuido (backend - frontend), asi como la instalacion y habilitacion de las herramientas necesarias.
- [Punto 2] Se observo la sencilles con la que se puede agregar endpoints a las aplicaciones.
- [Punto 3] De igual manera se pudo ver como configurar el frondend.
- [Punto 4] En mi opinion el punto mas importante del proyecto realizado, pues si bien docker te permite guardar distintas versiones de una imagen tambien es posible guardar imagenes corruptas, por lo cual una vuelta atras no es posible.
- [Punto 5] Se observo como es posible comunicar la aplicacion hacia el exterior.

### Dificultades encontradas
- [Dificultad 1 y cómo la resolviste] La mayor dificultad presente fue el manejo del metalLB pues se tenia o no la ip externa, esto no pudo ser solucionado, hubo momentos en los que la ip era asignada y otros en las que no respondia teniendo que utilizar el localhost para la mayoria de las pruebas.
- [Dificultad 2 y cómo la resolviste] Otra dificultad fue el manejo de versiones, pues debido a una mala configuracion se subio una imagen corrupta a docker ya no teniendo la posibilidad de hacer una vuelta atras. No se encontro solucion a esta dificultad y se inicio el proyecto de cero teniendo cuidado en revisar las configuraciones.

### Reflexión
[¿Cómo aplicarías esto en un proyecto real?]
Docker resulto se muy practico puediendo ser utilizado para proyecto sencillos como una pagina web de informacion hasta en sectores mas complejos como sistemas de compras a gran escala. En lo personal lo emplearia para proyectos como un sistema de inventario, pues el proyecto realizo es una buena base para empezar.
