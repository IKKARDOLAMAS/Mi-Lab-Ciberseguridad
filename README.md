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

## 📅 Día 5: Conectividad en Red Real y Pentesting Web

Hoy logré un hito importante en mi formación: pasé de entornos puramente virtuales a un **escenario de red física real**, conectando dos laptops en una arquitectura de ataque-víctima.

### 🌐 1. Establecimiento de Conectividad (Capa de Red)
Tras resolver problemas de sintaxis y configuración de Firewall, logré una conexión estable entre mi estación de ataque (Kali Linux) y mi objetivo físico.

**Evidencia de éxito (Ping Statistics):**
* **Paquetes:** 109 transmitidos / 109 recibidos.
* **Pérdida de datos:** 0%.
* **Latencia promedio (RTT):** 4.3ms.

> *Lección:* La estabilidad de la red es el cimiento de cualquier auditoría. Sin una conexión sólida (0% loss), las herramientas de explotación como Hydra o SQLMap darán falsos negativos.

### 💉 2. Introducción a Vulnerabilidades Web (SQL Injection)
Exploré la teoría y práctica de la inyección de código SQL, centrada en el **OWASP Top 10**.

* **Concepto:** Manipulación de consultas backend mediante el ingreso de caracteres lógicos (`' OR 1=1 --`) en formularios web.
* **Herramienta Analizada:** `sqlmap`. Aprendí que la resolución de nombres (DNS) y el uso de `--random-agent` son vitales para evadir bloqueos básicos de conexión.

### 🛠️ 3. Troubleshooting y Resolución de Problemas
Documento los errores superados hoy, ya que la capacidad de diagnóstico es lo que define a un analista senior:
1.  **Error de Sintaxis en IP:** Corregido el uso de caracteres especiales en la terminal.
2.  **Connection Refused:** Identificado como un fallo de servicios web inactivos o bloqueos de Firewall perimetral.
3.  **Resolución de Host:** Aprendí a mapear IPs manualmente en `/etc/hosts` cuando el DNS falla.

---
*"El hacking no es solo lanzar herramientas, es entender por qué cada bit llega a su destino."*

# 📅 Día 6: Ataque Man-in-the-Middle (MITM) mediante ARP Poisoning

## 📝 Descripción del Proyecto
En esta sesión de laboratorio, escalé de la fase de reconocimiento a la fase de **explotación de protocolos de red**. El objetivo fue posicionar mi estación de trabajo (Kali Linux) como un intermediario invisible entre una víctima física y el Gateway (Router), permitiendo la intercepción y análisis de tráfico de Capa 7.



## 🛠️ Entorno de Laboratorio
* **Atacante:** Kali Linux 2026 (IP: 192.168.1.2).
* **Víctima:** Laptop física (IP: 192.168.1.7).
* **Servidor de Prueba:** Instancia de `http.server` (Python) en puerto 80.
* **Herramienta de Intercepción:** `Ettercap 0.8.4` (Modo Texto/Unified Sniffing).

## 🚀 Implementación Técnica

### 1. Manipulación del Kernel (IP Forwarding)
Para evitar que la víctima perdiera la conexión a internet y el ataque fuera detectado (DoS), habilité el reenvío de paquetes en el núcleo de Linux:
```bash```
```sudo sysctl -w net.ipv4.ip_forward=1```

### 2. Envenenamiento de Tablas ARP
Utilicé Ettercap para enviar paquetes ARP falsificados. El objetivo fue asociar mi dirección MAC con la IP del Router en la tabla de la víctima, y viceversa.

Comando ejecutado: sudo ettercap -T -q -M arp:remote /192.168.1.1// /192.168.1.7//

### 3. Captura y Análisis de Tráfico
* **Se simuló una navegación por parte de la víctima hacia un servidor no cifrado (HTTP).

* **Resultado: Se confirmaron más de 325,000 bytes reenviados exitosamente.

