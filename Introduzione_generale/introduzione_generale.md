L'elettronica di potenza, a differenza di quella tradizionale, si occupa di adattare l'energia offerta da una sorgente alle esigenze di un carico, attraverso, per esempio, **convertitori**.

Esistono tanti ordini di grandezza per le potenze, dai micro ai giga watt: dipende tutto da quello che si deve fare.

In un telefono per esempio, alcuni componenti richiedono 3.3V, altri 1V ecc...se livellassimo le tensioni con dei semplici partitori resistivi, consumeremmo tantissima energia per niente. Per questo motivo si realizzato degli appositi circuiti che hanno efficienze alte.

Un'alimentatore passa dalla 220V a 15V $\rightarrow$ se l'efficienza è dell'80% al posto del 95%, un casino per una grande azienda.

L'inverter è un convertitore DC-AC PWM a frequenza variabile, non è l'elemento che raffredda nei condizionatori. I vecchi condizionatori avevano il motore attaccato direttamente alla 220V di casa a 50Hz che appunto gira ad una velocità fissa. Quindi o il condizionatore spara freddo a manetta, oppure 0, quindi ON/OFF.

Nel condizionatore con inverter:

- C'è un convertitore AC-DC che converte la 220V in DC per l'inverter.
- Questa DC va all'inverter che la converte di nuovo in AC ma decide lui la frequenza. Se gira a 20Hz, il motore gira piano, se gira a 60Hz va più forte. Ma non è che genera una sinusoide vera, ce lui faun'onda quadra con duty cycle che "varia con la sinusoide" cioè tipo "scriviamo" la forma della sinusoide" nella quadra poi si filtra e quindi esce una sinusoide.