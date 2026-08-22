# Analista de Códigos 🔍

Skill especializado em análise arquitetural de repositórios públicos. Entende a estrutura completa de código e gera insights sobre organização, padrões e boas práticas.

## O que faz

Este skill permite que você:
- Analise repositórios públicos sem modificá-los
- Entenda a arquitetura completa
- Identifique padrões de design e boas práticas
- Replique estruturas em seus próprios projetos
- Documente decisões arquiteturais

## Como usar

### Uso básico

```
/analista-codigos
```

Vai pedir o repositório a analisar.

### Uso direto

```
/analista-codigos repo=github.com/supabase/supabase

/analista-codigos repo=github.com/nextcloud/server

/analista-codigos repo=github.com/facebook/react
```

### Formatos de repositório aceitos

- `github.com/owner/repo`
- `owner/repo`
- `https://github.com/owner/repo`
- `github.com/owner/repo.git`

## O que você recebe

A análise inclui:

1. **Visão Geral**
   - Nome e descrição do projeto
   - Stack de tecnologias
   - Padrão arquitetural

2. **Estrutura de Diretórios**
   - Organização de pastas
   - Responsabilidade de cada camada
   - Convenções de nomes

3. **Padrões Encontrados**
   - Arquiteturais (MVC, MVVM, Monorepo, etc)
   - de Design (Singleton, Observer, etc)
   - de Organização

4. **Boas Práticas**
   - Type safety
   - Testes (unit, integration, e2e)
   - CI/CD
   - Documentação
   - Performance

5. **Como Replicar**
   - Steps passo-a-passo
   - Exemplos de código
   - Adaptações para contextos diferentes

6. **Integração com Serviços**
   - APIs
   - Autenticação
   - Real-time
   - Storage

## Exemplos de Análise

### Exemplo 1: Supabase
```
/analista-codigos repo=supabase/supabase

Resultado:
- Monorepo com Turbo + pnpm
- Design System centralizado
- TypeScript em 100%
- Testing em múltiplas camadas
- Como replicar em seu projeto...
```

### Exemplo 2: React
```
/analista-codigos repo=facebook/react

Resultado:
- Componentes funcionais
- Hooks pattern
- Virtual DOM
- Como implementar em seu app...
```

### Exemplo 3: Seu Próprio Repo
```
/analista-codigos repo=seu-user/seu-projeto

Resultado:
- Análise completa
- Sugestões de melhoria
- Documentação automática
```

## Opções Avançadas

### Análise focada
```
/analista-codigos repo=X aspect=arquitetura
/analista-codigos repo=X aspect=padroes
/analista-codigos repo=X aspect=testes
/analista-codigos repo=X aspect=performance
```

### Gerar documentação
```
/analista-codigos repo=X output=markdown
/analista-codigos repo=X output=diagram
/analista-codigos repo=X output=template
```

### Comparar dois repositórios
```
/analista-codigos compare=repo1 with=repo2
```

## Limitações

❌ **Não funciona com:**
- Repositórios privados (sem acesso)
- Código binário
- Arquivos muito grandes (>100MB)
- Linguagens obscuras

✅ **Funciona melhor com:**
- Repositórios públicos no GitHub
- TypeScript/JavaScript
- Python
- Go
- Java
- Qualquer linguagem comum

## Workflow Típico

```mermaid
1. Você invoca /analista-codigos
   ↓
2. Skill pede repositório (ou você fornece)
   ↓
3. Agent clona repositório
   ↓
4. Analisa estrutura, padrões, código
   ↓
5. Gera relatório estruturado
   ↓
6. Você estuda padrões
   ↓
7. Replica em seu projeto
```

## Casos de Uso

### Aprender Arquitetura
```
Você: /analista-codigos repo=mattermost/mattermost-server
Claude: Explicamos como um app chat real é organizado
Você: Replica a estrutura em seu projeto
```

### Melhorar Código
```
Você: Analize meu repo
Claude: Identifica padrões ruins
Você: Refactora baseado em boas práticas
```

### Design System
```
Você: /analista-codigos repo=chakra-ui/chakra-ui
Claude: Como organizar componentes reutilizáveis
Você: Cria seu próprio design system
```

### Infraestrutura
```
Você: /analista-codigos repo=kubernetes/kubernetes
Claude: Como estruturar projeto grande
Você: Aplica em seus projetos
```

## Tips

💡 **Dica 1:** Comece com repositórios menores (~1k arquivos)
💡 **Dica 2:** Leia o README primeiro para contexto
💡 **Dica 3:** Foque em 1-3 padrões por análise
💡 **Dica 4:** Peça exemplos de implementação
💡 **Dica 5:** Compare com seu projeto atual

## Próximos Passos

1. **Escolha um repositório** que quer aprender
2. **Invoque o skill** com `/analista-codigos`
3. **Estude a análise** gerada
4. **Implemente padrões** em seu projeto
5. **Compartilhe** o que aprendeu com o time

---

**Criado por:** Claude
**Versão:** 1.0
**Último update:** 2026-08-22
