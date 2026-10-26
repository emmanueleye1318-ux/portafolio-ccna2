#portafolio-ccna2
#Topologia de red: Router-on-a-stick con VLANs

##1.Diagrama de la topologia

```bash
 [ R1-Core (Router) ]
                        |

                        | (Interfaz G0/0/0 - Trunk Vlan 111, Router-on-a-Stick)

                        |

                 [ SW-Core ] ------------- Local:

                  /       \                └── VLAN 99: 1 PC Gestión

                 / (Trunk) \ (Trunk) Vlan 111 nativa

                /           \

          [ SW-Lab1 ]    [ SW-Lab2 ]


      Dispositivos en SW-Lab1:       Dispositivos en SW-Lab2:

      ├── VLAN 10: 3 PCs Admin       ├── VLAN 10: 3 PCs Admin

      ├── VLAN 20: 3 PCs Alumnos     ├── VLAN 20: 3 PCs Alumnos

      └── VLAN 30: 1 PC Dirección    └── VLAN 30: 1 PC Dirección

```


# Topología de red

## 1. Estructura general

La red estará compuesta por un router ubicado en la parte superior y tres switches colocados debajo, formando una estructura triangular.

- Router principal: R1-Core
- 
- Switch central: SW-Core
- 
- Switch izquierdo: SW-Lab1
- 
- Switch derecho: SW-Lab2
- 

## 2. Topología física


                  [ R1-Core ]
                       |
                  [ SW-Core ]
                    /       \
                   /         \
           [ SW-Lab1 ]     [ SW-Lab2 ]

## 3. Distribución de equipos en SW-Lab1

### VLAN 10 — ADMIN

Las primeras 3 computadoras estarán conectadas al switch izquierdo.

- PC-Admin1 — VLAN 10
- PC-Admin2 — VLAN 10
- PC-Admin3 — VLAN 10

### VLAN 20 — ALUMNOS

Las siguientes 3 computadoras estarán conectadas al switch izquierdo.

- PC-Alumno1 — VLAN 20
- PC-Alumno2 — VLAN 20
- PC-Alumno3 — VLAN 20

### VLAN 30 — DIRECCION

La séptima computadora estará conectada al switch izquierdo.

- PC-Direccion1 — VLAN 30

## 4. Resumen de SW-Lab1

| Nombre de equipo | VLAN | Área |

|---|---:|---|

| PC-Admin1 | 10 | ADMIN |

| PC-Admin2 | 10 | ADMIN |

| PC-Admin3 | 10 | ADMIN |

| PC-Alumno1 | 20 | ALUMNOS |

| PC-Alumno2 | 20 | ALUMNOS |

| PC-Alumno3 | 20 | ALUMNOS |

| PC-Direccion1 | 30 | DIRECCION |


**Total de computadoras en SW-Lab1: 7.**


## Configuración de un Switch

En cada switch ingresaremos los siguientes comandos para crear una VLAN, asignarle un nombre y configurar un puerto de acceso.

### 1. Acceder al modo privilegiado

Al entrar al switch, aparecerá el modo por defecto `Switch>`. Escribimos:

```bash
enable
```

### 2. Entrar al modo de configuración global

Ingresamos el siguiente comando:

```bash
configure terminal
```

**Nota:** Si la plataforma se traba o no reconoce el comando, presionamos `Enter` dos veces e intentamos nuevamente. También podemos utilizar el comando abreviado `config`.

### 3. Crear la VLAN y asignarle un nombre

Creamos la VLAN correspondiente y le asignamos un nombre según el área de trabajo.

Ejemplo para la VLAN 10 (ADMIN):

```bash
vlan 10
name ADMIN
end
```

### 4. Configurar la interfaz del switch

Volvemos a entrar al modo de configuración global:

```bash
configure terminal
```

Seleccionamos la interfaz que vamos a configurar. Por ejemplo, el puerto `FastEthernet 0/1`:

```bash
interface fa0/1
switchport mode access
switchport access vlan 10
end
```

### 5. Resultado

