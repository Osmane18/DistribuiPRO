# 🔥 Como Configurar o Firebase — DistribuiPRO
## Guia Completo Passo a Passo

---

## O que é o Firebase?

Firebase é um serviço gratuito do Google que salva os dados do sistema na nuvem.
Com ele, qualquer usuário que fizer login verá seus próprios dados, de qualquer computador.

**Plano gratuito (Spark)** é suficiente para uso normal da distribuidora.

---

## ✅ PASSO 1 — Criar conta no Firebase

1. Acesse **https://console.firebase.google.com**
2. Clique em **"Fazer login"** (use sua conta Google/Gmail)
3. Você verá o painel do Firebase

---

## ✅ PASSO 2 — Criar um novo projeto

1. Clique em **"Adicionar projeto"** (ou "Create a project")
2. **Nome do projeto:** Digite `distribuipro` (ou qualquer nome)
3. Clique em **"Continuar"**
4. Na tela do Google Analytics: pode **desativar** e clicar em **"Criar projeto"**
5. Aguarde alguns segundos até aparecer "Seu projeto está pronto"
6. Clique em **"Continuar"**

---

## ✅ PASSO 3 — Registrar o aplicativo Web

1. Na tela inicial do projeto, clique no ícone **`</>`** (Web)
2. **Apelido do app:** Digite `DistribuiPRO`
3. **NÃO** marque "Firebase Hosting"
4. Clique em **"Registrar app"**
5. Vai aparecer um bloco de código parecido com este:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "distribuipro-xxxxx.firebaseapp.com",
  projectId: "distribuipro-xxxxx",
  storageBucket: "distribuipro-xxxxx.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:xxxxxxxxxxxxxx"
};
```

6. **COPIE TODOS ESSES DADOS** — você vai precisar deles no Passo 6
7. Clique em **"Continuar no console"**

---

## ✅ PASSO 4 — Ativar o sistema de Login (Authentication)

1. No menu lateral esquerdo, clique em **"Authentication"** (ou "Build > Authentication")
2. Clique em **"Começar"** / **"Get started"**
3. Clique em **"E-mail/senha"** (Email/Password)
4. Clique no botão para **ativar** (ficará azul)
5. Clique em **"Salvar"**

✅ Agora o sistema pode criar contas e fazer login!

---

## ✅ PASSO 5 — Criar o banco de dados (Firestore)

1. No menu lateral, clique em **"Firestore Database"** (ou "Build > Firestore Database")
2. Clique em **"Criar banco de dados"** / **"Create database"**
3. Selecione **"Iniciar no modo de produção"** e clique em **"Avançar"**
4. Na localização, selecione **`southamerica-east1`** (São Paulo) — mais rápido para o Brasil
5. Clique em **"Ativar"** / **"Enable"**
6. Aguarde a criação (pode demorar 1-2 minutos)

---

## ✅ PASSO 5B — Configurar as Regras de Segurança do Firestore

As regras definem quem pode ler e gravar dados.

1. No Firestore, clique na aba **"Regras"** / **"Rules"**
2. **Apague tudo** que está lá e **cole estas regras:**

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // Usuário só acessa seus próprios dados
    match /usuarios/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;

      match /{subcollection}/{docId} {
        allow read, write: if request.auth != null && request.auth.uid == userId;
      }
    }
  }
}
```

3. Clique em **"Publicar"** / **"Publish"**

✅ Agora cada usuário só vê os próprios clientes, produtos e pedidos!

---

## ✅ PASSO 6 — Configurar o arquivo firebase-config.js

1. Abra o arquivo **`firebase-config.js`** com o Bloco de Notas ou qualquer editor
2. Você verá isso:

```javascript
export const firebaseConfig = {
  apiKey:            "COLE_AQUI_SUA_API_KEY",
  authDomain:        "COLE_AQUI.firebaseapp.com",
  ...
};
```

3. **Substitua cada valor** com as informações que você copiou no Passo 3:

```javascript
export const firebaseConfig = {
  apiKey:            "AIzaSyXXXXXXXXXXXX",       // ← cole o seu
  authDomain:        "distribuipro-xxxxx.firebaseapp.com",
  projectId:         "distribuipro-xxxxx",
  storageBucket:     "distribuipro-xxxxx.appspot.com",
  messagingSenderId: "123456789012",
  appId:             "1:123456789012:web:xxxxxx"
};
```

4. **Salve o arquivo**

---

## ✅ PASSO 7 — Rodar o sistema no navegador

O sistema usa módulos ES6, então **não pode ser aberto direto pelo arquivo** (duplo clique).
É necessário usar um servidor local. Opções simples:

### Opção A — VS Code + Live Server (recomendado)
1. Baixe o **VS Code**: https://code.visualstudio.com
2. Instale a extensão **"Live Server"** (busque nas extensões)
3. Abra a pasta do projeto no VS Code
4. Clique com botão direito no `index.html` → **"Open with Live Server"**
5. O navegador abrirá em `http://localhost:5500`

### Opção B — Node.js http-server
1. Instale o Node.js: https://nodejs.org
2. Abra o terminal na pasta do projeto
3. Execute: `npx http-server -p 8080`
4. Acesse `http://localhost:8080`

### Opção C — Python (se tiver instalado)
1. Abra o terminal na pasta do projeto
2. Execute: `python -m http.server 8080`
3. Acesse `http://localhost:8080`

---

## 📂 Estrutura de arquivos

```
distribuipro/
├── index.html              → Tela de login e cadastro
├── app.html                → Sistema principal (pedidos, clientes, produtos)
├── style.css               → Visual do sistema
├── firebase-config.js      → ← VOCÊ PREENCHE ESTE ARQUIVO
└── COMO-CONFIGURAR-FIREBASE.md  → Este guia
```

---

## ❓ Problemas comuns

| Problema | Solução |
|----------|---------|
| "Permissão negada" ao salvar | Verifique as regras do Firestore (Passo 5B) |
| "auth/configuration-not-found" | Ative o método Email/Senha no Authentication (Passo 4) |
| Tela em branco / erro JS | Confira se o `firebase-config.js` está preenchido corretamente |
| Não abre pelo duplo clique | Use o Live Server ou outro servidor local (Passo 7) |
| "Module not found" | Certifique-se de estar acessando via `http://localhost` e não `file://` |

---

## 🆓 Limites do plano gratuito Firebase

| Recurso | Limite gratuito |
|---------|----------------|
| Leituras/dia | 50.000 |
| Gravações/dia | 20.000 |
| Armazenamento | 1 GB |
| Usuários | Ilimitado |

Para uma distribuidora de pequeno/médio porte, o plano gratuito é mais que suficiente.

---

## 🎉 Pronto!

Após completar todos os passos:
1. Abra `http://localhost:PORTA` no navegador
2. Clique em **"Criar nova conta"**
3. Cadastre seu e-mail e senha
4. Comece a usar o sistema!

---

*DistribuiPRO — Sistema de Pedidos para Distribuidoras*
