<div align="center">

# BELLAVIA

**E-commerce de calças premium com checkout integrado e provador virtual por IA**

[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?logo=supabase&logoColor=white)](https://supabase.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-3-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![Licença: MIT](https://img.shields.io/badge/licen%C3%A7a-MIT-blue.svg)](./LICENSE)

</div>

---

## 📖 Sobre

A **BELLAVIA** é uma loja virtual de calças premium. O projeto oferece uma vitrine digital com identidade visual própria — navegação por coleção, detalhamento de produto, carrinho de compras e checkout completo — voltada a clientes que buscam peças de vestuário com apelo de exclusividade ("peças selecionadas para quem sabe onde quer chegar").

Além das funcionalidades tradicionais de um e-commerce, a BELLAVIA conta com dois diferenciais: um **provador virtual com inteligência artificial**, no qual o cliente envia uma foto, escolhe a peça, o tamanho e informa sua altura para visualizar como a roupa ficaria em seu corpo; e um **painel administrativo** com autenticação e controle de acesso por perfil, permitindo gerenciar produtos, acompanhar pedidos e a receita da loja.

No lado do backend, a aplicação utiliza o Supabase para banco de dados, autenticação e armazenamento, além de *edge functions* para processar transações de pagamento (PIX, cartão e boleto) por meio do gateway **IronPay** e para gerar as imagens do provador com um gateway de IA. Os pedidos são validados no servidor, com o total calculado a partir dos preços reais do banco de dados.

A loja é publicada como SPA, com configuração de deploy pronta para a Vercel. O público-alvo são consumidores de moda masculina/unissex interessados em calças de acabamento premium.

## ✨ Funcionalidades

- **Vitrine inicial** com banner hero, faixa de novidades (marquee), seção de diferenciais e produtos em destaque.
- **Catálogo de produtos** (`/produtos`) com ordenação por nome e por preço (menor/maior).
- **Detalhe do produto** (`/produto/:id`) com seleção de tamanho, cor e quantidade, além do botão de adicionar ao carrinho e atalho para o provador IA.
- **Carrinho de compras** persistente no navegador (Zustand + `localStorage`, chave `bellavia-cart`), com atualização de quantidades, remoção de itens, "limpar carrinho" e resumo do pedido.
- **Checkout** (`/checkout`) com dados pessoais (nome, e-mail, telefone e CPF), endereço de entrega completo e escolha da forma de pagamento: **PIX** (com QR Code gerado no cliente via `qrcode.react`), **cartão de crédito** (com parcelamento de 1 a 12 vezes) ou **boleto**. Validação de formulário com Zod.
- **Integração de pagamentos com o IronPay** por meio da edge function `ironpay-transaction`, com criação de transações, consulta de status, reembolso e sincronização de produtos.
- **Webhook de pagamento** (`ironpay-webhook`) que confirma o pagamento e atualiza o status do pedido.
- **Provador virtual com IA** (`/provador`): upload de foto, escolha da peça em catálogo, seleção de tamanho e altura, gerando uma prévia realista via edge function `virtual-fitting`.
- **Painel administrativo** (`/admin` e `/admin/dashboard`) com login/registro pelo Supabase Auth e verificação de perfil de administrador.
  - **Dashboard** com indicadores de produtos, pedidos pendentes e receita total.
  - **Gestão de produtos**: criação, edição, exclusão, ativação/desativação e **sincronização com o IronPay** (criação de produto e oferta) com registro do status da última sincronização.
  - **Gestão de pedidos**: acompanhamento e atualização de status (`pending` → `paid` → `shipped` → `delivered`).
- **Banco de dados Supabase** com tabelas `products`, `orders`, `user_roles` e `store_settings`, *bucket* de imagens `products`, políticas de RLS (escrita restrita a administradores) e função `create_validated_order` para validação segura de pedidos no servidor.
- **Deploy na Vercel** com regra de *rewrite* para SPA (todas as rotas servidas por `index.html`).

## 🛠️ Tecnologias

<p>
<img src="https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=white" alt="React" />
<img src="https://img.shields.io/badge/TypeScript-5-3178C6?logo=typescript&logoColor=white" alt="TypeScript" />
<img src="https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white" alt="Vite" />
<img src="https://img.shields.io/badge/Supabase-3ECF8E?logo=supabase&logoColor=white" alt="Supabase" />
<img src="https://img.shields.io/badge/Tailwind%20CSS-3-06B6D4?logo=tailwindcss&logoColor=white" alt="Tailwind CSS" />
<img src="https://img.shields.io/badge/shadcn%2Fui-000000?logo=shadcnui&logoColor=white" alt="shadcn/ui" />
<img src="https://img.shields.io/badge/Zustand-2C3E50?logo=react&logoColor=white" alt="Zustand" />
<img src="https://img.shields.io/badge/React%20Router-6-CA4245?logo=reactrouter&logoColor=white" alt="React Router" />
<img src="https://img.shields.io/badge/Zod-3-3E67B1?logo=zod&logoColor=white" alt="Zod" />
</p>

- **React 18** + **TypeScript** + **Vite**
- **Supabase** (Postgres, Auth, Storage e Edge Functions em Deno)
- **Tailwind CSS** + **shadcn/ui** (componentes Radix UI)
- **Zustand** (carrinho com persistência), **React Router DOM**, **TanStack React Query**
- **IronPay** (gateway de pagamentos) integrado via edge functions
- **qrcode.react** (QR Code PIX), **React Hook Form** + **Zod**, **date-fns**, **lucide-react** e **sonner**

## 📁 Estrutura do projeto

```
bellavia-2.0/
├── public/
│   └── logo-bellavia.png
├── scripts/
│   └── sync-public-logo.mjs     # Copia a logo de src/assets para public/
├── src/
│   ├── assets/
│   ├── components/
│   │   ├── ui/                  # Componentes shadcn/ui
│   │   ├── BannerSplit.tsx
│   │   ├── FeaturedProducts.tsx
│   │   ├── Features.tsx
│   │   ├── Footer.tsx
│   │   ├── Header.tsx
│   │   ├── HeroSection.tsx
│   │   ├── Marquee.tsx
│   │   └── ProductCard.tsx
│   ├── hooks/
│   │   └── useAdminAuth.ts      # Autenticação e verificação de admin
│   ├── integrations/supabase/   # Cliente e tipos do Supabase
│   ├── lib/
│   │   ├── store.ts             # Carrinho (Zustand + persist)
│   │   └── utils.ts
│   ├── pages/
│   │   ├── Index.tsx            # Página inicial
│   │   ├── Products.tsx         # Catálogo
│   │   ├── ProductDetail.tsx    # Detalhe do produto
│   │   ├── Cart.tsx             # Carrinho
│   │   ├── Checkout.tsx         # Checkout e pagamento
│   │   ├── VirtualFitting.tsx   # Provador virtual (IA)
│   │   ├── AdminLogin.tsx       # Login do administrador
│   │   ├── AdminDashboard.tsx   # Painel administrativo
│   │   └── NotFound.tsx
│   ├── App.tsx                  # Rotas da aplicação
│   ├── index.css
│   └── main.tsx
├── supabase/
│   ├── config.toml
│   ├── functions/
│   │   ├── ironpay-sync/        # Sincronização de produtos com o IronPay
│   │   ├── ironpay-transaction/ # Criação/consulta/reembolso de transações
│   │   ├── ironpay-webhook/     # Confirmação de pagamento
│   │   └── virtual-fitting/     # Geração do provador virtual (IA)
│   └── migrations/              # Esquema do banco, RLS e storage
├── index.html
├── package.json
├── tailwind.config.ts
├── tsconfig.json
├── vercel.json                  # Rewrites de SPA para deploy
└── vite.config.ts
```

## 🚀 Como rodar localmente

Pré-requisitos: **Node.js** e **npm**.

```bash
# 1. Instalar as dependências
npm install

# 2. Iniciar o servidor de desenvolvimento (http://localhost:8080)
npm run dev
```

> Observação: a aplicação depende de variáveis de ambiente do Supabase (URL e chave pública) e, para as *edge functions*, de credenciais do IronPay e do gateway de IA. Configure-as antes de executar o projeto.

Scripts disponíveis:

```bash
npm run dev                # Servidor de desenvolvimento (sincroniza a logo e inicia o Vite)
npm run build              # Build de produção (sincroniza a logo e gera o bundle)
npm run build:dev          # Build em modo de desenvolvimento
npm run preview            # Pré-visualização do build
npm run lint               # Análise estática com ESLint
npm run sync:public-logo   # Copia a logo para a pasta public/
```

## 📄 Licença

Distribuído sob a **Licença MIT**. Veja [`LICENSE`](./LICENSE).
