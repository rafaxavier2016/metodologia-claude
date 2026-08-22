# Repositorios — biblioteca de análises

Pasta onde **toda** análise feita pelo `/analista-codigos` fica salva e versionada.

## Por que existe

Análise que não é salva se perde quando a sessão acaba. O clone vai pro scratchpad
efêmero, o relatório sai no chat, e na sessão seguinte o aprendizado não existe mais —
é preciso reanalisar do zero. Esta pasta é o cofre: o que foi aprendido uma vez fica
disponível para todas as sessões futuras, em qualquer máquina.

## Regra

**Toda análise gera um arquivo aqui e é commitada.** Sem exceção. Se a análise não foi
salva em `Repositorios/`, ela não aconteceu.

## Nomenclatura

```
Repositorios/
├── README.md              ← este arquivo
├── _INDICE.md             ← tabela de tudo que já foi analisado
├── supabase.md            ← análise de github.com/supabase/supabase
├── <owner>-<repo>.md      ← padrão para repos de terceiros
└── meus/                  ← análises dos repos do próprio Rafael
    └── <repo>.md
```

Um arquivo por repositório. Reanálise **sobrescreve** o arquivo e atualiza a data —
não cria `supabase-v2.md`.

## O que cada análise precisa ter

Toda análise segue esta ordem. As duas primeiras seções são obrigatórias e não
negociáveis:

1. **Cabeçalho de verificação** — commit analisado, data, o que foi lido de fato
2. **Verificado vs. inferido** — separação explícita entre o que foi conferido no
   código e o que é leitura/opinião
3. Estrutura real de diretórios (colada de `ls`/`find`, não de memória)
4. Padrões identificados
5. O que dá para aplicar aqui — e o que **não** dá, com o motivo

## Regra nº 1: nada de estrutura inventada

Uma análise só serve se for confiável. Padrão copiado de uma estrutura que não existe
custa mais caro do que não ter análise nenhuma — o erro vai para dentro do projeto
novo e só aparece depois.

Portanto: **todo caminho de diretório, contagem e nome de arquivo sai de um comando
executado**, colado literalmente. Nada de "normalmente projetos assim têm `src/`".
Se não foi verificado, vai para a seção "inferido" — ou não entra.

> Caso real: a primeira análise do Supabase descreveu `apps/studio/src/{components,services,...}`.
> O repositório não tem `src/` nenhum. A estrutura foi escrita de memória, por
> parecer plausível. Foi corrigida em `supabase.md` — e é a razão desta regra existir.

## Regra nº 2: dado sensível não entra

Este repositório é **público**. Vale a mesma disciplina do `ampliar-corretora`:
token, cookie, telefone, hostname interno, id de credencial — nada disso entra numa
análise, nem em exemplo de código, nem em citação de `docker-compose`. Ao analisar
repositório próprio e encontrar segredo, a análise registra **que existe e onde**,
nunca o valor.

## Como usar depois

```sh
# o que já foi analisado
cat Repositorios/_INDICE.md

# procurar um padrão específico entre todas as análises
grep -ril "monorepo" Repositorios/

# antes de começar projeto novo: reler o que se aprendeu
ls Repositorios/
```
