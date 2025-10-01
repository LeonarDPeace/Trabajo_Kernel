# Compilación e Instalación Manual del Kernel 6.1 en Oracle Linux 8 (VirtualBox)

## Introducción

Este documento describe, con nivel universitario y máximo detalle, el proceso para compilar e instalar manualmente el kernel 6.1 en Oracle Linux 8 ejecutándose en una máquina virtual VirtualBox. Se incluyen instrucciones, explicaciones, recomendaciones y ejemplos de cómo documentar el proceso con capturas de pantalla.

---

## 1. Verificar la versión actual del kernel

Antes de iniciar cualquier actualización, es fundamental saber qué versión del kernel está corriendo actualmente.

```bash
uname -r
```

**Salida esperada:** Versión del kernel actual (ej: `5.15.0-312.187.5.3.el8uek.x86_64`)

![Versión actual del Kernel](img/uname-r_comando.png)
*Versión actual del kernel en ejecución*

---

## 2. Verificar conectividad y repositorios

Asegúrate de tener acceso a internet y que los repositorios estén correctamente configurados.

```bash
ping -c 4 www.google.com
sudo dnf repolist
```

**Captura de pantalla:**

![Repositorios configurados](img/dnf-repolist_comando.png)
*Repositorios DNF configurados y habilitados*

---

## 3. Buscar actualizaciones disponibles del kernel

```bash
sudo dnf check-update kernel
```

**Captura de pantalla:**

![Actualización disponible del kernel](img/dnf-check-update-kernel_comando.png)
*Verificación de actualizaciones disponibles del kernel*

---

## 4. Preparar el entorno de compilación

```bash
sudo dnf update -y
sudo dnf groupinstall "Development Tools" -y
sudo dnf install ncurses-devel bison flex elfutils-libelf-devel openssl-devel gcc make wget bc perl -y
```

**Capturas de pantalla del proceso:**

![Actualización del sistema](img/dnf-update_comando.png)
*Actualización de todos los paquetes del sistema*

![Instalación de herramientas de desarrollo 01](img/devtools-install_comando_01.png)
*Instalación de Development Tools - Parte 1*

![Instalación de herramientas de desarrollo 02](img/devtools-install_comando_02.png)
*Instalación de Development Tools - Parte 2*

---

## 5. Descargar el código fuente del kernel 6.1

```bash
wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.tar.xz
tar -xvf linux-6.1.tar.xz
cd linux-6.1
```

**Capturas de pantalla del proceso:**

![Descarga del kernel](img/wget-kernel_comando.png)
*Descarga del código fuente del kernel 6.1 desde kernel.org*

![Extracción del kernel](img/tar-kernel_comando.png)
*Extracción del archivo comprimido del kernel*

![Cambio de carpeta Linux 6.1](img/cd-linux-6.1_comando.png)
*Acceso al directorio del código fuente*

---

## 6. Configurar el kernel

1. **Copia la configuración actual como base:**
   ```bash
   cp /boot/config-$(uname -r) .config
   ```
   Esto te permite partir de una configuración probada y compatible con tu sistema.

2. **Abre el menú de configuración:**
   ```bash
   make menuconfig
   ```
   Se abrirá una interfaz en la terminal donde puedes personalizar las opciones del kernel.

3. **Revisa y ajusta las opciones recomendadas:**
   - Drivers de hardware: Activa los controladores necesarios para tu entorno (por ejemplo, red Intel PRO/1000, USB, almacenamiento).
   - Sistemas de archivos: Activa ext4 y otros sistemas que uses.
   - Virtualización: Activa soporte para KVM si lo necesitas.
   - Opciones de seguridad: Revisa SELinux y otras según tu entorno.

4. **Desactiva la verificación y firma de módulos:**
   - Navega a **Enable loadable module support**.
   - Desactiva las opciones:
     - `Require modules to be validly signed`
     - `Automatically sign all modules`
   - Si la opción **Module signature verification** aparece como activada y no se puede desactivar (`[*]` y `---`), es porque otra opción la fuerza.
   - Ve a **Security options** y desactiva cualquier opción relacionada con "lockdown" o restricciones de seguridad que puedan forzar la firma de módulos.

