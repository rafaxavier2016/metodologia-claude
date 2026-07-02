# Boas Práticas de Trabalho — guia genérico

> 📌 **PORTA DE ENTRADA ÚNICA: se só puder ler um arquivo, leia ESTE.** É completo e autossuficiente — 10 princípios + PRÉ-VOO (Seção 0) + disciplina + UX + armadilhas + economia de tokens. (`METODOLOGIA.md` é a versão original congelada das 9 regras; este aqui é o vivo, onde acrescentamos.)
>
> 🌐 **Endereço permanente (qualquer sessão, local ou nuvem):**
> `https://raw.githubusercontent.com/rafaxavier2016/metodologia-claude/master/BOAS_PRATICAS.md`
> Cópia local (quando no PC principal): `docs/BOAS_PRATICAS.md` do repositório principal de projetos — mantenha as duas em sincronia ao editar.
>
> Este é um guia de **boas práticas** para construir software com cuidado e profissionalismo — sugestões, não regras de um projeto específico. Serve para qualquer trabalho: automação, banco de dados, agentes de IA, backend, sites, apps. A ideia é simples: ler **um lugar** no começo de uma sessão e já saber como trabalhar bem, evitando retrabalho e amadorismo.
>
> Cada item abaixo nasceu de um erro real que custou tempo. Trate-os como conselhos de quem já tropeçou — adapte ao seu contexto, mas pense duas vezes antes de ignorar.

---

## 0. A regra-mãe: a disciplina vale DESDE A CRIAÇÃO — e é estrutural, não força de vontade

A maioria dos erros graves **nasce enquanto se constrói** — um script "temporário" com bug, uma suposição que não foi testada, um escopo que se ampliou sozinho. Na hora de "subir pra produção" geralmente já se é cuidadoso; o estrago foi plantado antes. **Por isso toda a disciplina abaixo vale para qualquer toque em estado compartilhado ou persistente** — criar/editar um fluxo, criar uma tabela/coluna, rodar um script que bate em produção, mexer numa credencial — **não só no momento do deploy.**

**Enforcement tem que ser estrutural, não voluntário.** Regra que depende de alguém "lembrar" será violada uma hora — ainda mais sob empolgação ou pressa. Prefira, nesta ordem de confiança:
1. **Trava mecânica** (hook/permissão/gate que bloqueia a ação) — não depende de ninguém lembrar.
2. **Pré-voo visível** (declarado na conversa, abaixo) — quem acompanha consegue cortar antes do estrago.
3. **Canário/reversível por padrão** — quando algo escapa, o raio é mínimo e volta em segundos.
4. **Auditoria de rotina** — pega o que passou em dias, não em semanas.

### PRÉ-VOO — declare ANTES de cada mudança que toca estado real (e PARE se algo for ❌)
- 🔒 **Backup/versão** do que vou tocar (sei reverter).
- 🔎 **Causa com evidência** (log, execução, linha de banco) — nunca chute.
- 🎯 **Escopo = exatamente o que foi pedido.** Não inferir nem ampliar. Ação financeira/sensível só com a **entidade nomeada e confirmada** por quem é dono.
- 🧪 **Testado isolado com caso/dados REAIS** (não um exemplo inventado/"magro").
- ✅ **Resultado validado de verdade** (o efeito concreto aconteceu — não confiar em "success: true").
- ↩️ **Reversão pronta** e à mão.

### Quando a disciplina escorrega (redobre o cuidado nestes momentos)
- Quando você está **confiante/empolgado** para resolver (pula direto pra solução).
- Quando parece **pequeno/trivial** ("é só um scriptzinho", "é temporário").
- Quando está **inferindo escopo** (fazendo mais do que foi pedido).
- Em **sessão longa**, sob pressão de contexto.
Nesses momentos: **mais** pré-voo, não menos.

### Fluxo fixo e como montar o "ambiente seguro" quando não há um pronto
`BACKUP → STAGING/ISOLAMENTO → REGRESSÃO → CANÁRIO → PRODUÇÃO`. Não pule etapas.
Se não existe um ambiente espelho, **você projeta o ensaio isolado** — escolha a forma mais isolada viável: ambiente espelho > branch efêmero > shadow/cópia > tabela/registro temporário > teste offline da lógica > dry-run. O canário (aplicar numa fatia mínima e reversível — ex.: 1 cliente, a própria linha, 1 tabela) é o último degrau antes de generalizar.

### A trava MECÂNICA: hook de pré-voo (como montar e por quê)

A camada mais forte do enforcement não depende de ninguém lembrar — é um **hook** que o próprio harness executa. No Claude Code, um hook `PreToolUse` roda **antes** de uma ferramenta e pode injetar contexto (`additionalContext`) ou pedir confirmação. A ideia: **antes de toda ação de ESCRITA em produção, o checklist de pré-voo é injetado automaticamente** no contexto — você não tem como "esquecer que ele existe".

