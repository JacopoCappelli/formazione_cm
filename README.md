# 📦 Struttura del Progetto Ansible per Gestione Container

## 🔍 Panoramica

Questo progetto utilizza **Ansible** e **Vagrant** per automatizzare la creazione, gestione e distribuzione di container Docker/Podman.  
Il sistema supporta entrambi i motori container, selezionabili tramite una variabile configurabile (`container_engine`).  
Il file `main.yaml` di ciascun ruolo funge da punto di accesso per le configurazioni relative a **Docker** o **Podman**.

---

## 🧩 Ruoli e Funzionalità

### 🛠️ Variabile di Configurazione Principale

- `container_engine`: determina quale motore container utilizzare (`"docker"` o `"podman"`)

### 🐧 `alpine-role` e `fedora-role`

- Creano container basati su **Alpine Linux** e **Fedora**
- Esposti sulla **porta 22** per accesso via SSH
- Utente **`vagrant`** con accesso alle chiavi SSH private e pubbliche
- Utilizzano **Dockerfile personalizzati** per ciascuna distribuzione

### ⚙️ `jenkins-role`

- Crea un'immagine Jenkins basata su **Alpine Linux**
- Include accesso a **Docker** e **Podman**
- Contiene un **JenkinsFile** per automatizzare il build delle immagini
- Supporta esecuzioni di **build containerizzate**

### 📤 `build-push-role`

- Gestisce un **registry container locale**
- Effettua **build** e **push** delle immagini nel registry locale
- Gestisce l'autenticazione al registry
- Le password sono cifrate e gestite con **Ansible Vault**

---

## 🧰 Utilizzo

### ✅ Prerequisiti

- `Vagrant`
- `Ansible`
- `VirtualBox` (o altro provider supportato da Vagrant)
- `Ansible Vault` (per gestione delle password cifrate)

### 📥 Clonazione del Repository

- Clonare il repository
- Configurare la variabile `container_engine` nei file `vars/main.yaml` dei ruoli

### 🔐 Cifrare le Password con Ansible Vault

```bash
ansible-vault encrypt roles/build-push-role/vars/vault.yaml
```

### 🚀 Esecuzione

```bash
# Avvia le macchine virtuali
vagrant up

# Esegui il playbook Ansible
ansible-playbook playbook.yaml --ask-vault-pass
```

### 🔄 Selezione del Motore Container

- Modificare la variabile `container_engine` in:

```yaml
# Esempio: roles/[role-name]/vars/main.yaml
container_engine: "docker"  # oppure "podman"
```

---

## 🔒 Sicurezza

- Le password sono gestite in modo sicuro tramite **Ansible Vault**
- Le chiavi SSH sono archiviate e usate in modo sicuro
- L'accesso ai container avviene esclusivamente via **SSH autenticato**

---

## 🎛️ Personalizzazione

- Personalizzare i **Dockerfile** nei rispettivi ruoli
- Modificare le **variabili di configurazione** nei file `vars/main.yaml`
- Personalizzare il **JenkinsFile** per adattare il workflow di build
- Cambiare porte e configurazioni di rete nel `Vagrantfile`

---

## 📝 Note

- Assicurarsi di avere risorse sufficienti per eseguire i container
- Verificare la disponibilità delle porte necessarie (es. 22, 5000)
- In caso di problemi di autenticazione, controllare le credenziali in `vault.yaml`
