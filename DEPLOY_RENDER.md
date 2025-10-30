# 🚀 Como Fazer Deploy no Render (SEM CARTÃO DE CRÉDITO!)

## ✨ Por Que Render?

- ✅ **100% GRÁTIS** - Sem precisar de cartão!
- ✅ **Seu domínio da Hostinger funciona!**
- ✅ **SSL automático** (https grátis)
- ✅ **Deploy em 5 minutos**
- ✅ **Conecta com GitHub** (atualização automática)

---

## 📋 Passo a Passo COMPLETO

### 🔹 ETAPA 1: Criar Conta no Render

1. Acesse: **https://render.com**
2. Clique em **"Get Started"** ou **"Sign Up"**
3. Escolha **"Sign up with GitHub"** (recomendado) OU use seu email
4. ✅ **NÃO vai pedir cartão de crédito!**

---

### 🔹 ETAPA 2: Colocar Seu Projeto no GitHub

#### Se você NÃO tem o projeto no GitHub ainda:

1. Acesse: **https://github.com**
2. Faça login (ou crie conta)
3. Clique no **"+"** no canto superior direito
4. Escolha **"New repository"**
5. Preencha:
   - **Nome**: `acai-prime` (ou o que quiser)
   - **Privacidade**: Pode ser **Private** (privado)
   - ❌ **NÃO** marque "Add README"
6. Clique em **"Create repository"**

#### Enviar o código para o GitHub:

**No terminal do Replit**, execute esses comandos:

```bash
# Inicializar Git (se ainda não tiver)
git init

# Adicionar todos os arquivos
git add .

# Fazer o primeiro commit
git commit -m "Projeto Açaí Prime pronto para deploy"

# Conectar ao GitHub (SUBSTITUA 'seu-usuario' pelo seu usuário do GitHub)
git remote add origin https://github.com/seu-usuario/acai-prime.git

# Enviar para o GitHub
git branch -M main
git push -u origin main
```

**⚠️ IMPORTANTE**: Substitua `seu-usuario` pelo seu nome de usuário do GitHub!

---

### 🔹 ETAPA 3: Criar o Web Service no Render

