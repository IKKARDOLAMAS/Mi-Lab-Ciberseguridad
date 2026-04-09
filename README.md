# Mi Laboratorio de Ciberseguridad 💻

¡Hola! Estoy aprendiendo ciberseguridad desde cero, enfocándome en el lado ofensivo (Red Teaming). En este repositorio iré documentando mi progreso, la configuración de mi entorno y los conceptos que voy dominando.

Este repositorio contiene mi progreso desde cero en el mundo del **Red Teaming** (Hacking Ético Ofensivo). 

## 📅 Día 1: Cimientos y Entorno de Trabajo
Hoy configuré mi entorno seguro y aprendí los comandos básicos de supervivencia en Linux.

### 🏗️ 1. Configuración del Entorno
* **Virtualizador:** Oracle VirtualBox.
* **Sistema Operativo:** Kali Linux (Distribución especializada en Pentesting).
* **Concepto clave:** Uso de Máquinas Virtuales (VM) para aislar herramientas de ataque y proteger el sistema anfitrión (Windows).

### ⌨️ 2. Comandos de Terminal (CLI) Dominados
| Comando | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `sudo apt update` | Actualiza la lista de programas (repositorios). | `sudo apt update` |
| `whoami` | Muestra el usuario actual en el sistema. | `whoami` |
| `pwd` | "Print Working Directory": Muestra dónde estoy parado. | `pwd` |
| `ls` | Lista los archivos y carpetas actuales. | `ls` |
| `mkdir` | Crea una carpeta nueva. | `mkdir mis_ataques` |
| `cd` | Cambia de directorio (entrar/salir de carpetas). | `cd mis_ataques` |
| `cd ..` | Sube un nivel (sale de la carpeta actual). | `cd ..` |
| `nano` | Editor de texto en terminal para crear scripts/notas. | `nano notas.txt` |
| `cat` | Muestra el contenido de un archivo en pantalla. | `cat notas.txt` |
| `ip a` | Muestra la configuración de red y mi dirección IP. | `ip a` |

### 🧠 3. Conceptos Teóricos
* **Modelo OSI:** Entendí que la comunicación ocurre en 7 capas.
* **Puertos y Protocolos:** Los puertos son "ventanas" de entrada. El puerto 80 (HTTP) y 443 (HTTPS) son los objetivos web principales.
* **Sombrero Blanco vs Negro:** Mi enfoque es el hacking ético (Sombrero Blanco) para ayudar a las empresas a ser más seguras.

---
*"La curiosidad es la mejor herramienta de un hacker."*

## 📅 Día 2: Reconocimiento y Escaneo de Redes (Nmap)
Hoy me enfoqué en la fase de **Reconocimiento**, aprendiendo a identificar dispositivos y servicios activos en una red de forma profesional.

### 🔍 1. Comandos de Nmap Dominados
Aprendí a usar la herramienta estándar de la industria para descubrir "puertas abiertas":

| Comando | Función | Objetivo |
| :--- | :--- | :--- |
| `nmap <IP_OBJETIVO>` | Escaneo básico | Identificar puertos abiertos comunes. |
| `nmap -sV` | Detección de Versiones | Saber qué software exacto corre en el puerto. |
| `nmap -sS` | SYN Scan (Stealth) | Escaneo sigiloso que no completa la conexión TCP. |
| `nmap -sn` | Ping Sweep | Descubrir qué máquinas están encendidas en la red. |
| `nmap -oN` | Guardar Resultados | Exportar el hallazgo a un archivo de texto (.txt). |

### 🛰️ 2. Descubrimiento de Red Local (Ofuscado)
Realicé un análisis de mi segmento de red interna para identificar la topología del laboratorio:

* **Mi IP Local:** `10.0.X.15` (Interfaz `eth0`).
* **Gateway detectado:** `10.0.X.2` (Puerta de enlace virtual).
* **Escaneo de Red:** Se identificaron 3 dispositivos activos en el segmento `/24`.

### 🎯 3. Análisis de Servicios Críticos
Al escanear el objetivo principal, identifiqué servicios de Windows que son vectores de ataque comunes:

* **Puerto 445 (SMB):** Servicio de compartición de archivos. Históricamente vulnerable a exploits como *EternalBlue*.
* **Puerto 135 (MSRPC):** Comunicación entre procesos.
* **Estado "Filtered":** Detecté la presencia de un Firewall bloqueando la mayoría de los puertos.

### 🧠 Concepto Clave del Día: El "Three-Way Handshake"
Aprendí cómo se comunican las máquinas mediante el saludo de 3 pasos (SYN, SYN-ACK, ACK). Entender este proceso es lo que permite a un atacante saber si una puerta está abierta, cerrada o protegida por un firewall sin ser detectado fácilmente.

---
*Notas: Todas las IPs han sido ofuscadas por seguridad siguiendo las mejores prácticas de ciberseguridad.*
