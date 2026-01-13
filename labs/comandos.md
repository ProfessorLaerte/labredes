# Comandos OSPF – Laboratórios Práticos

Universidade de Brasília (UnB)  
Departamento de Engenharia Elétrica  
Curso: Engenharia de Redes de Comunicação  
Disciplina: Protocolos de Transporte e Roteamento  

Professor: Prof. Dr. Laerte Peotta de Melo  

---

## 1. Acesso e Modos de Configuração

### Modo privilegiado
```
enable
```

### Modo de configuração global
```
configure terminal
```

### Definição do nome do roteador
```
hostname R1
```

---

## 2. Configuração de Interfaces

### Interface FastEthernet
```
interface fastEthernet 0/0
 ip address 172.16.0.1 255.255.255.192
 no shutdown
```

### Interface Serial
```
interface serial 0/0/0
 ip address 200.10.1.2 255.255.255.252
 clock rate 64000
 no shutdown
```

---

## 3. Interface Loopback (Router-ID)

```
interface loopback 0
 ip address 1.1.1.1 255.255.255.255
```

---

## 4. Configuração do OSPF

```
router ospf 1
 router-id 1.1.1.1
```

---

## 5. Anúncio de Redes

```
router ospf 1
 network 172.16.0.0 0.0.0.63 area 1
 network 200.10.1.0 0.0.0.3 area 0
```

---

## 6. Verificação

```
show ip ospf neighbor
show ip ospf interface
show ip ospf database
show ip route ospf
```

---

## 7. DR e BDR

```
show ip ospf interface fastEthernet 0/0
```

### Ajuste de prioridade
```
interface fastEthernet 0/0
 ip ospf priority 100
```

---

## 8. Enlace Virtual

```
router ospf 1
 area 100 virtual-link 3.3.3.3
```

---

## 9. Diagnóstico

```
debug ip ospf adj
clear ip ospf process
```

---

## 10. Salvamento

```
write memory
```
