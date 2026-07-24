Ok, fermi tutti. Ho capito esattamente dov'è il cortocircuito logico.
Il tuo cervello sta (giustamente) pensando: *"Se la tensione Vout è costante, vuol dire che il condensatore non si sta scaricando. E se non si scarica, ricevendo sempre colpi dall'induttore, dovrebbe andare all'infinito!"*

L'errore è confondere la **TENSIONE** con la **CARICA (CORRENTE)**.

### Il trucco dell'Oceano (Perché si scarica ma Vout non cala)

Il condensatore **SI SCARICA ECCOME!** Butta fiumi di corrente dentro la resistenza $R$ in ogni istante. Perde una quantità enorme di carica ($Q$).

Ma guarda la formula del condensatore: 
$$ \Delta V = \frac{\Delta Q}{C} $$
(Variazione di tensione = Carica persa / Capacità).

Se tu perdi molta carica $\Delta Q$, ma io ingegnere ho installato un condensatore $C$ **GIGANTESCO** (un oceano), il risultato della divisione $\Delta V$ diventa **zero virgola qualcosa**.

*   L'oceano perde migliaia di litri d'acqua (carica) al secondo per colpa del carico $R$? **SÌ.**
*   L'induttore in Fase 2 ributta dentro migliaia di litri d'acqua ripristinando esattamente l'equilibrio? **SÌ.**
*   Il livello dell'oceano (la tensione $V_{out}$) si alza e si abbassa a causa di questo scambio selvaggio? **NO, praticamente resta immobile!** Si muove al massimo di qualche millivolt.

Ecco perché io posso tranquillamente calcolare le mie equazioni assumendo che $V_{out}$ sia "costante". Non è costante perché è inattivo, è costante perché il "secchio" è così grande che non vedi increspature sulla superficie! E l'equilibrio è salvo: l'acqua entra e l'acqua esce. Niente va all'infinito.

---

### La Derivazione Ufficiale (I Passaggi Logici)

Adesso che abbiamo chiarito che $V_{out}$ è un numero fisso e stabile (es. $24V$) pur scambiando ferocemente energia, ecco come l'elettronica di potenza dimostra il guadagno.

Tutto ruota attorno a un'unica legge della fisica: in un circuito che lavora a regime (cioè non è appena stato acceso, ma gira stabile), **l'induttore deve iniziare e finire ogni ciclo esattamente con la stessa corrente**.
Se inizia un ciclo a 10A, a fine ciclo DEVE essere a 10A. (Se non fosse così, la corrente media starebbe salendo verso l'infinito o scendendo a zero).

La formula dell'induttore è: $v_L(t) = L \cdot \frac{di_L}{dt}$.
Se moltiplichi a destra e sinistra per $dt$ ottieni:
$v_L(t) dt = L \cdot di_L$.

Se **integriamo** questa formula su un intero ciclo (da $0$ a $T$), a destra la variazione totale di corrente $\Delta i_L$ sappiamo che deve fare **zero**.
Quindi otteniamo la Legge del **Bilanciamento dei Volt-Secondo**:
$$ \text{Area di } v_L \text{ nel ciclo intero} = 0 $$
L'area positiva deve cancellare l'area negativa.

Calcoliamo le aree nei due tempi:

**1. Sottofase ON (Tempo $t_{on} = D \cdot T$):**
L'interruttore collega l'induttore tra la batteria $V_g$ e la massa.
Tensione sull'induttore: $v_L = V_g$
L'area di questo rettangolo è:
**Area ON** = $V_g \cdot (D \cdot T)$

**2. Sottofase OFF (Tempo $t_{off} = (1-D) \cdot T$):**
L'interruttore collega l'induttore al condensatore d'uscita. 
Tensione sull'induttore: $v_L = V_g - V_{out}$. 
*(Qui usiamo l'Approssimazione dell'Oceano: $V_{out}$ è un numero fisso anche se si sta scaricando e caricando furiosamente di corrente).*
L'area di questo rettangolo è:
**Area OFF** = $(V_g - V_{out}) \cdot (1-D) \cdot T$

**3. L'Equazione di Bilancio:**
Sommiamo le due aree e le poniamo uguali a zero!
$$ V_g \cdot D \cdot T + (V_g - V_{out}) \cdot (1-D) \cdot T = 0 $$

*   Divido tutto per $T$ e sparisce il tempo:
    $$ V_g \cdot D + (V_g - V_{out}) \cdot (1-D) = 0 $$
*   Moltiplico la parentesi a destra:
    $$ V_g \cdot D + V_g \cdot (1-D) - V_{out} \cdot (1-D) = 0 $$
*   Raccogliamo $V_g$ nei primi due termini:
    $$ V_g \cdot (D + 1 - D) - V_{out} \cdot (1-D) = 0 $$
*   Dentro la parentesi $(D + 1 - D)$ le D si annullano, rimane solo $1$:
    $$ V_g \cdot (1) - V_{out} \cdot (1-D) = 0 $$
*   Porto il termine con $V_{out}$ dall'altra parte dell'uguale:
    $$ V_g = V_{out} \cdot (1-D) $$
*   E infine isolo il Guadagno:
    $$ \frac{V_{out}}{V_g} = \frac{1}{1-D} $$

Questa è la dimostrazione matematica pura, senza analogie.
E ti fa vedere come l'induttore, pur di mantenere l'equilibrio delle aree ($Area = 0$), costringe la $V_{out}$ (che è l'unica variabile libera in quella equazione) ad assestarsi esattamente al valore di $\frac{1}{1-D}$. E il condensatore, essendo un oceano, accetta questo livello di tensione fissato scambiando ferocemente l'acqua con la resistenza R per mantenere la pace.