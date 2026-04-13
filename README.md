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

Si el escaneo se rinde usas este comando mas tryhard :
Para evitar que el escaneo se rinda, vamos a usar un comando que sea más "paciente" o que ataque de forma más inteligente. Prueba con este:

sudo nmap -Pn -F --max-retries 1 scanme.nmap.org 

### 🛰️ 2. Descubrimiento de Red Local (Ofuscado)
Realicé un análisis de mi segmento de red interna para identificar la topología del laboratorio:

* **Mi IP Local:** `xx.x.X.15` (Interfaz `eth0`).
* **Gateway detectado:** `xx.x.X.2` (Puerta de enlace virtual).
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

### 🎯 4. Descubrimiento de Hosts en Red Local
Ejecuté un escaneo de red en el segmento `xx.x.2.0/24` obteniendo los siguientes resultados:
* **Hosts detectados:** 3 (IPs: .2, .3, .15).
* **Herramienta usada:** `nmap -sn` (Ping Sweep).
* **Conclusión:** Identifiqué la infraestructura de red virtual proporcionada por VirtualBox antes de proceder al escaneo de servicios.

* ### 🔎 5. Análisis de Puertos y Servicios (Escaneo SYN)
Realicé un escaneo sigiloso (`-sS`) al objetivo `xx.x.2.2` detectando servicios críticos de Windows:

* **Puerto 445 (SMB):** Servicio de intercambio de archivos. Es un vector de ataque histórico (ej. EternalBlue).
* **Puerto 135 (MSRPC):** Comunicación entre procesos de Windows.
* **Estado "Filtered":** 97 puertos están protegidos por un Firewall que no respondió a mis paquetes.

**Conclusión:** El objetivo es una máquina Windows. El siguiente paso lógico sería realizar una enumeración de SMB para ver si hay carpetas compartidas sin contraseña.

## 📅 Día 3: Análisis de Vulnerabilidades
Hoy aprendí a identificar debilidades específicas en los servicios detectados ayer.

### 🛠️ Herramientas de Investigación
| Herramienta | Función |
| :--- | :--- |
| `searchsploit` | Busca exploits (códigos de ataque) en la base de datos local de Kali. |
| **NSE Scripts** | Scripts de Nmap para verificar vulnerabilidades automáticamente. |
| **CVE** | Diccionario estándar de vulnerabilidades de seguridad conocidas. |

### 🔬 Práctica Real: Escaneo de Vulnerabilidades
Ejecuté un script de Nmap para verificar vulnerabilidades en el servicio SMB (Puerto 445):
`nmap -p 445 --script smb-vuln-* [IP_OBJETIVO]`

### 🧠 Concepto Clave: "Exploit" vs "Vulnerabilidad"
* **Vulnerabilidad:** Es el hueco en la seguridad (la ventana rota).
* **Exploit:** Es la herramienta o código que se usa para aprovechar ese hueco y entrar.

### 🕵️‍♂️ 4. Interpretación de Resultados de Scripts
Al ejecutar el script `ms17-010` contra el objetivo `XX.X.X.2`:
* **Resultado:** El puerto 445 está abierto, pero el script no reportó vulnerabilidad.
* **Lección:** Un puerto abierto no siempre significa una entrada directa. La ausencia de reporte del script indica que el sistema probablemente cuenta con los parches de seguridad (Security Patches) contra EternalBlue.

### 🛠️ Uso de Comodines en Scripts
Aprendí a usar el comodín `*` para lanzar múltiples escaneos de vulnerabilidad simultáneos:
`nmap -p 445 --script "smb-vuln-*"`

### 🛡️ 5. Resultados de Escaneo con Comodines
Ejecuté una batería de scripts `smb-vuln-*` para agotar las posibilidades de ataque en el puerto 445.

* **Resultado ms10-054:** Negativo (False). El sistema está parcheado contra este desbordamiento de memoria.
* **Resultado ms10-061:** Error de negociación. El sistema objetivo rechazó la conexión durante la fase de reconocimiento, lo que indica presencia de medidas de protección activas.
* **Conclusión Final del Día:** El reconocimiento avanzado permite descartar vectores de ataque inútiles, ahorrando tiempo y evitando ser detectado por intentar usar exploits que no funcionarán.

## 📅 Día 4: Ataques de Diccionario y Fuerza Bruta (Hydra)
Hoy pasé de la fase de análisis de vulnerabilidades a la fase de **explotación de credenciales**, aprendiendo a manejar diccionarios masivos y a depurar ataques fallidos.

### 🛠️ 1. Herramientas y Munición Digital
Para adivinar contraseñas no se ataca al azar; se utiliza "munición" basada en filtraciones reales:

* **Rockyou.txt:** El diccionario estándar con 14.3 millones de contraseñas. 
* **Ubicación:** `/usr/share/wordlists/`.
* **Dato curioso:** Al analizar el archivo con `head -n 3`, confirmé que las contraseñas más usadas son `123456`, `password` y `12345`.

### 🐉 2. Dominando Hydra (The Password Cracker)
Aprendí la sintaxis de **Hydra** para realizar ataques de fuerza bruta dirigidos.

| Flag | Función | Uso Práctico |
| :--- | :--- | :--- |
| `-l` | Usuario único | Cuando el nombre es conocido (ej. `admin`). |
| `-P` | Diccionario | Para cargar la lista `rockyou.txt`. |
| `-t` | Tasks (Hilos) | Controla la velocidad. `-t 1` es necesario para evitar bloqueos en SMB. |
| `-s` | Puerto | Para especificar un puerto manual si no es el estándar. |

### 🔬 3. Bitácora de Troubleshooting (Depuración)
En ciberseguridad, un error es una respuesta del servidor que debemos saber leer. Realicé pruebas contra dos servicios:

1. **Target SMB (Puerto 445):** * **Resultado:** `[ERROR] invalid reply from target`.
   * **Lección:** Los sistemas modernos detectan el flujo de intentos y cortan la conexión (Protección Anti-Brute Force). Es vital ajustar la velocidad con `-t 1`.
   
2. **Target SSH (Puerto 22):** * **Resultado:** `[ERROR] Connection refused`.
   * **Lección:** Confirmación de puerto cerrado. No se puede atacar lo que no está disponible; la enumeración previa es la clave del éxito.

### 🧠 Concepto Clave: Password Spraying
Aprendí que es más efectivo probar una sola contraseña común contra muchos usuarios diferentes, en lugar de muchas contraseñas contra un solo usuario. Esto evita el **Account Lockout** (bloqueo de cuenta) y permite pasar desapercibido ante los sistemas de monitoreo.

---
*Próximo objetivo: Ataques a aplicaciones Web (SQL Injection).*
