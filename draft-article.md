# La mappa che si muove

## Come sapere dove investire, quando cambiare, e cosa lasciare andare

---

In *Software Revolution* ho proposto un modello: quattro layer attraverso cui ogni prodotto dell'era dell'intelligenza muove dati da input grezzo a valore per l'utente. Enrichment, Inference, Interpretation, Delivery. EIID.

Il modello funziona. Scomponi un prodotto nei quattro layer e vedi subito dove manca un pezzo: un prodotto senza inference raccoglie dati e li presenta, ma non chiede mai quali pattern contengono. Un prodotto senza delivery ha insight che nessuno riceve al momento giusto.

Ma il modello ha un limite: **ti dice la forma, non ti dice dove sei.**

Sapere che il tuo prodotto ha un nodo di inference non ti dice se quel nodo è un esperimento che stai ancora validando o un problema risolto che dovresti comprare come servizio. E questa distinzione vale tutto. Costruire qualcosa che esiste già come commodity è spreco. Comprare qualcosa che è il cuore della tua proposta di valore è suicidio strategico.

---

## L'asse che mancava

Simon Wardley ha passato vent'anni a studiare come i componenti di un sistema evolvono. La sua osservazione centrale è semplice: tutto si muove da sinistra a destra. Da genesis (nuovo, incerto, nessuno sa come farlo) a commodity (standardizzato, decine di fornitori, compralo).

Quattro stadi:

**Genesis** — non esiste ancora un approccio consolidato. Qui crei valore. Qui investi.

**Custom** — capisci il problema nel tuo contesto, ma non esiste una soluzione standard. Costruisci, ma aspettati che evolva.

**Product** — il problema è ben compreso, esistono molteplici approcci. Usa quello che c'è, adatta al tuo contesto.

**Commodity** — standardizzato, molti fornitori. Compra. Costruire commodity è spreco.

Quando sovrapponi l'evoluzione ai quattro layer di EIID, il modello prende vita. Non stai più guardando una lista di componenti. Stai guardando una mappa.

---

## Cosa cambia con la mappa

Prendi un prodotto di pricing intelligence per e-commerce. Scomponilo in nodi, assegna ogni nodo a un layer, poi chiediti: dove si trova sull'asse dell'evoluzione?

Lo scraping dei prezzi? Commodity. Decine di servizi lo fanno. Compralo.

Il matching tra prodotti competitor e i tuoi SKU? Custom. La logica dipende dal tuo catalogo, nessun servizio lo fa per te. Costruiscilo.

La raccomandazione di prezzo — "abbassa a 24.90, mantieni il ranking, impatto margine -2%, recupero stimato in 3 giorni dal volume aggiuntivo"? Genesis. Nessuno lo fa così. Qui c'è il prodotto. Qui investi tutto.

Senza l'asse dell'evoluzione, questi tre nodi sono tutti "cose da costruire". Con l'asse, uno lo compri, uno lo costruisci, e uno è la ragione per cui il prodotto esiste.

In *Ricomporre il lavoro* scrivevo che senza la componente produttiva, un ingegnere industriale e un regista condividono il 92% delle competenze di valore. La stessa logica si applica qui: senza l'asse dell'evoluzione, tutti i nodi sembrano uguali. Con l'asse, vedi che la maggior parte del lavoro è commodity e il valore si concentra in uno o due punti.

---

## Il problema di Wardley

Le mappe di Wardley sono potenti ma statiche. Una mappa ti dice dove sei oggi. Non ti dice quando muoverti.

Un nodo classificato "genesis" oggi potrebbe essere commodity tra sei mesi. Un approccio sperimentale che funziona al 95% potrebbe meritare di essere sostituito da regole deterministiche. Ma quando? E con cosa?

Questa è la domanda che mancava: **non solo dove sei, ma quando cambi e verso dove.**

---

## Graduation

Ogni nodo del prodotto documenta due cose: **la condizione** (quando cambiare) e **la direzione** (cosa cambia).

"Quando l'accuracy supera il 95% per due settimane, sostituisci con regole deterministiche."

Questa è una graduation completa. Ha il trigger e la destinazione. "Quando l'accuracy supera il 95%" da sola non basta — sai quando agire ma non sai cosa fare.

La graduation va in entrambe le direzioni:

**Verso il basso.** I pattern si stabilizzano, quello che era sperimentale diventa routine. Un classificatore basato su modello diventa una lookup table.

**Verso l'alto.** Un approccio semplice raggiunge i suoi limiti, i casi limite giustificano la complessità. Un classificatore basato su regole ha bisogno di un modello.

Niente nella mappa è permanente. I nodi si muovono. La graduation è il meccanismo che rende esplicito il movimento.

---

## Il loop che si ottimizza da solo

Karpathy ha formalizzato un'idea che molti praticavano senza nominarla: autoresearch. Un agente che migliora se stesso entro vincoli rigidi.

Il meccanismo ha cinque parti fisse:

1. **Un file mutabile.** L'agente può cambiare un solo file per nodo. Tutto il resto è congelato. Questo impedisce all'agente di modificare la valutazione per far sembrare migliore la metrica.
2. **Una metrica.** Un numero. Nessuna ambiguità.
3. **Un budget di tempo fisso.** Ogni esperimento si completa entro una finestra fissa.
4. **Git come keep/discard.** Commit quando la metrica migliora. Reset quando non migliora. I miglioramenti si accumulano. I fallimenti spariscono.
5. **Loop autonomo.** L'agente legge, ipotizza, cambia, misura, tiene o scarta. Nessun umano nel loop.

