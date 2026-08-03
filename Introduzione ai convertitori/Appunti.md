Elettronica di potenza = gestire, adattare e convertire energia elettrica per le esigenze di un carico specifico. Se faccio partitore resistivo o con un semplice transistore lineare (almeno cosi posso controllare la corrente di base x regolare la corrente che sputo fuori) consumo un botto per niente perchè dissipano entrambi le soluzioni in calore.

Quindi per fare convertitori dobbiamo usare componenti che **non** dissipano: induttori, condensatori e transistor però usati come interruttori! Cioè CA/CC.

Comunque l'obiettivo è massimizzare l'efficienza.

Qual è l'idea dietro a tutti i convertitori? 100 anni si sono accorti che con una semplice bobina possiamo creare tensioni enormi. Quindi perchè non sfruttare questa cosa per decidere noi di quanto alzare? Ed ecco come è nato il primo convertitore Boost DC-DC.

Se ho un circuito semplicissimo con generatore di tensione e un interruttore che ha due collegamenti: o a massa o ad un carico, sostanzialmente il carico vedrà un'onda quadra 10V-0V-10V-10V: in base a quanto è il duty cycle, decido quant'è la media!!! Perchè è importante la media? Perchè se filtro tutta quella schiffezza in modo da tagliare via le armoniche della quadra...boom..ho una tensione continua in uscita che è solo una DC ma non a 10V...ma riscalata!! giusto? E non sto consumando niente! Un filtro L-C ideale non consuma energia, non dissipa calore! Ho riscalato la tensione senza sprecare un watt in calore!

Filtro passa-basso LC = del secondo ordine con solo due componenti! Non come quelli RC di elettronica di base, in elettronica di potenza non posso usare resistenza, l'efficienza conta tantissimo qui.

Se la tensione della batteria cala o il carico R cambia, serve un microcontrollare che modifichi in tempo reale il duty cycle. Nel caso del carico, cosa cambia se cambio R? Il guadagno DC! In realtà non è 1 ma c'è un partitore di resistenze tra la resistenza parassita dell'induttore e la resistenza di carico (in DC l'induttore è un CC).

Nel caso di un convertitore Boost DC-DC, si applica il Volt-Second-Balance: la condizione di equilibrio si ha quando la variazione di corrente nella prima parte del ciclo = alla variazione di corrente nel secondo ciclo. Ovviamente ci sono tantissime ipotesi tipo che la tensione sul condensatore varia pochissimo e che il carico comunque fa in modo che mentre sono nella prima parte del ciclo svuota il condensatore di quel che basta per mantenere sempre a quel valore di equilibrio Vo.

Il tiristore è un diodo con un terminale in più, quello di gate, di controllo. Anche se applico una tensione positiva, il tiristore rimane spento finchè non gli do un segnale di controllo di controllo di gate. Una volta accesso, però, perdo il controllo! Posso anche azzerare la corrente di gate, ma il tiristore resta acceso. Si spegne "da solo" quando la corrente del diodo scende sotto una soglia minima.

Quello che fa $i_g$ non è creare l'accensione, ma abbassare drasticamente la tensione necessaria per l'innesco. Infatti, rimane pur sempre un bel diodo! Si tratta di un diodo la cui tensione di innesco è programmabile elettricamente!