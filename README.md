# next-frontend-boilerplate

Boilerplate pessoal de frontend com Next.js, App Router, TypeScript, Tailwind CSS, React Query, Axios e Vitest. Criado para eliminar setup repetitivo e garantir consistência entre projetos de frontend que consomem uma API externa.

> **Importante**: Este é um boilerplate, não um projeto pronto. Ele fornece a estrutura, configurações e convenções base. Você vai adicionar páginas, componentes, rotas e lógica por cima disso.

---

## Índice

- [O que é este boilerplate?](#o-que-é-este-boilerplate)
- [Stack](#stack)
- [Pré-requisitos](#pré-requisitos)
- [Como usar este boilerplate](#como-usar-este-boilerplate)
- [Primeiros passos após clonar](#primeiros-passos-após-clonar)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Variáveis de ambiente](#variáveis-de-ambiente)
- [Comunicação com a API](#comunicação-com-a-api)
- [Testes](#testes)
- [CI/CD](#cicd)
- [Hooks de commit (Husky + lint-staged)](#hooks-de-commit-husky--lint-staged)
- [Scripts disponíveis](#scripts-disponíveis)
- [O que você precisa criar ao usar em um projeto real](#o-que-você-precisa-criar-ao-usar-em-um-projeto-real)
- [Como manter o boilerplate atualizado](#como-manter-o-boilerplate-atualizado)
- [Convenções de commit](#convenções-de-commit)

---

## O que é este boilerplate?

Este boilerplate é exclusivamente para o **frontend** — a parte visual da aplicação que o usuário interage. Ele não tem banco de dados, não tem servidor próprio de API, não tem Prisma. Ele consome dados de uma API externa (como o `node-express-boilerplate`) via HTTP.

Use este boilerplate quando:

- Você tem (ou vai ter) uma API separada para o backend
- Você quer um projeto Next.js limpo, sem acoplamento com backend
- Você quer integrar com `node-express-boilerplate` ou qualquer outra API REST

---

## Stack

| Categoria     | Tecnologia                          | Versão | Por quê                                                               |
| ------------- | ----------------------------------- | ------ | --------------------------------------------------------------------- |
| Framework     | Next.js (App Router)                | 16.x   | SSR, SSG, file-based routing, otimizações de performance              |
| Linguagem     | TypeScript                          | 5.x    | Tipagem estática, autocomplete, segurança em refactors                |
| Estilização   | Tailwind CSS                        | 4.x    | Utility-first, sem CSS files extras, consistência visual              |
| HTTP Client   | Axios                               | 1.x    | Chamadas HTTP para a API com interceptors e configuração centralizada |
| Data Fetching | React Query (@tanstack/react-query) | 5.x    | Cache, loading states, refetch automático, sincronização de dados     |
| Validação     | Zod                                 | 4.x    | Validação de formulários e responses da API com tipagem automática    |
| Testes        | Vitest + Testing Library            | 4.x    | Mais rápido que Jest, integração nativa com ESM                       |
| Formatação    | Prettier                            | 3.x    | Formato de código consistente e automático                            |
| Linting       | ESLint                              | 9.x    | Detecta erros e más práticas em tempo de desenvolvimento              |
| CI            | GitHub Actions                      | -      | Roda lint, format check e testes em todo push                         |
| Pre-commit    | Husky + lint-staged                 | -      | Garante qualidade antes de cada commit                                |

---

## Pré-requisitos

Antes de usar este boilerplate, você precisa ter instalado na sua máquina:

- **Node.js** 20 ou superior — [nodejs.org](https://nodejs.org)
- **pnpm** — `npm install -g pnpm`
- **Git** — [git-scm.com](https://git-scm.com)

> Não precisa de Docker neste boilerplate — ele não tem banco de dados. O Docker fica no backend.

---

## Como usar este boilerplate

Este repositório é um **GitHub Template**. Para usá-lo em um novo projeto:

### Opção A — Via GitHub (recomendado)

1. Acesse o repositório no GitHub
2. Clique em **"Use this template"** → **"Create a new repository"**
3. Dê um nome ao novo repositório e crie
4. Clone o novo repositório na sua máquina

### Opção B — Via degit (sem histórico do boilerplate)

```bash
npx degit seu-usuario/next-frontend-boilerplate nome-do-projeto
cd nome-do-projeto
git init
git remote add origin https://github.com/seu-usuario/nome-do-projeto.git
```

> **Nunca** clone diretamente este repositório para iniciar um projeto. Use uma das opções acima para manter o histórico de commits limpo.

---

## Primeiros passos após clonar

Execute os passos abaixo **na ordem** sempre que iniciar um projeto a partir deste boilerplate:

```bash
# 1. Instalar dependências
pnpm install

# 2. Criar o arquivo de ambiente
cp .env.example .env
# Edite o .env com a URL da sua API

# 3. Iniciar o servidor de desenvolvimento
pnpm dev
```

Acesse [http://localhost:3000](http://localhost:3000).

> Certifique-se de que a API que este frontend vai consumir está rodando antes de iniciar o desenvolvimento.

---

## Estrutura do projeto

```
next-frontend-boilerplate/
├── .github/
│   └── workflows/
│       └── ci.yml                    # Pipeline de CI (lint, format, testes)
├── .husky/
│   └── pre-commit                    # Roda lint-staged antes de cada commit
├── public/                           # Arquivos estáticos públicos (favicon, og images, etc)
├── src/
│   ├── app/                          # Rotas da aplicação (App Router do Next.js)
│   │   ├── layout.tsx                # Layout raiz — fonte, metadata global, providers
│   │   └── page.tsx                  # Página inicial (/)
│   ├── assets/                       # Arquivos estáticos usados no código (SVGs, imagens, fontes locais)
│   ├── components/                   # Componentes React reutilizáveis
│   │   └── ui/                       # Componentes de UI genéricos (Button, Input, Modal, Card, etc)
│   ├── hooks/                        # Custom hooks reutilizáveis
│   │   └── (ex: useDebounce.ts)      # Ex: useDebounce, useLocalStorage, useMediaQuery
│   ├── lib/
│   │   └── axios.ts                  # Instância do Axios configurada com baseURL da API
│   ├── providers/                    # React Context Providers
│   │   └── (ex: QueryProvider.tsx)   # Ex: React Query Provider — envolve toda a aplicação
│   ├── services/                     # Funções que fazem chamadas HTTP para a API
│   │   └── (ex: user.service.ts)     # Ex: getUser(), createUser(), deleteUser()
│   ├── styles/
│   │   └── globals.css               # Reset, variáveis CSS, configuração do Tailwind
│   ├── tests/                        # Arquivos de teste
│   │   ├── setup.ts                  # Setup global do Vitest (importa jest-dom)
│   │   └── example.test.ts           # Teste placeholder — substitua pelos seus
│   ├── types/                        # Tipos e interfaces TypeScript globais
│   │   └── (ex: user.types.ts)       # Ex: tipos que espelham os models da API
│   ├── utils/                        # Funções utilitárias puras e genéricas
│   │   └── (ex: format.ts)           # Ex: formatDate(), formatCurrency(), cn() do shadcn
│   └── validations/                  # Schemas de validação com Zod
│       └── (ex: user.validation.ts)  # Ex: schemas de formulários
├── .env                              # Variáveis de ambiente locais — NÃO commitar
├── .env.example                      # Template das variáveis de ambiente — commitar sempre
├── .gitignore                        # Arquivos ignorados pelo Git
├── .prettierignore                   # Arquivos ignorados pelo Prettier
├── .prettierrc                       # Configuração do Prettier
├── eslint.config.mjs                 # Configuração do ESLint
├── next.config.ts                    # Configuração do Next.js
├── package.json                      # Dependências e scripts
├── postcss.config.mjs                # Configuração do PostCSS (necessário pro Tailwind v4)
├── tsconfig.json                     # Configuração do TypeScript
└── vitest.config.ts                  # Configuração do Vitest
```

### Por que essa estrutura?

- **`app/`** contém apenas rotas. Lógica, componentes e utilitários ficam fora dela para facilitar reuso e testes.
- **`components/`** é para componentes reutilizáveis. Componentes específicos de uma rota ficam dentro da própria pasta de rota em `app/`.
- **`services/`** centraliza todas as chamadas HTTP. Um componente nunca usa o axios diretamente — ele chama uma função de `services/`.
- **`hooks/`** centraliza custom hooks. Se um hook é usado em mais de um lugar, ele vai para cá.
- **`providers/`** separa providers de componentes visuais — facilita identificar o que é contexto global.
- **`types/`** centraliza os tipos TypeScript. Os tipos aqui geralmente espelham os models da API.
- **`validations/`** centraliza os schemas Zod usados em formulários.
- **`lib/`** contém configurações de libs terceiras, como a instância do Axios.

---

## Variáveis de ambiente

O arquivo `.env.example` contém todas as variáveis necessárias com valores de exemplo. **Nunca commite o `.env`**.

```env
# App
NEXT_PUBLIC_APP_URL="http://localhost:3000"

# API
NEXT_PUBLIC_API_URL="http://localhost:3333"
```

> Variáveis prefixadas com `NEXT_PUBLIC_` são expostas no navegador. Nunca coloque secrets (tokens privados, chaves de API sensíveis) nelas — qualquer pessoa pode ver no DevTools.

Ao adicionar uma nova variável de ambiente:

1. Adicione no `.env` com o valor real
2. Adicione no `.env.example` com um valor de exemplo ou vazio
3. Documente aqui neste README para que ela serve

---

## Comunicação com a API

### Instância do Axios

O arquivo `src/lib/axios.ts` exporta uma instância do Axios já configurada com a `baseURL` da API:

```typescript
import { api } from "@/lib/axios";
```

Nunca importe o axios diretamente nos componentes ou services. Use sempre esta instância para que interceptors e configurações globais funcionem.

### Services

As chamadas HTTP ficam em `src/services/`. Cada recurso tem seu próprio arquivo:

```typescript
// src/services/user.service.ts
import { api } from "@/lib/axios";
import { User } from "@/types/user.types";

export async function getUsers(): Promise<User[]> {
  const { data } = await api.get("/users");
  return data;
}

export async function getUserById(id: string): Promise<User> {
  const { data } = await api.get(`/users/${id}`);
  return data;
}

export async function createUser(payload: CreateUserPayload): Promise<User> {
  const { data } = await api.post("/users", payload);
  return data;
}
```

### React Query

Use React Query para gerenciar o estado dos dados vindos da API. Ele cuida de cache, loading, erro e refetch automaticamente:

```typescript
// em um componente ou custom hook
import { useQuery } from "@tanstack/react-query";
import { getUsers } from "@/services/user.service";

export function useUsers() {
  return useQuery({
    queryKey: ["users"],
    queryFn: getUsers,
  });
}
```

### QueryProvider

Para o React Query funcionar, é necessário envolver a aplicação com o `QueryClientProvider`. Crie em `src/providers/QueryProvider.tsx` e adicione ao `src/app/layout.tsx`:

```typescript
// src/providers/QueryProvider.tsx
"use client";

import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { useState } from "react";

export function QueryProvider({ children }: { children: React.ReactNode }) {
  const [queryClient] = useState(() => new QueryClient());
  return (
    <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
  );
}
```

```typescript
// src/app/layout.tsx
import { QueryProvider } from "@/providers/QueryProvider";

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <QueryProvider>{children}</QueryProvider>
      </body>
    </html>
  );
}
```

---

## Testes

O projeto usa **Vitest** com **Testing Library**.

```bash
# Rodar em modo watch (re-executa ao salvar)
pnpm test

# Rodar todos os testes uma vez (usado no CI)
pnpm test --run

# Abrir a interface visual do Vitest
pnpm test:ui
```

### Onde criar testes

- Testes de componentes: `src/tests/components/`
- Testes de hooks: `src/tests/hooks/`
- Testes de utils/validations: `src/tests/unit/`

### Convenção de nomenclatura

```
NomeDoArquivo.test.ts     # teste unitário
NomeDoComponente.test.tsx # teste de componente React
```

### Exemplo de teste de componente

```typescript
import { render, screen } from "@testing-library/react";
import { Button } from "@/components/ui/Button";

it("should render button with correct text", () => {
  render(<Button>Click me</Button>);
  expect(screen.getByText("Click me")).toBeInTheDocument();
});
```

---

## CI/CD

O pipeline de CI roda automaticamente em todo push e pull request para `main` e `develop`.

### O que o CI verifica

1. **Lint** — `pnpm lint` — garante que não há erros de ESLint
2. **Format check** — `pnpm format:check` — garante que o código está formatado com Prettier
3. **Testes** — `pnpm test --run` — garante que todos os testes passam

---

## Hooks de commit (Husky + lint-staged)

O projeto usa **Husky** para rodar verificações automaticamente antes de cada commit, eliminando a necessidade de rodar `pnpm format` e `pnpm lint` manualmente.

### O que roda no pre-commit

| Arquivos                             | O que executa           |
| ------------------------------------ | ----------------------- |
| `*.ts, *.tsx, *.js, *.jsx`           | ESLint --fix + Prettier |
| `*.json, *.md, *.yml, *.yaml, *.css` | Prettier                |

O lint-staged garante que **apenas os arquivos staged** sejam verificados — não o projeto inteiro.

### Como funciona

Ao rodar `git commit`, o Husky intercepta e executa `pnpm lint-staged`. Se algum arquivo tiver erro de lint que não possa ser corrigido automaticamente, o commit é bloqueado até você corrigir.

> O Husky é inicializado automaticamente via script `prepare` no `package.json` ao rodar `pnpm install`.

---

## Scripts disponíveis

| Script          | Comando             | O que faz                                                       |
| --------------- | ------------------- | --------------------------------------------------------------- |
| Desenvolvimento | `pnpm dev`          | Inicia o servidor Next.js em modo desenvolvimento               |
| Build           | `pnpm build`        | Gera o build de produção                                        |
| Produção        | `pnpm start`        | Inicia o servidor em modo produção (requer build)               |
| Lint            | `pnpm lint`         | Roda o ESLint em todo o projeto                                 |
| Formatar        | `pnpm format`       | Formata todos os arquivos com Prettier                          |
| Checar formato  | `pnpm format:check` | Verifica se os arquivos estão formatados (usado no CI)          |
| Testes          | `pnpm test`         | Roda Vitest em modo watch                                       |
| Testes (CI)     | `pnpm test --run`   | Roda Vitest uma vez e encerra                                   |
| Testes UI       | `pnpm test:ui`      | Abre a interface visual do Vitest                               |
| Setup           | `pnpm setup`        | Instala dependências e cria o `.env` a partir do `.env.example` |

---

## O que você precisa criar ao usar em um projeto real

Ao iniciar um novo projeto a partir deste boilerplate, as seguintes coisas **não estão incluídas** e precisam ser criadas:

### Obrigatório

- [ ] **`.env`** — preencha com a URL da API (`cp .env.example .env`)
- [ ] **`src/providers/QueryProvider.tsx`** — crie e adicione ao `layout.tsx`
- [ ] **Páginas** em `src/app/` — crie as rotas da aplicação
- [ ] **Componentes** em `src/components/` — crie os componentes visuais
- [ ] **Services** em `src/services/` — crie as funções de chamada à API
- [ ] **Types** em `src/types/` — crie os tipos que espelham os models da API

### Dependendo do projeto

- [ ] **Autenticação** — middleware de auth em `src/app/middleware.ts`, interceptor no axios para enviar token
- [ ] **shadcn/ui** — se quiser componentes prontos: `pnpm dlx shadcn@latest init`
- [ ] **Internacionalização** — se precisar de múltiplos idiomas: `next-intl`
- [ ] **Variáveis de ambiente adicionais** — adicione no `.env` e `.env.example`
- [ ] **Deploy workflow** — adicione `.github/workflows/deploy.yml` para CD automático

### Adicionando interceptor de autenticação no Axios

Quando tiver autenticação, adicione um interceptor no `src/lib/axios.ts` para enviar o token automaticamente:

```typescript
api.interceptors.request.use((config) => {
  const token = localStorage.getItem("token");
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});
```

### Dockerfile base (quando necessário)

```dockerfile
FROM node:20-alpine AS base
RUN npm install -g pnpm

FROM base AS deps
WORKDIR /app
COPY package.json pnpm-lock.yaml ./
RUN pnpm install --frozen-lockfile

FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN pnpm build

FROM base AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=builder /app/.next/standalone ./
COPY --from=builder /app/.next/static ./.next/static
COPY --from=builder /app/public ./public
EXPOSE 3000
CMD ["node", "server.js"]
```

> Para usar o output standalone, adicione `output: "standalone"` no `next.config.ts`.

---

## Como manter o boilerplate atualizado

O boilerplate deve evoluir conforme você aprende e resolve problemas recorrentes. Regra geral: **se você configurou algo chato em um projeto, volta aqui e atualiza**.

### Quando atualizar

- Resolveu um problema de configuração (ESLint, Vitest, CI)
- Adicionou uma lib que faz sentido em todo projeto frontend
- Encontrou uma estrutura de pasta melhor
- Atualizou uma dependência major e precisou adaptar configs

### Como atualizar

```bash
# Clone o boilerplate diretamente (não via template)
git clone https://github.com/seu-usuario/next-frontend-boilerplate.git
cd next-frontend-boilerplate

# Faça as alterações necessárias
git add .
git commit -m "feat: add axios interceptor example to README"
git push origin main
```

### Atualizando dependências

```bash
# Ver dependências desatualizadas
pnpm outdated

# Atualizar interativamente
pnpm update --interactive --latest
```

> Após atualizar dependências major, sempre rode `pnpm dev`, `pnpm build` e `pnpm test --run` para garantir que nada quebrou.

### Convenção de commits no boilerplate

| Prefixo     | Quando usar                                      |
| ----------- | ------------------------------------------------ |
| `chore:`    | Atualização de dependências, configs, scripts    |
| `feat:`     | Adição de nova lib ou funcionalidade base        |
| `fix:`      | Correção de configuração quebrada                |
| `docs:`     | Atualização do README                            |
| `style:`    | Formatação, sem mudança de lógica                |
| `refactor:` | Reorganização de estrutura de pastas ou arquivos |

---

## Convenções de commit

Este boilerplate segue o padrão **Conventional Commits**:

```
tipo(escopo opcional): descrição curta

corpo opcional

rodapé opcional
```

Exemplos:

```
feat(auth): add axios interceptor for JWT token
fix(query): fix QueryProvider missing in layout
chore: update dependencies
docs: update README with shadcn setup instructions
refactor(services): split user service into smaller functions
```