5. **Soluciona problemas de certificados:**
   - Si la compilación falla por falta de `certs/ol_signing_keys.pem`, edita el archivo `.config`:
     - Busca la línea:
       ```
       CONFIG_SYSTEM_TRUSTED_KEYS="certs/ol_signing_keys.pem"
       ```
       y cámbiala por:
       ```
       CONFIG_SYSTEM_TRUSTED_KEYS=""
       ```
     - Busca la línea:
       ```
       CONFIG_SYSTEM_REVOCATION_KEYS="..."
       ```
       y cámbiala por:
       ```
       CONFIG_SYSTEM_REVOCATION_KEYS=""
       ```
   - Guarda los cambios y vuelve a intentar la compilación.

6. **Guarda la configuración:**
   - En el menú, selecciona `<Save>` y confirma el nombre del archivo (`.config`).
   - Sal del menú con `<Exit>`.

7. **Documenta el proceso:**
   - Toma capturas de pantalla de los pasos clave y guárdalas en la carpeta `img` para tu informe.

**Capturas de pantalla del menú de configuración:**

![Configuración del kernel 01](img/menuconfig_01.png)
*Pantalla principal del menú de configuración (menuconfig)*

![Configuración del kernel 02](img/menuconfig_02.png)
*Navegación por opciones específicas de configuración*

![Configuración del kernel 03](img/menuconfig_03.png)
*Configuración de módulos y drivers del kernel*

### Recomendaciones de configuración en menuconfig

Para el menú de configuración se siguieron las siguientes recomendaciones:

1. **Drivers de hardware (VirtualBox)**<br>
- Usa las flechas para navegar.<br>
- Ve a Device Drivers → Network device support → Ethernet driver support → Intel(R) PRO/1000 Gigabit Ethernet support.<br>
**Presiona M para activarlo como módulo (<M>).**<br>
- Ve a Device Drivers → USB support.<br>
**Activa USB xHCI HCD (USB 3.0) support con Y o M.**<br>
- Ve a Device Drivers → Block devices.<br>
**Activa Virtio block driver con Y o M.**<br>

2. **Sistemas de archivos**<br>
- Ve a File systems.<br>
**Activa ext4 con Y.**
**Activa otros sistemas de archivos que uses (por ejemplo, xfs, btrfs, NTFS) con Y o M según tu necesidad.**

3. **Virtualización**<br>
- Ve a Virtualization.<br>
**Activa KVM for Intel processors support con Y (si tu procesador es Intel).**<br>
**Activa KVM for AMD processors support con Y (si tu procesador es AMD).**<br>

4. **Seguridad**<br>
- Ve a Security options.<br>
**Activa Enable SELinux support con Y si tu sistema lo requiere.**<br>
**Activa otras opciones de seguridad según tus necesidades.**<br>

5. **Opciones generales**<br>
- Ve a General setup.<br>
**Verifica que 64-bit kernel esté activado ([*]).**<br>
**Revisa otras opciones generales y déjalas por defecto si no estás seguro.**<br>

6. **Guardar la configuración**<br>
- Presiona Tab para seleccionar <Save>.<br>
- Presiona Enter y confirma el nombre del archivo (.config).

7. **Salir del menú**<br>
- Selecciona <Exit> o presiona Esc Esc hasta salir completamente.

---

## 7. Compilar e instalar el kernel

### 7.1. Comandos de compilación

```bash
make -j$(nproc)
```

Este comando compila el kernel utilizando todos los núcleos del procesador disponibles, lo que acelera significativamente el proceso.

**⏱️ Tiempo estimado:** Entre 30 minutos y 2 horas, dependiendo del hardware.

### 7.2. Instalación de módulos y kernel

Una vez completada la compilación **sin errores**, ejecuta:

```bash
sudo make modules_install
sudo make install
```

**Importante:** Ejecuta estos comandos **en ese orden**. El comando `make install`:
- Copia `vmlinuz-6.1.0` a `/boot/`
- Copia `System.map-6.1.0` a `/boot/`
- Genera `initramfs-6.1.0.img` en `/boot/`
- Actualiza GRUB automáticamente

### 7.3. Errores comunes y soluciones

**Error 1: Missing file: arch/x86/boot/bzImage**

Si al ejecutar `sudo make install` aparece:
```
*** Missing file: arch/x86/boot/bzImage
*** You need to run "make" before "make install".
make: *** [arch/x86/Makefile:286: install] Error 1
```

**Causa:** No se ejecutó el comando `make` o la compilación falló antes de completarse.

**Solución:**
1. Verifica que no haya errores pendientes en la compilación.
2. Ejecuta de nuevo:
   ```bash
   make -j$(nproc)
   ```
3. Una vez completado sin errores, ejecuta:
   ```bash
   sudo make modules_install
   sudo make install
   ```

