# Proyecto InnovaSys - Configuración de Servidor DevOps con Ansible

## Descripción del Proyecto
Este proyecto Ansible fue desarrollado como parte de un desafío para configurar automáticamente un servidor Ubuntu 24.04 para la startup "InnovaSys". El servidor cumple dos funciones principales:
1.  Un portal de intranet (`Apache`) con una página de bienvenida personalizada.
2.  Un repositorio de archivos centralizado (`Samba`) para el equipo de desarrollo.

El objetivo fue demostrar la automatización, repetibilidad y buenas prácticas en la configuración de infraestructuras utilizando Ansible.

## Entorno de Laboratorio
El entorno de pruebas consistió en dos máquinas virtuales en VirtualBox:
-   **Nodo de Control:** `Linux Lite` (nombre de VM: `ansible-control`).
-   **Nodo Gestionado:** `Ubuntu Server 24.04` (nombre de VM: `servidor-innovasys`).

Ambas VMs deben estar configuradas para comunicarse entre sí (idealmente a través de una "Red Interna" en VirtualBox).

## Estructura del Proyecto Ansible
El proyecto sigue las mejores prácticas de Ansible, utilizando roles para modularidad:
-   `ansible.cfg`: Archivo de configuración global de Ansible.
-   `inventory.ini`: Inventario de los hosts gestionados.
-   `playbook.yml`: Playbook principal que orquesta la ejecución de los roles.
-   `roles/`: Directorio que contiene los roles:
    -   `apache/`: Rol para instalar y configurar el servidor web Apache.
    -   `samba/`: Rol para instalar y configurar el servidor de archivos Samba.

## Cómo Ejecutar el Playbook

Sigue estos pasos para desplegar la configuración del servidor InnovaSys:

1.  **Clonar el Repositorio:**
    Abre una terminal en tu máquina Linux Lite y clona este repositorio:
    ```bash
    git clone [https://github.com/GiovanniCF/TallerOperativo2.git](https://github.com/GiovanniCF/TallerOperativo2.git)
    cd innova_sys_project
    ```

2.  **Configurar el Inventario:**
    Abre el archivo `inventory.ini` y actualiza la `ansible_host` con la dirección IP de tu Ubuntu Server 24.04. Asegúrate de que `ansible_user` y `ansible_ssh_pass` sean las credenciales SSH correctas para el usuario en tu `servidor-innovasys`.
    ```bash
    nano inventory.ini
    ```
    Ejemplo de `inventory.ini`:
    ```ini
    [servers]
    servidor-innovasys ansible_host=192.168.10.100 ansible_user=ubuntu ansible_ssh_pass=TuContraseñaSSH
    ```

3.  **Ejecutar el Playbook:**
    Desde el directorio `innova_sys_project/` (donde se encuentra `playbook.yml`), ejecuta el playbook principal.

    Si el usuario configurado en `inventory.ini` necesita una contraseña para ejecutar comandos `sudo` en el servidor Ubuntu (es decir, no configuraste `NOPASSWD` en `sudoers`), se te pedirá esta contraseña al ejecutar el comando con el flag `--ask-become-pass`.
    ```bash
    ansible-playbook -i inventory.ini playbook.yml --ask-become-pass
    ```
    *(Omite `--ask-become-pass` si previamente configuraste `NOPASSWD` para el usuario SSH en el `sudoers` del Ubuntu Server.)*

    La ejecución exitosa del playbook no debería mostrar errores fatales y debería indicar `changed` en las tareas de configuración.

## Verificación de la Configuración Final

Una vez que el playbook se haya ejecutado con éxito, puedes verificar la configuración:

1.  **Verificar Intranet (Apache):**
    * Desde el navegador web de tu máquina Linux Lite, accede a la dirección IP de tu servidor Ubuntu.
    * **Ejemplo:** `http://192.168.10.100`
    * **Resultado esperado:** Deberías ver la página de bienvenida personalizada con el mensaje "Bienvenidos a la Intranet de InnovaSys".

2.  **Verificar Repositorio de Archivos (Samba):**
    * Desde el gestor de archivos de tu máquina Linux Lite, conéctate al recurso compartido Samba.
    * **Ruta de conexión:** `smb://192.168.10.100/Proyectos`
    * **Credenciales:**
        * **Usuario:** `devuser1`
        * **Contraseña:** `Innova.2025`
    * **Resultado esperado:** Deberías poder acceder al recurso compartido y tener permisos para crear y modificar archivos dentro de él.

---
**Autor:** Giovanni
