# Struttura del Progetto Ansible per Gestione Container

# Panoramica

Questo progetto utilizza Ansible e Vagrant per automatizzare la creazione, gestione e distribuzione di container Docker/Podman. Il sistema supporta sia Docker che Podman come motori container, selezionabili tramite una variabile configurabile. Il file 'main.yaml' di ciascun ruolo funge da punto di accesso per i file inerenti alle configurazioni di Docker o Podman.




# Ruoli e Funzionalità

Variabile di Configurazione Principale

container_engine: Determina quale motore container utilizzare ("docker" o "podman")
alpine-role e fedora-role

Creano container basati su Alpine Linux e Fedora con le seguenti caratteristiche:

Esposti sulla porta 22 per accesso SSH
Utente "vagrant" con permessi di accesso alle chiavi SSH private e pubbliche
File Dockerfile personalizzati per ciascuna distribuzione
jenkins-role

Crea un'immagine Jenkins basata su Alpine Linux con:

Accesso a Docker e Podman
JenkinsFile per automatizzare il build delle immagini
Capacity di eseguire build containerizzati
build-push-role

Gestisce il registry container locale con:

Build e push delle immagini nel registry locale
Autenticazione al registry
Gestione sicura delle password tramite Ansible Vault
Utilizzo

# Prerequisiti
- Vagrant  
- Ansible  
- VirtualBox o altro provider supportato da Vagrant  
- Ansible Vault (per la gestione delle password cifrate)<br><br>


# Clonare il repository
Configurare la variabile container_engine nei file delle variabili dei ruoli  

Cifrare le password con Ansible Vault:  

bash  

ansible-vault encrypt roles/build-push-role/vars/vault.yaml<br><br>

Esecuzione

bash
# Avvia le macchine virtuali con Vagrant
vagrant up

# Esegui il playbook Ansible
ansible-playbook playbook.yaml --ask-vault-pass
Selezione del Motore Container

Modificare la variabile container_engine nei file di variabili dei ruoli:

yaml
# roles/[role-name]/vars/main.yaml
container_engine: "docker"  # o "podman"
Sicurezza

Le password sono gestite tramite Ansible Vault
Le chiavi SSH sono gestite in modo sicuro
L'accesso ai container avviene tramite SSH autenticato
Personalizzazione

È possibile personalizzare:

I Dockerfile nei rispettivi ruoli
Le variabili di configurazione nei file vars/main.yaml
Il JenkinsFile per modificare il workflow di build
Le porte e le configurazioni di rete nel Vagrantfile
Note

Assicurarsi di avere sufficienti risorse per eseguire i container
Verificare che le porte necessarie siano disponibili
Per problemi di autenticazione, verificare le credenziali nel vault
