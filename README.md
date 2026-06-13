# DNS-Spoofing-DNS-Poisoning
DNS Spoofing / DNS Poisoning
Documentación Técnica Profesional
Autor
Darwing Peña
Matrícula: 20242690
________________________________________
1. Objetivo del Laboratorio
Implementar y analizar un escenario de DNS Spoofing/DNS Poisoning dentro de un entorno controlado utilizando máquinas virtuales Kali Linux conectadas mediante GNS3.
El objetivo principal fue demostrar cómo un atacante puede manipular el proceso de resolución de nombres de dominio para redirigir a una víctima hacia recursos controlados por el atacante.
________________________________________
2. Objetivo del Script
El script desarrollado tiene como finalidad actuar como un servidor DNS básico capaz de responder consultas DNS realizadas por los equipos de la red.
Durante la práctica, el servidor DNS respondió consultas para el dominio utilizado en el laboratorio, permitiendo comprobar el funcionamiento del proceso de resolución de nombres y posteriormente demostrar cómo una respuesta DNS alterada puede afectar la navegación de los usuarios.
________________________________________
3. Requisitos para Utilizar la Herramienta
Hardware
•	Tres máquinas virtuales Kali Linux.
•	Un router virtual.
•	Un switch virtual.
•	Equipo anfitrión con GNS3 instalado.
Software
•	Kali Linux.
•	Python 3.
•	Biblioteca socket.
•	Biblioteca threading.
Requisitos de Red
•	Conectividad entre todos los equipos.
•	Comunicación IP funcional.
•	Acceso al puerto UDP 53.
•	Servidor web operativo para pruebas de resolución DNS.
________________________________________
4. Documentación de la Red
Topología
Equipo	Función	Dirección IP
Kali Guest 1	Víctima	20.24.26.91
Kali Guest 2	Servidor DNS + Web Oficial	20.24.26.92
Kali Atacante	Equipo atacante	20.24.26.90
Router R1	Gateway	20.24.26.1
________________________________________
Interfaces
Equipo	Interfaz
Kali Atacante	eth1
Kali Guest 1	eth0
Kali Guest 2	eth0
Router R1	Gi0/2
________________________________________
Direccionamiento IP
Red utilizada:
20.24.26.0/24
Gateway:
20.24.26.1
DNS utilizado:
20.24.26.92
________________________________________
5. Parámetros Utilizados
Durante la ejecución del laboratorio se utilizaron los siguientes valores:
Parámetro	Valor
Dirección DNS	20.24.26.92
Dominio consultado	itla.edu.do
Puerto DNS	53/UDP
Dirección web oficial	20.24.26.92
Dirección atacante	20.24.26.90
________________________________________
6. Funcionamiento del Script
Inicialización
El programa crea un socket UDP y escucha permanentemente en el puerto 53.
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.bind(("0.0.0.0", 53))
________________________________________
Recepción de consultas DNS
Cada vez que un cliente realiza una consulta DNS, el servidor recibe el paquete y crea un nuevo hilo para procesarlo.
threading.Thread(target=handle, args=(data, addr, sock)).start()
________________________________________
Procesamiento de la consulta
El script analiza manualmente el paquete DNS para extraer el nombre de dominio solicitado por el cliente.
Ejemplo observado durante el laboratorio:
itla.edu.do
________________________________________
Generación de la respuesta
El servidor construye una respuesta DNS válida y devuelve la dirección IP configurada en el script.
ip = "20.24.26.92"
________________________________________
Respuesta al cliente
La respuesta DNS es enviada al equipo solicitante.
Ejemplo observado:
[DNS] Consulta: itla.edu.do de 20.24.26.91
[DNS] Respondido itla.edu.do -> 20.24.26.92
________________________________________
7. Desarrollo del Laboratorio
Fase 1: Configuración de Direccionamiento
Se asignaron las siguientes direcciones IP:
Equipo	Dirección
Atacante	20.24.26.90
Víctima	20.24.26.91
DNS Oficial	20.24.26.92
________________________________________
Fase 2: Implementación del Servidor Web Oficial
Se creó una página web de prueba:
<h1>Pagina OFICIAL del ITLA</h1>
La página fue publicada mediante:
sudo python3 -m http.server 80
________________________________________
Fase 3: Implementación del Servidor DNS
Se ejecutó el script DNS:
sudo python3 dns-server.py
________________________________________
Fase 4: Configuración de la Víctima
Se configuró el archivo resolv.conf para utilizar como DNS el servidor:
sudo sh -c 'echo "nameserver 20.24.26.92" > /etc/resolv.conf'
________________________________________
Fase 5: Verificación
Se verificó la resolución DNS:
curl http://20.24.26.92
Mostrando correctamente la página oficial.
________________________________________
Figura 1 
________________________________________
 <img width="791" height="572" alt="image" src="https://github.com/user-attachments/assets/f2e5a7b3-a2d4-4190-9c32-b904b01d546c" />
<img width="700" height="595" alt="image" src="https://github.com/user-attachments/assets/b1ebc003-28fc-457d-a742-ec700b007acf" />
<img width="940" height="529" alt="image" src="https://github.com/user-attachments/assets/5ed35f13-0b38-47a4-aa71-830e0f48572e" />
<img width="940" height="439" alt="image" src="https://github.com/user-attachments/assets/1ca6c1d3-6773-4ba2-b755-f5af07c9ab58" />
<img width="940" height="727" alt="image" src="https://github.com/user-attachments/assets/17996891-b9e6-4f3f-88cf-76cb840c6807" />

 
9. Contramedidas
Para reducir el riesgo de ataques DNS Spoofing y DNS Poisoning se recomienda:
DNSSEC
Implementar DNSSEC para validar criptográficamente las respuestas DNS.
Dynamic ARP Inspection (DAI)
Evitar ataques de envenenamiento ARP dentro de la red local.
DHCP Snooping
Permitir únicamente servidores DHCP autorizados.
Segmentación de Red
Separar usuarios y servicios críticos mediante VLANs.
HTTPS
Utilizar certificados válidos para evitar redirecciones fraudulentas.
IDS/IPS
Implementar soluciones como Snort o Suricata para detectar actividades sospechosas.
Monitoreo
Auditar periódicamente registros DNS y tráfico de red.
________________________________________
10. Conclusiones
Durante el laboratorio se logró implementar un servidor DNS funcional utilizando Python y comprobar el proceso de resolución de nombres dentro de una red local controlada.
La práctica permitió comprender el funcionamiento interno de DNS, identificar los riesgos asociados a la manipulación de respuestas DNS y analizar los mecanismos de protección que pueden emplearse para mitigar este tipo de ataques.
Los resultados demuestran la importancia de implementar controles de seguridad adecuados tanto en la infraestructura DNS como en la red local para prevenir ataques de DNS Spoofing y DNS Poisoning.
________________________________________
