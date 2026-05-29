<img width="642" height="266" alt="Captura de pantalla 2026-05-28 210027" src="https://github.com/user-attachments/assets/93dfe988-99d3-43bc-9ad6-6f9a4f59f7d3" />

# Resoluci-n-Maquina-Ghost

Maquina de la plataforma www.whoami-labs.com

Inicie la maquina vulnerable

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 002117" src="https://github.com/user-attachments/assets/a61335fd-6e44-4cee-bec8-bb7a783485e2" />

# ENUMERACION

Iniciada la maquina procedi a realizar un escaneo de puertos para ver con cuales contbamos abiertos, las distintas versiones que se encontraban ejecuanto si fuera el caso

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 002201" src="https://github.com/user-attachments/assets/371c08a0-298e-4f4f-bde3-c9d7ef9c8f59" />

Hice un fuzzing utilizando la herramienta feroxbuster ya que cuando ingresaba al puerto 80 mediante un navegador me aparecia el aviso del servidor apache, es importante decir que encontramos varias rutas que me llamaron bastante la atención

<img width="1914" height="1031" alt="Captura de pantalla 2026-05-28 213055" src="https://github.com/user-attachments/assets/3b46dc27-eaa8-4088-9dd1-9123f8978b71" />

Ya sabiendo cada una de las rutas fui a verificar cada una a ver si encontraba algo que me fuera util para ingresar a la maquina

En la primera pagina se identificó un directorio expuesto bajo la ruta /docs/ titulado "GhostLab Documentation". Esta página contiene detalles técnicos sobre los usuarios del sistema, restricciones de entorno, ubicaciones de archivos y binarios del sistema, lo que facilita vectores claros para el acceso inicial y la posterior escalada de privilegios.

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 002422" src="https://github.com/user-attachments/assets/7062cecd-fedb-49b1-90ce-25af58e588cb" />

Luego pase a verificar la siguiente pagina

En esta pagina encontre algo parecido a una flag la cual intente utilizarla pero me dio error, en este caso supuse que el vector de ataque exacto utilizado para ingresar seria un descifrado exitoso de un hash MD5 (MD5 crack) que permitió el acceso inicial. Mas adelante sabremos si estaba en lo correcto

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 002351" src="https://github.com/user-attachments/assets/633a3f22-f6d9-4903-9ae2-3868560a0eda" />

Segui buscando

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 002532" src="https://github.com/user-attachments/assets/ddf406f2-80a0-4ac3-83ac-8ef399236224" />

En esta fase encontre dos posibles usuarios

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 002552" src="https://github.com/user-attachments/assets/86f2ac40-ed93-41e7-9ed0-aee59b73089b" />

En la proxima imagen vemos el usuario ghost, luego un hash en md5, y en la parte de abajo encontramos un texto que nos dice segment2 que seria una parte de una posible contraseña

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 002604" src="https://github.com/user-attachments/assets/35239529-a674-4441-ad26-815d260df7fc" />

Descrifre el hash

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 003404" src="https://github.com/user-attachments/assets/da9ac7bd-c390-455e-92ed-a47e753b2f1a" />

# EXPLOTACION

Trate de ingresar por ssh utilizando el hash ya descifrado con el usuario ghost y obtuve acceso a la maquina

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 004526" src="https://github.com/user-attachments/assets/48760f71-03a3-42d7-9bfc-6f64dbbe76f5" />

Busque dentro de los posibles directorios a ver si lograba conseguir algo de importancia, y consegui el segmento1 del token de inicio

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 004849" src="https://github.com/user-attachments/assets/bc1d54e6-3557-4e0a-9ab8-ed5d66e532d0" />

La guarde en un documento de texto

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 005046" src="https://github.com/user-attachments/assets/0e324f08-fc83-4f70-a20e-c1c2a63d0489" />

Trate de utilizarla con el otro usuario y obtuve acceso a la maquina

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 005119" src="https://github.com/user-attachments/assets/88c4b600-75b7-4129-9e85-aca250dc3aeb" />

# ESCALADA DE PRIVILEGIOS

Ya dentro de la maquina procedi a listar los binarios con los que contaba la maquina a vr si alguno me funcionaba para escalar privilegios y encontre uno que me llamo mucho la atencion

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 005235" src="https://github.com/user-attachments/assets/f7cf54b9-51a6-4b90-a31f-451eb053aa90" />

Ya anteriormente donde encontramos los binarios expuestos en la pagina nos decia que abrieramos la ayuda de ese binario asi que procedimos a hacerlos a ver que nos ayudaba

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 005554" src="https://github.com/user-attachments/assets/d4d41944-c5ff-4e82-8fb8-4ed2440c5b49" />

Descubrimos que podiamos utilizar el comando "TEXTO ghost-deploy -a RUTA" podiamos agregar contenido como root, asi que procedimos a utilizarlos

Ingrese un usuario y una contraseña que utilizaria para tratar de escalar privilegios

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 005631" src="https://github.com/user-attachments/assets/93198cec-fe28-48f6-acad-fd04d18dbf53" />

Agregue el usuario hacker con su respectiva contraseña y obtuve acceso a root, ya qu eeste usuario lo creamos con permisos de root

<img width="1920" height="260" alt="Captura de pantalla 2026-05-26 005958" src="https://github.com/user-attachments/assets/03b46814-535d-4c5b-bf23-aade69fa7601" />

Encontramos a flag

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 010027" src="https://github.com/user-attachments/assets/21103e26-93b0-4e66-8287-83d574d49791" />

Buscamos la flag de usuario ya que se nos habia pasado

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 010146" src="https://github.com/user-attachments/assets/9f157b20-83e3-4f41-9909-3e618dd34e4f" />

# POWNED

<img width="1920" height="1140" alt="Captura de pantalla 2026-05-26 010218" src="https://github.com/user-attachments/assets/f0ab1e21-6540-4e24-91ab-c030f3fcc74b" />