De esta forma, el puerto `fa0/1` queda configurado en modo de acceso y asignado a la VLAN 10 (ADMIN).

**Importante:** Repetimos el procedimiento en cada switch, cambiando el número de VLAN, el nombre y la interfaz según corresponda a cada computadora y área de trabajo.


## Configuración de los puertos para las VLAN 10, 20 y 30

Una vez configurados los primeros puertos del switch, continuaremos asignando las interfaces correspondientes a cada computadora, respetando la VLAN a la que pertenece cada área.

### 1. Configuración de la VLAN 10 (ADMIN)

Las computadoras del área de ADMIN estarán conectadas a las interfaces `fa0/1`, `fa0/2` y `fa0/3`.

Configuramos cada interfaz de la siguiente manera:

```bash
configure terminal
interface fa0/1
switchport mode access
switchport access vlan 10
exit

interface fa0/2
switchport mode access
switchport access vlan 10
exit

interface fa0/3
switchport mode access
switchport access vlan 10
end
```

### 2. Configuración de la VLAN 20 (ALUMNOS)

Después de terminar con las tres computadoras de ADMIN, continuaremos con las computadoras del área de ALUMNOS, utilizando las interfaces `fa0/4`, `fa0/5` y `fa0/6`.

Primero, creamos la VLAN y le asignamos su nombre si todavía no existe:

```bash
configure terminal
vlan 20
name ALUMNOS
end
```

Posteriormente, asignamos los puertos correspondientes:

```bash
configure terminal
interface fa0/4
switchport mode access
switchport access vlan 20
exit

interface fa0/5
switchport mode access
switchport access vlan 20
exit

interface fa0/6
switchport mode access
switchport access vlan 20
end
```

### 3. Configuración de la VLAN 30 (DIRECCION)

Finalmente, configuraremos la computadora del área de DIRECCION, que estará conectada a la interfaz `fa0/7` y pertenecerá a la VLAN 30.

Si la VLAN todavía no existe, la creamos y le asignamos su nombre:

```bash
configure terminal
vlan 30
name DIRECCION
end
```

Ahora configuramos la interfaz:

```bash
configure terminal
interface fa0/7
switchport mode access
switchport access vlan 30
end
```

### 4. Resumen de la asignación de puertos

| Área | VLAN | Interfaces |
|---|---:|---|
| ADMIN | 10 | fa0/1, fa0/2, fa0/3 |
| ALUMNOS | 20 | fa0/4, fa0/5, fa0/6 |
| DIRECCION | 30 | fa0/7 |

**Total de computadoras configuradas en SW-Lab1: 7.**

**Nota:** Si ya creaste las VLAN anteriormente, no es necesario volver a crearlas. Solo configura las interfaces correspondientes. Puedes comprobar las VLAN con `show vlan brief` desde el modo privilegiado.




## Configuración del switch del lado derecho (SW-Lab2)

Una vez finalizada la configuración del switch del lado izquierdo, realizaremos el mismo procedimiento en el switch del lado derecho. Utilizaremos la misma distribución de VLAN y puertos, pero trabajando directamente en el switch derecho, no en el switch del medio.

### 1. Crear las VLAN

Primero, ingresamos al modo privilegiado y al modo de configuración global:

```bash
enable
configure terminal
```

Creamos las VLAN correspondientes y asignamos sus nombres:

```bash
vlan 10
name ADMIN
exit

vlan 20
name ALUMNOS
exit

vlan 30
name DIRECCION
end
```

### 2. Configurar las interfaces de la VLAN 10 (ADMIN)

Asignamos las primeras tres computadoras a la VLAN 10:

```bash
configure terminal
interface fa0/1
switchport mode access
switchport access vlan 10
exit

interface fa0/2
switchport mode access
switchport access vlan 10
exit

interface fa0/3
switchport mode access
switchport access vlan 10
end
```

### 3. Configurar las interfaces de la VLAN 20 (ALUMNOS)

Asignamos las siguientes tres computadoras a la VLAN 20:

