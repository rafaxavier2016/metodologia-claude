# Índice de análises

Tudo que já foi analisado pelo `/analista-codigos`. Atualizado a cada análise nova.

## Repositórios de terceiros

| Repositório | Arquivo | Analisado em | Principal lição |
|---|---|---|---|
| supabase/supabase | [`supabase.md`](supabase.md) | 2026-08-22 | `data/` e `state/` como camadas irmãs de `components/`; trava mecânica de gerente de pacote (`only-allow pnpm`) |

## Repositórios próprios

| Repositório | Arquivo | Analisado em | Estado |
|---|---|---|---|
| receitanet-cookie-renewer | [`meus/receitanet-cookie-renewer.md`](meus/receitanet-cookie-renewer.md) | 2026-08-22 | 🔴 segredo commitado em repo público — rotacionar token |
| ampliar-corretora | [`meus/ampliar-corretora.md`](meus/ampliar-corretora.md) | 2026-08-22 | 🟠 hook de segurança bom, com 2 furos de padrão; não distribuído para os outros repos |

## Ainda não analisados

- `rafaxavier2016/operare-metodologia` (privado)
- `rafaxavier2016/rj-metodologia` (privado)
- `rafaxavier2016/metodologia-claude` (este repo)

---

## Padrões recorrentes entre análises

Conforme a biblioteca cresce, os padrões que aparecem em mais de um repositório sobem
para cá.

### Trava mecânica > instrução escrita

Visto em dois lugares independentes:

- **Supabase**: `preinstall: npx only-allow pnpm` — `npm install` falha na hora
- **ampliar-corretora**: `.githooks/pre-commit` — commit com CPF/token é bloqueado

Nos dois casos a alternativa seria uma linha no README, que ninguém lê no dia em que
importa. A versão que funciona é a que quebra o comando.

**Corolário observado na prática (receitanet):** trava que existe num repositório não
protege os outros. Uma regra sem distribuição é uma regra que vale para um projeto só.

### Falhar fechado

Programa que sobe com credencial padrão publicada (`RENEW_AUTH_TOKEN` = `change-me-via-env`)
está mais inseguro do que um que se recusa a subir. O default de um segredo deve ser
"não existe", nunca um valor de exemplo.