Questo funziona dove il feedback è veloce e la metrica è pulita. Un nodo di enrichment che fa matching tra prodotti può eseguire 12 esperimenti all'ora. Un nodo di interpretation che genera raccomandazioni di prezzo non può: il segnale (il venditore ha accettato?) richiede giorni.

La mappa cattura questa distinzione esplicitamente. Ogni nodo dichiara se il suo loop è autoresearch (automatizzabile), manual review (serve giudizio umano), o N/A (commodity, basta monitorare).

---

## Dove il fossato non è dove pensi

Tre prodotti diversi. Stessa struttura, pattern diversi.

Un prodotto di pricing intelligence ha la genesis nell'interpretation — la raccomandazione di prezzo è il cuore. Autoresearch funziona sugli Enrichment e Inference nodes, che hanno feedback veloce. Ma il nodo più prezioso ha il feedback più lento.

Un prodotto di recruiting language ha la genesis nell'interpretation — la riscrittura calibrata dei job posting. Ma il fossato reale è nell'enrichment: il dataset che collega il linguaggio usato ai risultati di assunzione. Senza quei dati, tutto il sistema sta indovinando. Il valore strategico è in un asset di dati, non nell'intelligenza.

Un prodotto di brand governance ha la genesis nell'inference, non nell'interpretation. Il valore è nella capacità di rilevare il drift del brand tra canali e lingue. La rilevazione stessa è l'insight. L'intero layer di delivery è replicabile; il fossato è nella detection quality.

La genesis non è sempre nello stesso layer. Il fossato non è sempre dove ti aspetti. L'autoresearch aiuta dove il feedback è veloce, ma il nodo più prezioso ha spesso il feedback più lento.

Senza la mappa, queste distinzioni sono invisibili.

---

## Dal prodotto all'organizzazione

In *Ricomporre il lavoro* proponevo tre modelli organizzativi alternativi alla gerarchia funzionale: flussi di valore, sistema nervoso, protocollo. In *Informazione libera* analizzavo la tesi di Dorsey: la velocità di un'organizzazione è funzione della velocità con cui l'informazione si muove al suo interno. La gerarchia diventa ostacolo quando si lega al potere attraverso il controllo dell'informazione.

La mappa che si muove porta queste idee a una conseguenza operativa.

Se scomponi un'organizzazione nello stesso modo in cui scomponi un prodotto, ogni capability è un nodo. Ogni nodo ha un'evoluzione. Ogni nodo ha una graduation.

Il recruiting è genesis o commodity? Dipende. Se stai assumendo profili standard con processi standard, è commodity. Compra un ATS e un'agenzia. Se stai costruendo un team in un dominio dove le competenze non hanno ancora un nome — dove il 92% del valore è nelle competenze trasversali che l'organigramma non vede — allora è genesis. Investi.

La formazione interna è genesis o commodity? Se stai insegnando a usare Excel, è commodity. Se stai costruendo la capacità dell'organizzazione di scomporre e ricomporre il lavoro intorno alle nuove forme di valore, è genesis.

Il coordinamento tra team è genesis o commodity? Dorsey direbbe: dipende da quanto è libera l'informazione. Se il coordinamento richiede sei riunioni e tre livelli di approvazione, hai un nodo commodity trattato come custom. Se il coordinamento emerge da un protocollo condiviso dove l'AI elabora e le persone decidono, hai un nodo che sta graduando verso il basso — da custom a commodity — e questo è un bene.

La graduation organizzativa funziona come quella di prodotto. "Quando il tempo medio di decisione su questa classe di problemi scende sotto le 4 ore, il comitato di approvazione non serve più." Condizione e direzione. Il trigger e la destinazione.

---

## Il contesto come infrastruttura

Ogni output di questo modello è contesto strutturato. Un documento che un agente AI — o una persona che si siede alla scrivania lunedì mattina — può leggere con zero contesto pregresso e sapere: dove siamo, cosa conta, cosa misurare, quando cambiare approccio.

Se il contesto è stantio, è peggio dell'assenza di contesto. Porta agenti e persone a ottimizzare la cosa sbagliata. Il contesto deve essere fedele alla realtà, e quando la realtà cambia, il contesto si aggiorna.

Questo è il punto di congiunzione con *Informazione libera*. L'informazione che Dorsey vuole liberare dalla gerarchia non è un flusso indifferenziato. È contesto strutturato: dove siamo sull'asse dell'evoluzione, cosa misurare, quando cambiare. Senza struttura, liberare l'informazione produce rumore. Con la struttura della mappa, produce decisioni.

---

## Il gioco vero

Il gioco non è costruire software più in fretta. Quello è il gioco vecchio, e l'AI lo ha reso banale.

Il gioco non è nemmeno sapere cosa costruire. EIID risponde a questa domanda.

Il gioco vero è sapere **dove sei, quando muoverti, e cosa lasciare andare.** È una mappa che non è mai ferma, dove ogni nodo evolve, dove il valore si sposta, dove quello che era sperimentale diventa routine e quello che era routine viene sostituito.

Vale per un prodotto. Vale per un'organizzazione. Vale per una carriera.

Il nodo genesis della tua carriera — quello che ti rende insostituibile — non è lo stesso di cinque anni fa. Sta per graduare? Verso dove?

La domanda non è mai stata "come costruisco questo?" La domanda è sempre stata "dove investo, quando cambio, e cosa lascio andare?"

La mappa si muove. Tu ti muovi con lei, o resti fermo su un nodo che sta diventando commodity.
