# Metodologia de Trabalho — Claude

> **Instrução pro Claude:** leia este arquivo INTEIRO no início de TODA sessão, antes de
> qualquer mudança. Estas regras valem pra qualquer projeto da equipe (automação n8n,
> Supabase, agentes IA, sites, apps, scripts). Foram aprendidas com erros reais que
> custaram semanas — não são sugestões.
>
> *Base: metodologia consolidada em 12/06/2026. Esta é a versão portável/sanitizada pra
> compartilhar. Os detalhes vivos de cada projeto ficam no `CLAUDE.md` do próprio projeto.*

---

## AS 9 REGRAS DE ENGENHARIA (invioláveis)

1. **PESQUISAR ANTES DE CONSTRUIR.** 1h de pesquisa (docs oficiais, GitHub, mercado) antes
   de qualquer build novo. Já mantivemos semanas de scrape pra algo que a API oficial
   fazia por R$ 29,90/mês.
2. **DESENHAR ANTES DE BUILDAR.** Fluxo novo = sample dialogs + diagrama Mermaid em
   `docs/spec_*.md`, APROVADO pelo dono do projeto ANTES do primeiro node/linha de código.
   Edge cases viram branches explícitos no desenho.
3. **VERSIONAR ANTES DE MEXER.** Backup/export + `git commit` ANTES de toda sessão de
   edição. Nunca editar produção sem ter como voltar.
4. **NADA SOBE SEM REGRESSÃO.** Mudança em produção só depois da bateria de testes passar
   no staging (3x verde). Cada bug real vira caso permanente de teste.
5. **STAGING PRIMEIRO.** Editar e testar em ambiente de staging. Produção só recebe
   promoção de coisa testada. Em treinamento/onboarding: trabalhe SOMENTE no staging.
   **➜ Detalhado na seção "STAGING" abaixo — é a regra central.**
6. **DADO DE TESTE NUNCA FICA NO BANCO.** Teste usa números/nomes claramente marcados
   (prefixo de teste); TODA sessão termina deletando o que criou. Já tivemos 197 registros
   fantasma confundindo o sistema por semanas.
7. **LLM NÃO DECIDE LÓGICA DE NEGÓCIO.** LLM só classifica intenção, extrai dados e redige
   texto. Quem decide é código: state machine + Switch + SQL + templates. Erro composto:
   4 LLMs encadeados a 95% = 81% de acerto — inaceitável.
8. **SILÊNCIO TAMBÉM É FALHA.** Cron crítico tem dead-man switch; erro tem alerta. Já
   ficamos 3 semanas com uma ferramenta quebrada sem ninguém perceber.

## 🔴 REGRA 9 — SEGURANÇA (acima de tudo)

- **NUNCA** escreva token, senha, API key, cookie ou CPF/dado de cliente em: arquivo
  versionado, prompt de workflow, chat, print, site ou deck. Segredo vive em credencial do
  n8n ou variável de ambiente.
- **NUNCA** dê push de repositório que contenha segredos no histórico. Publicação = só
  snapshot sanitizado com gate de verificação.
- **NUNCA** copie arquivos de credenciais (`~/.claude.json`, exports decriptados) entre
  máquinas.
- **NUNCA** exponha nome do fundador, marca pessoal ou dados internos em material público
  — usar termos institucionais ("o time", "nosso especialista").
- Achou segredo exposto? PARE tudo e avise imediatamente.

---

## STAGING — a regra central, detalhada

**Princípio:** nada toca a produção sem antes ter sido provado num ambiente de ensaio
isolado. A regra #5 é tão importante que ganha esta seção própria.

**Sua responsabilidade — você PROJETA o staging.** Não importa o tipo de trabalho
(workflow, app, banco, script, config, conteúdo): antes de executar, você desenha o
ambiente de ensaio que melhor se aplica àquela tarefa, apresenta pro dono do projeto, e só
então roda. NÃO existe "esse caso não dá pra testar antes". Sempre dá — muda só a forma. Se
você achar que não dá, é sinal de que ainda não pensou no staging certo: pense de novo.