```bash
configure terminal
interface fa0/4
switchport mode access
switchport access vlan 20
exit

interface fa0/5
switchport mode access
switchport access vlan 20
exit

interface fa0/6
switchport mode access
switchport access vlan 20
end
```

### 4. Configurar la interfaz de la VLAN 30 (DIRECCION)

Asignamos la séptima computadora a la VLAN 30:

```bash
configure terminal
interface fa0/7
switchport mode access
switchport access vlan 30
end
```

### 5. Resumen de la asignación de puertos

| Área | VLAN | Interfaces |
|---|---:|---|
| ADMIN | 10 | fa0/1, fa0/2, fa0/3 |
| ALUMNOS | 20 | fa0/4, fa0/5, fa0/6 |
| DIRECCION | 30 | fa0/7 |

**Total de computadoras configuradas en SW-Lab2: 7.**

### 6. Verificar la configuración

Para comprobar que las VLAN y los puertos estén configurados correctamente, ejecutamos:

```bash
show vlan brief
```

**Importante:** Todos estos comandos deben ejecutarse en el switch del lado derecho. No debemos confundirlo con el switch del medio, que tendrá su propia configuración.



## Configuración y verificación del router

Una vez configuradas las VLAN y los enlaces trunk en los switches, continuamos con la configuración del router `R-CORE-AELS`, el cual permitirá la comunicación entre las diferentes VLAN.

### 1. Verificar las interfaces del router

Para comprobar las interfaces configuradas, ingresamos al modo privilegiado y ejecutamos el siguiente comando:

```bash
enable
show ip interface brief
```

Este comando muestra las interfaces del router, sus direcciones IP y su estado actual.

### 2. Identificar las subinterfaces

En nuestro router encontramos las siguientes subinterfaces:

| Interfaz | Dirección IP | VLAN |
|---|---|---:|
| GigabitEthernet0/0.10 | 192.168.10.254 | 10 |
| GigabitEthernet0/0.20 | 192.168.20.254 | 20 |
| GigabitEthernet0/0.30 | 192.168.30.254 | 30 |
| GigabitEthernet0/0.99 | 192.168.99.254 | 99 |
| GigabitEthernet0/0.111 | Sin IP asignada | 111 |

La interfaz física `GigabitEthernet0/0` se encuentra activa y funciona como enlace principal para las subinterfaces.

### 3. Comprobar el estado de las interfaces

En la salida del comando `show ip interface brief`, las interfaces configuradas aparecen con el estado `up/up`.

Esto significa que tanto la interfaz como su protocolo están activos. Sin embargo, todavía es necesario verificar la encapsulación 802.1Q y la configuración de los enlaces trunk.

### 4. Verificar la configuración completa

Para consultar los detalles de las interfaces y sus subinterfaces, ejecutamos:

```bash
show running-config
```

Debemos comprobar que cada subinterfaz tenga la encapsulación correspondiente:

- VLAN 10: `encapsulation dot1Q 10`
- VLAN 20: `encapsulation dot1Q 20`
- VLAN 30: `encapsulation dot1Q 30`
- VLAN 99: `encapsulation dot1Q 99 native`, si se utiliza como VLAN nativa.


### 5. Resultado

El router cuenta con subinterfaces para las VLAN 10, 20, 30 y 99. Estas subinterfaces permiten realizar el enrutamiento entre las redes cuando la encapsulación, los enlaces trunk y las direcciones IP de las computadoras están configurados correctamente.


## Pruebas de comunicación entre computadoras

Una vez finalizada la configuración de los switches, las VLAN y el router, procederemos a realizar pruebas de comunicación entre las computadoras de nuestra topología en Cisco Packet Tracer. El objetivo principal es comprobar que los dispositivos puedan enviar y recibir información correctamente, tanto cuando pertenecen a la misma VLAN como cuando se encuentran en VLAN diferentes.

Para realizar estas pruebas utilizaremos el comando `ping`, que permite enviar paquetes de prueba a otro dispositivo mediante su dirección IP. De esta manera, podemos verificar si existe comunicación entre las computadoras y comprobar que la configuración de nuestra red funciona de acuerdo con lo esperado.

