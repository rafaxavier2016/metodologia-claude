# ampliar-corretora — análise

| | |
|---|---|
| **Repositório** | github.com/rafaxavier2016/ampliar-corretora |
| **Visibilidade** | privado |
| **Commit analisado** | `3db2992` (clone `--depth 1` de 2026-08-22) |
| **Analisado em** | 2026-08-22 |

---

## Estrutura real — `find . -type f`

```
README.md                   86 linhas
CLAUDE.md                  124 linhas
DIARIO.md                   72 linhas
docs/LISTA_PENDENCIAS.md    63 linhas
.githooks/pre-commit
.gitignore
```

**Não é um repositório de código.** É um canal de comunicação entre máquinas e entre
sessões de Claude — decisões, pendências e diário. 345 linhas de markdown, zero linha
de aplicação.

Isso importa para calibrar expectativa: nenhum padrão de arquitetura de software
(monorepo, camadas, design system) tem o que fazer aqui.

---

## O que este repo faz de melhor

### 1. Trava mecânica em vez de boa intenção

`.githooks/pre-commit` bloqueia commit que contenha CPF, CNPJ, token do GitHub, chave de
API, JWT, senha ou string de conexão com credencial. Sete padrões, mensagem de erro que
explica o que fazer, e `exit 1`.

A frase que sustenta o desenho está no próprio hook:

> *Conservador de propósito — bloquear à toa é melhor que deixar passar.*

É o padrão mais valioso dos três repositórios. Vale a mesma leitura do `preinstall:
only-allow pnpm` do Supabase: instrução em README é ignorada, comando que falha não é.

### 2. `.gitignore` que bloqueia por padrão

Em vez de listar o que excluir, exclui categorias inteiras que *podem* conter dado de
cliente — imagem, vídeo, planilha, banco, `*cookie*`, `*token*` — e depois reabre
exceção explícita para `*.md`.

Inverter o default (negar tudo, liberar o conhecido) é a postura certa para material que
vem de portal de seguradora.

### 3. README que explica o *porquê* da regra

> *"Privado não é seguro — é privado do público."*
> *"O histórico do git é irreversível. Um CPF que entrar num commit fica lá para sempre."*

Regra com motivo é regra que sobrevive. Regra sem motivo vira `--no-verify`.

---

## 🟠 Achado 1 — a trava não viaja no clone

O README já sinaliza:

> ⚠️ A terceira linha não é opcional. O hook não viaja sozinho no clone.

Correto e bem avisado — mas continua dependendo de alguém lembrar de rodar
`git config core.hooksPath .githooks`. É instrução, não trava; exatamente o que o resto
do repositório evita.

Fecha assim: um `setup.sh` de duas linhas na raiz, que o README manda rodar em vez de
mandar copiar o comando.

```sh
#!/bin/sh
git config core.hooksPath .githooks
echo "✅ hook instalado — confira: git config core.hooksPath"
```

Não é uma trava perfeita (ainda dá para não rodar), mas troca "lembrar de um comando"
por "rodar o script do setup", que é o que todo mundo faz por hábito.

---

## 🟠 Achado 2 — dois furos nos padrões do hook

Confirmado por teste, com a linha real do `docker-compose.yml` do
`receitanet-cookie-renewer`:

```
UAZAPI_TOKEN_ALERTA: <uuid>     → NÃO É BLOQUEADO
```

Duas causas:

1. O padrão exige `access[_-]?token` ou `api[_-]?key`. Nome de variável real quase nunca
   segue esse formato — `UAZAPI_TOKEN_ALERTA`, `N8N_API_KEY`, `TOKEN_WPP` passam todos.
2. Não há regra para **UUID solto**, que é o formato de metade dos tokens de serviço
   brasileiro (UazAPI, Evolution, Z-API).

Sugestão:

```sh
check "Token/segredo" '[A-Za-z0-9_]*(token|secret|key|senha|password|cookie)[A-Za-z0-9_]*[[:space:]]*[:=][[:space:]]*["'"'"']?[A-Za-z0-9_@#$%^&*!.-]{8,}'
check "UUID literal"  '[:=][[:space:]]*["'"'"']?[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}'
check "Telefone BR"   '[:=][[:space:]]*["'"'"']?55[0-9]{10,11}["'"'"']?'
```

O terceiro pega telefone pessoal, que é PII e hoje passa livre.

---

## 🟡 Achado 3 — a disciplina não saiu deste repositório

Este é o achado que liga os três repos. O `ampliar-corretora` tem hook e `.gitignore`
defensivo. O `receitanet-cookie-renewer`, que é **público**, tem token real e telefone
commitados (ver `Repositorios/meus/receitanet-cookie-renewer.md`).

A trava existe onde o risco é menor e falta onde é maior. Não é falha de desenho — é
falha de **distribuição**: a regra nasceu num projeto e ficou nele.

Caminho: promover `.githooks/pre-commit` + `.gitignore` a arquivos de metodologia neste
repositório (`metodologia-claude`), com um `install.sh` que instala em qualquer repo
novo. A regra passa a ter um dono único em vez de uma cópia por projeto.

---

## O que não se aplica aqui

Para o registro, evitando cerimônia inútil: monorepo, Turbo, camada `data/`, design
system, CI de build — **nada disso tem lugar num repositório de 345 linhas de markdown**.
O que faria diferença aqui é só o que já existe (hook, gitignore, README com o porquê),
melhor distribuído.
