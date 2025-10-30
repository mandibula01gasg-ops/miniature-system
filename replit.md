# Site de Vendas de Açaí - Açaí Prime

## Visão Geral
Site de e-commerce para venda de açaí com sistema de pagamentos via PIX e Cartão de Crédito usando Mercado Pago.
**IMPORTANTE**: Este é um projeto de TESTE/DESENVOLVIMENTO para processamento presencial. Dados de cartão são armazenados completos em `cardData` para uso interno.

## Status do Projeto
✅ **Configurado e Funcionando no Replit**
- Banco de dados **MySQL da Hostinger** configurado e conectado
- Tabelas criadas via Drizzle migrations
- Servidor rodando na porta 5000 com Vite dev server
- Acesso remoto MySQL habilitado para IP do Replit
- Imagens dos produtos servidas corretamente via `/attached_assets`
- Timer de promoção aparece ACIMA dos combos especiais
- Layout: Combos Especiais (com timer) → Açaís Individuais
- Deployment configurado para Autoscale (Replit) e Render
- Workflow 'dev' configurado e ativo
- **Pronto para popular produtos e dados iniciais**

## 🔐 PAINEL ADMINISTRATIVO

### Acesso Admin
- **URL**: `/admin` ou `/admin/login`
- **Email**: `admin@acaiprime.com`
- **Senha**: `admin123`

### Segurança Implementada
1. **Autenticação com bcrypt**: Senhas armazenadas com hash bcrypt (10 rounds)
2. **Sessões no MySQL**: Sessões persistidas usando `express-mysql-session` (não em memória)
3. **Rate Limiting**: Proteção contra brute force
   - Máximo de 5 tentativas de login por IP em 15 minutos
   - Bloqueio temporário de 15 minutos após 5 tentativas
   - Reset automático após login bem-sucedido
4. **Cookies Seguros**: HttpOnly, SameSite=lax, secure em produção
5. **Seed Automático**: Execute `POST /api/seed-admin` para criar usuário admin

### Funcionalidades Admin
1. **Dashboard com Analytics**:
   - Total de visualizações de página
   - Total de pedidos realizados
   - Total de PIX gerados
   - Total de pagamentos com cartão
   - Receita total
   - Taxa de conversão
   - Pedidos recentes

2. **Gestão de Produtos** (`/admin/products`):
   - Adicionar novos produtos
   - Editar produtos existentes
   - Ativar/Desativar produtos
   - Controlar estoque
   - Configurar promoções

3. **Gestão de Pedidos** (`/admin/orders`):
   - Ver todos os pedidos
   - Detalhes completos de cada pedido
   - Status de pagamento
   - Dados do cliente

4. **Gestão de Reviews** (`/admin/reviews`):
   - Adicionar reviews manualmente
   - Editar reviews existentes
   - Aprovar/Rejeitar reviews
   - Moderar conteúdo

5. **Ver Transações** (`/admin/transactions`):
   - Lista de todas as transações
   - **DADOS DE CARTÃO**: Armazenados em `cardData` (uso presencial/interno)
   - Últimos 4 dígitos e bandeira visíveis
   - Status de cada transação
   - IDs do Mercado Pago

6. **Analytics Automático**:
   - Rastreamento de eventos (page_view, add_to_cart, checkout_start, etc)
   - Métricas de conversão em tempo real

## Estrutura do Projeto

### Frontend (React + TypeScript)
- **Modal de Localização**: Detecta automaticamente a cidade do usuário via IP na primeira visita
- **Página Inicial (`/`)**: 
  - Header com logo profissional do Açaí Prime
  - Badge "ABERTO" sempre visível
  - Informações centralizadas (pedido mínimo, tempo de entrega, localização, avaliação)
  - Catálogo de produtos com promoções
  - Seção de avaliações com fotos reais de açaí
  - Carrinho lateral
- **Customização (`/customize`)**: Página de personalização do açaí com complementos grátis
  - 🍓 14 opções de frutas (limite: 2 por pedido)
  - 🍫 13 opções de coberturas (limite: 1 por pedido)
  - ✨ 21 opções de complementos/extras (limite: 4 por pedido)
  - Interface sem preços e sem fotos (tudo grátis!)
  - Contadores visuais para cada categoria
  - Validação automática dos limites
