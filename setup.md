# Setup

Questo è un tutorial per chi utilizza questo materiale con [Obsidian](https://obsidian.md/) .

## Git
Clona la repository dove contiene il materiale
```bash
git clone https://github.com/MattiaPasquini/appuntisupsi.git
```

## Obsidian
Scarica Obsidian dal sito ufficiale, [clicca qui](https://obsidian.md/).

Ora bisogna aprire un vault (se avete Obsidian già scaricato, andate su `manage vaults`). 
Clicca `Open`, scegliendo la cartella dove hai clonato la repo (la cartella `appuntisupsi`).

Per aggiornare le nuove risorse dalla repo, seguite il comando nella cartella `appuntisupsi`
```bash
git pull
```

### Per i collaboratori
Notate ora che la struttura delle cartelle è ben organizzata. Nella cartella `primo_semestre` ovviamente (come disse un saggio: *grazie al cazzo*) contiene i moduli di quel semestre. Ogni cartella del modulo contiene il file `index.md` che contiene le informazioni del modulo e la lista dei capitoli. Ogni capitolo che scriviamo va salvato nella cartella `capitolo` nel modulo appropriato, il filename deve essere `capitolo<NN>` dove NN è un numero intero incrementale.

Andate nelle impostazioni > Files and links e fate queste cose:
- Disattivate `[[Wikilinks]]` perché altrimenti sul browser non mostra correttamente le foto e i link
- Il default location per gli attachment (principalmente per le immagini) va settato in: `In subfolder under current folder`
- Il nome del subfolder deve essere: `attachments`

Il template del capitolo è il seguente:

```a
# <materia>

**Data lezione**: <data>

**Autori**: <autore1>, <autore2>, ...

[back](./../index.md)

# Capitolo NN - <nome capitolo>

<contenuto>
```