**Por que vale a pena:** a disciplina escrita num doc só funciona se for lida e seguida; o hook torna o lembrete inevitável, bem no instante da ação. É a diferença entre "regra que se confia que vão seguir" e "trilho".

**Como montar (genérico, adapte ao seu stack)** — em `settings.json`, um `PreToolUse` com:
- **Matcher nas ferramentas de escrita do seu ambiente** — ex.: as ferramentas MCP que alteram banco/automação (migration, update/create/delete, gerenciar credenciais) → lembrete SEMPRE.
- **Matcher em `Bash`** com um comando que lê o stdin e só lembra se o comando casar um **padrão de escrita** (nomes de scripts de deploy/baixa/fix, ou métodos `PUT/PATCH/DELETE` apontando pro host de produção). Leitura (probes, `SELECT`, `ls`) **não** dispara.
- O comando do hook imprime `{"hookSpecificOutput":{"hookEventName":"PreToolUse","additionalContext":"<checklist de pré-voo>"}}`. Mantenha **não-bloqueante** (só lembra); se quiser mais rígido para ações destrutivas, use `permissionDecision:"ask"` (o harness pede aprovação ao dono, mostrando o checklist).

**Cuidados:** (1) prefira **conservador** — lembrar "à toa" é melhor que deixar passar; (2) garanta que **leituras não disparam** (filtre por padrão de comando); (3) gerencie/desligue pelo comando `/hooks`; (4) `settings.json` é **por máquina** — cada ambiente precisa montar o seu (não viaja junto com este doc); (5) teste o comando do hook isolado (pipe um payload de exemplo) antes de confiar — hook que silenciosamente não faz nada é pior que nenhum.

---

## 1. Princípios de engenharia (o coração)

1. **Pesquise antes de construir.** Uma hora lendo a documentação oficial, o GitHub e o que o mercado já resolveu costuma poupar semanas. Antes de escrever um integrador do zero, pergunte: já existe uma API/biblioteca pronta para isso?

2. **Desenhe antes de codar.** Para qualquer fluxo novo, comece por um rascunho — exemplos de uso reais (diálogos, entradas/saídas esperadas) e um diagrama. Combine o desenho com quem é dono do resultado **antes** da primeira linha de código. É muito mais barato corrigir um diagrama do que um sistema.

3. **Versione antes de mexer.** Backup/export + commit **antes** de toda sessão de edição. Nunca toque em produção sem ter um caminho de volta claro. "Como eu reverto isso?" deve ter resposta antes da mudança, não depois.

4. **Nada sobe sem teste de regressão.** Mudança só vai para produção depois de uma bateria de testes passar de forma estável (idealmente repetível, várias vezes verde). Todo bug real que você corrige vira um caso de teste permanente — para nunca voltar despercebido.

5. **Teste num ambiente separado primeiro.** Edite e valide em staging (ou cópia/sandbox). Produção só recebe o que já foi promovido depois de testado. Em fase de aprendizado de um sistema novo, fique só no ambiente seguro.

6. **Dado de teste não mora no banco de verdade.** Use marcadores óbvios de teste (prefixos, nomes claramente fictícios) e **limpe tudo ao fim de cada sessão**. Registros de teste esquecidos confundem o sistema e as pessoas por muito tempo.

7. **Lógica de negócio é do código, não do modelo de linguagem.** Deixe o LLM fazer o que ele faz bem — entender intenção, extrair dados, redigir texto. Decisões (o que fazer, quanto cobrar, qual caminho seguir) devem morar em código determinístico: condicionais, máquina de estados, SQL fixa, templates. Erros se acumulam: encadear vários passos probabilísticos derruba a confiabilidade total rápido.

8. **Silêncio também é falha.** O que é crítico precisa avisar quando para de funcionar — alerta em erro, "dead-man switch" em tarefa agendada. O pior cenário é algo quebrado por semanas sem ninguém perceber.

9. **Segurança acima de tudo.** Segredos (token, senha, API key, cookie, dado pessoal) nunca entram em arquivo versionado, prompt, chat, print ou material público — eles vivem em cofre de credencial ou variável de ambiente. Nada de publicar repositório com segredo no histórico; nada de copiar arquivos de credencial entre máquinas. Em material público, evite expor nomes/dados internos — use termos institucionais. Achou um segredo exposto? Pare tudo e avise na hora.

10. **Meça o DEPOIS, não só o antes.** *(adicionado 2026-06-08)* As regras 1-9 garantem que nada **quebrou**; nenhuma delas garante que algo **melhorou**. Toda mudança de comportamento (prompt, fluxo, régua, copy) deve ter uma **métrica de sucesso definida ANTES** ("essa mudança melhora X") **e verificada DEPOIS** com dado real de produção (taxa de conversão, tempo de resposta, erros por dia). Sem isso, avalia-se mudança "no olho" — e o olho aprova o que parece bom, não o que funciona. Se o sistema já registra eventos, a métrica costuma ser uma consulta simples; o que falta é o hábito de defini-la e olhá-la.