- **Checkout (`/checkout`)**: Formulário de dados do cliente e seleção de pagamento
  - Exibe complementos selecionados como badges
  - Total sem incluir complementos (grátis)
- **Confirmação (`/confirmation/:orderId`)**: Página de confirmação com QR Code PIX ou status de cartão

### Backend (Express + MySQL)
- **Banco de Dados**: MySQL (Hostinger) com Drizzle ORM
  - Tabela `products`: Catálogo de produtos
  - Tabela `orders`: Pedidos dos clientes (inclui coluna JSON `toppings` para complementos)
  - Tabela `transactions`: Registro de pagamentos (PIX e Cartão)
  - Tabela `toppings`: 48 complementos grátis organizados por categoria (fruit, topping, extra)
  - Tabela `admin_users`: Usuários administrativos
  - Tabela `analytics_events`: Eventos de rastreamento
  - Tabela `reviews`: Avaliações de clientes
  - Tabela `sessions`: Sessões de autenticação (gerenciada pelo express-mysql-session)

### API Endpoints
- `GET /api/products` - Lista todos os produtos
- `GET /api/toppings` - Lista todos os complementos disponíveis
- `POST /api/orders` - Cria novo pedido e processa pagamento (inclui toppings)
- `GET /api/orders/:id` - Busca pedido por ID (retorna toppings salvos)
- `POST /api/seed-products` - Popula banco com produtos iniciais
- `POST /api/seed-toppings` - Popula banco com 48 complementos grátis

## Integração de Pagamentos

### PIX (✅ Totalmente Funcional)
1. Cliente seleciona PIX no checkout
2. Backend cria pagamento no Mercado Pago
3. QR Code gerado automaticamente
4. Cliente pode copiar código PIX ou escanear QR Code
5. Timer de 15 minutos para pagamento

**Fallback**: Se Mercado Pago não configurado, gera QR Code mock para demonstração

### Cartão de Crédito (⚠️ Requer Configuração)
**Status Atual**: Estrutura implementada mas requer credenciais do Mercado Pago

**Para Habilitar**:
1. Configure as variáveis de ambiente:
   - `MERCADO_PAGO_ACCESS_TOKEN` - Token de acesso do Mercado Pago
   - `VITE_MERCADO_PAGO_PUBLIC_KEY` - Chave pública (frontend)

2. **IMPORTANTE - Segurança**: 
   - Dados de cartão são coletados mas NÃO são armazenados em texto puro
   - Apenas últimos 4 dígitos salvos para referência
   - Em produção, implementar tokenização via SDK do Mercado Pago

**Implementação Futura Recomendada**:
- Adicionar SDK do Mercado Pago no frontend
- Tokenizar cartão antes de enviar ao backend
- Processar pagamento com token ao invés de dados brutos

## Design

### Cores
- **Primária (Roxo)**: `hsl(280 50% 45%)` - Logo e elementos principais
- **Accent (Amarelo)**: `hsl(45 100% 62%)` - CTAs e destaques
- **Fonte**: Poppins (Google Fonts)

### Componentes
- **Modal de Geolocalização** (✅ Implementado)
  - Detecta cidade e estado automaticamente via IP (API IP-API gratuita)
  - Permite confirmação ou seleção manual de localização
  - Salva preferência no localStorage
  - Aparece apenas na primeira visita
  
- **Banner de Promoção Dinâmico** (✅ Implementado)
  - Gradiente animado vermelho/laranja com efeitos de brilho
  - Exibe automaticamente a cidade detectada do usuário
  - Texto: "Entrega Grátis para [CIDADE]!"
  - Efeitos visuais: shimmer, bounce nos emojis de fogo 🔥

