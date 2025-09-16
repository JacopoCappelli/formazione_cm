Struttura del Progetto Ansible per Gestione Container

Panoramica

Questo progetto utilizza Ansible e Vagrant per automatizzare la creazione, gestione e distribuzione di container Docker/Podman. Il sistema supporta sia Docker che Podman come motori container, selezionabili tramite la variabile configurabile "container_engine".

alpine-role e fedora-role

Creano container basati su Alpine Linux e Fedora con le seguenti caratteristiche:

Esposti sulla porta 22 per accesso SSH.
L' utente "vagrant" ha permessi di accesso alle chiavi SSH private e pubbliche.
Ciascuno dei due ruoli ha un Dockerfile personalizzato.

jenkins-role

Crea un'immagine Jenkins basata su Alpine Linux con:


Gestisce il registry container locale con:

Build e push delle immagini nel registry locale
Autenticazione al registry
Gestione sicura delle password tramite Ansible Vault


Accesso a Docker e Podman
JenkinsFile per automatizzare il build delle immagini
Capacity di eseguire build containerizzati
build-push-role