---

**Error 2: Failed to generate BTF for vmlinux (pahole no disponible)**

Si durante la compilación aparece:
```
BTF: .tmp_vmlinux.btf: pahole (pahole) is not available
Failed to generate BTF for vmlinux
Try to disable CONFIG_DEBUG_INFO_BTF
make[1]: *** [scripts/Makefile.vmlinux:34: vmlinux] Error 1
make: *** [Makefile:1236: vmlinux] Error 2
```
Esto ocurre porque la herramienta `pahole` no está disponible en los repositorios de Oracle Linux 8.

**Solución:**
1. Abre el archivo `.config` en el directorio del kernel:
   ```bash
   nano .config
   ```
2. Busca la línea `CONFIG_DEBUG_INFO_BTF=y` y cámbiala por:
   ```
   CONFIG_DEBUG_INFO_BTF=n
   ```
3. Guarda los cambios (Ctrl+O, Enter) y cierra el editor (Ctrl+X).
4. Limpia los archivos generados previamente:
   ```bash
   make clean
   ```
5. Vuelve a compilar:
   ```bash
   make -j$(nproc)
   ```

**Explicación:** BTF (BPF Type Format) requiere la herramienta `pahole` que no está disponible en Oracle Linux 8. Desactivar esta opción permite compilar sin problemas.

---

### 7.4. Documentación del proceso

Documenta todos los pasos de la compilación e instalación:

![Compilación del kernel](img/make-kernel_comando.png)
*Proceso de compilación del kernel con make*

![Instalación de módulos](img/modules-install_comando.png)
*Instalación de módulos del kernel*

![Instalación del kernel](img/make-install_comando.png)
*Instalación final del kernel en /boot*

---

## 8. Actualizar GRUB y reiniciar

### 8.1. Verificar la instalación del kernel antes del reinicio

**Antes de reiniciar**, es crucial verificar que el kernel se instaló correctamente en `/boot/`:

```bash
ls -lh /boot/vmlinuz-6.1*
ls -lh /boot/initramfs-6.1*
ls -lh /boot/System.map-6.1*
```

**Debes ver:** Tres archivos para el kernel 6.1 (vmlinuz, initramfs y System.map). Si no aparecen, ejecuta:

```bash
cd /root/linux-6.1
sudo make modules_install
sudo make install
```

### 8.2. Actualizar GRUB

Una vez confirmado que los archivos del kernel están en `/boot/`, actualiza GRUB:

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

**Salida esperada:** GRUB detectará automáticamente el nuevo kernel 6.1 y lo añadirá al menú de arranque.

### 8.3. Reiniciar el sistema

```bash
sudo reboot
```

**⚠️ Importante:** Durante el reinicio, puede aparecer una pantalla negra con mensajes de VirtualBox (como "VBOX CD-ROM" o "PXE-E06"). Esto es normal. Espera unos segundos y aparecerá el menú de GRUB mostrando el nuevo kernel 6.1.

**Capturas de pantalla:**

![Actualización de GRUB](img/grub-update_comando.png)
*Actualización del gestor de arranque GRUB*

![Reinicio tras compilación](img/reboot-kernel61_comando.png)
*Reinicio del sistema para cargar kernel 6.1*

---

## 9. Verificar el kernel activo tras el reinicio

```bash
uname -r
```

**Salida esperada:** `6.1.0` o versión similar

![Kernel 6.1 activo](img/uname-r-kernel61_comando.png)
*Verificación del kernel 6.1 activo después del reinicio*

---

## 10. Ejemplo de estructura de carpetas para la entrega

```
Trabajo_Kernel/
├── Paso_a_Paso_Actualizacion_Kernel.md
├── Explicacion_Comandos_Kernel.md
├── README.md
└── img/
    ├── uname-r_comando.png
    ├── dnf-repolist_comando.png
    ├── dnf-check-update-kernel_comando.png
    ├── dnf-update_comando.png
    ├── devtools-install_comando_01.png
    ├── devtools-install_comando_02.png
    ├── wget-kernel_comando.png
    ├── tar-kernel_comando.png
    ├── cd-linux-6.1_comando.png
    ├── menuconfig_01.png
    ├── menuconfig_02.png
    ├── menuconfig_03.png
    ├── make-kernel_comando.png
    ├── modules-install_comando.png
    ├── make-install_comando.png
    ├── grub-update_comando.png
    ├── reboot-kernel61_comando.png
    └── uname-r-kernel61_comando.png
```