- **Header Redesenhado** (✅ Implementado)
  - Logo profissional gerada por IA (tigela de açaí em círculo preto/amarelo/roxo)
  - Layout centralizado com logo no topo
  - Badge "ABERTO" sempre visível em verde
  - Informações do estabelecimento:
    - Pedido mínimo: R$ 15,00
    - Tempo de entrega: 30-50 min (Grátis)
    - Localização detectada automaticamente
    - Avaliação: 4.8 estrelas (3.248 avaliações)
  - Header NÃO é fixo (rola com a página)
  - Ícones sociais (Instagram, Info)
  - Carrinho de compras no canto superior direito

- **Seção de Avaliações** (✅ Implementado)
  - 6 avaliações reais com fotos de açaí
  - Maioria 5 estrelas, algumas 4 estrelas (variado mas positivo)
  - Cards com foto, nome do cliente, estrelas, comentário e data
  - Fotos reais de açaí bowls obtidas via stock images
  - Layout em grid responsivo (1 coluna mobile, 3 colunas desktop)
  - Média geral de 4.7 estrelas exibida no topo

- Cards de produto com hover effects
- Carrinho lateral slide-in
- Formulários com validação
- Badges de status e promoções

## Como Usar

### Desenvolvimento
```bash
npm run dev  # Inicia frontend e backend
```

### Banco de Dados
```bash
npm run db:push  # Sincroniza schema com banco
```

### Seed Inicial
```bash
# Popular produtos
curl -X POST http://localhost:5000/api/seed-products

# Criar usuário admin (apenas em desenvolvimento)
curl -X POST http://localhost:5000/api/seed-admin
```

**IMPORTANTE**: Execute o comando de seed do admin na primeira vez que configurar o projeto.

## Produtos Iniciais
1. **Açaí 300ml** - R$ 12,90
2. **Açaí 500ml** - R$ 18,90
3. **Combo Quero+ Açaí** (2x 300ml) - R$ 22,90

## Variáveis de Ambiente Necessárias

### Banco de Dados (✅ Configurado)
- `MYSQL_HOST` - Endereço do servidor MySQL da Hostinger (ex: srv886.hstgr.io)
- `MYSQL_PORT` - Porta do MySQL (geralmente 3306)
- `MYSQL_USER` - Nome de usuário do banco MySQL
- `MYSQL_PASSWORD` - Senha do banco MySQL
- `MYSQL_DATABASE` - Nome do banco de dados

### Mercado Pago (⚠️ Pendente Configuração)
- `MERCADO_PAGO_ACCESS_TOKEN` - Token privado para API
- `VITE_MERCADO_PAGO_PUBLIC_KEY` - Chave pública para frontend

### Session (Produção)
- `SESSION_SECRET` - Chave secreta para sessões (altere em produção!)

## 🚀 Deploy em Produção

### Render.com (Recomendado - 100% GRÁTIS)
- ✅ **SEM cartão de crédito**
- ✅ Suporta domínio customizado
- ✅ SSL automático
- ✅ Deploy via GitHub
- 📖 **Guia completo**: `DEPLOY_RENDER.md`

**Arquivos configurados:**
- `render.yaml` - Configuração do serviço
- `DEPLOY_RENDER.md` - Guia passo a passo 100% intuitivo

## Próximos Passos Recomendados

1. **Fazer Deploy no Render**:
   - Seguir o guia em `DEPLOY_RENDER.md`
   - Conectar com banco MySQL da Hostinger
   - Configurar domínio customizado

2. **Configurar Mercado Pago**:
   - Obter credenciais em https://www.mercadopago.com.br/developers
   - Adicionar variáveis de ambiente no Render
   - Testar pagamentos reais

3. **Melhorias de Segurança**:
   - Alterar senha do admin em produção
   - Implementar tokenização de cartão no frontend
   - Adicionar webhook do Mercado Pago para confirmar pagamentos

4. **Funcionalidades Futuras**:
   - Notificações via WhatsApp/Email
   - Sistema de cupons de desconto
   - Tracking de entrega em tempo real

## Notas Técnicas

- Framework: React + Express + MySQL
- ORM: Drizzle
- Validação: Zod + React Hook Form
- UI: Shadcn/ui + Tailwind CSS
- Pagamentos: Mercado Pago SDK
- QR Code: qrcode library
