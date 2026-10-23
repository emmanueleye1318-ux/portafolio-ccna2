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
