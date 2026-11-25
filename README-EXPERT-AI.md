# Attivare Expert AI

La pagina `expert-ai.html` prova prima un modello locale nel browser (WebLLM, Qwen2.5 0.5B). Questo percorso non richiede una chiave API o un backend, ma richiede WebGPU e scarica circa 945 MB di modello al primo utilizzo. Se il browser non supporta WebGPU, la pagina può usare il backend facoltativo `api/chat.js`.

## Modalità locale (senza backend)

Pubblica `expert-ai.html` e `ai-config.js` insieme al sito. I visitatori con un browser e un dispositivo compatibili con WebGPU potranno usare il modello locale. La prima apertura può richiedere un download grande e richiede una connessione internet; il modello viene poi memorizzato nella cache del browser. Non funziona su tutti i dispositivi e la qualità del modello leggero è inferiore a quella di un servizio AI più grande.

## Backend facoltativo, per i dispositivi senza WebGPU

1. Chiedi a un genitore o tutore di aiutarti con l'account API e con l'hosting. L'uso dell'API OpenAI è separato da ChatGPT e può comportare costi.
2. Collega il repository GitHub a un nuovo progetto Vercel e pubblica il progetto dalla cartella principale del repository. La cartella `api` contiene la funzione server.
3. Nelle impostazioni Vercel del progetto, apri **Settings → Environment Variables** e aggiungi:
   - `OPENAI_API_KEY`: la chiave API privata, copiata dal pannello API. Non inserirla mai nei file HTML, JavaScript o in GitHub.
   - `OPENAI_MODEL`: facoltativa; se non impostata, viene usato `gpt-6.2-astra`.
   - `ALLOWED_ORIGINS`: obbligatoria. Inserisci l'origine del sito (schema e dominio, senza percorso), per esempio `https://nomeutente.github.io` oppure `https://nome-progetto.vercel.app`.
4. Dopo aver salvato le variabili, fai un nuovo deploy da Vercel.
5. Se pubblichi tutto su Vercel, apri `/expert-ai.html` sul dominio Vercel: il fallback `/api/chat` funzionerà sullo stesso dominio se il modello locale non parte.

## Se vuoi tenere il sito su GitHub Pages

GitHub Pages ospita solo i file statici, quindi non può eseguire `api/chat.js`. In questo caso:

1. Pubblica comunque il progetto su Vercel per ottenere il backend.
2. In `ai-config.js`, inserisci l'indirizzo pubblico della funzione Vercel, per esempio `https://nome-progetto.vercel.app/api/chat`.
3. Nella variabile Vercel `ALLOWED_ORIGINS`, inserisci l'origine GitHub Pages del sito, per esempio `https://nomeutente.github.io` (senza il percorso del repository).
4. Carica il nuovo `ai-config.js` e `expert-ai.html` su GitHub Pages.

## Prima di condividerlo

- Configura limiti di spesa e controlla l'utilizzo dell'API. L'API ha una fatturazione separata da ChatGPT.
- La funzione limita le richieste a otto messaggi per chiamata e 2000 caratteri per messaggio. Prima di condividere il sito, attiva anche i controlli del traffico disponibili nel servizio di hosting: il controllo dell'origine non impedisce da solo tutte le chiamate automatizzate.
- La chat non salva la conversazione nel sito e invia le richieste con `store: false`; il fornitore AI riceve comunque il testo necessario per generare la risposta. Aggiungi una pagina privacy adeguata prima di invitare altre persone.
- Non chiedere agli utenti password o dati privati. Per un sito usato da ragazzi, mantieni risposte e protezioni adatte alla loro età.
