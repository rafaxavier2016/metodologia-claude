# Ideias de apps inovadores com potencial de estourar

> Brainstorm de 21/08/2026. Critério de seleção: dor real e frequente, "por que agora"
> claro (algo mudou em tecnologia ou comportamento que torna a ideia viável hoje),
> e caminho de validação barato — de preferência começando como serviço/automação
> (n8n + IA) antes de virar produto.

## Como ler cada ideia

- **Dor**: o problema que a pessoa já sente (não precisa ser educada para sentir).
- **Por que agora**: o destravamento recente que torna isso possível/barato.
- **Como estoura**: o mecanismo de crescimento (não basta ser útil, precisa se espalhar).
- **MVP barato**: como validar em semanas, não meses.

---

## 1. Secretária de WhatsApp (second brain por áudio)

- **Dor**: brasileiro vive no WhatsApp e manda áudio para si mesmo para não esquecer
  compromisso, ideia, gasto, recado. Nada disso vira ação — vira rolagem infinita.
- **O quê**: um contato no WhatsApp que recebe áudio/texto/foto e devolve organização:
  compromisso vai para a agenda, gasto vira planilha, ideia vira nota pesquisável,
  "me lembra de X" vira lembrete que realmente chega.
- **Por que agora**: transcrição e extração estruturada por LLM ficaram baratas e
  confiáveis; API oficial do WhatsApp acessível; ninguém quer "mais um app" — quer
  inteligência dentro do app que já usa.
