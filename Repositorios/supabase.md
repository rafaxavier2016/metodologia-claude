# Supabase — análise arquitetural

| | |
|---|---|
| **Repositório** | github.com/supabase/supabase |
| **Commit analisado** | clone `--depth 1` de 2026-08-22 |
| **Analisado em** | 2026-08-22 |
| **Licença** | Apache-2.0 |
| **Escopo desta leitura** | `package.json` raiz, `README.md`, listagem de `apps/`, `packages/`, `apps/studio/` |

> ⚠️ **Correção de versão anterior.** A primeira análise descreveu uma estrutura
> `apps/studio/src/{components,pages,services,hooks,utils,context,types,styles}` e afirmou
> "TypeScript em 100%, zero uso de `any`". **As duas coisas eram falsas** — escritas de
> memória, sem conferir. O studio não tem `src/`, e há 645 ocorrências de `: any` só nele.
> Esta versão só afirma o que foi verificado por comando.

---

## 1. O que foi verificado (comando executado)

### Aplicações — `ls apps/`

```
design-system   docs   learn   lite-studio   studio   ui-library   www
```

Sete apps, não quatro.

### Estrutura real do `apps/studio/` — `ls -d */`

```
__mocks__/  api/     app/      compat/   components/  data/
evals/      fonts/   hooks/    lib/      pages/       public/
routes/     scripts/ state/    static-data/  styles/  tests/   types/
```

**Não existe `src/`.** As pastas ficam direto na raiz do app.

### Packages — `find packages -maxdepth 1 -type d`

```
ui  config  api-types  build-icons  shared-data  generator  common
icons  eslint-config-supabase  dev-tools  marketing  ui-patterns
pg-meta  ai-commands  tsconfig
```

### Toolchain — `package.json` raiz

| Item | Valor |
|---|---|
| Build orchestrator | `turbo` 2.9.14 |
| Package manager | `pnpm@11.13.1` (com `preinstall: npx only-allow pnpm`) |
| Node exigido | `>=22.13` |
| Formatador | Prettier + `prettier-plugin-sql-cst` |
| Lint | ESLint 9 |

### Tipagem — `grep -rn ": any" apps/studio --include="*.ts*" | wc -l`

```
645
```

Projeto é TypeScript, mas **`any` é usado à vontade**. Type safety é uma direção, não
uma regra aplicada.

---

## 2. Padrões que valem a pena (verificados)

### 2.1 `data/` e `state/` como camadas de primeira classe

O achado mais interessante. Em vez de espalhar chamada de API dentro de componente, o
studio tem duas pastas irmãs de `components/`:

```
apps/studio/data/     ← uma pasta por domínio
    access-tokens/  ai/  analytics/  api-keys/  api-settings/  ...

apps/studio/state/    ← estado global, um arquivo por área
    advisor-state.ts  ai-assistant-state.tsx  ai-chat-front-sync.ts  ...
```

Cada domínio em `data/` é autocontido: os hooks de query e mutation daquele domínio
moram juntos. Componente não sabe de onde o dado vem — importa o hook.

**Por que importa:** é o padrão que evita o `fetch` espalhado por 200 componentes. A
regra é "componente renderiza, `data/` busca, `state/` lembra".

### 2.2 `catalog:` para versão única de dependência

```json
"devDependencies": {
  "@types/node": "catalog:",
  "typescript": "catalog:",
  "tailwindcss": "catalog:"
}
```

Recurso do pnpm: a versão fica declarada em um lugar só (`pnpm-workspace.yaml`) e todos
os packages referenciam `catalog:`. Elimina a deriva de "app A com React 18.2 e app B
com 18.3".

### 2.3 Trava de gerente de pacote

```json
"preinstall": "npx only-allow pnpm"
```

`npm install` no repo falha na hora, com mensagem explicando. É a mesma filosofia do
hook de pré-commit do `ampliar-corretora`: **trava mecânica em vez de instrução no
README**. Instrução no README é ignorada; comando que falha, não.

### 2.4 Scripts nomeados por alvo, não por ferramenta

```
dev:studio   dev:docs   dev:www   dev:design-system
build:studio build:docs
test:ui      test:studio  test:docs
e2e:docs:a11y
```

O nome diz o que roda, não como. Quem chega no projeto lê `package.json` e entende o
mapa sem abrir o Turbo.

---

## 3. O que é inferência, não verificação

Marcado explicitamente porque não foi conferido nesta leitura:

- **Cobertura de teste** — existem `tests/`, `evals/`, `e2e/` e scripts de teste. O
  *percentual* não foi medido. Nenhum número deve ser citado.
- **Fluxo interno de auth/realtime** — o `README` descreve GoTrue, PostgREST, Realtime,
  mas esses são **repositórios separados**, não estão neste monorepo. O código não foi
  lido.
- **Qualidade dos componentes de `packages/ui`** — a pasta existe; o conteúdo não foi
  aberto.

---

## 4. Aplicabilidade aos repos do Rafael — leitura honesta

Aqui está o ponto que a primeira análise passou por cima.

**Quase nada disso se aplica hoje.** Os repositórios atuais são:

| Repo | O que é | Tamanho |
|---|---|---|
| `metodologia-claude` | documentação (markdown) | 3 arquivos |
| `ampliar-corretora` | documentação + hook | 345 linhas de md |
| `receitanet-cookie-renewer` | 1 script Python + Docker | 236 linhas |

Monorepo, Turbo, design system e camada `data/` resolvem problema de **coordenação entre
muitas pessoas e muitos apps**. Aplicar isso a um script de 236 linhas adiciona só
cerimônia — pastas vazias, build tool para nada, sete arquivos de config para um `.py`.

### O que aproveitar agora (real)

1. **Trava mecânica > instrução** (§2.3) — já é praticado no `ampliar-corretora` e é o
   padrão mais forte que os dois projetos compartilham. Vale reforçar, não copiar.
2. **Scripts nomeados por alvo** (§2.4) — cabe em qualquer projeto, inclusive num
   `Makefile` de 5 linhas.
3. **Separar busca-de-dado de uso-de-dado** (§2.1) — o `renew.py` já faz isso sem nome:
   `renovar()` busca, `atualizar_credential_n8n()` publica, `alertar_rafael()` notifica.
   O padrão já está certo; só não está nomeado.

### O que guardar para depois

Monorepo com `catalog:` e camada `data/` — no dia em que existir um app de verdade com
front + back e mais de uma pessoa mexendo. Antes disso, é resposta procurando pergunta.

---

## 5. Como reproduzir esta análise

```sh
git clone --depth 1 https://github.com/supabase/supabase
cd supabase
ls apps/
cd apps/studio && ls -d */
grep -rn ": any" . --include="*.ts" --include="*.tsx" | wc -l
```
