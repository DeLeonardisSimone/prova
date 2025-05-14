# 📘 Relazione Tecnica - Gioco degli scacchi ♟️

### 📌 Indice

1. [📖 Introduzione](#-1-introduzione)  
2. [🧠 Modello di Dominio](#-2-modello-di-dominio)  
3. [✅ Requisiti Specifici](#-3-requisiti-specifici)  
   - [3.1 Requisiti Funzionali](#31-requisiti-funzionali)    
   - [3.2 Requisiti Non Funzionali](#32-requisiti-non-funzionali)   
4. [🔍 Analisi retrospettiva](#-8-analisi-retrospettiva)  
   - [8.1 Sprint 0](#81-sprint-0)  
5. [📁 Struttura cartelle/files principali progetto](#-struttura-cartellefiles-principali-progetto)  
6. [📊 Stato attuale dello sviluppo](#-stato-attuale-dello-sviluppo)  

---

## 📖 1. Introduzione

Il presente documento descrive l'architettura e l'implementazione di un **Gioco degli scacchi** completo sviluppato in _Python 3_, progettato secondo il pattern *ECB (Entity-Control-Boundary)*. L'applicazione offre:

- Un'interfaccia a riga di comando (_CLI_) interattiva;
- Le regole standard degli scacchi per spostare i pedoni;
- Gestione dello stato di gioco.

## 🧠 2. Modello di Dominio

![modello di dominio](./img/report/modello_dominio.PNG)

## ✅ 3. Requisiti Specifici

### 3.1 Requisiti Funzionali

> <i>Clicca su ogni voce per saperne di più</i>

<details>
<summary> <b>🆘 US1 – Visualizzazione dell’help </b></summary>
<br>

| 🏷️ **Elemento**        | 📋 **Contenuto** |
|------------------------|------------------|
| **Descrizione**     | Il sistema deve fornire un elenco dei comandi disponibili. |
| **Attori**          | Giocatore |
| **Comando**         | `/help` |
| **Parametri**  | `--help`, `-h`  |
| **Comportamento atteso** | Mostrare una descrizione concisa dell'app con l'elenco dei comandi uno per riga.  |

> **Esempio di output:**  
> ```
> /gioca       Inizia una nuova partita di scacchi
> /esci        Chiude il gioco e termina il programma
> /patta       Propone un pareggio all'avversario
> /abbandona   Abbandona la partita in corso, concedendo la vittoria all'avversario
> /scacchiera  Visualizza la scacchiera con la posizione attuale dei pezzi
> /mosse       Mostra la cronologia delle mosse giocate finora nella partita
> ```

</details>

<details>
<summary> <b>🎮 US2 – Avvio nuova partita </b></summary>
<br>

| 🏷️ **Elemento**    | 📋 **Contenuto** |
|--------------------|------------------|
| **Descrizione** | Il giocatore può iniziare una nuova partita. |
| **Attori**      | Giocatore |
| **Comando**     | `/gioca` |
| **Comportamento atteso** | Visualizzare la scacchiera iniziale; il sistema attende la mossa del Bianco. |

</details>

<details>
<summary> <b>♟️ US3 – Visualizzazione scacchiera </b></summary>
<br>

| 🏷️ **Elemento**    | 📋 **Contenuto** |
|--------------------|------------------|
| **Descrizione** | Il giocatore può richiedere di vedere lo stato attuale della scacchiera. |
| **Attori**      | Giocatore |
| **Comando**     | `/scacchiera` |
| **Comportamento atteso** | Suggerire il comando `/gioca` se la partita non è iniziata; altrimenti mostrare lo stato attuale della scacchiera. |

</details>

<details>
<summary> <b>🏳️ US4 – Abbandono partita </b></summary>
<br>

| 🏷️ **Elemento**    | 📋 **Contenuto** |
|--------------------|------------------|
| **Descrizione** | Il giocatore può abbandonare la partita in corso. |
| **Attori**      | Giocatore |
| **Comando**     | `/abbandona` |
| **Comportamento atteso** | Chiedere conferma abbandono: se sì, comunicare all’avversario la vittoria; se no, attendere nuovi comandi dal giocatore in turno. |

</details>

<details>
<summary> <b>🤝 US5 – Proposta di patta </b></summary>
<br>

| 🏷️ **Elemento**    | 📋 **Contenuto** |
|--------------------|------------------|
| **Descrizione** | Il giocatore può proporre la patta. |
| **Attori**      | Giocatore |
| **Comando**     | `/patta` |
| **Comportamento atteso** | Chiedere conferma all’avversario: se accetta, partita finita in pareggio; se rifiuta, attesa di nuovi comandi dal giocatore in turno. |

</details>

<details>
<summary> <b>❌ US6 – Uscita dal gioco </b></summary>
<br>

| 🏷️ **Elemento**    | 📋 **Contenuto** |
|--------------------|------------------|
| **Descrizione** | Il giocatore può chiudere l’applicazione. |
| **Attori**      | Giocatore |
| **Comando**     | `/esci` |
| **Comportamento atteso** | Chiedere conferma: se confermato, chiudere l’app; se annullato, ritornare in attesa di comandi. |

</details>

<details>
<summary> <b>🚶 US7 – Mossa di un pedone </b></summary>
<br>

| 🏷️ **Elemento**    | 📋 **Contenuto** |
|--------------------|------------------|
| **Descrizione** | Il giocatore può muovere un pedone. |
| **Attori**      | Giocatore |
| **Comando**     | Notazione algebrica italiana. |
| **Comportamento atteso** | Muovere un pedone. Aggiornare scacchiera se la mossa risulta valida; altrimenti comunicare l'errore. |

</details>

<details>
<summary> <b>📜 US8 – Visualizzazione mosse giocate </b></summary>
<br>

| 🏷️ **Elemento**    | 📋 **Contenuto** |
|--------------------|------------------|
| **Descrizione** | Il giocatore può visualizzare la cronologia delle mosse. |
| **Attori**      | Giocatore |
| **Comando**     | `/mosse` |
| **Comportamento atteso** | Mostrare le mosse in notazione algebrica abbreviata. |

> **Esempio:**  
> ```
> 1. e4 c6
> 2. d4 d5
> ```

</details>


### 3.2 Requisiti Non Funzionali

> <i>Clicca su ogni voce per saperne di più</i>

<details>
<summary><b>💡 RNF1 – Esecuzione in container docker</b></summary>
<br>

| 🏷️ **Elemento**        | 📋 **Contenuto** |
|------------------------|------------------|
| **Descrizione**     | L’applicazione deve essere eseguita all’interno di un container Docker. |
| **Aspetti chiave**   | - Compatibilità con ambienti containerizzati<br>- Deployment standardizzato |

</details>

<details>
<summary><b>💡 RNF2 – Compatibilità terminali</b></summary>
<br>

| 🏷️ **Elemento**        | 📋 **Contenuto** |
|------------------------|------------------|
| **Descrizione**     | Il programma deve essere compatibile con diversi terminali nei principali sistemi operativi. |
| **Aspetti chiave**   | - Supporto per <i>Terminal di Linux</i><br>- Supporto per <i>Terminal di MacOS</i><br>- Supporto per <i>PowerShell di Windows</i><br>- Supporto per <i>Git Bash di Windows</i> |

</details>

<details>
<summary><b>💡 RNF3 – Supporto UTF-8</b></summary>
<br>

| 🏷️ **Elemento**        | 📋 **Contenuto** |
|------------------------|------------------|
| **Descrizione**     | I simboli dei pezzi degli scacchi devono essere rappresentati usando caratteri UTF-8. |
| **Aspetti chiave**   | - Supporto per i simboli: ♔ ♕ ♖ ♗ ♘ ♙ ♚ ♛ ♜ ♝ ♞ ♟<br>- Rendering corretto su tutti i terminali supportati |

</details>


## 🔍 8. Analisi retrospettiva

### 8.1 Sprint 0

![lavagna](./img/report/sprint_retrospective.png)

#### Azioni correttive da intraprendere

Nel corso dell’attività progettuale sono emerse alcune dinamiche di gruppo che possono essere migliorate per ottimizzare l’efficienza e la collaborazione.  
A tal fine, si individuano le seguenti _azioni correttive_ da mettere in atto.

**INIZIARE A...**  

- **Affrontare le task con maggiore obiettività**, concentrandosi su quanto richiesto prima di rifinire il lavoro.  
- **Gestire il tempo dei meeting in modo più efficace**, stabilendo limiti chiari e rispettando la durata prevista.  
- **Partecipare attivamente alle revisioni**, rispondendo prontamente alle richieste dei membri del team.  
- **Favorire il confronto costruttivo**, valorizzando anche idee contrastanti per trovare la soluzione più adatta.

**SMETTERE DI...**

- **Focalizzarsi prematuramente sui dettagli**, trascurando la visione d’insieme e gli obiettivi principali.
- **Prolungare inutilmente i meeting**, soffermandosi su aspetti secondari non prioritari.
- **Tollerare ritardi personali**, poiché anche brevi interruzioni possono compromettere il flusso di lavoro del team.

**CONTINUARE A...**

- **Sostenere e collaborare attivamente con i membri del gruppo**, mantenendo un clima di disponibilità reciproca.
- **Produrre codice e documentazione di qualità**, curando forma e contenuti in ogni fase del progetto.
- **Favorire il dialogo tecnico**, condividendo idee e soluzioni per affrontare problematiche in modo efficace.
- **Pianificare meeting regolari**, per monitorare l’avanzamento e garantire un allineamento costante tra i membri.
- **Documentare in modo preciso gli errori rilevati nelle pull request**, proponendo soluzioni chiare e contestualizzate.
- **Seguire le linee guida di sviluppo**, in particolare il GitHub Workflow, con commit descrittivi e coerenti.
- **Comunicare tempestivamente l’inizio delle attività individuali**, evitando sovrapposizioni e conflitti.

---

## 📁 Struttura cartelle/files principali progetto

```
docs/
│   ├── img/
│       └── ...
│   ├── CODE_OF_CONDUCT.md
│   ├── Guida configurazione repo.md
│   ├── ISPIRATORE.md
│   ├── Manuale.md
│   ├── Report.md
│   └── ...
scacchi/
├── boundary/
│   ├── ComandiGioco.py
│   ├── ElementiBoundary.py
│   ├── Interfaccia.py
│   └── ...
├── control/
│   ├── Dispatcher.py
│   ├── Partita.py
│   ├── Setup.py
│   └── ...
├── entity/
│   ├── Alfiere.py
│   ├── Cavallo.py
│   ├── Cella.py
│   ├── Giocatore.py
│   ├── Pedone.py
│   ├── Pezzo.py
│   ├── Re.py
│   ├── Regina.py
│   ├── Scacchiera.py
│   ├── Torre.py
│   └── ...
├── utility/
│   ├── Eccezioni.py
│   ├── MessaggiStampa.py
│   ├── UI.py
│   └── ...
├── main.py
└── ...
tests/
└── ...
```

---

## 📊 Stato attuale dello sviluppo

**READY:**
- Struttura modulare pronta
- Funzioni base operative
- Grafica _CLI_ curata e adattata al tipo di terminale  

**IN PROGRESS:**
- Mosse complete dei pezzi ancora
- Gestione regole complesse (scacco, scacco matto, arrocco, en passant)
