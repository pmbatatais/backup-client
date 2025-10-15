# **💾 Guia de Instalação – Cliente de Backup com Backrest no FreeBSD**

Este guia descreve o processo completo de instalação e configuração do **cliente de backup Backrest**, que será utilizado para enviar backups ao **servidor Rest Server**.  
A versão adotada é a **1.9.2**, considerada **a mais estável e recente**.

---

## **🙏 Agradecimentos**

O **Backrest** é um cliente moderno de backup desenvolvido por [**Gareth George**](https://github.com/garethgeorge/backrest), compatível com **Restic** e **Rest Server**, oferecendo interface web e automação no envio e verificação de backups.

Eu, **Leonardo Ribeiro**, adaptei este manual para o ambiente **FreeBSD**, utilizado na **Prefeitura Municipal de Batatais**.  
Repositório institucional do cliente: <https://github.com/pmbatatais/backup-client.git>

---

## **⚙️ Ambiente utilizado**

- **Sistema operacional:** FreeBSD 14.3
- **Cliente de backup:** [Backrest 1.9.2](https://github.com/garethgeorge/backrest/releases/download/v1.9.2/backrest_Freebsd_x86_64.tar.gz)
- **Servidor de destino:** REST Server (compatível com Restic)
- **Armazenamento remoto:** Repositório REST acessível via HTTP ou HTTPS

---

## **🔗 Integração com o Servidor REST da Prefeitura**

Antes de configurar o **Backrest (cliente)**, é **obrigatório que o servidor REST Server já esteja implantado e funcional**.  
A Prefeitura de Batatais mantém um manual técnico completo para isso, disponível em:

👉 [Repositório oficial – Servidor de Backup (REST Server)](https://github.com/pmbatatais/backup-server)

### **📘 O que você encontrará no manual do servidor:**

- Instalação completa do **REST Server** em FreeBSD
- Configuração de armazenamento ZFS com compressão
- Criação de datasets e permissões corretas
- Script `install.sh` adaptado para FreeBSD
- Guia para criar **usuário SFTP somente leitura** (para auditoria de backups)

💡 **Importante:**  
Sem o servidor configurado conforme o manual acima, o cliente Backrest **não terá um destino válido** para armazenar os backups.  
Certifique-se de que o servidor REST:

- Está em execução e acessível pela rede;
- Tem a porta configurada (ex.: `8081`);
- Possui o repositório REST ativo e pronto para receber conexões.

---

## **🧠 Sobre o Backrest**

O **Backrest** é uma interface moderna para o **Restic**, permitindo configurar, agendar e monitorar backups de forma simples e automatizada.

Principais vantagens:

- 🌐 **Interface Web** integrada (`http://localhost:9898`)
- 🔒 **Criptografia ponta a ponta (Restic)**
- 📋 **Logs detalhados**
- ⚙️ **Serviço rc.d automático (FreeBSD)**
- 💡 **Integração direta com REST Server**

---

## **📦 Instalação passo a passo**

### **1️⃣ Instalar dependências**

```
sudo pkg install -y git curl
```

---

### **2️⃣ Clonar o repositório institucional do cliente**

```
git clone https://github.com/pmbatatais/backup-client.git && cd backup-client
```

> O diretório conterá o script `install.sh` e demais arquivos de configuração.

---

### **3️⃣ Fazer download do binário oficial do Backrest**

```
fetch https://github.com/garethgeorge/backrest/releases/download/v1.9.2/backrest_Freebsd_x86_64.tar.gz
tar -xzf backrest_Freebsd_x86_64.tar.gz
```

---

### **4️⃣ Conceder permissão de execução ao instalador**

```
chmod +x install.sh
```

---

### **5️⃣ Executar o instalador**

```
sudo sh install.sh
```

> 🔧 O script detectará automaticamente o sistema (FreeBSD) e criará o serviço **rc.d** do Backrest.

O processo irá:

- Copiar o binário para `/usr/local/bin/backrest`
- Criar o serviço `/usr/local/etc/rc.d/backrest`
- Ativar o serviço no boot (`sysrc backrest_enable=YES`)
- Iniciar o serviço automaticamente

---

## **▶️ Comandos úteis do serviço**

- **Iniciar o Backrest:**

  ```
  sudo service backrest start
  ```
- **Parar o Backrest:**

  ```
  sudo service backrest stop
  ```
- **Verificar o status:**

  ```
  sudo service backrest status
  ```
- **Ver logs em tempo real:**

  ```
  tail -f /var/log/backrest.log
  ```

---

## **🌐 Acesso à Interface Web**

Após a instalação, acesse:

👉 [http://localhost:9898](http://localhost:9898/)

Na interface, é possível:

- Adicionar e configurar repositórios REST remotos
- Agendar e iniciar backups
- Consultar histórico e logs
- Visualizar o status dos jobs

---

## **📂 Localização dos arquivos importantes**

| **Tipo** | **Caminho** | **Descrição** |
|--------------------|--------------------------------------|--------------------------|
| Binário principal | `/usr/local/bin/backrest` | Executável principal |
| Script de serviço | `/usr/local/etc/rc.d/backrest` | Script rc.d |
| Logs do processo | `/var/log/backrest.log` | Log do serviço |
| Configuração local | `~/.config/backrest/` | Configurações do usuário |
| Logs adicionais | `~/.local/share/backrest/processlogs/` | Logs detalhados |

---

## **⚡ Dica: Conectar ao Servidor REST**

1. Acesse a interface web do Backrest.
2. Clique em **“Add Repository”**.
3. Informe o endereço do servidor REST configurado, por exemplo:

   ```
   http://ip_do_servidor:8081
   ```
4. Digite a senha do repositório (mesma usada no servidor).
5. Clique em **Connect** para testar a conexão.

💡 **Sugestão:** Use senhas distintas para cada cliente, mantendo o controle de acesso.

---

## **🧰 Atualizações**

Para atualizar o Backrest para uma nova versão:

```
sudo service backrest stop
fetch https://github.com/garethgeorge/backrest/releases/download/v1.9.2/backrest_Freebsd_x86_64.tar.gz
tar -xzf backrest_Freebsd_x86_64.tar.gz
sudo cp backrest /usr/local/bin/backrest
sudo service backrest start
```

---

## **🔗 Referências**

- **Backrest:** <https://github.com/garethgeorge/backrest>
- **Rest Server:** <https://github.com/restic/rest-server>
- **Manual institucional (Servidor REST):** <https://github.com/pmbatatais/backup-server>
- **Ferramenta Restic:** [https://restic.net](https://restic.net/)
- **Documentação ZFS (FreeBSD):** <https://docs.freebsd.org/pt-br/books/handbook/zfs/>
- **Repositório do cliente:** <https://github.com/pmbatatais/backup-client.git>

---

## **📜 Autor**

**Leonardo Ribeiro**  
Prefeitura Municipal de Batatais  
Responsável técnico pela padronização dos sistemas de backup e infraestrutura de servidores.
