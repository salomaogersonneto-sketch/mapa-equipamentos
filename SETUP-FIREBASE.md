# Ativar o tempo real (Firebase) — passo a passo de ~20 minutos

Sem esta configuração o painel funciona em **modo demonstração**: tudo aparece, mas nada é salvo. Depois destes passos, qualquer alteração feita por qualquer pessoa aparece na tela de todo mundo em cerca de 1 segundo.

Custo: R$ 0,00 (plano gratuito Spark do Firebase, do Google). Para uma frota de obra, o limite gratuito (50 mil leituras/dia) não chega perto de estourar.

## Passo 1 — Criar o projeto

1. Acesse https://console.firebase.google.com e entre com uma conta Google (recomendo uma conta da obra, não pessoal).
2. Clique em **Criar um projeto** (ou "Adicionar projeto").
3. Nome: `mapa-equipamentos-tfpm` (ou outro). Pode desativar o Google Analytics quando perguntar. Conclua.

## Passo 2 — Criar o banco (Firestore)

1. No menu lateral: **Criação → Firestore Database → Criar banco de dados**.
2. Localização: `southamerica-east1 (São Paulo)`.
3. Modo: **produção** (vamos definir as regras no passo 4).

## Passo 3 — Ativar o login anônimo

Isso permite que a equipe use o painel sem criar conta nem senha.

1. Menu lateral: **Criação → Authentication → Vamos começar**.
2. Aba **Sign-in method** → **Anônimo** → Ativar → Salvar.

## Passo 4 — Regras de segurança

1. Volte em **Firestore Database → aba Regras**.
2. Apague o que estiver lá e cole exatamente isto:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /equipamentos/{doc} {
      allow read, write: if request.auth != null;
    }
  }
}
```

3. Clique em **Publicar**.

O que isso faz: só quem abriu o painel (e recebeu o login anônimo) lê e grava, e somente na coleção `equipamentos`. Ninguém acessa outra coisa no projeto.

## Passo 5 — Registrar o app e copiar a configuração

1. Engrenagem no topo do menu → **Configurações do projeto** → seção **Seus apps**.
2. Clique no ícone **`</>`** (Web). Apelido: `painel`. Não precisa marcar Hosting. **Registrar app**.
3. Vai aparecer um bloco `const firebaseConfig = { ... }`. Copie só o miolo (as linhas `apiKey`, `authDomain`, `projectId`, `storageBucket`, `messagingSenderId`, `appId`).

## Passo 6 — Colar no index.html

1. Abra o `index.html` e procure o bloco `FIREBASE_CONFIG` no início do script (tem um aviso "COLE AQUI").
2. Preencha os campos com os valores copiados. Exemplo de como fica:

```js
const FIREBASE_CONFIG = {
  apiKey: "AIzaSyD...xyz",
  authDomain: "mapa-equipamentos-tfpm.firebaseapp.com",
  projectId: "mapa-equipamentos-tfpm",
  storageBucket: "mapa-equipamentos-tfpm.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abc123"
};
```

3. Commit no GitHub. Pronto: ao abrir o link, o selo no topo muda de "Modo demonstração" para **"Tempo real ativo"** (bolinha verde).

Nota: esses valores não são senha. Podem ficar públicos no GitHub sem problema; quem protege o banco são as regras do passo 4.

## Passo 7 — Carga inicial da frota

Com o tempo real ativo, o banco começa vazio. Duas opções:

- Cadastrar pelo próprio painel (**+ Equipamento**); ou
- Clicar em **Importar** e selecionar o `equipamentos.json` (o de exemplo ou um que você montar com a frota real). Tudo sobe para a nuvem de uma vez.

## Problemas comuns

| Sintoma | Causa provável | Solução |
|---|---|---|
| Selo fica em "Modo demonstração" | `FIREBASE_CONFIG` vazio ou com erro de digitação | Confira vírgulas e aspas no bloco colado |
| Selo "Sem conexão" | Regras não publicadas ou login anônimo desativado | Refaça passos 3 e 4 |
| Erro `permission-denied` no console | Regras diferentes das do passo 4 | Cole as regras exatamente como acima |
| Alteração não aparece para outra pessoa | Ela está com o painel aberto de antes da configuração | Basta recarregar a página uma vez |

---

Marka Engenharia Ltda · Contrato Vale S.A. — TFPM · São Luís — MA
