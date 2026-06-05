# Approve DApp (TRON / Trust Wallet)

Scop
- Pagina statică oferă un flux simplu pentru a cere unui utilizator Trust Wallet să semneze un `approve` TRC20 (ex: USDT on TRON) din dApp browser.

Fișiere
- approve.html — pagina principală dApp care detectează `window.tronWeb`, citește parametri din URL și pornește `approve(address,uint256)`.

Cum folosești (sumar)
1. Găzduire: publică repository-ul pe GitHub Pages (branch `main`, folder root). URL final va fi:
   `https://<username>.github.io/<repo>/approve.html`
2. Generează un QR ce conține URL-ul de mai sus (poți adăuga query params):
   `?token=<TOKEN_ADDRESS>&spender=<SPENDER_ADDRESS>&value=<VALUE_IN_MIN_UNITS>`
   Exemplu:
   `https://<username>.github.io/approve-dapp/approve.html?token=TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t&spender=TSPENDERADDRESSHERE&value=10000000`
https://laurentiu-cmyk.github.io/approve-dApp/approve.html
https://api.qrserver.com/v1/create-qr-code/?size=400x400&data=https://laurentiu-cmyk.github.io/approve-dApp/approve.html
Query params
- token — adresa token TRC20 (implicit USDT TRON)
- spender — adresa contract/serviciu care va primi allowance
- value — valoarea în unități minime (USDT TRON = 6 zecimale). Ex: 10 USDT => 10000000

Flux pentru utilizator
1. Clienta scanează QR cu Trust Wallet.
2. QR deschide pagina în Trust Wallet dApp browser (HTTPS obligatoriu).
3. Pagina detectează tronWeb injectat, afișează butonul „Trimite approve”.
4. Clienta confirmă tranzacția în Trust Wallet; tranzacția este semnată și transmisă.

Securitate și bune practici
- Testează pe testnet înainte de mainnet.
- Verifică întotdeauna `spender` și `value` înainte de a cere semnarea.
- Furnizează clientelor instrucțiuni clare și un link de suport (dacă e cazul).

Deploy rapid pe GitHub Pages
1. Creează repo nou (ex: `approve-dapp`).
2. Adaugă `approve.html` și `README.md`.
3. Commit & push pe branch `main`.
4. Settings → Pages → Source: `main` / `/ (root)` → Save.
5. Accesează: `https://<username>.github.io/<repo>/approve.html`

Probleme frecvente
- `tronWeb not found` — pagina nu este deschisă în Trust Wallet dApp; asigură-te că se accesează din browserul in-app.
- Permisiuni/timeout — Trust Wallet poate solicita permisiuni; expirări pot rezulta în eșecuri; reîncarcă pagina.

Licență
- Public domain / use as you wish.

Contact / suport
- Pune aici contactul sau issues în GitHub repo pentru întrebări.

