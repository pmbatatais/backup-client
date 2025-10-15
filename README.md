# **💾 Guia de Instalação – Cliente de Backup com Backrest no FreeBSD**

Este guia descreve o processo completo de instalação e configuração do **cliente de backup Backrest**, que será utilizado para enviar backups ao **servidor Rest Server**.  
A versão adotada é a **1.9.2**, considerada **a mais estável e recente**.

---

## **🙏 Agradecimentos**

O **Backrest** é um cliente moderno de backup desenvolvido por [**Gareth George**](https://github.com/garethgeorge/backrest), compatível com **Restic** e **Rest Server**, oferecendo interface web e automação no envio e verificação de backups.

Eu, **Leonardo Ribeiro**, adaptei este manual para o ambiente **FreeBSD**, utilizado na Prefeitura Municipal de Batatais.

Repositório oficial: <https://github.com/garethgeorge/backrest>  
Repositório institucional (PMB): <https://github.com/pmbatatais/backup-client.git>

---

## **⚙️ Ambiente utilizado**

- **Sistema operacional:** FreeBSD 14.3
- **Cliente de backup:** [Backrest 1.9.2](https://github.com/garethgeorge/backrest/releases/download/v1.9.2/backrest_Freebsd_x86_64.tar.gz)
- **Servidor de destino:** REST Server (compatível com Restic)
- **Armazenamento remoto:** Repositório acessível via HTTP ou HTTPS

---

## **🧠 Sobre o Backrest**

O **Backrest** é uma interface simples e eficiente para o **Restic**, permitindo que técnicos configurem, agendem e monitorem backups de forma amigável e automatizada.

Principais vantagens:

- 🌐 **Interface Web** integrada (`http://localhost:9898`)
- 🔒 **Criptografia ponta a ponta (Restic)**
- 📋 **Logs detalhados** de cada backup
- ⚙️ **Serviço automático (rc.d no FreeBSD)**
- 💡 **Compatível com servidores REST locais ou remotos**

---

## **📦 Instalação passo a passo**

### **1️⃣ Instalar dependências**

Antes de tudo, garanta que o Git e o curl estejam disponíveis:

```
sudo pkg install -y git curl
```

---

### **2️⃣ Clonar o repositório institucional**

```
git clone https://github.com/pmbatatais/backup-client.git && cd backup-client
```

> O diretório conterá o script `install.sh` e demais arquivos de configuração.

---

### **3️⃣ Fazer download do binário oficial do Backrest**

Baixe e extraia o binário da versão **1.9.2** (compilado para FreeBSD):

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

> 🔧 O script detectará automaticamente o sistema operacional (FreeBSD) e criará o serviço **rc.d** para o Backrest.

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

A interface permite:

- Configurar repositórios REST remotos
- Criar e agendar backups
- Monitorar progresso e histórico
- Ver logs e status dos jobs

---

## **📂 Localização dos arquivos importantes**

| **Tipo** | **Caminho** | **Descrição** |
|---------------------------|--------------------------------------|-------------------------------------|
| Binário principal | `/usr/local/bin/backrest` | Executável principal |
| Script de serviço | `/usr/local/etc/rc.d/backrest` | Script de inicialização do serviço |
| Logs do processo | `/var/log/backrest.log` | Saída principal do serviço |
| Diretório de configuração | `~/.config/backrest/` | Arquivos de configuração do cliente |
| Logs adicionais | `~/.local/share/backrest/processlogs/` | Logs detalhados por operação |

---

## **⚡ Dica: Conexão com Servidor REST**

Para conectar o Backrest a um servidor REST Server existente:

1. Abra a interface web do Backrest.
2. Clique em **“Add Repository”**.
3. Informe a URL do servidor, exemplo:

   ```
   http://ip_do_servidor:8081
   ```
4. Informe a senha do repositório (mesma utilizada no servidor).
5. Clique em **Connect** para validar a conexão.

💡 **Dica:** É recomendável criar uma senha específica para cada cliente.

---

## **🧰 Atualizações futuras**

Para atualizar o Backrest:

```
sudo service backrest stop
fetch https://github.com/garethgeorge/backrest/releases/download/v1.9.2/backrest_Freebsd_x86_64.tar.gz
tar -xzf backrest_Freebsd_x86_64.tar.gz
sudo cp backrest /usr/local/bin/backrest
sudo service backrest start
```

---

## **🔗 Referências**

- Projeto **Backrest:** <https://github.com/garethgeorge/backrest>
- Projeto **Rest Server:** <https://github.com/restic/rest-server>
- Ferramenta **Restic:** [https://restic.net](https://restic.net/)
- Guia do **ZFS no FreeBSD:** <https://docs.freebsd.org/pt-br/books/handbook/zfs/>
- Repositório institucional (PMB): <https://github.com/pmbatatais/backup-client.git>

---

## **📜 Autor**

**Leonardo Ribeiro**  
Prefeitura Municipal de Batatais  
Responsável técnico pela padronização dos sistemas de backup e infraestrutura de servidores.