---

## 2. Disciplina de execução (o dia a dia)

- **Plano de ação visível e aprovado antes de executar.** Apresente o plano onde a pessoa consegue ler e responder, não escondido num popup. Trabalhe em **ondas** com critério de aceite explícito para cada uma — "esta onda está pronta quando X".
- **Diagnostique antes de consertar.** Reproduza o bug, ache a causa raiz com **evidência** (um log, uma execução, uma linha do banco), e só então mude. Nunca conserte "no chute" — um palpite que parece funcionar mascara o problema real.
- **Confirme que a escrita aplicou de verdade.** Depois de salvar/atualizar, leia de volta e confira. Não confie que "deu certo" porque a chamada retornou 200.
- **Valide o resultado real, não um flag de sucesso.** "sucesso: true" não significa que funcionou — significa que a chamada não deu erro. Cheque o efeito concreto (o dado chegou? o número bate? o boleto baixou?). Já tivemos automação que respondia "sucesso" e não fazia nada.
- **Teste com o caso real, não com um exemplo inventado.** Um payload/entrada "magro" que você montou na mão passa por caminhos que o dado real não passa. Capture e use a entrada de verdade — é a diferença entre "passou no meu teste" e "funciona".
- **Relate com honestidade.** Teste que falhou é "falhou", não "quase passou". Pulou uma etapa? Diga. Confiança fingida custa caro depois.
- **Avise antes de violar uma boa prática.** Se for preciso quebrar uma destas (pressa, exceção real), diga explicitamente "vou abrir mão de X por causa de Y" — vigilância nos dois sentidos.

---

## 3. Padrões de UX em mensageria (quando aplicável)

Decisões que funcionam bem para conversas automatizadas (WhatsApp e similares):

- **Confirmação = botões** (poucos, rótulo curto; o detalhe completo vai no corpo da mensagem, porque rótulo de botão costuma ter limite de caracteres).
- **Escolha entre muitas opções = lista** com rótulos curtos e detalhes no corpo.
- **Dê sempre uma saída livre** ao usuário — uma opção tipo "✍️ Outro — me explica o que precisa", para quem não se encaixa nas opções.
- **Nunca peça de novo um dado que a pessoa já deu** — leia o que você tem antes de perguntar.
- **Vários dados de uma vez:** um por linha, com marcador.
- **Datas no formato local consistente** (ex: DD/MM/AAAA). Tom humano e natural, nunca robótico.
- **Em relatórios, cada métrica em sua própria linha** — listas inline com vírgulas viram parede de texto.

---

## 4. Armadilhas de ferramentas (documente as suas)

Toda ferramenta tem comportamentos que só se aprende apanhando. A boa prática genérica é: **quando descobrir uma armadilha, anote-a num lugar que a próxima sessão vai ler.** Abaixo, exemplos reais que ilustram os tipos de pegadinha a vigiar — úteis mesmo fora do contexto original:

- **Cache de runtime depois de salvar.** Alguns sistemas salvam a mudança no banco mas continuam executando a versão antiga em memória. Sintoma traiçoeiro: você edita, a leitura confirma a edição, e o comportamento não muda. Saiba como forçar o reload (recarregar/reativar) e confirme que pegou.
- **Operações que "mesclam" vs "substituem".** Saiba se um passo sobrescreve o objeto inteiro ou só os campos que você mandou — ler o estado anterior explicitamente evita perder dados.
- **Resultado vazio que mata a cadeia em silêncio.** Em pipelines, um passo que não retorna nada pode interromper tudo sem erro visível. Garanta que passos críticos sempre emitam algo e continuem.
- **Texto livre como parâmetro.** Conteúdo do usuário (com vírgulas, quebras de linha, aspas) quebra parser de parâmetros e regex ingênuos. Extraia/sanitize o que precisa **antes** de passar adiante; valide com a entrada real, não com texto limpo.
- **Encoding.** Garanta UTF-8 ponta a ponta — acentos quebram em fronteiras de ferramenta (terminais, libs HTTP) de formas silenciosas.
- **Não derrube serviços compartilhados por engano.** Operações de "desligar/religar" via API podem ter efeitos colaterais (perder um endpoint, dropar uma conexão) que a interface gráfica não tem. Quando uma mudança afeta algo que muitos consomem, prefira o caminho mais seguro e teste o impacto isolado primeiro.
- **Dados duplicados entre fontes.** Quando o mesmo fato chega por dois caminhos (ex: um pagamento que aparece no gateway **e** no sistema de baixa), somar as fontes conta em dobro. Para qualquer número que importa, **deduplique por uma chave de negócio** (ex: identidade + período + valor) e tenha uma única consulta canônica — não deixe o número ser improvisado a cada vez.
- **Camadas aditivas de prompt/config são dívida técnica.** Corrigir um prompt/configuração empilhando blocos "este sobrescreve o anterior" funciona no curto prazo, mas depois de várias camadas o documento vira arqueologia: instruções contraditórias convivem, contexto é desperdiçado e ninguém sabe mais o que vale. **Consolide periodicamente** num documento único e limpo — e faça essa consolidação COM uma bateria de regressão pronta, porque achatar camadas é exatamente o tipo de mudança que quebra comportamento em silêncio.

