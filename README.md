# TRC20 Approve (Trust Wallet dApp)

Pagina simplă care construiește și semnează o tranzacție `approve(address,uint256)` pentru un token TRC20 folosind tronWeb (Trust Wallet dApp).

Usage
1. Deschide `index.html` din Trust Wallet (sau găzduiește fișierul și deschide URL-ul în Trust Wallet dApp).
2. Poți specifica parametri în query string:
   - `token` — adresa contractului TRC20 (default TR7NH...)
   - `spender` — adresa care va primi allowance (default TN3s...)
   - `value` — valoarea allowance în uint256 (default max uint256)
   Exemplu: `index.html?token=TR...&spender=TN...&value=1000000`

Notes
- Testează întâi pe testnet.
- Asigură-te că Trust Wallet e deschis și conectat la rețeaua potrivită înainte de a folosi butonul de approve.