* **Evidencia: Los logs del servidor en la víctima registraron peticiones GET con código 200 OK, validando que el tráfico pasó íntegro a través del nodo atacante.

🔬 Lecciones Aprendidas y Hardening
* **Vulnerabilidad de Protocolos Base: ARP es un protocolo sin estado y sin autenticación, lo que facilita la suplantación.

* **Importancia de TLS: El tráfico interceptado en HTTP es totalmente legible; el uso de HTTPS/HSTS es crítico para la confidencialidad.

* **Defensa: En entornos corporativos, es vital implementar Dynamic ARP Inspection (DAI) y Port Security para mitigar estos riesgos.

## 🛡️ Día 07: Intercepción de Tráfico y Ataque MITM

En esta sesión se realizó una prueba de concepto sobre la interceptación de datos en una red local (LAN) utilizando una arquitectura de ataque **Man-in-the-Middle**. El objetivo fue capturar credenciales de un dispositivo físico externo para validar la inseguridad de los protocolos sin cifrado.

### 🔍 Resumen del Laboratorio
* **Escenario:** Una máquina atacante (Kali Linux) y una víctima (Portátil física) conectadas a la misma red Wi-Fi.
* **Técnica principal:** ARP Spoofing (Envenenamiento de tablas ARP) mediante **Ettercap**.
* **Estrategia de captura:** Implementación de un **Sinkhole HTTP** (Servidor web falso) para interceptar peticiones `POST` directas.



### 🛠️ Desafíos y Soluciones
1. **Filtro de Tráfico:** Se identificó ruido de red (tráfico a IPs de Azure/Microsoft) que dificultaba la captura. Se solucionó configurando un servidor local en Kali para forzar una conexión directa.
2. **Error 404/501:** Durante la fase de servidor, se resolvieron errores de ruta de archivos y permisos de método `POST` mediante la configuración correcta de `http.server` de Python desde el directorio raíz del formulario.
3. **Visibilidad de Datos:** Se utilizó la función *Follow TCP Stream* de Wireshark para reconstruir el paquete y obtener el usuario y la contraseña en texto plano.

### 💡 Aprendizajes Clave
* **Vulnerabilidad de HTTP:** Se confirmó que cualquier dato enviado por el puerto 80 es susceptible de ser leído si un atacante se posiciona entre el cliente y el servidor.
* **Persistencia:** La importancia de adaptar el ataque cuando el plan original (captura pasiva) no es suficiente debido a configuraciones de red o cifrado.

> **Status:** ✅ Laboratorio completado con éxito. Credenciales capturadas y documentadas.

## 🛡️ Día 08: Análisis de Cifrado y Protocolos TLS (Defensa)

Tras el éxito del ataque MITM del Día 7, en esta sesión se analizó la efectividad del cifrado de extremo a extremo (**HTTPS**) como medida de protección fundamental contra la interceptación de datos.

### 🔍 Resumen del Laboratorio
* **Objetivo:** Comparar la visibilidad de los datos en una red interceptada cuando se utilizan protocolos seguros.
* **Escenario:** Captura de tráfico real desde la portátil física hacia dominios con certificados SSL (Wikipedia/GitHub) a través de Kali Linux.
* **Técnica:** Interceptación pasiva de paquetes mediante **Wireshark** bajo un entorno de ARP Spoofing activo.



### 💡 Hallazgos Clave
1. **Confidencialidad:** Al aplicar la técnica de *Follow TCP Stream* en paquetes de datos de aplicación (puerto 443), se confirmó que la información es totalmente ilegible (**Ciphertext**).
2. **Handshake SSL/TLS:** Se identificó el proceso de negociación entre cliente y servidor, validando cómo se establecen las llaves de cifrado antes del envío de información sensible.
3. **Impenetrabilidad:** A diferencia del Día 7 (HTTP), donde las claves eran visibles, el cifrado TLS garantiza que, aunque el atacante posea el paquete, no posee la información.

