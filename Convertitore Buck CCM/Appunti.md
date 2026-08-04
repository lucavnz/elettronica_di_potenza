# Appunti Convertitore Buck (Step-Down) in CCM

## 1. L'idea di Base: L'interruttore
Il convertitore Buck serve ad abbassare una tensione continua $V_i$ per fornirne una più piccola in uscita $V_o$, **senza dissipare potenza** (cosa che accadrebbe disastrosamente usando una resistenza).

Come si fa? Si usa un interruttore elettronico che si apre e chiude a frequenza altissima $f_s$:
* **Posizione 1 (ON):** Passa tutta la tensione $V_i$ per un tempo $t_{ON} = D \cdot T_s$.
* **Posizione 2 (OFF):** La tensione va a zero per un tempo $t_{OFF} = D' \cdot T_s$.

Dove **$D$ è il Duty Cycle**, ovvero la percentuale di tempo (sul periodo totale $T_s$) in cui l'interruttore è chiuso.
Così facendo "affettiamo" la batteria (moduliamo) e generiamo un'**onda quadra** che salta continuamente tra $V_i$ e 0. 
La tensione continua (DC) che ci interessa in uscita non è altro che il **valore medio** di questa onda quadra, ed è pari alla sua area diviso il periodo:
$$ \langle v_s \rangle = \frac{1}{T_s} \cdot (D \cdot T_s) \cdot V_i = D \cdot V_i $$

Visto che $D$ è sempre un numero compreso tra 0 e 1, avremo costantemente in uscita che $V_o \le V_i$.

## 2. Il Filtro e il Ripple: La cruda realtà
Non possiamo di certo mandare l'onda quadra grezza direttamente al carico. Ci piazziamo in mezzo un **filtro passa-basso LC** (induttore e condensatore) per prendere solo la componente continua (il valore medio DC) e "tagliare via" le armoniche di commutazione. Essendo componenti reattivi, filtrano senza dissipare energia.

**Ma perché c'è comunque il ripple se abbiamo il filtro?**
* **In frequenza:** Un filtro reale non ha un'attenuazione infinita. Anche se posizioniamo la frequenza di taglio $f_c$ molto più piccola della frequenza di commutazione $f_s$, una minima traccia (bleeding) dell'onda quadra riesce a passare.
* **Nel tempo (intuitivo):** L'induttore si carica e si scarica ciclicamente, generando una corrente a **forma di triangolo** ($i_L$). Questa corrente "altalenante" va a finire nel nodo del condensatore d'uscita. Il condensatore fa da serbatoio per livellare la tensione, ma ricevendo questa corrente variabile sarà costretto a riempirsi un pochino e svuotarsi un pochino ad ogni ciclo. Questa continua altalena di carica si traduce in un'altalena di tensione su di esso: ecco svelato il **ripple** $v_{ripple}(t)$.

**Small Ripple Approximation (SRA):** 
Visto che progettiamo appositamente i convertitori scegliendo $L$ e $C$ sufficientemente grandi, questo ripple è un'increspatura davvero minuscola rispetto al valore medio della tensione d'uscita ($|v_{ripple}| \ll V_o$). 
Pertanto, per non complicarci inutilmente la vita nei calcoli matematici, **facciamo finta che il ripple non esista**. 
Tutte le tensioni e correnti nel tempo le approssimiamo ai loro valor medi, considerandole piatte e costanti, indicandole con le lettere MAIUSCOLE ($V_o, I_L$). Diremo quindi semplicemente che $v_o(t) \approx V_o$.

## 3. L'Induttore: Carica e Scarica ($t_{ON}$ e $t_{OFF}$)
Grazie alla SRA, sappiamo che l'uscita è fissa a $V_o$. Il comportamento dell'induttore è dettato dalla formula $v_L = L \frac{di_L}{dt}$. A seconda di come è posizionato l'interruttore, l'induttore "vede" tensioni diverse:

* **Fase 1 (ON - Interruttore Chiuso):** L'induttore è collegato tra $V_i$ e $V_o$. La tensione ai suoi capi è **positiva e costante**: $v_L = V_i - V_o$. 
  Dalla formula ricaviamo che la pendenza della corrente è positiva: $\frac{di_L}{dt} = \frac{V_i - V_o}{L}$.
  **Cosa succede:** La corrente sale creando una rampa (triangolo in salita). L'induttore si sta caricando, immagazzinando energia magnetica presa dalla batteria.

* **Fase 2 (OFF - Interruttore Aperto):** La batteria viene staccata e l'induttore viene cortocircuitato a massa a sinistra (tramite un diodo o uno switch). La tensione ai suoi capi diventa **negativa e costante**: $v_L = 0 - V_o = -V_o$.
  Dalla formula ricaviamo che la pendenza della corrente è negativa: $\frac{di_L}{dt} = \frac{-V_o}{L}$.
  **Cosa succede:** La corrente scende creando una rampa (triangolo in discesa). L'induttore fa da generatore e rilascia al carico l'energia accumulata prima.