1. Volte para o **Render** (https://dashboard.render.com)
2. Clique no botão azul **"New +"** (canto superior direito)
3. Escolha **"Web Service"**
4. Conecte seu GitHub:
   - Clique em **"Connect GitHub"** (se ainda não conectou)
   - Autorize o Render a acessar seus repositórios
5. Encontre e selecione o repositório **"acai-prime"**
6. Clique em **"Connect"**

#### Configurar o Web Service:

Preencha os campos assim:

- **Name**: `acai-prime` (ou qualquer nome único)
- **Region**: Escolha **"Oregon (US West)"** ou **"Frankfurt (Europe)"**
- **Branch**: `main`
- **Runtime**: **Node**
- **Build Command**: `npm install && npm run build`
- **Start Command**: `npm start`
- **Instance Type**: Escolha **"Free"** ✅

#### Scroll para baixo e clique em **"Advanced"**:

Adicione as **Environment Variables** (Variáveis de Ambiente):

| Key | Value |
|-----|-------|
| `NODE_ENV` | `production` |
| `DATABASE_URL` | `postgresql://usuario:senha@host-hostinger:5432/banco?sslmode=require` |
| `SESSION_SECRET` | `qualquer-texto-secreto-aleatorio-forte-123456` |

**⚠️ SUBSTITUA** os valores do `DATABASE_URL` com as credenciais do seu banco da Hostinger:
- `usuario` → Seu usuário PostgreSQL
- `senha` → Sua senha PostgreSQL
- `host-hostinger` → Host do banco (ex: `abc123.hostinger.com`)
- `banco` → Nome do seu banco de dados

**💡 Dica**: Peça as credenciais do banco PostgreSQL no painel da Hostinger em **Banco de Dados → PostgreSQL**

---

### 🔹 ETAPA 4: Fazer o Deploy!

1. Clique no botão azul **"Create Web Service"** no final da página
2. ⏳ Aguarde o build (vai aparecer os logs em tempo real)
3. ✅ Quando aparecer **"Live"** com bolinha verde = Está no ar!
4. Copie a URL que aparece (algo como `https://acai-prime.onrender.com`)

---

### 🔹 ETAPA 5: Migrar o Banco de Dados

Agora que está no ar, precisamos criar as tabelas no banco da Hostinger:

#### Opção A: Rodar Localmente (Recomendado)

No terminal do **Replit**, configure temporariamente o DATABASE_URL:

```bash
# Cole a mesma URL que você usou no Render
export DATABASE_URL="postgresql://usuario:senha@host-hostinger:5432/banco?sslmode=require"

# Criar as tabelas
npm run db:push
```

#### Opção B: Via Shell do Render

1. No Dashboard do Render, clique no seu serviço **acai-prime**
2. Vá em **"Shell"** no menu lateral
3. Digite: `npm run db:push`
4. Pressione Enter

---

### 🔹 ETAPA 6: Popular o Banco com Dados

Execute esses comandos para adicionar produtos, admin e complementos:

```bash
# Substitua pela sua URL do Render
curl -X POST https://acai-prime.onrender.com/api/seed-products
curl -X POST https://acai-prime.onrender.com/api/seed-admin
curl -X POST https://acai-prime.onrender.com/api/seed-toppings
```

**✅ Pronto!** Seu site já está funcionando!

---

## 🌐 ETAPA 7: Conectar Seu Domínio da Hostinger

Agora vamos fazer seu domínio apontar para o Render!

### No Render:

1. No Dashboard do Render, clique no seu serviço **acai-prime**
2. Vá em **"Settings"** (menu lateral)
3. Procure a seção **"Custom Domains"**
4. Clique em **"Add Custom Domain"**
5. Digite seu domínio: `seudominio.com`
6. Clique em **"Verify"**

O Render vai te mostrar um registro DNS para adicionar. Algo como:

```
Type: CNAME
Name: @
Value: acai-prime.onrender.com
```

### Na Hostinger:

1. Acesse o painel da **Hostinger**
2. Vá em **Domínios → Gerenciar**
3. Clique em **"Zona DNS"**
4. **Delete** o registro A existente do `@` (se tiver)
5. Clique em **"Adicionar Registro"**
6. Preencha:
   - **Tipo**: `CNAME`
   - **Nome**: `@`
   - **Aponta para**: `acai-prime.onrender.com`
   - **TTL**: `3600` (1 hora)
7. Clique em **"Adicionar"**

#### Para o subdomínio WWW:

8. Clique novamente em **"Adicionar Registro"**
9. Preencha:
   - **Tipo**: `CNAME`
   - **Nome**: `www`
   - **Aponta para**: `acai-prime.onrender.com`
   - **TTL**: `3600`
10. Clique em **"Adicionar"**

### De Volta ao Render:

1. Volte para a página de **Custom Domains** no Render
2. Clique em **"Verify"** ao lado do seu domínio
3. ⏳ Aguarde alguns minutos (pode levar até 48h, mas geralmente é rápido!)
4. ✅ Quando verificar, o SSL será emitido automaticamente!

**🎉 Agora seu site está acessível em `https://seudominio.com`!**

---

## 🔑 Acessar o Admin

- **URL**: `https://seudominio.com/admin`
- **Email**: `admin@acaiprime.com`
- **Senha**: `admin123`

**⚠️ IMPORTANTE**: Altere a senha do admin após o primeiro acesso!

---

## 🔄 Como Atualizar o Site Depois

**É automático!** Toda vez que você fizer push para o GitHub, o Render atualiza sozinho:

```bash
# 1. Faça suas alterações no código

# 2. Commit
git add .
git commit -m "Descrição das mudanças"

# 3. Push
git push

# ✅ O Render detecta e faz deploy automático!
```

---

## 🛠️ Comandos Úteis

### Ver Logs em Tempo Real:
1. Dashboard do Render → Seu serviço
2. Clique em **"Logs"** no menu lateral

### Acessar Shell (Terminal):
1. Dashboard do Render → Seu serviço
2. Clique em **"Shell"** no menu lateral
3. Digite comandos como `npm run db:push`

### Reiniciar o Serviço:
1. Dashboard do Render → Seu serviço
2. Clique em **"Manual Deploy"** → **"Clear build cache & deploy"**

---

## ⚠️ Limitações do Plano Gratuito

- ❌ **App "dorme" após 15 min sem uso** (primeiro acesso demora ~30 seg)
- ✅ **Solução**: Use um serviço gratuito de ping como **UptimeRobot** ou **Cron-Job.org** para manter o site acordado

### Manter o Site Sempre Acordado (Grátis):

1. Acesse: **https://uptimerobot.com** (grátis, sem cartão)
2. Crie uma conta
3. Adicione um **"New Monitor"**:
   - **Monitor Type**: HTTP(s)
   - **URL**: `https://seudominio.com`
   - **Monitoring Interval**: 5 minutes
4. ✅ Pronto! Seu site nunca vai dormir!

---

## 🎯 Checklist Final

- ✅ Conta criada no Render (sem cartão)
- ✅ Código enviado para o GitHub
- ✅ Web Service criado e rodando
- ✅ Variáveis de ambiente configuradas
- ✅ Banco de dados migrado (`npm run db:push`)
- ✅ Dados populados (seed-products, seed-admin, seed-toppings)
- ✅ Domínio da Hostinger conectado
- ✅ SSL ativado automaticamente
- ✅ Site acessível 24/7

---

## 💰 Custos

### ✅ **TUDO GRÁTIS!**

- Render: **R$ 0,00**
- Domínio: Já pago na Hostinger
- Banco de Dados: Já tem na Hostinger
- SSL: Grátis no Render
- UptimeRobot (opcional): **R$ 0,00**

---

## 🆘 Problemas Comuns

### 1. "Build failed" no Render
**Solução**: Verifique se o `package.json` está correto e se todos os arquivos estão no GitHub.

### 2. "Application Error" ao acessar
**Solução**: Verifique os logs no Render. Provavelmente erro no `DATABASE_URL`.

### 3. Domínio não conecta
**Solução**: Aguarde até 48h para propagação DNS. Use `https://dnschecker.org` para verificar.

### 4. Banco de dados não conecta
**Solução**: Verifique se o banco PostgreSQL da Hostinger permite conexões externas.

---

## 🎉 Pronto!

Seu site de açaí está:
- ✅ **No ar 24/7**
- ✅ **Com seu domínio**
- ✅ **SSL ativado (https)**
- ✅ **100% GRÁTIS**
- ✅ **Atualização automática via GitHub**

**Qualquer dúvida, consulte este guia novamente!** 📖
