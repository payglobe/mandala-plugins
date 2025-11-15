Mandala Gateway for WooCommerce è un gateway di pagamento sviluppato da PayGlobe / Mandala, progettato come drop-in replacement compatibile con le API Stripe.

Significa che WooCommerce continua a funzionare come se usasse Stripe, ma le transazioni vengono processate dal gateway Mandala, garantendo:

prestazioni elevate

commissioni ottimizzate

infrastruttura italiana / europea


nessuna modifica al flusso dell’e-commerce

------------------------------------------------------------------------------------------------------------------------------------------------------------


mandala-gateway.php (bootstrap del plugin)

includes/

class-wc-gateway-mandala.php (gateway WooCommerce completo)

class-wc-gateway-mandala-api.php (wrapper API Mandala)

class-wc-gateway-mandala-webhook-handler.php (webhook con X-PayGlobe-Signature)


assets/mandala-icon.svg

readme.md

uninstall.php

Come usarlo

Scarica lo zip.

Rinominalo se vuoi in woocommerce-mandala-gateway.zip.

In WordPress: Plugin → Aggiungi nuovo → Carica plugin.

Attiva Mandala Gateway for WooCommerce.

Vai su WooCommerce → Impostazioni → Pagamenti → Mandala e inserisci:

Mandala Secret Key (sk_...)

Webhook signing secret

opzionale: API Base URL per sandbox.

------------------------------------------------------------------------------------------------------------------------------------------------------------