### 1. Comprender cómo se comunican las computadoras

En nuestra topología contamos con diferentes áreas de trabajo, las cuales están organizadas mediante VLAN para mantener el tráfico de la red separado y ordenado.

- **VLAN 10 (ADMIN):** corresponde a las computadoras del área administrativa.
- **VLAN 20 (ALUMNOS):** corresponde a las computadoras del área de alumnos.
- **VLAN 30 (DIRECCION):** corresponde a las computadoras del área de dirección.
- **VLAN 99:** se utiliza como VLAN nativa de los enlaces trunk, de acuerdo con la configuración realizada.

Cada VLAN funciona como una red lógica independiente. Esto significa que las computadoras de distintas VLAN no se comunican directamente mediante un switch de capa 2. Para permitir la comunicación entre ellas, utilizamos el router, que se encarga de dirigir los paquetes hacia la red correspondiente mediante sus subinterfaces.

### 2. Ingresar a una computadora en Packet Tracer

Para comenzar las pruebas, debemos seleccionar una computadora de nuestra topología. Podemos utilizar cualquier equipo que tenga configurada correctamente su dirección IP, máscara de subred y puerta de enlace.

Una vez seleccionada la computadora, seguimos estos pasos:

1. Hacemos clic en la computadora dentro del área de trabajo de Cisco Packet Tracer.
2. Entramos en la pestaña **Desktop**.
3. Seleccionamos la opción **Command Prompt**.
4. Esperamos a que aparezca la consola de comandos.
5. Escribimos el comando `ping` seguido de la dirección IP del dispositivo al que queremos enviar paquetes.

Desde esta consola podemos realizar distintas pruebas para verificar la comunicación entre los equipos de nuestra red.

### 3. Verificar la comunicación dentro de la misma VLAN

Primero comprobaremos la comunicación entre dos computadoras que pertenezcan a la misma VLAN. Por ejemplo, podemos seleccionar una computadora del área ADMIN conectada al switch izquierdo y enviar paquetes a otra computadora de ADMIN conectada al switch derecho.

Si la computadora de destino tiene configurada la dirección IP `192.168.10.11`, podemos ejecutar el siguiente comando desde la computadora de origen:

```bash
ping 192.168.10.11
```

La dirección utilizada es un ejemplo y debe sustituirse por la dirección IP real del equipo de destino.

Si recibimos respuestas, significa que existe comunicación entre las computadoras. Esta prueba permite comprobar que los puertos de acceso están asignados a la VLAN correcta y que los enlaces trunk transportan el tráfico de esa VLAN entre los switches.

### 4. Comprobar la comunicación entre VLAN diferentes

Después de verificar la comunicación dentro de una misma VLAN, realizaremos una prueba entre computadoras que pertenezcan a VLAN diferentes.

Por ejemplo, desde una computadora de ADMIN intentaremos comunicarnos con una computadora de ALUMNOS. Si el dispositivo de destino tiene la dirección IP `192.168.20.10`, ejecutamos:

```bash
ping 192.168.20.10
```

En este caso, los paquetes deben llegar al router mediante la puerta de enlace de la VLAN 10. El router analiza la dirección IP de destino y dirige los paquetes hacia la red de la VLAN 20.

Si la configuración es correcta, la computadora de ALUMNOS podrá recibir los paquetes y responder. De esta manera, comprobamos que las subinterfaces del router permiten el enrutamiento entre ambas VLAN.

### 5. Realizar una prueba con el área de DIRECCION

También podemos verificar la comunicación entre una computadora de ADMIN y otra perteneciente al área de DIRECCION.

Si la computadora de DIRECCION tiene configurada la dirección IP `192.168.30.10`, escribimos el siguiente comando desde la computadora de origen:

```bash
ping 192.168.30.10
```

Para que esta prueba funcione, la computadora de origen debe tener configurada la puerta de enlace de su propia VLAN, mientras que la computadora de destino debe utilizar la puerta de enlace correspondiente a la VLAN 30.

