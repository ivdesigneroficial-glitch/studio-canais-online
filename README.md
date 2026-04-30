# Studio de Canais — Equipe Canais Darks (versão online sincronizada)

Versão online com **login restrito** + **dados sincronizados em tempo real** entre os 2 admins via Firebase.

## Logins da equipe

| Usuário  | Senha       |
|----------|-------------|
| admin1   | darks2026   |
| admin2   | canais2026  |

> Para trocar as senhas, edite o objeto `USERS` no `<script>` de login dentro de `index.html`.

---

## ⚠️ Antes do deploy: criar o Firebase (5 minutos, grátis)

Sem isso o sistema funciona, mas cada um terá dados separados. Com isso, qualquer alteração que um fizer aparece imediatamente para o outro.

### 1. Criar projeto Firebase
1. Vá em https://console.firebase.google.com/
2. Clique **Adicionar projeto** → nome livre (ex: `studio-canais`) → desabilite Analytics → **Criar**.

### 2. Criar o Realtime Database (não é Firestore!)
1. No menu da esquerda → **Build → Realtime Database**.
2. **Criar banco de dados** → escolha localização (ex: `us-central1`) → **Iniciar no modo de teste** → **Ativar**.
3. Vá na aba **Regras** e cole isso, depois **Publicar**:
   ```json
   {
     "rules": {
       ".read": true,
       ".write": true
     }
   }
   ```
   (Como o login é client-side e a URL do banco é "secreta", para 2 pessoas isso é suficiente. Se quiser mais segurança me avisa.)

### 3. Pegar as credenciais
1. Engrenagem ⚙️ no topo → **Configurações do projeto**.
2. Role até **Seus apps** → ícone **</>** (Web) → registre um app (qualquer nome) → **Registrar app**.
3. Vai aparecer um bloco `firebaseConfig = { apiKey: "...", ... }`. **Copie esse bloco inteiro.**

### 4. Colar no `index.html`
Abra o `index.html` no Bloco de Notas, procure por `window.FB_CONFIG = {` e substitua pelo que você copiou. Deve ficar algo como:
```js
window.FB_CONFIG = {
  apiKey: "AIzaSy...",
  authDomain: "studio-canais.firebaseapp.com",
  databaseURL: "https://studio-canais-default-rtdb.firebaseio.com",
  projectId: "studio-canais",
  storageBucket: "studio-canais.appspot.com",
  messagingSenderId: "1234567890",
  appId: "1:1234567890:web:abc..."
};
```
> Se o `databaseURL` não aparecer no config, pegue na aba **Realtime Database** (URL no topo da página).

### 5. Salvar.

---

## Deploy no ar (Vercel + GitHub)

### A) Criar repositório no GitHub
1. https://github.com/new
2. Nome: `studio-canais-online` (ou outro)
3. **Private** (recomendado)
4. **Create repository**

### B) Subir os arquivos
1. No repositório → **uploading an existing file**
2. Arraste TODOS os arquivos da pasta `Studio-Canais-Online`: `index.html`, `vercel.json`, `StudioCanais.ico`, `README.md`
3. **Commit changes**

### C) Deploy no Vercel
1. https://vercel.com/new → login com GitHub
2. **Import** no repo `studio-canais-online`
3. Deixe tudo como está → **Deploy**
4. Em ~30s o link estará no ar (ex: `https://studio-canais-online.vercel.app`)

### D) Compartilhar
- Mande o link do Vercel para o parceiro
- Mande o login dele (`admin2` / `canais2026`) por canal seguro
- Pronto: os 2 logam, os 2 enxergam e editam o mesmo conjunto de dados, em tempo real

---

## Como funciona a sincronização
- Cada vez que um de vocês cria/edita/apaga um canal, ideia, tema ou monetização, o estado é gravado no Firebase.
- O outro recebe a atualização em segundos e a tela se redesenha sozinha.
- Funciona offline também (os dados ficam no `localStorage` como cache) e sincronizam quando voltar a internet.

## Atualizar o código depois
Se quiser mudar algo (login, layout, etc.), edite o `index.html` localmente, faça upload no GitHub novamente — o Vercel re-publica sozinho.
