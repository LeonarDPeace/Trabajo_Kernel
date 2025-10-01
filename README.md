# ## 📁 Estructura del Repositorio

```
Trabajo_Kernel/
├── README.md                               # Este archivo
├── Paso_a_Paso_Actualizacion_Kernel.md    # Guía paso a paso con comandos
├── Explicacion_Comandos_Kernel.md         # Explicaciones técnicas detalladas
└── img/                                    # Capturas de pantalla del proceso
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
## Instalación Manual del Kernel 6.1 en Oracle Linux 8

## Estructura del Repositorio

```
TrabajoKernel/
├── README.md                               # Este archivo
├── Paso_a_Paso_Actualizacion_Kernel.md    # Guía paso a paso con comandos
├── Explicacion_Comandos_Kernel.md         # Explicaciones técnicas detalladas
└── img/                                    # Capturas de pantalla del proceso
    ├── uname-r_comando.png
    ├── dnf-update_comando.png
    ├── menuconfig_01.png
    └── ... (más capturas)
```

## Documentos Incluidos

### 1. [Paso_a_Paso_Actualizacion_Kernel.md](./Paso_a_Paso_Actualizacion_Kernel.md)
Guía práctica con todos los comandos necesarios, organizada en 10 secciones:
- Verificación del sistema
- Preparación del entorno
- Descarga del código fuente
- Configuración del kernel
- Compilación e instalación
- Actualización de GRUB
- Verificación post-instalación
- Solución de errores comunes

### 2. [Explicacion_Comandos_Kernel.md](./Explicacion_Comandos_Kernel.md)
Documento técnico complementario que explica:
- Qué hace cada comando
- Por qué se utiliza
- Detalles técnicos del proceso
- Troubleshooting avanzado
- Glosario de términos

## Entorno Utilizado

- **Sistema Operativo:** Oracle Linux 8
- **Virtualización:** VirtualBox
- **Kernel Original:** 5.15.0-312.187.5.3.el8uek.x86_64
- **Kernel Objetivo:** Linux 6.1
- **Herramientas:** DNF, GCC, Make, GRUB2

## Errores Documentados y Resueltos

Este proyecto documenta las soluciones a los siguientes errores reales encontrados durante la compilación:

1. **Certificados de firma de módulos faltantes** (`ol_signing_keys.pem`)
2. **Archivo bzImage no generado** (ejecución incorrecta de comandos)
3. **Fallo de generación BTF** (herramienta `pahole` no disponible)

##  Capturas de Pantalla

El directorio `img/` contiene capturas de pantalla de cada paso crítico del proceso, facilitando la replicación exacta del procedimiento.

## Autor

**Eduard Criollo Yule**  
Código: 2220335  
Universidad Autónoma de Occidente  
Sistemas Operativos - 7mo Semestre