- **Como estoura**: o usuário encaminha o contato para amigos ("manda áudio pra esse
  número, é surreal"). Compartilhamento de contato é viral por natureza.
- **MVP barato**: n8n + WhatsApp API + Claude. É literalmente a stack que já domino.

## 2. Tradutor de burocracia

- **Dor**: chegou uma citação judicial, uma multa, um contrato de aluguel, uma carta
  do banco. A pessoa não entende o que diz, o que precisa fazer, nem até quando.
- **O quê**: fotografa o documento → explicação em linguagem simples + "o que fazer
  agora" + prazos extraídos e agendados + nível de urgência.
- **Por que agora**: visão computacional + LLMs leem documento oficial com alta
  qualidade; o custo por documento caiu para centavos.
- **Como estoura**: momentos de pânico geram busca ativa ("recebi uma intimação o que
  fazer") — SEO/ASO captura essa intenção. Cada documento traduzido é compartilhável
  com a família.
- **MVP barato**: fluxo por WhatsApp antes de app. Monetiza por documento ou
  assinatura leve; advogados parceiros como upsell.

## 3. Contador invisível para MEI e autônomos

- **Dor**: milhões de autônomos não fazem gestão nenhuma: nota fiscal, DAS, quanto
  sobrou no mês, quanto guardar para imposto. Contador é caro; planilha ninguém mantém.
- **O quê**: manda foto do comprovante ou áudio ("recebi 300 do serviço da Ana") via
  WhatsApp → livro-caixa automático, lembrete de DAS, resumo mensal ("você lucrou X,
  separa Y pra imposto"), emissão de nota guiada.
- **Por que agora**: extração estruturada por IA tornou o "lançamento contábil por
  foto" trivial; a formalização MEI cresce todo ano.
- **Como estoura**: rede de indicação entre autônomos (diarista indica manicure que
  indica eletricista). Preço de app (R$ 20–30/mês), não de contador.
- **MVP barato**: n8n + planilha + WhatsApp para os 20 primeiros clientes pagantes.

## 4. Central da escola dos filhos

- **Dor**: pais afogados em grupos de WhatsApp da escola, agenda de papel, bilhetes,
  lista de material, "amanhã é dia do brinquedo". Quem tem 2+ filhos vive em caos.
- **O quê**: encaminha as mensagens do grupo/bilhetes/circulares para o app → ele
  extrai eventos, tarefas e pagamentos, monta o calendário da família e avisa na hora
  certa ("amanhã: camiseta azul, levar fruta").
- **Por que agora**: LLM lê comunicação escolar caótica (texto, foto de bilhete,
  PDF) e estrutura com precisão. Antes disso, era impossível sem digitação manual.
- **Como estoura**: uma mãe organizada no grupo da escola é a melhor propaganda do
  mundo. Efeito de rede por turma: se metade da sala usa, a outra metade entra.
- **MVP barato**: bot de encaminhamento no WhatsApp; app só depois da tração.

## 5. Treinador de conversas difíceis

- **Dor**: as conversas que mais definem a vida — pedir aumento, entrevista, vender,
  terminar relacionamento, dar feedback — a pessoa entra sem nunca ter treinado.
- **O quê**: roleplay por voz com IA que interpreta o interlocutor (o chefe durão, o
  cliente cético, o recrutador) e depois dá feedback: onde você cedeu rápido demais,
  muletas verbais, tom. "Duolingo de habilidades sociais."
- **Por que agora**: voz em tempo real com latência baixa e atuação convincente é
  coisa de 2025+. Antes era chatbot de texto sem emoção.
- **Como estoura**: conteúdo nativo de rede social — clipes de "IA me entrevistando"
  e rankings ("aguentei 4 min de negociação salarial") são altamente compartilháveis.
- **MVP barato**: um único cenário (entrevista de emprego) como produto de nicho, e
  expandir por vertical (vendas, RH, dating).

## 6. Guardião de assinaturas e tarifas

- **Dor**: todo mundo paga assinatura esquecida, tarifa bancária indevida e plano de
  celular pior que a oferta atual. Cancelar/renegociar é chato de propósito.
- **O quê**: conecta e-mail/extrato → detecta cobranças recorrentes → um toque para
  cancelar, e um agente que executa a burocracia (formulários, chats de atendimento,
  protocolo). Modelo: fica com % do que economizar.
- **Por que agora**: agentes que navegam interfaces e conduzem chats de atendimento
  ficaram viáveis; alinhamento perfeito de incentivo (só ganho se você economizar).
- **Como estoura**: "o app se paga sozinho" é o pitch mais fácil de indicar. Print
  de "economizei R$ 87/mês" é viral.
- **MVP barato**: começar como serviço manual-assistido (concierge) para provar a
  economia média por usuário; automatizar depois.

## 7. Check-in diário para idosos que moram sozinhos

- **Dor**: filhos adultos vivem com culpa e medo — "será que meu pai está bem?".
  Ligações diárias não escalam; câmeras são invasivas.
- **O quê**: uma ligação de voz diária, calorosa e natural, com a IA ("bom dia seu
  João, dormiu bem? tomou o remédio da pressão?"). Detecta sinais de alerta (confusão,
  queda, tristeza) e avisa a família com resumo diário.
- **Por que agora**: voz empática em tempo real com memória de longo prazo; população
  envelhecendo rápido no Brasil e no mundo.
- **Como estoura**: quem paga é o filho, e filhos se indicam entre si em rodas de
  conversa sobre pais idosos. Mídia adora a pauta.
- **MVP barato**: telefonia + voice AI via API; 10 famílias piloto pagantes validam.

## 8. Diagnóstico de casa por vídeo

- **Dor**: barulho estranho na máquina de lavar, infiltração, tomada esquentando.
  A pessoa não sabe se é grave, quanto custa, nem em quem confiar.
- **O quê**: filma o problema → IA diagnostica (com nível de confiança), estima custo
  justo da região, diz se dá pra resolver sozinho (com passo a passo) ou chama um
  profissional verificado do marketplace.
- **Por que agora**: modelos multimodais entendem vídeo + áudio (o barulho importa!).
  A ponte diagnóstico→orçamento→profissional nunca existiu num fluxo só.
- **Como estoura**: cada diagnóstico evitando um orçamento abusivo vira história
  contada. Monetiza no marketplace (comissão), diagnóstico gratuito como isca.
- **MVP barato**: vertical única (ex.: máquina de lavar) para nível de precisão alto.

## 9. Cofre de legado digital

- **Dor**: quando alguém morre, a família não acessa contas, senhas, cripto, apólices,
  nem sabe o que existe. É tabu, então ninguém organiza em vida.
- **O quê**: inventário guiado do patrimônio digital + instruções ("se eu faltar,
  avisem X, cancelem Y, a senha mestra está com Z") + liberação controlada por
  gatilho (dead man's switch + verificação humana).
- **Por que agora**: primeira geração 100% digital começando a envelhecer; cripto e
  contas digitais tornaram o problema material e caro de ignorar.
- **Como estoura**: seguradoras e planejadores financeiros como canal B2B2C; a dor
  vira pauta recorrente na mídia. Assinatura anual barata com retenção altíssima
  (ninguém cancela um testamento).
- **MVP barato**: começa como checklist premium + cofre criptografado simples.

## 10. Prova de habilidade (currículo executável)

- **Dor**: contratar por currículo é loteria — e currículos agora são escritos por
  IA, ficaram todos iguais. Recrutador não confia em mais nada.
- **O quê**: plataforma onde o candidato executa desafios reais da vaga (atender um
  cliente difícil simulado, revisar uma planilha com erros, escrever um e-mail de
  cobrança) e sai com um perfil de habilidades demonstradas, não declaradas.
- **Por que agora**: IA consegue gerar desafios sob medida e avaliar desempenho de
  forma consistente e barata — antes isso exigia avaliador humano caro.
- **Como estoura**: dois lados se puxam — candidato quer se diferenciar do mar de
  currículos de IA; empresa quer parar de entrevistar no escuro. B2B paga.
- **MVP barato**: uma vertical (ex.: atendimento/SDR) com 3 empresas piloto.

---

## Padrões por trás das 10 ideias (o que procurar em qualquer nova ideia)

1. **WhatsApp como plataforma, não app novo** — no Brasil, distribuição é WhatsApp.
   O app vem depois da tração (ideias 1–4).
2. **IA que faz, não IA que conversa** — chatbot genérico morreu; o valor está em
   agente que executa a burocracia inteira (ideias 2, 6).
3. **Momento de pânico = aquisição barata** — documento assustador, barulho na
   máquina, pai idoso: a pessoa procura ativamente, não precisa ser convencida
   (ideias 2, 7, 8).
4. **Incentivo alinhado** — cobrar % da economia ou preço que "se paga sozinho"
   elimina a objeção de venda (ideia 6).
5. **Serviço antes de software** — todas validam com n8n + WhatsApp + 10-20 clientes
   pagantes antes de escrever uma linha do app. Se ninguém paga pelo serviço manual,
   não vai pagar pelo app.

## Próximo passo sugerido

Escolher 1 ideia, rodar como serviço concierge por 30 dias com meta de 10 pagantes,
e só então decidir se vira produto. As ideias 1 e 3 são as de menor distância da
stack atual (n8n + IA + WhatsApp).

---

# Segunda leva (21/08/2026) — 10 ideias novas

## 11. Financeiro de casal

- **Dor**: dinheiro é a maior causa de briga de casal. Contas divididas em planilha
  torta, "quem pagou o mercado?", um gasta escondido do outro.
- **O quê**: espaço financeiro compartilhado sem juntar contas: cada um conecta o
  seu banco, o app separa o que é "nosso" do que é "meu", faz o acerto do mês
  automaticamente (via Pix) e dá visão de metas conjuntas (viagem, entrada do apê).
- **Por que agora**: Open Finance maduro no Brasil torna a leitura multi-banco
  trivial; IA categoriza e arbitra ("isso é do casal ou seu?") com precisão.
- **Como estoura**: aquisição vem em dupla por definição — cada usuário traz outro.
  Momentos de vida (morar junto, casar) são gatilhos buscáveis.
- **MVP barato**: começa sem Open Finance — foto do extrato/comprovante + acerto
  mensal assistido.

## 12. Carteira de saúde da família

- **Dor**: exames em PDF espalhados por e-mail, ninguém entende o laudo, e o médico
  tem 8 minutos de consulta. Histórico da família (pais idosos, filhos) é caos.
- **O quê**: guarda todos os exames/laudos/receitas da família → traduz para
  linguagem leiga, monta linha do tempo (colesterol nos últimos 5 anos), prepara a
  lista de perguntas para a próxima consulta e alerta tendências.
- **Por que agora**: LLMs leem laudo e exame com qualidade clínica de apoio (sem
  diagnosticar — organizar e explicar); a dor cresceu com telemedicina fragmentada.
- **Como estoura**: quem cuida da saúde da família (geralmente uma pessoa) adota
  para todos os membros — cada conta puxa 3-5 perfis. Mídia adora a pauta.
- **MVP barato**: fluxo por WhatsApp (manda o PDF do exame, recebe a explicação).
  Cuidado regulatório: posicionar como organização/educação, nunca diagnóstico.

## 13. Pechincheiro — agente de compras pessoal

- **Dor**: brasileiro pesquisa preço em 5 sites, caça cupom, espera promoção e ainda
  paga caro. Cashback e cupom são um jogo de quem tem paciência.
- **O quê**: você diz o que quer ("tênis de corrida até R$ 300") → o agente monitora
  preços, aplica cupons, cruza cashback e avisa (ou compra) na hora certa. Também
  audita: "isso que você vai comprar está R$ 40 mais barato em tal lugar".
- **Por que agora**: agentes navegam sites de varejo de forma confiável; APIs de
  afiliados pagam a conta — o app pode ser grátis para o usuário.
- **Como estoura**: print de economia é viral; Black Friday é pico anual de aquisição
  gratuita. Monetiza por afiliado/cashback, incentivo alinhado.
- **MVP barato**: uma categoria (eletrônicos) + alerta por WhatsApp.

## 14. Advogado de bolso do consumidor

- **Dor**: voo cancelado, produto que não chegou, cobrança indevida, plano de saúde
  negando exame. O consumidor tem direito e não exerce porque a briga é exaustiva.
- **O quê**: descreve o problema (ou manda os prints) → o agente monta o caso, abre
  reclamação no canal certo (SAC, consumidor.gov.br, Procon), redige tudo, acompanha
  prazos e escala até proposta de acordo. Se precisar de Juizado, prepara o kit.
- **Por que agora**: agentes executam o processo burocrático de ponta a ponta;
  jurisprudência de consumo é padronizada o suficiente para automação segura.
- **Como estoura**: cada vitória ("recebi R$ 1.400 da companhia aérea sem advogado")
  é uma história contada. Modelo de % sobre o que recuperar alinha incentivo.
- **MVP barato**: nicho único com dano tabelado — voos atrasados/cancelados — onde o
  processo é quase mecânico.

## 15. Otimizador de conta de luz

- **Dor**: todo mundo acha a conta de luz cara e ninguém sabe o que fazer além de
  desligar o ar-condicionado.
- **O quê**: foto da conta → o agente audita (bandeira, tributos, erros de leitura),
  simula alternativas (mercado livre de energia, assinatura de energia solar por
  cooperativa, mudança de modalidade tarifária) e executa a troca por você.
- **Por que agora**: a abertura do mercado livre de energia está descendo para
  consumidores cada vez menores no Brasil, e quase ninguém entende como aproveitar —
  janela clássica de "serviço de troca" (como foi comparador de seguro no Reino Unido).
- **Como estoura**: "reduzi 18% da conta de luz sem instalar nada" se espalha
  sozinho. Monetiza por comissão da geradora/cooperativa — grátis para o usuário.
- **MVP barato**: auditoria da conta por WhatsApp + parceria com 1 cooperativa solar.

## 16. Agente do inquilino

- **Dor**: alugar imóvel é humilhante: caçar anúncio duplicado, visitar 10 imóveis
  ruins, contrato leonino, vistoria de entrada mal feita que vira briga na saída.
- **O quê**: o agente caça imóveis pelos seus critérios em todos os portais, agenda
  visitas, analisa o contrato (cláusulas abusivas), negocia o valor com dados da
  região e documenta a vistoria com fotos organizadas e laudo.
- **Por que agora**: agentes cruzam portais e leem contratos; o mercado de aluguel
  está aquecido e o lado do inquilino nunca teve ferramenta — tudo serve o dono.
- **Como estoura**: geração que aluga é digital e indica em grupo de amigos. Cobra
  taxa única por contratação fechada (fração do que uma imobiliária cobra do dono).
- **MVP barato**: análise de contrato + kit de vistoria como produtos avulsos.

## 17. GPS do SUS

- **Dor**: navegar o sistema público de saúde é um labirinto: onde marcar, que
  documento levar, quanto tempo de fila, que UBS tem a vacina, como conseguir o
  medicamento de alto custo.
- **O quê**: assistente que conhece o caminho: te diz o passo a passo do seu caso,
  os documentos, a unidade certa, acompanha sua posição na fila de regulação e avisa
  quando algo anda. Lembra exames de rotina e vacinas da família.
- **Por que agora**: dados públicos de saúde cada vez mais abertos + IA para
  transformar burocracia em passo a passo. Ninguém fez o "Waze" desse labirinto.
- **Como estoura**: 7 em cada 10 brasileiros dependem do SUS. Distribuição via
  boca a boca em comunidade é natural. Monetização: B2B (empregadores, planos
  populares, prefeituras) — o usuário final não paga.
- **MVP barato**: uma cidade, um fluxo (ex.: consulta com especialista via
  regulação) dominado de ponta a ponta.

## 18. Aposta em você mesmo

- **Dor**: o Brasil virou o país das bets — dinheiro do salário indo embora em
  aposta. A mecânica (risco, recompensa, dopamina) é viciante; o objeto é ruim.
- **O quê**: vira o jogo: você "aposta" em metas próprias (correr 3x/semana,
  guardar R$ 200/mês, estudar 20h). Cumpriu, resgata com rendimento + prêmios do
  pote de quem falhou. Falhou, o valor vai para o pote (ou para caridade, você
  escolhe). Compromisso com dinheiro na mesa funciona — é ciência comportamental.
- **Por que agora**: a discussão nacional anti-bet abre espaço para o contraponto;
  verificação de meta por IA (foto, integração com apps de treino/banco) elimina a
  fraude que matava essa ideia antes.
- **Como estoura**: desafios em grupo ("nossa turma apostou que vai treinar até
  dezembro") são virais e sociais por natureza. Pauta de mídia garantida.
- **MVP barato**: desafios em grupo com Pix manual e verificação por foto no
  WhatsApp. Atenção regulatória: estruturar como poupança/compromisso, não aposta.

## 19. Vendedor que nunca dorme (B2B)

- **Dor**: pequeno comércio perde venda todo dia: cliente chama no WhatsApp às 22h,
  ninguém responde, ele compra do concorrente. Dono não tem equipe de atendimento.
- **O quê**: agente de vendas no WhatsApp do comércio: conhece o catálogo e o
  estoque, responde em segundos, tira dúvida, monta o pedido, cobra via Pix e passa
  para o humano só o que precisa. Relatório diário do que vendeu e do que perdeu.
- **Por que agora**: agentes com tool use confiável + catálogo estruturado; o dono
  de comércio já entendeu que atendimento lento = venda perdida.
- **Como estoura**: B2B por resultado ("te entrego X vendas recuperadas/mês").
  Cada comércio atendido é vitrine para os vizinhos do bairro. É também o produto
  mais próximo do meu trabalho atual de freelance — dá para vender já.
- **MVP barato**: n8n + WhatsApp API + catálogo em planilha. Primeiro cliente em
  uma semana.

## 20. Histórias do vovô

- **Dor**: quando os avós morrem, as histórias morrem junto. Todo mundo "sempre
  quis gravar" e nunca grava.
- **O quê**: a IA liga (ou conversa por áudio no WhatsApp) semanalmente com o avô/avó,
  puxa histórias com perguntas boas ("como a senhora conheceu o vovô?"), e transforma
  o acervo em livro impresso, podcast da família ou memorial com a própria voz.
- **Por que agora**: voz natural + longa memória de conversa tornam a "entrevista
  semanal infinita" possível; clonagem de voz (com consentimento) permite o memorial.
- **Como estoura**: é O presente de Dia das Mães/Pais/80 anos — compra emocional,
  giftável, com pico sazonal. Cada livro impresso circula na família inteira (20+
  pessoas veem o produto).
- **MVP barato**: entrevistas por WhatsApp + diagramação assistida por IA + gráfica
  sob demanda. Margem de produto físico premium.

## Onde estas se encaixam na tese do "agente pessoal"

As ideias 13, 14, 15 e 16 são "cabeças" alternativas do mesmo agente que executa
burocracia (tese da seção anterior). A 19 é a versão B2B da mesma tecnologia — e a
de caixa mais rápido. A 18 e a 20 são apostas de produto independentes, com
mecânica emocional/social própria.

---

# Terceira leva (21/08/2026) — mercado de tecnologia (dev tools e infra de IA)

> Por que esse mercado "daria": o comprador é técnico (decide rápido, paga em dólar,
> compra self-service sem reunião), a distribuição é por conteúdo/comunidade (barata),
> e eu sou o próprio público-alvo — conheço a dor de dentro. O risco é ser um mercado
> global e competitivo: a defesa é nicho + velocidade.

## 21. Resgate de app "vibe-coded"

- **Dor**: a explosão de gente não-técnica criando apps com IA gerou uma geração de
  apps que funcionam na demo e quebram na vida real: sem segurança (chaves expostas,
  banco aberto), sem backup, sem deploy decente, sem manutenção.
- **O quê**: serviço/produto que audita o app gerado por IA (segurança, custo,
  escalabilidade), corrige o crítico e assume a manutenção por assinatura. "O adulto
  na sala do seu app feito com IA."
- **Por que agora**: 2025-2026 é o pico do vibe coding; a onda de incidentes
  (vazamento, conta de cloud surpresa) está começando — a dor vai explodir junto.
- **Como estoura**: cada história de terror pública ("meu app vazou os dados") é
  marketing gratuito. SEO em cima de "app lovable/bolt/v0 problema X".
- **MVP barato**: auditoria automatizada como isca (relatório gratuito) + correção
  e assinatura de manutenção como produto. Dá para operar solo desde a semana 1.

## 22. CI/CD de prompts e agentes

- **Dor**: todo time que coloca LLM em produção descobre que mudar um prompt é
  deploy sem teste: quebrou o formato do JSON, o agente parou de chamar a tool, o
  custo dobrou — e ninguém percebe até o cliente reclamar.
- **O quê**: "esteira de testes" para IA: test sets versionados, avaliação automática
  a cada mudança de prompt/modelo (formato, tool calling, qualidade, custo,
  latência), diff entre versões e bloqueio de regressão antes do deploy.
- **Por que agora**: a massa de agentes em produção cruzou o ponto em que "testar no
  olho" não escala; os frameworks de eval existentes são para dev hardcore, não para
  quem constrói em n8n/Make/Zapier.
- **Como estoura**: foco no nicho low-code (n8n primeiro): plugin/node nativo,
  templates de test set por caso de uso, conteúdo em comunidade. É literalmente o
  meu skill agent-eval virando produto.
- **MVP barato**: já tenho os scripts — falta empacotar como serviço web + node n8n.

## 23. Gerador de MCP server (qualquer sistema vira tool de IA)

- **Dor**: toda empresa quer que a IA acesse seus sistemas internos (ERP antigo,
  API interna, banco), mas transformar isso em tools seguras para agentes exige dev
  especializado.
- **O quê**: aponta para uma API (OpenAPI/Swagger, ou até só a documentação) → gera
  um MCP server pronto, com autenticação, permissões por tool, rate limit e log de
  auditoria. "Zapier da era dos agentes."
- **Por que agora**: MCP virou o padrão de conexão de agentes a sistemas; a demanda
  por "conectar a IA no meu sistema" cresce mais rápido que a oferta de quem sabe
  fazer.
- **Como estoura**: cauda longa de sistemas legados que ninguém vai integrar à mão.
  Modelo: gratuito para APIs públicas, pago para privadas/self-hosted.
- **MVP barato**: gerador para 1 formato (OpenAPI → MCP) como ferramenta CLI/web.

## 24. Caixa de aprovação humana para agentes (human-in-the-loop como serviço)

- **Dor**: agentes em produção precisam de aprovação humana nos passos críticos
  (enviar e-mail ao cliente, dar desconto, pagar boleto) — e cada time reinventa
  isso com gambiarras de Slack/WhatsApp.
- **O quê**: um "inbox de aprovações" plugável: o agente pausa, a pessoa aprova/
  edita/rejeita pelo celular com contexto completo, o fluxo continua. Com trilha de
  auditoria, SLA, escalonamento e regras ("acima de R$ X, aprovação dupla").
- **Por que agora**: a primeira onda de "agente 100% autônomo" frustrou; o padrão
  vencedor em 2026 é autonomia com checkpoints — e não existe um padrão de mercado
  para o checkpoint.
- **Como estoura**: nodes prontos para n8n/Make/LangChain viram distribuição; quem
  adota nunca mais tira (vira infraestrutura de compliance).
- **MVP barato**: node n8n + inbox web simples. Sou usuário zero.

## 25. FinOps de LLM para quem constrói com low-code

- **Dor**: a conta de API de IA cresce sem ninguém saber qual workflow, cliente ou
  prompt está queimando dinheiro. Em agência/freelance, sem isso não dá nem para
  precificar o cliente direito.
- **O quê**: painel que rastreia custo por workflow, por cliente e por modelo, alerta
  anomalias ("esse fluxo custou 4x mais hoje") e sugere/executa troca de modelo onde
  a qualidade não cai (router de custo).
- **Por que agora**: multi-provider virou norma (Claude + GPT + Gemini + local);
  o custo virou a segunda maior objeção de projeto de IA, atrás só de confiabilidade.
- **Como estoura**: "descobri que 60% do meu custo era um loop esquecido" é o print
  que circula em toda comunidade de automação. Preço por % de economia ou assinatura.
- **MVP barato**: proxy de API + dashboard. Meu skill llm-migration é metade disso.

## 26. Loja de automações prontas (produtizar o freelance)

- **Dor**: toda PME quer "o orçamento que responde sozinho" ou "a cobrança
  automática", mas projeto custom custa caro; todo freelancer refaz o mesmo fluxo
  em cada cliente.
- **O quê**: catálogo de automações empacotadas por nicho (clínica, imobiliária,
  e-commerce): instala em minutos no n8n do cliente (ou hospedado), configura por
  formulário, atualiza como app. Freelancers revendem e ganham comissão.
- **Por que agora**: n8n explodiu em adoção; existe template gratuito de sobra mas
  quase nada "instalável por leigo com suporte" — o gap é empacotamento, não código.
- **Como estoura**: efeito marketplace duplo: criadores publicam (renda passiva),
  agências revendem. Começa como loja própria, vira plataforma.
- **MVP barato**: 3 automações do meu próprio portfólio, empacotadas, com página de
  venda. Valida com o que eu já entrego em consultoria.

## 27. Contratar um agente, não uma pessoa (marketplace de trabalho feito por IA)

- **Dor**: PME precisa de "alguém que responda meu Instagram", "alguém que faça
  minha conciliação" — mas não quer contratar nem aprender ferramenta nenhuma.
- **O quê**: marketplace onde se contrata o resultado: "agente social media",
  "agente de cobrança", "agente de conciliação" — com preço mensal fixo, SLA e
  operador humano supervisionando por trás (centauro: IA faz, humano garante).
- **Por que agora**: a tecnologia dos agentes está pronta para 80% dessas tarefas;
  o que falta é a embalagem comercial que a PME entende — "contratei alguém".
- **Como estoura**: é a versão B2B em escala do que já vendo como freelancer. Cada
  vertical que funciona vira um "cargo" novo no catálogo. Potencial de ser a maior
  ideia das três levas — e a mais pesada de operar.
- **MVP barato**: um "cargo" só (ex.: agente de cobrança para clínicas), 5 clientes,
  eu como operador humano. Se a unidade economics fechar, escala.

## Qual eu escolheria no mercado tech

Caminho de menor risco e maior sinergia: **22 (CI de prompts) ou 24 (caixa de
aprovação)** — ambos nascem de skills que já tenho, vendem para a comunidade onde
já estou, e são infraestrutura (retenção alta). O **21 (resgate de vibe-coded)** é
o de caixa mais rápido como serviço. O **27** é a aposta grande — melhor chegar
nela depois de validar 26.
