# Analista de Códigos

Um skill Claude Code que analisa repositórios e extrai insights sobre arquitetura, padrões e boas práticas.

## Características

- ✅ Análise completa de repositórios públicos
- ✅ Documentação automática
- ✅ Identificação de padrões arquiteturais
- ✅ Sugestões de boas práticas
- ✅ Exemplos de implementação
- ✅ Sem modificações no código (apenas leitura)

## Instalação

O skill já está pronto em `.claude/skills/analista-codigos/`.

Para usar:

```bash
/analista-codigos
```

## Exemplo de Uso

### Comando Simples
```
/analista-codigos
→ "Qual repositório quer analisar?"
→ Você: github.com/supabase/supabase
→ Agent começa análise...
→ Relatório gerado ✅
```

### Comando Direto
```
/analista-codigos repo=supabase/supabase
→ Análise imediata
```

## Output

Você recebe um relatório Markdown com:

```markdown
# Análise Arquitetural: [Projeto]

## 🎯 Visão Geral
- Stack de tecnologias
- Padrão arquitetural
- Estrutura geral

## 🏗️ Arquitetura
- Componentes principais
- Diagrama de dependências
- Fluxos de dados

## 📁 Estrutura de Diretórios
- Organização de pastas
- Responsabilidade de cada camada
- Convenções

## 🔄 Padrões Encontrados
- Arquiteturais
- de Design
- de Organização

## ✅ Boas Práticas
- Type safety
- Testes
- CI/CD
- Performance

## 💡 Como Replicar
- Step-by-step
- Exemplos de código
- Adaptações

## 📊 Métricas
- Tamanho do projeto
- Linguagens
- Cobertura de testes
```

## Repositórios Recomendados

### Para Aprender Estrutura
- `supabase/supabase` - Monorepo moderno
- `nextcloud/server` - Backend robusto
- `mattermost/mattermost-server` - App completa

### Para Aprender Padrões
- `facebook/react` - Componentes
- `kubernetes/kubernetes` - Escalabilidade
- `stripe/stripe-go` - SDKs

### Para Aprender DevOps
- `docker/docker` - Containerização
- `hashicorp/terraform` - IaC
- `prometheus/prometheus` - Monitoring

## Workflow Recomendado

1. **Escolha um repositório** similar ao seu contexto
2. **Invoque /analista-codigos**
3. **Estude a análise** gerada
4. **Identifique 2-3 padrões** para usar
5. **Implemente** em seu projeto
6. **Compare** resultado com análise

## Exemplo Prático

```
Seu objetivo: Criar um app de delivery

Passo 1: Analisar como funciona Uber
/analista-codigos repo=uber/uber-engineering

Passo 2: Analisar padrões de real-time
/analista-codigos repo=supabase/supabase aspect=realtime

Passo 3: Entender autenticação
/analista-codigos repo=auth0/auth0-github aspect=auth

Passo 4: Estruturar seu projeto
→ Replicar padrões aprendidos
→ Adaptar para seu contexto
→ Implementar features
```

## Limitações

- Repositórios privados: Precisa de acesso autenticado
- Arquivos muito grandes: Pode ser lento (>100MB)
- Código binário: Não pode analisar

## Próximas Versões

- [ ] Suporte a repositórios privados
- [ ] Análise de performance
- [ ] Geração de diagramas
- [ ] Comparação entre repositórios
- [ ] Recomendações automáticas

## Feedback

Encontrou algo errado ou quer melhorias?

Abra uma issue ou compartilhe no Discord!

---

**Criado com ❤️ para ajudar você aprender arquitetura de código**