### Os 4 atributos de um bom staging

1. **Isolamento** — o que você faz no ensaio NÃO pode afetar produção nem usuários reais.
   Ambiente, credenciais, destinos e dados separados.
2. **Paridade** — o ensaio precisa parecer com produção o suficiente pra que "passou no
   staging" signifique algo. Quanto mais perto do real, mais confiável.
3. **Dados seguros** — dados de teste, cópias mascaradas ou registros-canário controlados.
   Nunca dado real sensível no ensaio; nunca dado de teste na produção (regra #6).
4. **Reversibilidade** — antes de promover, tenha o caminho de volta pronto (backup,
   commit, snapshot). Se a promoção der errado, você desfaz em um passo (regra #3).

### Como BOLAR o staging certo (procedimento, pra qualquer tarefa)

Antes de executar qualquer mudança, responda na conversa:

1. **Qual o "blast radius"?** O que pode quebrar se der errado? Quem é afetado? Isso define
   o quão sério o staging precisa ser.
2. **Qual a forma de ensaio mais isolada que ainda tem paridade útil?** Escolha a mais
   forte que for viável, nesta ordem:
   - **Ambiente espelho dedicado** (instância/projeto separado de produção) — ideal.
   - **Branch / ambiente efêmero** (preview, branch de banco, worktree) — ótimo.
   - **Shadow / paralelo** (roda junto da produção sem efeito colateral real) — bom.
   - **Mock / sandbox** (dependências externas falsas, destinos redirecionados) — ok.
   - **Dry-run** (executa a lógica mas não comita o efeito final) — mínimo aceitável.
3. **Como vou validar?** Defina o critério de aceite ANTES de rodar — incluindo re-rodar
   os casos que já funcionavam (regressão, regra #4).
4. **Qual o caminho de volta?** Backup/commit/snapshot feito ANTES de promover.

### Fluxo de promoção (sempre nesta ordem)

```
1. BACKUP     → ponto de retorno garantido (commit / export / snapshot)
2. STAGING    → aplica e valida no ensaio isolado (3x verde)
3. REGRESSÃO  → re-roda os casos que já passavam; nada quebrou
4. CANÁRIO    → 1 caso real controlado, monitorado de perto (idealmente com o dono junto)
5. PRODUÇÃO   → libera de fato, com o dead-man switch (regra #8) ativo
```

Nunca pule do passo 1 direto pro 5. Se algum passo falhar, pare e avise — não force.

### Exemplos de staging por tipo de trabalho (referência, não amarra)

- **Workflow / automação (ex: n8n):** instância/projeto de staging separado, com
  credenciais e webhooks próprios (workflows com prefixo tipo `[STG]`); ou redirecionar
  destinos externos pra mock. Promoção via export → import. Backup do workflow antes.
- **App web / serviço:** ambiente de staging ou deploy preview com env vars próprias e
  banco separado. Promoção via merge/deploy. Feature flag pra ligar gradual.
- **Banco de dados / migration:** branch de banco ou shadow DB; roda a migration lá,
  confere schema e dados, só então aplica no real. Dump/backup antes.
- **Script / job:** flag `--dry-run` que loga o que FARIA sem efetivar; ou aponta pra
  base/bucket de teste. Depois roda pra valer num subconjunto pequeno primeiro.
- **Config / infra:** aplica numa máquina/recurso de teste; valida; versiona o antes e o
  depois pra poder reverter.

> Se nenhum exemplo encaixar, é seu papel **inventar** o ensaio que respeite os 4
> atributos. Apresente o desenho do staging junto com o Plano de Ação.

---

## Disciplina de execução (como trabalhamos)

- **Plano de Ação sempre NA CONVERSA** (cabeçalho `## Plano de Ação`), aprovado antes de
  executar. Nunca só em popup.
- Trabalho em **ondas** com critério de aceite explícito por onda.
- Backup pré e pós mudança em `backups/AAAA-MM-DD_descricao/`.
- **GET-after-PUT sempre**: nunca confie que uma escrita aplicou — leia de volta e confirme.
- **Diagnóstico antes de conserto**: reproduza o bug, ache a causa raiz com EVIDÊNCIA (log,
  execução, linha de banco), só então mude. Nunca conserte "no chute".
- **Relate com honestidade**: teste que falhou é "falhou", não "quase passou". Pulou etapa?
  Diga. Erro no meio de uma aplicação? Cheque o estado real antes de reaplicar — aplicação
  parcial é o pior cenário; se houver, considere restaurar do backup.
- **Vigilância bidirecional**: avise o dono do projeto sempre que estiver prestes a violar
  uma destas regras ("cometer um pecado") — e quando ELE pedir algo que viole uma regra,
  lembre-o da regra e do caso-símbolo ANTES de obedecer.
- **Recomende, não faça catálogo**: quando houver decisão, dê SUA recomendação com o porquê
  — não uma lista neutra de opções.
- **Idioma: português do Brasil.**

---

## UX padrão WhatsApp (decisões permanentes — se o projeto tiver atendimento)

- **Confirmação** = botões (máx 3, rótulo curto; detalhe completo no CORPO da mensagem).
- **Seleção múltipla** = lista "Ver opções" com rótulos curtos; detalhes no corpo.
- **Cliente sempre tem saída livre**: 3º botão/último item `✍️ Outro — me explica o que precisa`.
- Nunca re-pedir dado que o cliente já informou (ler banco antes de perguntar).
- Pedido de múltiplos dados: um por linha com marcador.
- Datas sempre DD/MM/AAAA. Tom caloroso, humano, natural — nunca robótico.
- Cada métrica em LINHA PRÓPRIA em relatórios (sem vírgulas inline).

---

## Quirks do n8n que VÃO te morder (se o projeto usar n8n — aprendidos a tapa)

- **PUT salva no BANCO mas o runtime pode continuar executando a versão VELHA em cache.**
  Sintoma: você edita, o GET mostra a edição, e o comportamento não muda. Após editar
  workflow ATIVO: `POST /workflows/{id}/deactivate` + `/activate` e confirmar `active:true`.
  SEMPRE GET-after-PUT + reload.
- Node Postgres/Redis **substitui** o item (não mescla) → leia dados anteriores via
  `$('NomeDoNode').first()`.
- Postgres com 0 linhas mata a chain silenciosamente → `alwaysOutputData: true` em todo
  node de cadeia crítica, com `onError: continueRegularOutput`.
- **`queryReplacement` do node Postgres faz split por VÍRGULA** → nunca passe texto livre
  (mensagem de cliente) como parâmetro: a vírgula trunca o `$N`. Pior: expressão que resolve
  pra string VAZIA é DROPADA → erro `there is no parameter $N`. Solução: extraia o que
  precisa NA EXPRESSÃO (JS roda antes do split) e passe valor curto com default não-vazio
  (ex: `... .match(/dia\s*(\d{1,2})/)?.[1] || '0'`).
- `ORDER BY CASE` direto após `UNION ALL` é SQL inválido → embrulhe em subselect.
- Corpo de request HTTP DEVE ser UTF-8 (curl do Windows quebra acentos); jsonBody com
  `JSON.stringify(...)`, nunca template literal.
- n8n novo bloqueia `$env`/`process.env` em Code nodes e bloqueia campo de dados chamado
  `arguments` (sandbox) — renomeie pra `args`.
- Instância que reinicia marca execuções em voo como `crashed` (todas com mesmo stoppedAt).
  Com `saveExecutionProgress: true`, execuções crashed ainda guardam dados node a node —
  ouro pra debug.
- API pública rejeita `settings` extras no PUT → enviar só chaves permitidas
  (executionOrder, saveData*, timezone...).
- Webhook de teste: payload "magro" morre em nodes Merge → fazer replay de payload REAL
  capturado.
