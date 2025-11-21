# SCREEN-C.BAS  
### Decodifica del byte attributo in modalità testo VGA  
**Autore**: Marco da Venezia  
**Data originale**: Domenica 2 ottobre 1988  
**Compatibilità**: QuickBASIC
**Licenza**: Libera per nostalgici e archeologi digitali

---

## Descrizione

Questo script esplora il byte attributo restituito da `SCREEN(riga, colonna, 1)` in modalità testo VGA.  
Il suo scopo è verificare empiricamente la formula che codifica il colore del testo e dello sfondo in un 
singolo byte, e ricostruire da esso i valori originali.

Il programma stampa a video (e su file `prova.txt`) una tabella con:
- il colore del testo (`testo`)
- il colore dello sfondo (`sfondo`)
- il valore letto da `SCREEN(1,1,1)`
- i valori ricostruiti (`colore.testo`, `colore.sfondo`)
- il valore calcolato (`colore.letto`)

---

## Struttura del byte attributo

Il byte attributo in modalità testo VGA è così strutturato:

| Bit | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|-----|---|---|---|---|---|---|---|---|
|     | intensità testo | sfondo (3 bit) | testo (4 bit) |

---

## Formula di codifica

```basic
colore.letto = testo + sfondo * 16 - 112 * (testo > 15)
Nota: il termine -112 * (testo > 15) corregge l’overflow del bit 4 del testo, che altrimenti finirebbe
nel bit 7 (intensità) e altererebbe il valore totale.

Formule inverse (decodifica)
colore.testo  = (colore.letto AND 15) + (colore.letto \ 8 AND 16)
colore.sfondo = (colore.letto \ 16) AND 7

## Uso

    Esegui il programma

    Verrà generata una tabella a video e su file prova.txt

    Puoi confrontare i valori letti da SCREEN con quelli calcolati

    (Opzionale) Rimuovi l’apostrofo da SLEEP per vedere l’output passo-passo


Note storiche

Questo script fu completato il 2 ottobre 1988, in un’epoca in cui la documentazione era scarsa
e l’unico debugger era l’intuizione. È un esempio di reverse engineering creativo, dove la
logica binaria incontra la poesia del colore.
