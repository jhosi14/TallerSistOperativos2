# Proyecto InnovaSys - Configuración de Servidor DevOps con Ansible

## Descripción del Proyecto
Este proyecto fue desarrollado usando Ansible para automatizar la configuración de un servidor Ubuntu 24.04 destinado a la startup "InnovaSys". El servidor implementa dos funciones clave:

1.  Un portal interno (`Apache`) con una página de inicio personalizada.
2.  Un servicio de archivos compartidos (`Samba`) accesible por el equipo de desarrollo.

El enfoque principal fue aplicar principios de automatización y buenas prácticas en infraestructura, asegurando consistencia y repetibilidad en el despliegue.

## Entorno de Laboratorio
El entorno se ejecuta en dos máquinas virtuales configuradas en VirtualBox:

-   **Nodo de Control:** `Linux Lite` (nombre de la VM: `ansible-control`).
-   **Nodo Gestionado:** `Ubuntu Server 24.04` (nombre de la VM: `servidor-innovasys`).

Ambas máquinas deben tener conectividad entre sí, preferiblemente mediante una red interna definida en VirtualBox.

## Estructura del Proyecto Ansible
La organización del proyecto sigue las mejores prácticas de Ansible, utilizando roles para separar funcionalidades:

-   `ansible.cfg`: Configuración general de Ansible.
-   `inventory.ini`: Lista de servidores que serán gestionados.
-   `playbook.yml`: Playbook principal que ejecuta los roles definidos.
-   `roles/`: Carpeta que agrupa los distintos roles:
    -   `apache/`: Encargado de instalar y configurar el servidor web Apache.
    -   `samba/`: Responsable de instalar y preparar el servicio de archivos Samba.

## Cómo Ejecutar el Playbook

Sigue estos pasos para configurar el servidor de forma automatizada:

1.  **Clonar el Repositorio:**
    Abre una terminal en tu máquina con Linux Lite y ejecuta lo siguiente:
    ```bash
    git clone https://github.com/jhosi14/TallerSistOperativos2.git
    cd innova_sys_project
    ```

2.  **Editar el Inventario:**
    Modifica el archivo `inventory.ini`, agregando la IP de tu servidor Ubuntu, el usuario y la contraseña SSH.
    ```bash
    nano inventory.ini
    ```
    Ejemplo de configuración:
    ```ini
    [servers]
    servidor-innovasys ansible_host=192.168.10.100 ansible_user=ubuntu ansible_ssh_pass=TuContraseñaSSH
    ```

3.  **Lanzar el Playbook:**
    Desde el directorio `innova_sys_project/`, ejecuta el siguiente comando:

    ```bash
    ansible-playbook -i inventory.ini playbook.yml --ask-become-pass
    ```

    *(Puedes omitir `--ask-become-pass` si el usuario tiene permisos sudo sin contraseña (NOPASSWD) ya configurado en el servidor Ubuntu).*

    Si el proceso se ejecuta correctamente, verás tareas marcadas como `changed` y sin errores críticos.

## Verificación de la Configuración Final

Una vez completado el playbook, verifica los siguientes servicios:

1.  **Verificar Intranet (Apache):**
    * Abre un navegador en Linux Lite y accede a la IP del servidor Ubuntu.
    * **Ejemplo:** `http://192.168.10.100`
    * **Resultado esperado:** Se visualizará una página personalizada con el mensaje: "Bienvenidos a la Intranet de InnovaSys".

2.  **Verificar Repositorio de Archivos (Samba):**
    * Utiliza el explorador de archivos de Linux Lite para conectarte al recurso compartido.
    * **Ruta:** `smb://192.168.10.100/Proyectos`
    * **Credenciales:**
        * **Usuario:** `devuser1`
        * **Contraseña:** `Innova.2025`
    * **Resultado esperado:** Acceso al recurso compartido con capacidad de crear y modificar archivos.

---

**Autor:** Jhoselyn Fernandez

