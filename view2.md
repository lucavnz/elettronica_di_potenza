Edited view2.md
Edited introduzione_ai_convertitori.md
Viewed introduzione_ai_convertitori.md:1-9

Eccoci! Queste due slide sono il "ponte" definitivo tra la teoria della commutazione e la pratica dei convertitori reali (con tanto di retroazione che ormai conosci bene). Analizziamole al microscopio.

---

### 📌 Slide 12: La Tensione Media (Come trasformare un'onda quadra in DC)
Nelle slide precedenti abbiamo visto che l'interruttore si accende e si spegne velocemente. Quindi la tensione che "esce" dall'interruttore ($v_s(t)$) non è continua, ma è un'onda quadra: vale $V_i$ (quando è chiuso) e $0$ (quando è aperto).

**Guarda bene il grafico:**
*   L'asse orizzontale è il tempo. L'intero ciclo dura $T_s$ (periodo di commutazione).
*   L'interruttore sta acceso per un tempo $DT_s$ (dove $D$ è il Duty Cycle, un numero tra 0 e 1. Ad esempio, se $D=0.3$, sta acceso per il 30% del tempo).
*   In quel tempo, si forma un rettangolo colorato di rosso.
*   **Qual è l'area di questo rettangolo?** Base $\times$ Altezza = $(DT_s) \times V_i$.

