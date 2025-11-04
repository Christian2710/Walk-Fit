# Limitazioni delle API Utilizzate

## API Implementate nel Progetto

### 1. API-Ninjas Nutrition API

**Funzione:**
L'API-Ninjas Nutrition viene utilizzata per recuperare informazioni nutrizionali dettagliate degli alimenti. Quando l'utente inserisce il nome di un cibo, l'applicazione interroga questa API per ottenere dati precisi su calorie, proteine, carboidrati e grassi.

**Endpoint utilizzato:**
```
GET https://api.api-ninjas.com/v1/nutrition?query={nome_cibo}
```

**Limitazioni:**
- Limite richieste: 10.000 al mese (piano gratuito)
- Risposta in lingua prevalentemente inglese
- Database non completo per tutti gli alimenti
- Richiede connessione internet attiva

### 2. Pedometer API (Sensore Accelerometro)

**Funzione:**
Il package Pedometer interfaccia con il sensore accelerometro del dispositivo Android per rilevare e contare i passi dell'utente in tempo reale. Il sensore fornisce un conteggio continuo dei passi dal riavvio del dispositivo.

**Limitazioni:**
- Richiede permesso ACTIVITY_RECOGNITION (Android 10+)
- Precisione variabile (circa 95% in condizioni normali)
- Consumo batteria aggiuntivo (circa 2-3% al giorno)
- Dipendente dalla qualità del sensore hardware del dispositivo

## Limitazioni Database Locale Cibi

### Copertura Limitata

Il database locale implementato nell'applicazione contiene un insieme ristretto di alimenti, prevalentemente della cucina italiana:

**Alimenti disponibili:** 40 cibi comuni
**Categorie coperte:**
- Frutta: mela, banana, arance, fragole, pesche
- Carboidrati: pasta, riso, pane, pizza, cereali
- Proteine: pollo, manzo, pesce, tonno, salmone, uova
- Latticini: latte, formaggio, yogurt
- Verdure: patate, carote, pomodoro, insalata
- Dolci: gelato, cioccolato, biscotti
- Condimenti: olio, burro
- Bevande: caffè, tè, acqua, vino, birra

### Limitazioni Identificate

**1. Numero Limitato di Alimenti**
Il database locale contiene solamente 40 alimenti di uso comune. Cibi più specifici o di nicchia non sono presenti e richiedono l'interrogazione dell'API esterna o l'inserimento manuale dei valori nutrizionali.

**2. Bias Culturale Italiano**
La selezione degli alimenti è fortemente orientata verso la cucina italiana, risultando meno utile per utenti con abitudini alimentari di altre culture.

**3. Dati Nutrizionali Standardizzati**
Tutti i valori sono riferiti a porzioni da 100 grammi, richiedendo calcoli proporzionali per quantità diverse.

**4. Assenza Alimenti Elaborati**
Mancano piatti composti o preparazioni elaborate (es. lasagne, tiramisù, piatti etnici).

## Strategie di Mitigazione Implementate

### Fallback Multi-Livello

L'applicazione implementa una strategia di fallback per gestire le limitazioni:

1. **Ricerca Database Locale** (sempre disponibile, 0 latenza)
2. **Interrogazione API-Ninjas** (se connesso, espande possibilità)
3. **Input Manuale** (sempre disponibile come ultima risorsa)

Questa architettura garantisce che l'utente possa sempre inserire un alimento, indipendentemente dalla disponibilità della connessione o dalla presenza del cibo nei database.

### Cache Intelligente

I risultati delle chiamate API vengono salvati nel database SQLite locale, riducendo le richieste successive per lo stesso alimento e migliorando le performance.

## Conclusioni

Le limitazioni identificate rappresentano compromessi accettabili per un'applicazione di questo tipo, considerando che:
- Il database locale copre i cibi più comuni dell'alimentazione quotidiana
- L'API esterna espande significativamente le possibilità di ricerca
- L'input manuale garantisce flessibilità completa
- Il sistema di fallback assicura funzionamento continuo anche offline
