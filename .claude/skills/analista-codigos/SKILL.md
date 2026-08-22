---
name: analista-codigos
description: Analisa a estrutura e a arquitetura de um repositório (de terceiros ou próprio) e salva o resultado em Repositorios/. Use quando o pedido for entender como um projeto foi montado, extrair padrões replicáveis, ou auditar a organização de um repo. Só lê — nunca altera o repositório analisado.
---

# Analista de Códigos

Analisa um repositório, extrai o que dá para aprender dele, e **salva a análise em
`Repositorios/`** do repositório de metodologia.

## Regra que define este skill

**Nada de estrutura inventada.** Todo caminho de diretório, contagem, nome de arquivo e
número citado tem que sair de um comando executado nesta sessão, colado literalmente.

Não vale escrever "projetos assim normalmente têm `src/services/`" porque é plausível.
Se não foi verificado com `ls`, `find` ou `grep`, ou vai para a seção "inferido", ou não
entra na análise.

> Isso não é zelo abstrato. A primeira análise do Supabase descreveu
> `apps/studio/src/{components,services,hooks}` — o repositório não tem `src/` nenhum —
> e afirmou "zero uso de `any`" quando há 645 ocorrências. Padrão copiado de estrutura
> inexistente entra no projeto novo e só aparece semanas depois.

## Procedimento

### 1. Obter o repositório

Terceiro, público:

```sh
git clone --depth 1 https://github.com/OWNER/REPO /tmp/analise/REPO
```

Próprio e privado: `add_repo` primeiro, depois clonar no caminho que a ferramenta
indicar.

### 2. Mapear antes de ler

Nunca abrir arquivo antes de ter o mapa. Nesta ordem:

```sh
ls -A                                    # raiz
find . -path ./.git -prune -o -type f -print | head -40
find . -maxdepth 2 -type d -name node_modules -prune -o -type d -print
cat package.json / pyproject.toml / go.mod   # o que existir
```

**Calibrar o esforço pelo tamanho.** Repositório de 5 arquivos não precisa de análise
arquitetural — precisa de leitura completa. Repositório de 17 mil arquivos precisa de
amostragem declarada.

### 3. Ler o que decide

- `README.md` — o que o projeto diz que é
- manifesto de dependências — toolchain, versões, scripts
- 2–3 arquivos de código no caminho principal
- config de CI, hooks, `.gitignore`

### 4. Verificar toda afirmação numérica

Antes de escrever qualquer número, rodar o comando:

```sh
grep -rn ": any" src --include="*.ts" | wc -l
find . -name "*_test.go" | wc -l
```

Sem comando, sem número.

### 5. Escrever em `Repositorios/`

Terceiros → `Repositorios/<owner>-<repo>.md`
Próprios → `Repositorios/meus/<repo>.md`

Reanálise **sobrescreve** o arquivo. Não cria `-v2`.

Seções obrigatórias:

1. Tabela de cabeçalho: repositório, visibilidade, commit analisado, data, escopo lido
2. **O que foi verificado** — com o comando e a saída colada
3. **Padrões que valem a pena** — cada um ancorado em arquivo:linha
4. **O que é inferência, não verificação** — explícito, separado
5. **Aplicabilidade** — o que dá para usar *e o que não dá, com o motivo*

### 6. Atualizar o índice

Adicionar a linha em `Repositorios/_INDICE.md`. Se o padrão já apareceu em outra
análise, promover para a seção "Padrões recorrentes".

### 7. Commitar

```sh
git add Repositorios/ && git commit -m "Análise: OWNER/REPO"
```

Análise não commitada morre com a sessão. É o passo que dá sentido a todos os outros.

## Duas coisas que não podem faltar

### Dizer o que *não* se aplica

A parte mais útil e a mais fácil de pular. Monorepo com Turbo é excelente para sete apps
e trinta pessoas; num script de 236 linhas é cerimônia pura — pastas vazias e config
para nada.

Toda análise fecha dizendo o que ignorar e por quê. Análise que só elogia padrão gera
projeto inchado.

### Higiene de segredo

`Repositorios/` mora num repositório **público**. Ao analisar repo próprio e encontrar
token, cookie, telefone, hostname interno ou id de credencial: registrar **o arquivo e a
linha**, nunca o valor.

Encontrou segredo commitado: dizer na análise que rotacionar vem antes de remover — o
histórico do git é irreversível, apagar o arquivo não desfaz a exposição.

## Escopo

Só leitura. O skill nunca edita, commita ou faz push no repositório analisado — o único
lugar onde escreve é `Repositorios/` do repo de metodologia.

## Uso

```
/analista-codigos                                  # pergunta qual repo
/analista-codigos supabase/supabase                # terceiro
/analista-codigos rafaxavier2016/ampliar-corretora # próprio
```

Antes de analisar de novo: `cat Repositorios/_INDICE.md` — pode já estar feito.