El router se encargará de dirigir los paquetes entre ambas redes. Si obtenemos respuestas, podremos confirmar que la comunicación entre las VLAN 10 y 30 funciona correctamente.

### 6. Interpretar los resultados obtenidos

Al ejecutar el comando `ping`, Packet Tracer mostrará los resultados de los paquetes enviados. Es importante interpretar estos resultados para determinar si la comunicación se está realizando correctamente.

- **Reply from:** indica que se recibió una respuesta del dispositivo de destino.
- **Request timed out:** significa que no se recibió una respuesta dentro del tiempo esperado.
- **Destination host unreachable:** indica que el dispositivo no pudo alcanzar el destino o que existe un problema de conectividad.

Si el primer paquete falla, podemos repetir la prueba. En ocasiones, el primer intento puede coincidir con el proceso de resolución de direcciones mediante ARP.

Si continúan los errores, debemos revisar la dirección IP, la máscara de subred, la puerta de enlace, la asignación de las VLAN, los enlaces trunk y la configuración de las subinterfaces del router.

### 7. Comprobar la configuración de las computadoras

Si alguna computadora no puede comunicarse con otra, debemos revisar su configuración de red antes de modificar los switches o el router.

Para hacerlo, seleccionamos la computadora y entramos en **Desktop → IP Configuration**. Allí verificamos que tenga una dirección IP válida para su VLAN, una máscara de subred correcta y la puerta de enlace correspondiente.

En nuestra configuración de ejemplo, las puertas de enlace son las siguientes:

| VLAN | Área | Puerta de enlace |
|---:|---|---|
| 10 | ADMIN | 192.168.10.254 |
| 20 | ALUMNOS | 192.168.20.254 |
| 30 | DIRECCION | 192.168.30.254 |

Estas direcciones corresponden a las subinterfaces que observamos configuradas en el router. Las direcciones IP de las computadoras deben pertenecer a la red correspondiente y no deben duplicarse entre dispositivos.

### 8. Verificar la comunicación general de la topología

Una vez revisadas las configuraciones, realizaremos varias pruebas entre computadoras conectadas a diferentes switches y pertenecientes a distintas VLAN.

Primero probaremos la comunicación entre equipos de ADMIN. Después, realizaremos pruebas entre ADMIN y ALUMNOS, y finalmente entre ADMIN y DIRECCION.

Estas pruebas nos permiten comprobar diferentes aspectos de la red: la comunicación dentro de una VLAN, el transporte del tráfico a través de los enlaces trunk y el enrutamiento entre redes mediante el router.

Si las pruebas se completan correctamente, tendremos evidencia de que los dispositivos pueden intercambiar paquetes de acuerdo con la configuración establecida.

### 9. Resultado final de las pruebas

Al terminar las pruebas de conectividad, podremos evaluar si nuestra topología funciona correctamente. Las computadoras estarán organizadas por áreas mediante las VLAN 10, 20 y 30, mientras que los switches permitirán distribuir las conexiones y transportar el tráfico entre ellos mediante enlaces trunk.

El router `R-CORE-AELS`, mediante sus subinterfaces, permitirá la comunicación entre las diferentes redes. La VLAN 99 se mantendrá como VLAN nativa de los enlaces trunk, de acuerdo con la configuración establecida en la práctica.

### 10. Conclusión

En conclusión, esta práctica nos permite comprender la importancia de las VLAN para organizar y separar el tráfico de una red, así como el funcionamiento de los enlaces trunk para transportar el tráfico de varias VLAN entre switches.

También aprendimos que el router puede utilizar subinterfaces para permitir la comunicación entre diferentes redes mediante el enrutamiento entre VLAN. Finalmente, el comando `ping` nos permite comprobar la conectividad entre computadoras y detectar posibles errores en la configuración.

De esta manera, verificamos el funcionamiento de nuestra topología en Cisco Packet Tracer y comprendemos cómo interactúan los switches, las VLAN y el router para permitir el intercambio de información entre los dispositivos de la red.
