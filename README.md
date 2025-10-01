# Compilación e Instalación Manual del Kernel 6.1 en Oracle Linux 8

## 📋 Descripción del Proyecto

Este repositorio contiene documentación detallada, nivel universitario, sobre el proceso completo de compilación e instalación manual del kernel Linux 6.1 en Oracle Linux 8 ejecutándose en VirtualBox.

## 📁 Estructura del Repositorio

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

## 📖 Documentos Incluidos

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

## 🎯 Objetivos del Proyecto

1. Documentar el proceso completo de compilación de kernel Linux
2. Proporcionar soluciones a errores comunes en Oracle Linux 8
3. Crear material de referencia para estudiantes de Sistemas Operativos
4. Demostrar competencias en administración de sistemas Linux

## ⚙️ Entorno Utilizado

- **Sistema Operativo:** Oracle Linux 8
- **Virtualización:** VirtualBox
- **Kernel Original:** 5.15.0-312.187.5.3.el8uek.x86_64
- **Kernel Objetivo:** Linux 6.1
- **Herramientas:** DNF, GCC, Make, GRUB2

## 🔧 Errores Documentados y Resueltos

Este proyecto documenta las soluciones a los siguientes errores reales encontrados durante la compilación:

1. **Certificados de firma de módulos faltantes** (`ol_signing_keys.pem`)
2. **Archivo bzImage no generado** (ejecución incorrecta de comandos)
3. **Fallo de generación BTF** (herramienta `pahole` no disponible)

## 📸 Capturas de Pantalla

El directorio `img/` contiene capturas de pantalla de cada paso crítico del proceso, facilitando la replicación exacta del procedimiento.

## 👨‍🎓 Autor

**Eduard Criollo Yule**  
Código: 2220335  
Universidad Autónoma de Occidente  
Sistemas Operativos - 7mo Semestre

## 📝 Notas Importantes

- Este proyecto fue realizado como parte de un trabajo de exoneración del parcial semestral
- Todos los comandos y procedimientos fueron probados y verificados en un entorno real
- Las capturas de pantalla son evidencia del proceso completo ejecutado

## 🚀 Cómo Usar Esta Documentación

1. Lee primero `Paso_a_Paso_Actualizacion_Kernel.md` para tener una visión general
2. Consulta `Explicacion_Comandos_Kernel.md` cuando necesites entender el detalle técnico
3. Sigue los pasos en orden, verificando cada resultado antes de continuar
4. Revisa las secciones de errores comunes si encuentras problemas

## ⚠️ Advertencias

- Siempre haz un snapshot de tu VM antes de comenzar
- La compilación puede tomar entre 30 minutos y 2 horas
- Asegúrate de tener suficiente espacio en disco (mínimo 15 GB libres)
- Este proceso es específico para Oracle Linux 8 en VirtualBox

## 📚 Referencias

- [Kernel.org - Official Linux Kernel Archives](https://www.kernel.org/)
- [Oracle Linux Documentation](https://docs.oracle.com/en/operating-systems/oracle-linux/)
- [The Linux Kernel Documentation](https://www.kernel.org/doc/html/latest/)

---

**Última actualización:** Octubre 2025
