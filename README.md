# PB Compass DevSecOps - Linux - Atividade individual
- Este repositório contém a atividade individual da sprint de Linux do programa de bolsas DevSecOps da Compass.uol
- Criado por: Amadeu Chacar
- Objetivo: Documentar todo o processo de instalação do Oracle Linux versão 8.6
- Link para download da ISO Oracle Linux 8.6: https://yum.oracle.com/oracle-linux-isos.html

# Especificação das VMs
Foi utilizado o Virtual Box para criação da máquina virtual.

**Memória RAM**: 2 GB

**HD**: 30 GB

*Obs: No Virtual Box, é necessário, nas configurações de rede da máquina, alterar o adaptador de rede para modo Bridge, já que por padrão ele assume o NAT.*

# Instalação do Oracle Linux

Após a inicialização da máquina com a ISO correta, o processo de instalação é bastante intuitivo. Podem ser seguidos os seguintes passos:

## Localization

- **Keyboard**: Selecionar a versão do seu teclado
- **Language support**: English
- **Time & Date**: Selecionar o seu fuso-horário

## Software

- **Software selection**: Minimall install

*Obs: A seleção da versão Minimal fornece a instalação do Linux sem interface gráfica*

## System

- **Installation destination**: Na opção "storage configuration", selecionar o campo "custom" para realizar a configuração manual das partições de disco. Ao selecionar "done", abrirá uma página para a criação das partições, que devem ser configuradas da seguinte forma:

**/boot**
- Tamanho (size): 4.0 GB
- Device type: Standard partition
- File system: ext4

**/home**
- Tamanho (size): 10.0 GB
- Device type: Standard partition
- File system: ext4

**swap**
- Tamanho (size): 4.0 GB
- Device type: Standard partition
- File system: swap

**/**
- Tamanho (size): Sem definir o tamanho para pegar todo o restante
- Device type: LVM Thin Provisioning
- File system: ext4

## Network & Hostname

Deve ser habilitada a rede do computador.

- **Ethernet**: ON

## User settings

- **Root password**: adicionar uma senha para o usuário root.
- **Create user**: criar um usuário comum (opcional, pode ser feito posteriormente)

## Begin installation

Após as modificações, basta clicar no botão "begin installation" para iniciar a instalação do Oracle Linux 8.6.

# Após a instalação

Ao finalizar a instalação, já no terminal, algumas configurações devem ser feitas para o melhor uso do sistema operacional.

## Atualização das bibliotecas

- Digite o comando **sudo yum update** para que ocorra a atualização dos componentes.

## Configurar a rede

Deve ser configurada a rede e fornecer um IP fixo para a máquina virtual.

Para realizar este processo, deve-se acessar o diretório **/etc/sysconfig/network-scripts** e alterar o arquivo do adaptador de rede **ifcfg-enp0s3** (o nome do arquivo pode variar de acordo com o nome do seu adaptador de rede)

No arquivo **ifcfg-enp0s3**, devem ser alterados os seguintes campos:
- Mudar o BOOTPROTO=dhcp para BOOTPROTO=none
- Adicionar os campos abaixo ao final do arquivo:

IPADDR0=192.168.1.200

PREFIX=24 

GATEWAY0=192.168.1.254

NETMASK=255.255.255.0

DNS1=192.168.1.1

DNS2=8.8.8.8

NETWORK=192.168.1.0

*Obs: todos estes valores acima são exemplos, devem ser adicionados valores correspondentes à sua rede local*

Após salvar o arquivo, deve ser reiniciado o serviço de Network Manager e a interface de rede com o comando **systemctl restart NetworkManager** ou reiniciar a VM utilizando o comando **reboot**.

