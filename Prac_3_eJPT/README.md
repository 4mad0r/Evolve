# Práctica 3 – Pivoting y Movimiento Lateral  
**Autor:** Ignacio Amador López  

## 📌 Descripción  
Esta práctica tiene como objetivo simular un entorno corporativo segmentado y llevar a cabo un ejercicio completo de **penetración interna**, poniendo en práctica técnicas de:

- Enumeración interna  
- Explotación de vulnerabilidades  
- Persistencia en sistemas comprometidos  
- Pivoting entre redes aisladas  
- Compromiso de un dominio Active Directory  

El laboratorio se compone de cuatro máquinas conectadas mediante diferentes redes segmentadas, lo que permite aplicar movimientos laterales utilizando diversas herramientas.

---

## 🏗️ 1. Configuración del laboratorio  

Se prepara un entorno con:

- **Kali Linux** (máquina atacante)  
- **Windows 7** (primer host comprometido)  
- **Ubuntu Server** (segundo salto)  
- **Active Directory Windows Server** (objetivo final)  

Redes utilizadas:

- Adaptador puente  
- Host-Only #2 (Windows ↔ Ubuntu)  
- Host-Only #3 (Ubuntu ↔ AD)  

La Kali es el único punto de acceso inicial, por lo que todos los pivoting deben establecerse desde las máquinas intermedias.

---

## 🖥️ 2. Máquina #1 – Windows 7  

### 🔍 Enumeración  
Se detectan los siguientes puntos clave:

- SMBv1 habilitado  
- `signing: False` (susceptible a NTLM Relay)  
- Vulnerable a **CVE-2017-0144 – EternalBlue**

### 💥 Explotación  
Se utiliza **Metasploit** para explotar EternalBlue obteniendo sesión `meterpreter`.

### 🔒 Persistencia  
Se crea una tarea programada (`schtasks`) que ejecuta una reverse shell cada minuto usando Netcat.

### 🔁 Pivoting desde Windows 7  
Se prueban varias técnicas, destacando:

#### ✔️ **Ligolo-ng**  
Requiere compilar versiones antiguas con Go 1.20.14 para compatibilidad.  
Permite crear un túnel estable desde Windows a Kali, y redirigir tráfico a Ubuntu.

#### ✔️ **Chisel**  
También utilizado compilando versiones antiguas para Windows 7 (Go 1.20.4).  
Útil para túneles SOCKS5 usando `proxychains`.

---

## 🐧 3. Máquina #2 – Ubuntu  

### 🔍 Enumeración  
Servicios expuestos:

- FTP (anónimo permitido)  
- SSH  
- HTTP  

Mediante fuzzing y análisis web se encuentran credenciales en `/ifp/config.txt` que permiten:

- Acceso SSH  
- Acceso NFS  
- Acceso a FTP autenticado

### 💥 Explotación  
Se identifica un **SUID en rsync**, lo que permite escalar privilegios a `root` utilizando técnica de GTFOBins.

### 🔒 Persistencia  
Se habilita acceso persistente vía claves SSH (`authorized_keys`).

### 🔁 Pivoting desde Ubuntu  
Se crea un segundo túnel con **Chisel**, encadenando:

Kali → Windows 7 → Ubuntu → Active Directory  

permitiendo alcanzar la red interna del AD.

---

## 🛑 4. Máquina #3 – Active Directory  

### 🔍 Enumeración  
Se identifican puertos expuestos mediante:

- proxychains  
- escaneo propio desde Ubuntu  
- escáner personalizado (herramienta de compañeros)

Se detecta un servicio web en el puerto **81**, que revela nombres de usuario del dominio.

### 🔍 Validación de usuarios  
Con *kerbrute* se valida un usuario activo: **julian**

### 🛠️ Pruebas de ataques Kerberos  
- AS-REP Roasting descartado (no tiene la flag "Do not require preauth").

### 📂 Enumeración SMB  
Acceso anónimo limitado.  
Se prueban módulos de NetExec (`nxc smb -L`) detectando:

### 💥 Vulnerabilidad crítica: **CVE-2020-1472 – Zerologon**  
El AD es vulnerable, permitiendo:

- Autenticar como Domain Controller sin contraseña  
- Resetear la contraseña de la cuenta de máquina  
- Dumps del archivo **ntds.dit** (hashes NTLM)

### 🗝️ Compromiso total del Dominio  
Se utiliza:

- NetExec para dumpear ntds.dit  
- Autenticación NTLM con hashes  
- Acceso al sistema vía **WinRM** como Administrador de dominio

Resultado: **Control total del Active Directory**.

---

## 🎯 Resultados finales

| Objetivo | Estado |
|---------|--------|
| Explotar Windows 7 (EternalBlue) | ✔️ |
| Movimiento lateral hacia Ubuntu | ✔️ |
| Escalado a root en Ubuntu | ✔️ |
| Pivoting hacia AD | ✔️ |
| Enumeración y explotación del AD | ✔️ |
| Compromiso total del dominio | ✔️ |

---

## 🛠️ Herramientas utilizadas  
- **Nmap**, **Netdiscover**  
- **Metasploit**  
- **NetExec (nxc)**  
- **Kerbrute**  
- **GTFOBins**  
- **Chisel**, **Ligolo-ng**  
- **Proxychains**  
- **Evil-WinRM**  

---

## 📝 Conclusión  
Esta práctica permite realizar un recorrido completo de un ataque interno en un entorno corporativo simulado, aplicando técnicas avanzadas de pivoting, explotación, escalada de privilegios y compromiso total de un dominio Active Directory.  
Se han utilizado herramientas reales, configurado túneles encadenados y explotado vulnerabilidades críticas para demostrar la importancia de la segmentación, el hardening y las actualizaciones de seguridad.
