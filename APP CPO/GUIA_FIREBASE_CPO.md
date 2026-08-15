# Guia de instalação — Escala CPO (Firebase)

## Ficheiros

| Ficheiro | Função |
|---|---|
| `escala-cpo.html` | A aplicação |
| `manifest-cpo.json` | Torna-a instalável (PWA) |
| `sw-cpo.js` | Funcionamento offline |
| `icon-*.png`, `apple-touch-icon.png` | Ícones |

---

## 1. Criar o projeto Firebase

1. Vai a **https://console.firebase.google.com** e clica **"Add project"**.
2. Dá um nome ao projeto (ex: `escala-cpo`). Podes desativar o Google Analytics se não precisares.
3. No menu lateral, clica em **Build → Authentication**:
   - Clica **"Get started"**
   - Em **Sign-in method**, ativa **Email/Password**
   - Guarda
4. No menu lateral, clica em **Build → Firestore Database**:
   - Clica **"Create database"**
   - Escolhe **"Start in production mode"** (mais seguro)
   - Escolhe a região mais próxima (ex: `europe-west1`)
5. Ainda no Firestore, vai ao separador **Rules** e substitui o conteúdo por:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /app_state/{doc} {
      allow read, write: if request.auth != null && request.auth.token.email_verified == true;
    }
  }
}
```

Publica as regras.

---

## 2. Obter as credenciais

1. No menu lateral, clica na **roda dentada → Project settings**.
2. Em **"Your apps"**, clica em **"</> Web"** (adicionar app web).
3. Dá um nome (ex: `escala-cpo-web`) e clica **"Register app"**.
4. Copia o objeto `firebaseConfig` que aparece — tem este aspeto:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "escala-cpo.firebaseapp.com",
  projectId: "escala-cpo",
  storageBucket: "escala-cpo.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abc123def456"
};
```

---

## 3. Configurar a app

Abre `escala-cpo.html` num editor de texto e preenche o bloco no topo do script:

```js
const firebaseConfig = {
  apiKey:            "AIzaSy...",     // <-- colar aqui
  authDomain:        "...",
  projectId:         "...",
  storageBucket:     "...",
  messagingSenderId: "...",
  appId:             "..."
};

const ADMIN_EMAILS = ["o-teu@email.pt"];  // <-- o teu email
```

---

## 4. Colocar online (Netlify Drop)

1. Vai a **https://app.netlify.com/drop**
2. Arrasta a pasta com todos os ficheiros
3. Recebes um link `https://nome-aleatorio.netlify.app` — é esse que partilhas

> A PWA só funciona por `https://` — não por duplo-clique no ficheiro.

---

## 5. Criar a tua conta de administrador

1. Na app online, clica em **"Administrador → Gestão de acessos"**
2. Preenche o teu nome + email (o mesmo que puseste em `ADMIN_EMAILS`) + palavra-passe
3. Clica **"Criar conta"** — recebes um email de confirmação
4. Confirma o email e entra com as tuas credenciais
5. O separador "Administrador" aparece só para ti

---

## 6. Criar contas para os operacionais

No painel Administrador → Gestão de acessos:
- Preenche nome + email + palavra-passe inicial
- Clica "Criar conta"
- O operacional recebe email de confirmação; só depois de confirmar consegue entrar

---

## 7. Instalar no telemóvel

Com o link aberto no telemóvel:
- **Android (Chrome):** barra "Instalar como app" ou menu ⋮ → "Adicionar ao ecrã principal"
- **iPhone (Safari):** botão Partilhar → "Adicionar ao ecrã principal"
