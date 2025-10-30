# 🔑 Guia Completo - Integração Mercado Pago (PIX)

## ✅ Status Atual

O código do PIX **JÁ ESTÁ IMPLEMENTADO** no projeto! Só falta configurar as credenciais do Mercado Pago.

**Atualmente:**
- ⚠️ PIX está usando **dados MOCK** (código falso)
- ✅ Código real do Mercado Pago está pronto
- ✅ Geração de QR Code funciona
- ✅ Timer de 15 minutos implementado

**Depois de configurar:**
- ✅ PIX **REAL** do Mercado Pago
- ✅ Pagamentos processados automaticamente
- ✅ QR Code válido para qualquer banco

---

## 📋 Passo a Passo Para Obter as Credenciais

### 🔹 ETAPA 1: Criar Conta no Mercado Pago

1. Acesse: **https://www.mercadopago.com.br**
2. Clique em **"Criar conta"** ou **"Entrar"** (se já tiver)
3. Preencha seus dados pessoais
4. ✅ **É GRÁTIS!** Não precisa pagar nada

---

### 🔹 ETAPA 2: Acessar o Painel de Desenvolvedores

1. Depois de fazer login, acesse: **https://www.mercadopago.com.br/developers**
2. Ou vá em: **Seu nome → Suas integrações**
3. Clique em **"Suas integrações"** ou **"Criar aplicação"**

---

### 🔹 ETAPA 3: Criar uma Aplicação

1. Clique no botão **"+ Criar aplicação"** ou **"Nova aplicação"**
2. Preencha os dados:
   - **Nome da aplicação**: `Açaí Prime` (ou qualquer nome)
   - **Tipo de integração**: Escolha **"Pagamentos online"** ou **"Marketplace"**
   - **Modelo de negócio**: **"Receber pagamentos"**
3. Clique em **"Criar"**

---

### 🔹 ETAPA 4: Copiar as Credenciais

Após criar a aplicação, você verá duas abas importantes:

#### **A) Credenciais de TESTE** (Para testar antes de ir pro ar)

1. Clique na aba **"Credenciais de teste"**
2. Você verá:
   - **Public Key** (Chave Pública): `TEST-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`
   - **Access Token**: `TEST-xxxxxxxxxxxx-xxxxxxxxxxxx-xxxxxxxxxxxx-xxxxxxxxxxxx`

#### **B) Credenciais de PRODUÇÃO** (Para usar de verdade)

1. Clique na aba **"Credenciais de produção"**
2. Você verá:
   - **Public Key**: `APP_USR-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`
   - **Access Token**: `APP_USR-xxxxxxxxxxxx-xxxxxxxxxxxx-xxxxxxxxxxxx-xxxxxxxxxxxx`

**📝 IMPORTANTE**: Copie e guarde essas credenciais em um lugar seguro!

---

### 🔹 ETAPA 5: Configurar no Replit

#### **Para Ambiente de Desenvolvimento (aqui no Replit):**

1. No Replit, vá em **Tools → Secrets** (cadeado no menu lateral)
2. Clique em **"+ Add new secret"**

Adicione 2 secrets:

**Secret 1 - Access Token (Servidor):**
- **Key**: `MERCADO_PAGO_ACCESS_TOKEN`
- **Value**: Cole o **Access Token** que você copiou
  - **Teste**: `TEST-xxxxxxxxxxxx...`
  - **Produção**: `APP_USR-xxxxxxxxxxxx...`

**Secret 2 - Public Key (Frontend):**
- **Key**: `VITE_MERCADO_PAGO_PUBLIC_KEY`
- **Value**: Cole a **Public Key** que você copiou
  - **Teste**: `TEST-xxxxxxxx...`
  - **Produção**: `APP_USR-xxxxxxxx...`

3. Clique em **"Add secret"** para cada um

---

### 🔹 ETAPA 6: Reiniciar o Servidor

Depois de adicionar os secrets:

1. Vá em **Tools → Console** (ou terminal no Replit)
2. Reinicie o servidor para carregar as novas variáveis:
   - Clique no botão **Stop** e depois **Run**
   - Ou pressione `Ctrl + C` e digite `npm run dev` novamente

---

### 🔹 ETAPA 7: Testar o PIX!

1. Acesse seu site no Replit
2. Adicione produtos ao carrinho
3. Vá para o checkout
4. Preencha os dados
5. Escolha **"PIX"** como forma de pagamento
6. Clique em **"Finalizar Pedido"**

**✅ Se configurado corretamente, você verá:**
- QR Code **REAL** do Mercado Pago
- Código PIX **VÁLIDO** para copiar e colar
- Timer de 15 minutos

---

## 🧪 Como Testar Pagamentos (Modo Teste)

