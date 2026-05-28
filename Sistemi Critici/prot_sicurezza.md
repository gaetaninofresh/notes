# Specifica di protocolli di sicurezza

Nella specifica di protocolli le ASM consentono di modellare non solo le specifiche del protocollo ma anche dei threat actors. È accettato come standard un modello di MiM che può:
- Intercettare (ed eventualmente analizzare) ogni messaggio che sia stato spedito da un agente
- Formare (sintetizzare) nuovi messaggi utilizzando la sua base di conoscenze
- Cambiare destinatario di un messaggio

Analogamente la crittografia ha delle proprietà standard:
- Un messaggio può essere decriptato solo se si ha la chiave giusta
- Un agente può generare sempre nonces nuove
- Un agente autenticato è non compromesso se la sua chiave privata non è nota alla spia