**Le formule (L'Integrale):**
La formula con l'integrale sembra complicata, ma dice una cosa banalissima: "Per trovare la tensione *media*, calcola l'area del rettangolo e dividila per la lunghezza di tutto il periodo $T_s$".
Se tu prendi "quel mucchietto rosso" e lo spalmi uniformemente su tutto l'asse orizzontale (come fosse burro sul pane), ottieni l'altezza della linea tratteggiata $\langle v_s \rangle$.

La matematica dà come risultato: **$\langle v_s \rangle = D \cdot V_i$**.

**Cosa significa fisicamente e perché cita Fourier?**
Significa che se hai una batteria in ingresso $V_i = 100V$ e un duty cycle $D = 0.3$, la tensione media in uscita è $30V$. Hai appena abbassato la tensione (Convertitore Buck/Abbassatore) **senza usare una resistenza che spreca calore!**
Il professore scrive *"primo termine serie di Fourier"* perché se metti un filtro passa-basso dopo questo inter

ruttore (lo vedremo nella slide 24), il filtro "ammazza" tutte le armoniche e fa passare solo la componente media (la DC, a frequenza zero).

---

### 📌 Slide 13: Il Sistema di Controllo (Chi decide il valore di $D$?)
Qui chiudiamo il cerchio con la tua domanda sugli amplificatori operazionali.

**Guarda lo schema a blocchi:**
1.  **Switching Converter:** Questo è il blocco di potenza (interruttori, induttori, condensatori). Prende energia grezza ("Power input") e sputa fuori energia ("Power output").
2.  **Controller (Microcontrollore):** Questo è il famoso "cervello" (il blocco di retroazione). Non ha a che fare con la potenza, usa solo segnali logici (pochi volt).

**Come funziona il giro di valzer:**
*   Il convertitore eroga potenza al carico.
*   Il **Controller** "spia" di nascosto la tensione di uscita tramite il cavo "Control input".
*   Dentro il Controller (che può essere un amplificatore operazionale analogico o un processore digitale) c'è la famigerata comparazione: $V_{ref} - V_{misurata} = \text{Errore}$.
*   **LA DIFFERENZA CHIAVE RISPETTO ALLA SLIDE 6:** Se l'errore aumenta, il Controller **non** spara più o meno corrente nella base del transistore per usarlo a metà potenza (come nella slide 6). Il Controller usa l'errore per **modificare il Duty Cycle ($D$)**.
    *   *La $V_{out}$ si sta abbassando troppo?* Il Controller allarga il rettangolo rosso (aumenta $D$). Il transistore resta acceso più a lungo a ogni ciclo, la media sale, e l'uscita viene "pompata" su.
    *   *La $V_{out}$ sta salendo troppo?* Il Controller restringe il rettangolo rosso (diminuisce $D$). L'uscita scende.

**In Sintesi (Ottimo per i tuoi appunti in `.md`):**
Mentre il regolatore lineare della slide 6 usava la retroazione per variare la *resistenza interna* del transistore (sprecando vagonate di Watt), il convertitore switching usa la retroazione per variare **la larghezza dell'impulso (il Duty Cycle $D$)** tramite un processo chiamato **PWM** (Pulse Width Modulation). Visto che il transistore lavora sempre e solo come interruttore ideale (ON/OFF), l'efficienza rimane altissima (spesso >95%) indipendentemente da come il Controller varia $D$!

---

Ok, via le analogie. Parliamo il linguaggio puro dell'elettronica a stato solido, delle giunzioni PN e dei datasheet.

I 4 grafici della Slide 15 rappresentano i **Limiti Operativi** del dispositivo. 
Gli assi sono esattamente le variabili ai morsetti principali:
*   **Asse X (Orizzontale):** Tensione applicata al dispositivo quando il terminale di controllo gli impone di stare **SPENTO** ($I_B = 0$ per BJT, $V_{GS} = 0V$ per MOSFET). 
    *   Destra ($V > 0$): $V_{CE} > 0$ oppure $V_{DS} > 0$.
    *   Sinistra ($V < 0$): $V_{CE} < 0$ oppure $V_{DS} < 0$.
*   **Asse Y (Verticale):** Corrente che attraversa il dispositivo quando il terminale di controllo gli impone di stare **ACCESO** (in saturazione o regione lineare).
    *   Alto ($I > 0$): $I_C > 0$ (da C a E) oppure $I_D > 0$ (da D a S).
    *   Basso ($I < 0$): $I_C < 0$ (da E a C) oppure $I_D < 0$ (da S a D).

Vediamo i due componenti reali:

### 1. BJT NPN (Single-Quadrant Switch)
*   **Spento ($I_B = 0$):** 
    Applichi $V_{CE} > 0$ (Collettore positivo rispetto all'Emettitore). La giunzione Base-Collettore (PN) è polarizzata inversamente. La regione di svuotamento regge la tensione (es. 100V). **Regge $V_{CE} > 0$.**
    Applichi $V_{CE} < 0$. Cosa succede fisicamente? L'Emettitore (N) si trova a potenziale maggiore del Collettore (N). Se la Base è a 0V, mandi in breakdown (effetto valanga/Zener) la giunzione Base-Emettitore (che nei BJT regge solo 5-7V). Il dispositivo va in conduzione inversa incontrollata o si fonde. **NON sa bloccare $V_{CE} < 0$.**
*   **Acceso (Saturazione, $I_B \gg 0$):**
    Inietti portatori minoritari dall'Emettitore. La fisica del BJT è asimmetrica (il drogaggio dell'Emettitore è immensamente superiore a quello del Collettore). La corrente $I_C$ può fluire solo dal Collettore all'Emettitore ($I_C > 0$). **Non può sostenere correnti $I_C < 0$**.
*   **Conclusione BJT:** Quadrante in alto a destra. Stop.

### 2. Power MOSFET N-Channel (Current-Bidirectional)
*   **Spento ($V_{GS} = 0V$):**
    Applichi $V_{DS} > 0$. La giunzione tra il Body (P) e la regione di Drift del Drain (N-) è inversamente polarizzata. Sostiene tranquillamente anche 600V. **Regge $V_{DS} > 0$.**
    Applichi $V_{DS} < 0$ (Source più alto del Drain). Qui entra in gioco la manifattura: nei MOSFET di potenza, per evitare il latch-up del BJT parassita interno, i costruttori **cortocircuitano fisicamente** il terminale di Source con il substrato Body (P). 
    Risultato? Hai una giunzione PN gigante che va dal Source (Body P) al Drain (Regione N-). Questo è il famoso **Body Diode**. Se applichi $V_{DS} < 0$, questo diodo è polarizzato direttamente. Si accende a 0.7V e conduce migliaia di ampere. **Il MOSFET NON PUÒ, fisicamente, bloccare $V_{DS} < 0$.**
*   **Acceso ($V_{GS} = 10V$):**
    Sotto l'ossido del Gate si forma il Canale di Inversione (canale N). 
    Questo canale è letteralmente un pezzo di semiconduttore drogato che si comporta come un puro resistore ($R_{DS(on)}$). Non è una giunzione PN, è un ponte. 
    Se $V_{DS} > 0$, la corrente $I_D$ scorre da Drain a Source.
    Se $V_{DS} < 0$, la corrente $I_D$ **scorre all'indietro** (da Source a Drain) *attraverso il canale*, saltando il Body Diode perché il canale ha una resistenza molto più bassa. 
    **Sostiene correnti sia $I_D > 0$ che $I_D < 0$.**
*   **Conclusione MOSFET:** Asse X bloccato solo a destra. Asse Y conduttivo sia in alto che in basso. Ovvero, il secondo grafico della Slide 15: Bidirezionale in Corrente.