### Com Credenciais de TESTE:

1. Use as credenciais **TEST-xxxx** (não cobra de verdade)
2. Gere o QR Code PIX
3. O Mercado Pago vai criar um pagamento de teste
4. **NÃO será cobrado dinheiro real!**

### Para Testar PAGAMENTO REAL:

O Mercado Pago não permite simular pagamentos PIX em teste. Para testar de verdade:

1. Use as credenciais de **PRODUÇÃO** (`APP_USR-xxxx`)
2. Faça um pedido pequeno (ex: R$ 1,00)
3. Pague o PIX do próprio celular
4. **Será cobrado de verdade!** (mas você pode estornar depois)

---

## 🔄 Para Usar no Render (Produção)

Quando fizer deploy no Render, adicione as mesmas credenciais lá:

1. No **Dashboard do Render**, vá no seu serviço
2. Clique em **"Environment"** (menu lateral)
3. Adicione as variáveis:

```
MERCADO_PAGO_ACCESS_TOKEN = APP_USR-xxxxxxxxxxxx... (PRODUÇÃO)
VITE_MERCADO_PAGO_PUBLIC_KEY = APP_USR-xxxxxxxx... (PRODUÇÃO)
```

4. Clique em **"Save Changes"**
5. O Render vai reiniciar automaticamente

---

## 🛡️ Segurança - IMPORTANTE!

### ✅ O Que o Código Faz (Correto):

1. **Access Token** fica **APENAS no servidor** (backend)
2. **Public Key** fica no frontend (pode ser exposta)
3. Nenhum dado sensível é armazenado
4. Mercado Pago processa o pagamento de forma segura

### ⚠️ NUNCA Faça Isso:

- ❌ Não compartilhe o **Access Token** publicamente
- ❌ Não comite o token no GitHub (use secrets/variáveis de ambiente)
- ❌ Não exponha o token no código frontend

---

## 📊 Como Funciona o Fluxo de Pagamento PIX

1. **Cliente** faz o pedido e escolhe PIX
2. **Seu servidor** chama a API do Mercado Pago
3. **Mercado Pago** gera:
   - QR Code válido
   - Código PIX (copia e cola)
   - Link de pagamento
4. **Cliente** paga via app do banco
5. **Mercado Pago** confirma o pagamento automaticamente
6. ✅ Pagamento aprovado!

---

## 🎯 Checklist de Configuração

- ✅ Conta criada no Mercado Pago
- ✅ Aplicação criada no painel de desenvolvedores
- ✅ **Access Token** copiado
- ✅ **Public Key** copiada
- ✅ Secrets configurados no Replit:
  - `MERCADO_PAGO_ACCESS_TOKEN`
  - `VITE_MERCADO_PAGO_PUBLIC_KEY`
- ✅ Servidor reiniciado
- ✅ Teste realizado com sucesso

---

## 🆘 Problemas Comuns

### 1. "Mercado Pago not configured"
**Solução**: Verifique se os secrets foram adicionados corretamente e reinicie o servidor.

### 2. QR Code não aparece
**Solução**: Abra o console do navegador (F12) e veja se há erros. Verifique se a Public Key está correta.

### 3. Erro 401 (Unauthorized)
**Solução**: Access Token está incorreto ou expirado. Copie novamente do painel do Mercado Pago.

### 4. Erro 400 (Bad Request)
**Solução**: Verifique se o email do cliente está válido e se o valor não é zero.

### 5. PIX expira rápido demais
**Isso é normal!** O Mercado Pago gera PIX com validade de 30 minutos por padrão.

---

## 💰 Taxas do Mercado Pago

### PIX:
- **Taxa**: ~1% por transação
- **Recebimento**: Instantâneo (cai na hora!)
- **Saque**: Grátis para conta bancária

### Quando Cai o Dinheiro?
- **PIX**: Cai **imediatamente** na sua conta Mercado Pago
- Você pode transferir para o banco quando quiser (grátis)

---

## 🎉 Pronto!

Agora você tem:
- ✅ **PIX real** funcionando
- ✅ **QR Code válido** do Mercado Pago
- ✅ **Pagamentos processados** automaticamente
- ✅ **Integração 100% segura**

**Próximo passo:** Testar com um pedido real de R$ 1,00! 💜

---

## 📚 Links Úteis

- **Painel Mercado Pago**: https://www.mercadopago.com.br/developers
- **Documentação PIX**: https://www.mercadopago.com.br/developers/pt/docs/checkout-api/integration-configuration/integrate-with-pix
- **Suporte**: https://www.mercadopago.com.br/developers/pt/support

---

**Dúvidas?** Consulte este guia novamente ou acesse o suporte do Mercado Pago! 🚀
