# receitanet-cookie-renewer — análise

| | |
|---|---|
| **Repositório** | github.com/rafaxavier2016/receitanet-cookie-renewer |
| **Visibilidade** | 🔴 **PÚBLICO** |
| **Commit analisado** | clone `--depth 1` de 2026-08-22 |
| **Analisado em** | 2026-08-22 |

> ⚠️ Este arquivo mora num repositório público. Nenhum valor de segredo é reproduzido
> aqui — apenas o arquivo e a linha onde ele está.

---

## Estrutura real — `find . -type f`

```
README.md           93 linhas
renew.py           236 linhas
docker-compose.yml  28 linhas
Dockerfile          25 linhas
arteck/q.html       40 linhas
```

Um script Python com Playwright que renova cookie de sessão do ReceitaNet e grava numa
credential do n8n. Roda de duas formas: `python renew.py` (uma vez) ou `--serve` (HTTP
na 8080, disparado por cron externo).

---

## 🔴 Achado 1 — segredo real commitado em repositório público

**`docker-compose.yml`, seção `environment`.** Enquanto `RECEITANET_USER`, `N8N_API_KEY`
e `RENEW_AUTH_TOKEN` corretamente usam `${VAR}`, quatro valores estão **literais**:

| Linha | Variável | O que é |
|---|---|---|
| ~19 | `N8N_API_URL` | hostname do n8n de produção |
| ~22 | `N8N_CREDENTIAL_ID` | id da credential real |
| ~28 | `UAZAPI_TOKEN_ALERTA` | 🔴 **token de API, valor completo** |
| ~29 | `RAFAEL_WHATSAPP` | número de telefone pessoal |

O `UAZAPI_TOKEN_ALERTA` é o grave: token de instância do UazAPI, em claro, num repo que
qualquer pessoa lê. `RAFAEL_WHATSAPP` e o hostname do n8n também estão no docstring do
`renew.py` (linhas 21–23) e no `README.md`.

**Ação, nesta ordem:**

1. **Rotacionar o token do UazAPI agora.** Trocar o arquivo não resolve nada — o valor
   já está no histórico do git, público, e provavelmente já indexado.
2. Trocar os quatro literais por `${VAR}` no `docker-compose.yml`, e limpar docstring e
   README.
3. Decidir se o repositório precisa mesmo ser público. Se não, torná-lo privado reduz
   exposição futura — mas **não** desfaz a de agora; o passo 1 continua obrigatório.

### A ironia que ensina

O `ampliar-corretora` tem hook de pré-commit e `.gitignore` defensivo, e um README que
abre com *"Trate todo arquivo como sensível... o histórico do git é irreversível"*.

Essa disciplina existe no repositório **privado** e falta no **público** — exatamente o
inverso do necessário. A regra foi escrita em um lugar e não viajou para o outro.

### E o hook não teria pego

Testado com o padrão real do `.githooks/pre-commit` do ampliar contra a linha do token:

```sh
grep -Ei '(senha|password|passwd|secret|api[_-]?key|access[_-]?token|client[_-]?secret|cookie)[[:space:]]*[:=]...'
# resultado: não casa
```

`UAZAPI_TOKEN_ALERTA` não bate em `access[_-]?token` (falta o "access") nem em
`api[_-]?key`. **Correção sugerida no hook:** aceitar `token` isolado e UUID solto —

```sh
check "Token/segredo" '[A-Za-z0-9_]*(token|secret|key|senha|password|cookie)[A-Za-z0-9_]*[[:space:]]*[:=][[:space:]]*["'"'"']?[A-Za-z0-9_@#$%^&*!.-]{8,}'
check "UUID literal"  '[:=][[:space:]]*["'"'"']?[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}'
```

Vai dar mais falso positivo. É o trade-off certo: o próprio hook diz *"bloquear à toa é
melhor que deixar passar"*.

---

## 🟠 Achado 2 — o endpoint `/renew` falha aberto

`renew.py:197`

```python
AUTH_TOKEN = os.environ.get('RENEW_AUTH_TOKEN', 'change-me-via-env')
```

Se `RENEW_AUTH_TOKEN` não estiver setado, o servidor **sobe assim mesmo** com o token
`change-me-via-env` — que está publicado neste repositório. Quem souber o endereço faz
`POST /renew` com esse header e dispara login no ReceitaNet à vontade.

Esquecer de setar a variável é o caso provável (deploy novo, recriar o serviço no
easypanel, subir em outra máquina). O comportamento seguro é recusar:

```python
AUTH_TOKEN = os.environ.get('RENEW_AUTH_TOKEN', '').strip()
if not AUTH_TOKEN:
    raise RuntimeError("RENEW_AUTH_TOKEN é obrigatório no modo --serve")
```

Mesma filosofia do hook de pré-commit: **falhar fechado**. O programa se recusa a subir
inseguro em vez de confiar em quem faz o deploy lembrar.

Vale junto: comparar o token com `hmac.compare_digest` em vez de `!=` (linha 203) — é
uma linha e tira o canal de timing.

---

## 🟡 Achado 3 — `arteck/q.html` não pertence aqui

40 linhas de HTML de um questionário da "ArTeck", sem nenhuma relação com renovação de
cookie. Nada no `renew.py`, no `Dockerfile` ou no `docker-compose.yml` referencia esse
arquivo.

Ou é resto de outro projeto que entrou por engano, ou tem um propósito que não está
escrito em lugar nenhum. Nos dois casos: mover para o repositório certo, ou apagar, ou
explicar em uma linha no README. Arquivo órfão em repositório público é dívida — quem
abrir daqui a seis meses não vai saber se pode remover.

---

## 🟡 Achado 4 — `except: pass` engole tudo

`renew.py:121`

```python
except: pass
```

`except` pelado captura inclusive `KeyboardInterrupt` e `SystemExit`. O escopo aqui é
pequeno (ler mensagem de erro do Keycloak), então o risco é baixo — mas o custo de
corrigir é menor ainda:

```python
except Exception:
    pass
```

---

## ✅ O que já está bom

Vale registrar, porque é o que **não** precisa mudar:

- **Separação de responsabilidades por função** — `renovar()` busca,
  `atualizar_credential_n8n()` publica, `alertar_rafael()` notifica. É exatamente a
  ideia da camada `data/` do Supabase, em escala de script. Está certo.
- **Log com passo numerado** (`step 1:`… `step 8:`) — quando quebra às 3h da manhã,
  o log diz onde parou. Melhor que muito projeto grande.
- **Validação explícita do resultado** — o script não confia no redirect: checa se
  voltou para o Keycloak (linha 114) e se o `receitanet_session` realmente apareceu
  (linha 160). Falha alto em vez de gravar cookie vazio na credential.
- **Comentário que explica o *porquê*** (linhas 124–126: por que é preciso navegar em
  páginas autenticadas antes de capturar cookie). Esse é o comentário que vale — o que
  não dá para deduzir do código.
- **Alerta em falha, não só em sucesso** (`main()` avisa nos dois caminhos).

---

## Fila sugerida

| # | Ação | Urgência |
|---|---|---|
| 1 | Rotacionar token do UazAPI | 🔴 agora |
| 2 | `${VAR}` no compose + limpar docstring/README | 🔴 hoje |
| 3 | `RENEW_AUTH_TOKEN` obrigatório (fail-closed) | 🟠 esta semana |
| 4 | Ampliar o padrão do hook e instalar nos outros repos | 🟠 esta semana |
| 5 | Resolver `arteck/q.html` | 🟡 quando der |
| 6 | `except Exception` + `compare_digest` | 🟡 quando der |
