# Projeto ASA - Infraestrutura Automatizada (IaC)

Este projeto implementa um ambiente de infraestrutura como código (IaC) para hospedar uma aplicação **WordPress** completa, utilizando **Vagrant** para virtualização, **Ansible** para provisionamento e **Docker** para orquestração de containers.

O diferencial desta arquitetura é a inclusão de um **Load Balancer Nginx** personalizado operando na Camada 4 (TCP), servindo como proxy para a aplicação.

## 🚀 Tecnologias Utilizadas

* **Vagrant:** Gerenciamento da Máquina Virtual.
* **VirtualBox:** Provider de virtualização.
* [cite_start]**Ansible:** Automação da configuração do ambiente e instalação do Docker[cite: 5].
* **Docker & Docker Compose:** Containerização dos serviços.
* [cite_start]**Nginx:** Proxy TCP (Stream Context)[cite: 1].
* **WordPress & MySQL:** Aplicação e Banco de Dados.

## 🏗️ Arquitetura

[cite_start]O ambiente é provisionado automaticamente em uma VM **Debian Bookworm (64-bit)**[cite: 3]. Dentro desta VM, o Docker Compose orquestra três serviços principais:

1.  **webproxy (Nginx):**
    * [cite_start]Configurado como Load Balancer de Camada 4 (TCP/UDP) através do bloco `stream`[cite: 1].
    * [cite_start]Escuta na porta **8080** e encaminha tráfego para o servidor web[cite: 1].
    * [cite_start]Construído a partir de uma imagem personalizada (`Dockerfile`) baseada no `nginx:latest`[cite: 2].
2.  **webserver (WordPress):**
    * [cite_start]Imagem oficial do WordPress[cite: 5].
    * Acessível apenas via rede interna ou através do proxy.
3.  **database (MySQL):**
    * [cite_start]Versão 5.7 (estável para WP)[cite: 5].
    * Persistência de dados via volumes Docker.

## 📋 Pré-requisitos

Certifique-se de ter instalado em sua máquina host:

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)
* [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) (Obrigatório para o provisionamento do Vagrant)

## 🔧 Instalação e Execução

1.  **Clone este repositório:**
    ```bash
    git clone <URL_DO_SEU_REPOSITORIO>
    cd <NOME_DA_PASTA>
    ```

2.  **Suba o ambiente:**
    Execute o comando abaixo na raiz do projeto. [cite_start]O Vagrant irá criar a VM, e o Ansible irá instalar o Docker e subir os containers automaticamente[cite: 5].
    ```bash
    vagrant up
    ```

3.  **Acesse a Aplicação:**
    Uma vez finalizado o processo, a aplicação estará disponível no IP estático definido:

    👉 **URL:** `http://192.168.56.118:8080`

## 📂 Estrutura do Projeto

* [cite_start]`Vagrantfile`: Define a VM com IP `192.168.56.118` e 1GB de RAM[cite: 3, 4].
* [cite_start]`playbook_ansible.yml`: Playbook que instala o Docker Engine, cria diretórios e executa o `docker compose up`[cite: 5].
* [cite_start]`docker-compose.yml`: Define a stack (MySQL, WordPress, Nginx Proxy)[cite: 5].
* [cite_start]`nginx.conf`: Configuração do Nginx para encaminhamento de tráfego TCP na porta 8080[cite: 1].
* [cite_start]`Dockerfile`: Script de build para a imagem do proxy[cite: 2].

## 🔐 Credenciais (Ambiente de Desenvolvimento)

Conforme definido no `docker-compose.yml`:

* **Banco de Dados:** `wordpress`
* **Usuário do Banco:** `wordpress_user`
* **Senha do Banco:** `wordpress_password`
* **Senha Root (DB):** `senha_root_secreta`

## 🛠️ Comandos Úteis

* **Acessar a VM via SSH:**
    ```bash
    vagrant ssh
    ```
* **Parar a VM:**
    ```bash
    vagrant halt
    ```
* **Destruir o ambiente (remover VM):**
    ```bash
    vagrant destroy
    ```

---
**Disciplina:** ASA
