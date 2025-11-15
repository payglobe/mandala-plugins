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
Come funziona il flusso Hosted Redirect

Cliente sceglie Mandala Gateway al checkout.

Magento piazza l’ordine e chiama getOrderPlaceRedirectUrl()
→ redirect a /mandala/payment/start.

Start:

legge l’ultimo ordine (getLastRealOrder)

chiama Mandala /v1/payment_intents con:

amount (in centesimi)

currency

confirm = true

metadata[order_id] + metadata[magento_order_id]

return_url = https://.../mandala/payment/callback

prende next_action.redirect_to_url.url oppure hosted_url

fa redirect verso la pagina di pagamento Mandala.

Il cliente paga su Mandala (3DS, wallet, ecc.).

Mandala:

chiama il webhook su /mandala/webhook/index con:

header X-PayGlobe-Signature

JSON evento payment_intent.succeeded o payment_intent.payment_failed

il modulo verifica la firma HMAC (sha256(payload, webhook_secret)).

aggiorna l’ordine Magento a processing su successo.

Al termine del pagamento, Mandala richiama la return_url, ovvero
/mandala/payment/callback → che rimanda il cliente alla success page standard di Magento.

Config in admin

In Stores → Configuration → Sales → Payment Methods → Mandala Gateway trovi i campi:

Enable (payment/mandala_gateway/active)

Title (label mostrato al checkout)

Mandala Secret Key (sk_...) (payment/mandala_gateway/api_key)

Mandala API Base URL (payment/mandala_gateway/api_base, default https://api.mandala-gateway.com)

Webhook signing secret (payment/mandala_gateway/webhook_secret)

Debug log (payment/mandala_gateway/debug – per ora solo da usare lato PHP se aggiungi logging)

 Endpoint da configurare in Mandala

Hosted Redirect return URL:
https://TUO-DOMINIO/mandala/payment/callback

Webhook URL:
https://TUO-DOMINIO/mandala/webhook/index

Firma webhook:

Header: X-PayGlobe-Signature

Contenuto: HMAC SHA-256 del raw body con secret = webhook_secret configurato in Magento.

🧪 Installazione su Adobe Commerce

Copia la cartella Mandala/AdobeGateway dentro app/code/Mandala/AdobeGateway.

Da CLI:

bin/magento module:enable Mandala_AdobeGateway
bin/magento setup:upgrade
bin/magento cache:flush


Vai in admin e configura il metodo pagamento.
