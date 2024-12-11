# Automação de TI com Ansible - Roteiro Prático

## Objetivo
Este roteiro ensina como usar o Ansible para configurar um servidor web Apache em um ambiente Linux de forma automatizada.

## Pré-requisitos

1. Ter um servidor Linux disponível (por exemplo, Ubuntu Server 20.04 ou 22.04).
2. Ter o Python instalado no servidor (necessário para o Ansible).
3. Acesso SSH configurado nos servidores de destino.

## Passo a Passo

### 1. Instalar o Ansible no Controlador
No sistema que será usado como controlador (máquina local ou servidor de gerenciamento):
```bash
sudo apt update
sudo apt install ansible -y
```
### 2. Configurar o inventário
Crie um arquivo chamado inventory para listar os servidores gerenciados:
```bash
mkdir ansible-automation
cd ansible-automation
nano inventory
```
Adicione o seguinte conteúdo ao arquivo:
```bash
[webservers]
192.168.1.10 ansible_user=ubuntu
```
Substitua 192.168.1.10 pelo IP do seu servidor e ubuntu pelo usuário usado para conexão SSH.
### 3. Criar um playbook
Crie um arquivo chamado apache-setup.yml:
```bash
nano apache-setup.yml
```
Adicione o seguinte conteúdo:
```bash
---
- name: Configurar Servidor Web Apache
  hosts: webservers
  become: true

  tasks:
    - name: Atualizar os pacotes do sistema
      apt:
        update_cache: yes

    - name: Instalar o Apache
      apt:
        name: apache2
        state: present

    - name: Copiar página inicial personalizada
      copy:
        src: index.html
        dest: /var/www/html/index.html

    - name: Reiniciar o Apache
      service:
        name: apache2
        state: restarted
```
### 4. Criar o arquivo da página HTML
Crie um arquivo chamado index.html:
```bash
nano index.html
```
Adicione o conteúdo abaixo:
```bash
<!DOCTYPE html>
<html>
<head>
    <title>Bem-vindo ao Apache Automático!</title>
</head>
<body>
    <h1>Automação com Ansible</h1>
    <p>Esta página foi configurada automaticamente!</p>
</body>
</html>
```
### 5. Executar o playbook
Execute o Playbook para configurar o servidor:
```bash
ansible-playbook -i inventory apache-setup.yml
```
### 6. Verificar o resultado
Acesse o endereço do servidor no navegador: http://192.168.1.10 (substitua pelo IP do seu servidor).
Você verá a página personalizada criada no passo 4.
### Integrantes do grupo
Gabriel Neri Ferreira Santos - 20203018973 <br>
Marcelo Morais Barbosa - 20203014713 <br>
Wagner Augusto Álvares de Freitas Filho - 20223002756 <br>
Caio Cézar Miranda Carvalho - 20223002200 <br>