### 📊 Comparativa de Sesiones
* **Día 07:** Explotación exitosa de tráfico sin cifrar (Vulnerabilidad detectada).
* **Día 08:** Validación de controles de seguridad criptográficos (Mitigación comprobada).

> **Status:** ✅ Análisis de cifrado completado. La integridad y confidencialidad de los datos se mantienen intactas frente a ataques MITM gracias a TLS.

## 🛡️ Día 09: Administración Segura y Hardening con SSH

En esta sesión se configuró un entorno de administración remota entre una estación de trabajo (Portátil - Wi-Fi) y un servidor controlado (Kali Linux - Cable).

### 🔍 Hallazgos Técnicos
* **Intercambio de IPs:** Se validó mediante Wireshark el flujo de datos donde la IP de origen (Portátil) envía peticiones al puerto seguro del servidor (Kali).
* **Análisis de Protocolo:** A diferencia del Día 7 y 8, se observó que el protocolo SSH cifra la comunicación desde el establecimiento de la conexión, impidiendo el análisis de comandos ejecutados vía MITM.
* **Hardening:** Se realizó el cambio del puerto por defecto (22 → 2222) para reducir la superficie de exposición ante escaneos automatizados.

### 📊 Conclusión
El uso de túneles cifrados (SSH) es la única forma segura de gestionar infraestructura. Incluso en una red híbrida, la confidencialidad de la sesión administrativa se mantiene íntegra.

## 🛡️ Día 10: Auditoría de Red y Descubrimiento de Activos (Reconocimiento Activo)

En la décima sesión, pasamos de la administración segura a la **Auditoría de Infraestructura**. El objetivo fue realizar un reconocimiento detallado de una estación de trabajo dentro de una red híbrida para identificar posibles vectores de ataque.

### 🔍 Resumen del Laboratorio
* **Herramienta utilizada:** Nmap (Network Mapper).
* **Objetivo:** Análisis de superficie de ataque en un host Windows 10 (192.xxx.x.x).
* **Técnicas aplicadas:** Escaneo de versiones (`-sV`), detección de OS (`-O`) y escaneo agresivo de servicios (`-A`).

### 📊 Hallazgos Técnicos (Resultados de Nmap)

| Puerto | Estado | Servicio | Versión / Observación |
|--------|--------|----------|-----------------------|
| 135/tcp| Open   | msrpc    | Microsoft Windows RPC |
| 139/tcp| Open   | netbios  | NetBIOS-SSN |
| 445/tcp| Open   | SMB      | Microsoft-ds (Firma de mensajes habilitada pero no requerida) |

### 💡 Análisis de Seguridad (Mentalidad de Auditor)
1. **Detección de Sistema Operativo:** Nmap identificó con precisión el uso de **Windows 10 (Build 1709 - 21H2)**. Esta información es crítica para un analista, ya que permite buscar vulnerabilidades específicas (CVE) no parcheadas.
2. **Vectores Críticos (SMB):** La exposición del puerto **445 (SMB)** es un hallazgo de alta prioridad en entornos financieros, debido a su historial de explotación por Ransomware (ej. WannaCry). 
3. **Análisis de Configuración:** Se detectó que el protocolo SMB permite la conexión sin requerir obligatoriamente la firma de mensajes (`Message signing enabled but not required`), lo que expone al sistema a ataques de **SMB Relay**.
4. **Validación de Hardening:** El puerto personalizado **2222 (SSH)** configurado el Día 9 no apareció en el escaneo estándar de 1,000 puertos, lo que demuestra la efectividad de la "Seguridad por Oscuridad" frente a escaneos rápidos.

### 🚀 Conclusión para el Sector Fintech
El reconocimiento activo permitió identificar servicios que, aunque son estándar en Windows, aumentan la superficie de ataque. La recomendación técnica es restringir el acceso a SMB y MSRPC mediante reglas de firewall perimetral y forzar el firmado de mensajes para proteger la integridad de los datos financieros.

> **Status:** ✅ Auditoría completada. Capacidad de identificación de activos y análisis de riesgos demostrada.
