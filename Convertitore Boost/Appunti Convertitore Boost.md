Convertitore Boost = innalzare una tensione. Da Vin arrivo a Vo.

A regime, vale il VSB = Volt Second Balance = la corrente media dell'induttore è costante nei cicli ($\int_0^{T_s} v_L dt = 0$).

### 1. Ipotesi di $V_o$ costante e Small Ripple Approximation
* **Cosa succede davvero:** Durante $t_{ON}$ il condensatore alimenta il carico da solo (si scarica), mentre durante $t_{OFF}$ l'induttore gli spara dentro corrente e lo ricarica. Quindi $v_o(t)$ oscilla in continuazione!
* **Perché diciamo che $V_o$ è costante:** Usiamo la *Small Ripple Approximation*. Se il condensatore è grande e la frequenza di switching $f_s$ è alta, l'oscillazione $\Delta v_o$ è piccolissima (tipo 1\% di $V_o$). Facciamo finta che $v_o(t) \approx V_o$ (costante pari al valore medio) così le tensioni sull'induttore diventano rettangoli e possiamo calcolare facilmente il VSB e il guadagno a regime:
$$ M(D) = \frac{V_o}{V_i} = \frac{1}{1-D} $$

### 2. Perché non mettiamo un condensatore gigante ($C \to \infty$)? (Il Trade-off)
Se mettessimo un condensatore gigante il ripple andrebbe a zero, ma avremmo dei problemi enormi:
* **Transitorio di accensione infinito:** Per riempire un "mare" ci servono tantissimi cicli.
* **Corrente di spunto (Inrush Current) letale:** All'istante zero $V_c = 0$, quindi il condensatore scarico è un corto circuito. Finché $V_o < V_i$, la derivata della corrente nell'induttore $\frac{di}{dt}$ resta sempre positiva sia in $t_{ON}$ che in $t_{OFF}$! L'induttore continua a caricarsi senza mai scaricarsi, creando un picco di corrente iniziale enorme prima di stabilizzarsi.
* **Risposta dinamica lentissima:** Se il carico cambia, il sistema ci mette una vita a riassestarsi.
* **Soluzione reale:** Invece di usare un $C$ gigante, si aumenta la frequenza di switching $f_s$. Dalla formula del ripple $\Delta v_o = \frac{V_o \cdot D}{R \cdot C \cdot f_s}$, vedo che alzando $f_s$ posso tenere $C$ piccolo (componenti compatti e veloci) mantenendo comunque il ripple bassissimo.

### 3. Autoadattamento del circuito ideale vs perché serve il microcontrollore
* **Nel circuito IDEALE:** Se aumento il carico (scende $R$), il condensatore si scarica di più in $t_{ON}$ e $V_o$ scende un attimo. Di conseguenza la derivata in $t_{OFF}$ ($\frac{V_i - V_o}{L}$) diventa meno negativa, quindi l'induttore si scarica di meno e $i_L$ cresce ciclo dopo ciclo. Il circuito si **autoadatta da solo** fino a trovare un nuovo equilibrio dove $I_L$ è più alta e $V_o$ torna ESATTAMENTE a $\frac{V_i}{1-D}$.
* **Perché allora serve la retroazione (controllo PWM / microcontrollore)?**
  1. **Resistenze parassite reali ($R_L, R_{DS(on)}, V_D$):** Più aumenta $I_L$, più aumentano le cadute di tensione parassite. In realtà la tensione scende sotto carico: $V_o \approx \frac{V_i - I_L R_L - V_D}{1-D}$. Il microcontrollore serve ad alzare $D$ per compensare le perdite e tenere $V_o$ fissa.
  2. **Batteria che si scarica:** Se $V_i$ cala nel tempo, il microcontrollore adegua $D$.
  3. **Smorzamento e velocità:** Evita oscillazioni risonanti (ringing del circuito LC) durante i cambi di carico bruschi.

### 4. Ampere-Second Balance (ASB) e "Il trucco del Trapezio"
* **ASB (Ampere-Second Balance):** A regime, la corrente media che entra nel condensatore in un ciclo deve essere nulla: $\int_0^{T_s} i_c dt = 0$. Ovvero: la carica persa in scarica deve essere uguale a quella recuperata in ricarica.
* **Corrente sul condensatore $i_c(t)$:**
  * **In Fase 1 ($t_{ON}$, durata $D \cdot T_s$):** Il diodo è spento. Il condensatore fa tutto da solo prestando energia al carico. Quindi $i_c = -\frac{V_o}{R}$. (Rettangolo negativo).
  * **In Fase 2 ($t_{OFF}$, durata $D' \cdot T_s$):** L'induttore spara la sua corrente sul nodo. Una parte va al carico, il resto ricarica il condensatore. $i_c(t) = i_L(t) - \frac{V_o}{R}$. 
* **Il trucco dell'Area (Small Ripple Approximation vs Realtà):**
  * Nella realtà, durante la Fase 2, l'induttore si scarica, quindi $i_L(t)$ scende a **rampa**. Il grafico di $i_c$ in Fase 2 non è un rettangolo piatto come spesso si disegna per semplicità, ma è un **trapezio**.
  * Come mai per calcolare l'ASB usiamo la banale formula del rettangolo sostituendo $I_L$ (il valore medio)? Perché l'area di un trapezio lineare (la rampa) è matematicamente identica all'area di un rettangolo avente per altezza il valore medio della rampa stessa! 
  * Risolvendo l'ASB con le aree dei due "rettangoli" (quello vero di Fase 1 e quello "finto" di Fase 2), si ottiene: $(-\frac{V_o}{R} \cdot D) + (I_L - \frac{V_o}{R}) \cdot D' = 0 \implies I_L = \frac{V_o}{D' R} = \frac{I_o}{D'}$. Questo ci dice che per generare 1A in uscita, l'induttore deve "pompare" molta più corrente (visto che $D'<1$). 