---

## 5. Economia de tokens (custo de LLM é custo de engenharia) *(adicionado 2026-07-02)*

Todo prompt reenviado, toda camada redundante e todo histórico sem poda é dinheiro saindo em silêncio. Práticas, em ordem de impacto:

1. **Prompt caching primeiro — a maior alavanca (50–90% no contexto repetido).** Regra de ouro: **estável no TOPO, variável no FIM.** Qualquer byte dinâmico no início do prompt (timestamp, nome do cliente, dado da sessão) invalida o cache de tudo que vem depois. System prompt e definições de ferramentas ficam idênticos entre chamadas; o que muda entra depois deles. Ao editar um prompt de produção, pergunte: "isso quebra o prefixo estável?"

2. **Corte redundância, não clareza.** Prompt enxuto ≠ prompt telegráfico. Encurtar instruções removendo artigos e conectivos ("estilo homem das cavernas") economiza centavos e cria ambiguidade — e instrução ambígua em produção custa horas de debug e comportamento errado, muito mais caro que os tokens poupados. O desperdício real está em: camadas sobrepostas ("este bloco sobrescreve o anterior"), exemplos além do necessário e a mesma regra repetida com palavras diferentes. Consolidar isso corta 30–40% sem perder precisão. Estilo telegráfico só para dados estruturados entre sistemas (JSON, tags, logs) — nunca para instruções.

3. **Janela de histórico com poda.** Conversa longa não entra inteira a cada turno: últimas N mensagens + resumo do resto. Reenviar a conversa completa faz cada turno novo repagar todos os anteriores.

4. **Modelo certo por tarefa.** O spread de preço dentro do portfólio de um mesmo provedor é de 8–23x. Classificação, extração e turno trivial → modelo pequeno; raciocínio multi-passo e precisão crítica → modelo forte. Roteie por complexidade quando o volume justificar a manutenção de dois caminhos.

5. **`max_tokens` definido em toda chamada.** Limite a saída ao necessário; em milhares de chamadas a diferença acumula.

6. **Meça custo por unidade de negócio.** Tokens por conversa, por cliente, por execução — a resposta da API já traz `usage`; grave junto do registro da conversa. Sem isso, otimização é chute (padrão da indústria: a quase totalidade do gasto de IA fica enterrada em contas genéricas que ninguém atribui). Conecta direto com o princípio 10: custo é métrica de "depois".

7. **Batch/assíncrono quando a resposta não é urgente.** APIs em lote costumam custar metade do preço para análise em massa, relatórios e reprocessamento.

---

## 6. Como uma nova sessão começa bem

1. Leia este arquivo inteiro (boas práticas) e qualquer guia específico do projeto.
2. Entenda o estado atual antes de propor mudança — o que já existe, o que está em produção, o que está quebrado.
3. **Cheque se a infraestrutura da disciplina existe NESTE projeto** *(adicionado 2026-06-08)*: há staging? bateria de regressão? backup/export versionado? alerta de erro e dead-man switch? Uma metodologia construída para um projeto **não vale automaticamente para o próximo** — cada projeto novo precisa herdar os mecanismos, não só as regras. Se algum mecanismo não existe, **criá-lo é prioridade zero antes de mudanças de comportamento**: regra sem trava é violada na primeira sessão longa.
4. Apresente um plano em ondas com critério de aceite, e só execute depois de combinado.
5. Faça backup/versão antes de tocar em qualquer coisa — e **declare o PRÉ-VOO (seção 0) antes de cada mudança, inclusive ao criar/editar** (não só no deploy).
6. Trabalhe no ambiente seguro, valide com casos reais, confirme o resultado de verdade, limpe os dados de teste.
7. Ao fim, **verifique a métrica de sucesso** definida antes da mudança (princípio 10) — e documente o que aprendeu, inclusive as armadilhas, para a próxima sessão começar mais esperta que esta.

---

*Este guia é um companheiro genérico de boas práticas. Um projeto pode ter, além dele, regras específicas próprias (ambientes, nomes, ferramentas, convenções) que detalham como estes princípios se aplicam ali.*
