# Desafio de Penetration Testing: Brute Force e Enumeração (DIO)

Este repositório contém a documentação e artefatos da entrega do desafio de ataque de força bruta e enumeração de serviços utilizando o ecossistema Kali Linux.

## 🛠 Tecnologias e Ferramentas
* **Reconhecimento:** Nmap
* **Ataque (Brute Force):** Medusa
* **Enumeração SMB:** Enum4linux
* **Alvo:** DVWA (Damn Vulnerable Web Application) & Serviços de Rede (FTP/SMB)

---

## 📑 Metodologia de Execução

### 1. Reconhecimento de Rede (Footprinting)
O primeiro passo consistiu na identificação de portas abertas e serviços ativos no target. O `nmap` foi executado com flags de detecção de serviço e scripts default para mapear a superfície de ataque.

#### Scan de Portas - Nmap

<img width="1222" height="317" alt="nmap_01" src="https://github.com/user-attachments/assets/8a08ed88-e686-4195-80f2-0a94c48f52d9" />

### 2. Ataque de Força Bruta - Serviço FTP
Após identificar o serviço FTP ativo, foram geradas wordlists customizadas (`users.txt` e `pass.txt`). Utilizamos o **Medusa** para realizar o ataque paralelo.

#### Execução do ataque:

<img width="1283" height="250" alt="medusa_ftp_01" src="https://github.com/user-attachments/assets/74430a32-36bd-40c2-9750-95e6a4ce1e67" />

#### Validação do Acesso:

Após a captura das credenciais, validamos a persistência e o acesso ao sistema de arquivos do FTP.

<img width="585" height="264" alt="medusa_ftp_02" src="https://github.com/user-attachments/assets/1d636a6e-01ad-4049-9452-7f6dc01a9a95" />

### 3. Ataque em Formulário Web (HTTP-POST)
O alvo foi a página de login da aplicação **DVWA**. 

#### Interceptação/Análise:
Utilizando o *DevTools* do navegador, isolamos os parâmetros de requisição (POST data) e as strings de erro necessárias para configurar o módulo `http-form` do Medusa.

<img width="1366" height="620" alt="medusa_form_01" src="https://github.com/user-attachments/assets/b8532a9a-ce01-48ca-9811-35cdc3556c21" />

#### Exploração:
Execução do Medusa apontando para o endpoint `/login.php`.

<img width="1366" height="488" alt="medusa_form_02" src="https://github.com/user-attachments/assets/c806f35c-1c63-46fe-bb13-523a87dc7c65" />

#### Confirmação de Login:

<img width="1366" height="679" alt="medusa_form_03" src="https://github.com/user-attachments/assets/8f2b9944-afbc-4b25-a179-1f1db908f6fb" />

### 4. Enumeração e Exploração SMB
Migramos a análise para o protocolo SMB (porta 445).

#### Enumeração de Usuários:
O `enum4linux` foi fundamental para extrair SIDs e listar usuários válidos do sistema, refinando nossa wordlist para `smb_users.txt`.

<img width="958" height="529" alt="enum4linux_01" src="https://github.com/user-attachments/assets/ebbacd1b-9094-4259-a969-e3bb4040fa1b" />

#### Password Spraying / Brute Force:
Com a lista refinada, executamos o ataque via Medusa para comprometer o compartilhamento de rede.

<img width="1304" height="294" alt="smbclient_01" src="https://github.com/user-attachments/assets/74e07309-67a8-4097-8cab-a712016a9467" />

#### Acesso ao Compartilhamento:
Validação final acessando os diretórios via `smbclient`.

<img width="766" height="335" alt="smbclient_02" src="https://github.com/user-attachments/assets/daa9218e-d772-4aa2-9d21-126c6ce45a24" />

---

## 📁 Estrutura do Repositório
* `/images`: Capturas de tela dos procedimentos (evidências).
* `users.txt` / `pass.txt`: Wordlists utilizadas.
* `smb_users.txt` / `senhas_spray.txt`: Listas específicas para o vetor SMB.

---