* **La tensione $v_o(t)$:** In Fase 1 $v_o(t)$ scende lungo una retta. In Fase 2, essendo $i_c$ una rampa, $v_o(t)$ salirebbe seguendo una **parabola**. Ma per la Small Ripple, la disegniamo come un'onda triangolare diritta.

### 5. Correnti Medie e Nodi (Valori Istantanei vs Valori Medi)
* Un errore classico è confondere le equazioni ai nodi dei valori istantanei con quelle dei valori medi.
* **Valori Istantanei (le minuscole):** In ogni singolo istante di tempo, al nodo di uscita vale Kirchhoff: $i_D(t) = i_C(t) + i_o(t)$. 
  * In Fase 1 ($t_{ON}$): il diodo non conduce ($i_D = 0$) $\implies i_C = -i_o = -\frac{V_o}{R}$.
  * In Fase 2 ($t_{OFF}$): il diodo conduce la corrente dell'induttore ($i_D = i_L(t)$) $\implies i_C = i_L(t) - \frac{V_o}{R}$.
* **Valori Medi (le Maiuscole):** Se prendiamo l'equazione del nodo e ne facciamo la media su un intero periodo $T_s$, otteniamo l'equazione dei valori medi: $I_D = I_C + I_o$.
  * Per via dell'ASB, sappiamo che il condensatore in un ciclo si carica tanto quanto si scarica, quindi la sua corrente media è nulla ($I_C = 0$).
  * Sostituendo $I_C = 0$ nell'equazione, scopriamo che **$I_D = I_o$**. Tutta la corrente media "sputata" dal diodo va inevitabilmente a finire nel carico.
* **Il ruolo fisico del condensatore:** Si comporta da ammortizzatore idraulico. Assorbe le "botte" (picchi) di corrente erogate dal diodo durante la Fase 2, e cede docilmente questa corrente al carico durante la Fase 1. In questo modo fa il lavoro sporco istante per istante, assicurandosi che il carico beva una $I_o$ bella costante.
* **Calcolo della $I_D$ dal grafico:** La corrente istantanea del diodo $i_D$ è nulla in Fase 1 e uguale a $i_L$ (rampa decrescente) in Fase 2. Per trovare $I_D$ bisogna integrare sull'intero periodo e dividere per $T_s$. L'integrale non è altro che l'area del solito trapezio ($I_L \cdot D' T_s$). Dividendo per $T_s$, si ottiene **$I_D = I_L \cdot D' = (1-D)I_L$**.

### 6. Modalità Continua (CCM) vs Discontinua (DCM) e il Rapporto di Conversione
* **CCM (Continuous Conduction Mode):** La corrente nell'induttore oscilla ma **non scende mai a zero**. Il ciclo ha solo due fasi: $t_{ON}$ (durata $DT$) in cui l'interruttore è chiuso, e $t_{OFF}$ (durata $D'T = (1-D)T$) in cui l'interruttore è aperto. Il Volt-Second Balance (integrale della tensione su $T_s$) ci regala la classica formula $M = \frac{1}{1-D}$.
* **DCM (Discontinuous Conduction Mode):** Se il carico è troppo leggero (corrente media bassa) o l'induttanza è piccola, la corrente scende così tanto in fase 2 che **sbatte sullo zero prima che finisca il periodo**. 
  * In questo caso la Fase 2 non dura più tutto il tempo rimanente ($D'T$), ma dura di meno! Chiamiamo questa durata $D_2 T$ (con $D_2 < 1-D$).
  * Compare una **Fase 3** in cui l'induttore rimane completamente scarico (corrente nulla, tensione nulla) fino all'inizio del nuovo ciclo.
* **Cosa cambia nei conti:** Quando calcoliamo il Volt-Second Balance in DCM, il "rettangolo" negativo della fase 2 è più stretto (dura solo $D_2 T$). L'equazione delle aree (integrale) diventa:
  $$ V_g \cdot D + (V_g - V_o) \cdot D_2 + 0 \cdot D_3 = 0 $$
  Risolvendola scopriamo che la formula del guadagno cambia completamente:
  $$ \frac{V_o}{V_g} = \frac{D + D_2}{D_2} = 1 + \frac{D}{D_2} $$
* **Conseguenza fondamentale:** Poiché il tempo $D_2$ in cui l'induttore si scarica dipende da quanta energia assorbe il carico, **in DCM il rapporto di conversione non dipende più solo dal duty cycle $D$, ma inizia a dipendere fortemente anche dal carico $R$ e dall'induttanza $L$!**