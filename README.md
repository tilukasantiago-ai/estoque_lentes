# Estoque Express · Lentes & Blocos

Sistema próprio de estoque de lentes e blocos. Fica hospedado no **GitHub Pages**, e os dados e o login ficam no **Firebase**.

Antes de configurar qualquer coisa, dá para abrir o `index.html` direto no navegador: ele roda em **modo demonstração**, com os dados salvos só naquele navegador. Em **Ajustes**, clique em **Carregar exemplo** para testar com lentes fictícias.

---

## Passo 1: Criar o projeto no Firebase (≈10 min)

1. Acesse https://console.firebase.google.com e clique em **Adicionar projeto**. Sugestão de nome: `estoque-lentes`. O Google Analytics pode ficar desligado.
2. **Login**: no menu, vá em **Authentication → Vamos começar → E-mail/senha**, ative e salve.
   - Depois, na aba **Users**, clique em **Adicionar usuário** e cadastre o seu e-mail e uma senha forte.
3. **Banco de dados**: no menu, vá em **Firestore Database → Criar banco de dados**.
   - Local: `southamerica-east1 (São Paulo)`.
   - Modo: **produção**.
4. **Regras de segurança**: em Firestore, abra a aba **Regras**, apague o conteúdo e cole o arquivo `firestore.rules`.
   - **Troque `SEU-EMAIL@gmail.com` pelo e-mail do passo 2** e clique em **Publicar**.
5. **Chaves do app**: em ⚙️ **Configurações do projeto → Seus apps**, clique no ícone **`</>`** (Web). Dê um nome e registre.
   - Copie o bloco `firebaseConfig` que aparece.
6. Abra o `index.html` num editor (pode ser o próprio GitHub) e cole os valores no bloco `FIREBASE_CONFIG`, logo no início do `<script>`:

```js
const FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "estoque-lentes.firebaseapp.com",
  projectId: "estoque-lentes",
  storageBucket: "estoque-lentes.appspot.com",
  messagingSenderId: "123...",
  appId: "1:123...:web:abc..."
};
```

> Essas chaves **não são senhas**. Elas podem ficar públicas no GitHub sem problema. Quem protege os dados são o login e as regras do passo 4.

## Passo 2: Publicar no GitHub Pages (≈5 min)

1. Crie um repositório novo em https://github.com/new, por exemplo `estoque-lentes`. Ele pode ser **privado** se o seu plano permitir Pages privado; caso contrário, deixe público. Os dados continuam protegidos pelo login.
2. Clique em **Add file → Upload files** e envie o `index.html` (e, se quiser, este README).
3. Vá em **Settings → Pages → Branch: `main` / `(root)` → Save**.
4. Em 1 ou 2 minutos o site fica no ar em `https://SEU-USUARIO.github.io/estoque-lentes/`.
5. **Importante**: volte ao Firebase em **Authentication → Configurações → Domínios autorizados** e adicione `SEU-USUARIO.github.io`.

Quer usar um domínio próprio (ex.: `estoque.suaotica.com.br`)? Em **Settings → Pages → Custom domain**, informe o domínio e crie o registro CNAME no seu provedor. Depois, adicione esse domínio também nos **Domínios autorizados** do Firebase.

## Passo 3: Trazer os dados do MarketUP

1. No MarketUP, exporte o cadastro de produtos com estoque em Excel ou CSV.
2. No sistema, vá em **Ajustes → Importar planilha** e escolha o arquivo.
3. Confira qual coluna é o quê. Se o nome e o grau estiverem juntos numa coluna só, use **"Descrição completa"**: o sistema separa sozinho, e a prévia mostra o resultado.
4. Confira a prévia e clique em **Importar**. São cerca de 3.200 itens, que levam poucos segundos.
5. Em **Ajustes → Linhas de produto**, confira o status de cada linha (Ativo / Inativo / Desativo) e os preços. Dá para mudar a linha inteira de uma vez.

---

## Como funciona

| Tela | Para quê |
|---|---|
| **Painel** | Resumo do estoque, saídas do mês por ótica, itens para repor e alerta de backup |
| **Estoque** | Lista completa com busca e filtros (tipo, status, zerados, abaixo do mínimo). Clique num item para editar |
| **Entrada** | Entrada em massa: escolha a linha e digite as quantidades em cada grau (Enter pula para o próximo), ou cole uma lista do Excel |
| **Saída** | OS, produto, grau/base, ótica, quantidade e data. Os preços de custo e venda entram sozinhos. O botão ↺ desfaz uma saída |
| **Perdas** | Quebras e avarias com motivo, gráfico por motivo e prejuízo do mês |
| **Relatórios** | Planilha mensal em Excel, posição do estoque, lista de compra, folha de saídas em papel e folha de contagem |
| **Ajustes** | Importar do MarketUP, linhas de produto, regras de status, óticas, backup, restaurar backup |

**Status:**
- **Ativo**: compra para repor. Aparece na lista de compra quando fica abaixo do mínimo.
- **Inativo**: vende, mas não repõe.
- **Desativo**: não se encontra para comprar.

**Mês no topo**: as setas ‹ › trocam o mês das listas de saídas, entradas e perdas, do painel e da planilha mensal.

**Nada some**: estornar uma saída, entrada ou perda não apaga o registro. Ele fica riscado no histórico e a quantidade volta ao estoque.

## Custos do Firebase (plano gratuito Spark)

O plano gratuito permite 50 mil leituras e 20 mil gravações por dia. Cada vez que o sistema é aberto, ele lê os cerca de 3.200 itens, mas o cache do navegador reduz bastante isso nas aberturas seguintes. Para um único administrador, sobra com folga.

## Backup

Faça um backup em **Ajustes → Baixar backup** (ou pelo alerta do Painel) pelo menos 1 vez por semana e guarde o arquivo `.json` no Google Drive. Para voltar um backup, use **Restaurar backup…**.
