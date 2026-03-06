# 🏰 SaberQuest — Aprenda Brincando!

App educativo gamificado com IA generativa para crianças de 6 a 10 anos (1ª a 5ª série).
Trilíngue: Português 🇧🇷, English 🇺🇸, Español 🇪🇸.

## Visão Geral

SaberQuest é um app onde crianças aprendem através de um mapa de mundos temáticos, guiadas por **Zuki**, um mascote-coruja inteligente alimentado por IA generativa. Cada mundo representa uma matéria escolar, com fases progressivas que se adaptam ao nível da criança.

## Funcionalidades

### 🗺️ Mundos Temáticos
| Mundo | Ícone | Fases |
|-------|-------|-------|
| Matemática | 🔢 | Adição → Subtração → Multiplicação → Divisão → Operações mistas |
| Português | 📖 | Vogais → Sílabas → Completar palavras → Frases → Criação |
| Ciências | 🔬 | Animais → Planeta Terra → Corpo humano → Plantas → Experimentos |
| Geografia | 🌎 | Bairro → Estados do Brasil → Continentes → Culturas → Desafio global |
| Idiomas | 🗣️ | Cumprimentos → Cores/Números → Animais trilíngue → Frases do dia → Desafio poliglota |

### 🦉 Zuki — Mascote com IA
- Conversa com a criança no idioma selecionado
- Explica dúvidas de forma simples e acolhedora
- Dá feedback inteligente quando a criança erra (não só "certo/errado")
- Gera exercícios dinâmicos adaptados ao nível

### 🧠 IA Generativa (Claude API)
- **Exercícios adaptativos**: gerados dinamicamente pela IA, respeitando rigorosamente o tema de cada fase
- **Feedback didático**: explicações passo a passo com exemplos do cotidiano
- **Dificuldade adaptativa**: acertou → sobe; errou → desce automaticamente
- **Retry inteligente**: ao errar, a criança pode tentar novamente com uma nova pergunta (evita decorar)
- **Fallback offline**: banco de exercícios didáticos por mundo/fase/idioma para quando a IA não está disponível

### 🌐 Trilíngue
- Interface completa em PT/EN/ES (troca com bandeiras no header)
- Mundo dedicado de idiomas para aprender vocabulário
- Em qualquer mundo, a criança pode alternar o idioma do exercício

### 🏅 Gamificação
- 8 medalhas desbloqueáveis (Primeiro Passo, Explorador, Em Chamas!, Poliglota, etc.)
- Sistema de pontuação por dificuldade (Fácil: 10pts, Médio: 20pts, Difícil: 30pts)
- Streak de acertos consecutivos
- Fases progressivas com desbloqueio sequencial

### ♿ Inclusividade
- Todas as explicações evitam referências a partes do corpo como ferramenta de ensino
- Analogias usam objetos universais (pratinhos, estrelinhas, lápis, reta numérica)
- Linguagem acolhedora e encorajadora
- O prompt da IA inclui instrução explícita de inclusividade

## Arquitetura do Projeto

```
saberquest/
├── index.html                    # Entry point HTML
├── package.json                  # Dependências e scripts
├── vite.config.js                # Config do Vite + path alias
├── tailwind.config.js            # Tailwind com animações customizadas
├── postcss.config.js             # PostCSS
├── README.md                     # Esta documentação
│
└── src/
    ├── main.jsx                  # Bootstrap React
    ├── App.jsx                   # Componente principal (estado global, navegação)
    │
    ├── components/
    │   ├── index.js              # Barrel exports
    │   ├── Mascot.jsx            # Componente do mascote Zuki
    │   ├── LanguageSelector.jsx  # Seletor de idioma (bandeiras)
    │   ├── WorldCard.jsx         # Card de cada mundo temático
    │   ├── PhaseNode.jsx         # Nó de fase no mapa
    │   ├── ExerciseScreen.jsx    # Tela de exercício (IA + fallback + retry)
    │   ├── ZukiChat.jsx          # Chat com o Zuki (IA conversacional)
    │   ├── MedalsScreen.jsx      # Tela de medalhas/conquistas
    │   └── MedalBadge.jsx        # Badge individual de medalha
    │
    ├── data/
    │   ├── constants.js          # Mundos, fases, traduções, medalhas, UI text
    │   └── fallbackExercises.js  # Exercícios offline por mundo/fase/idioma
    │
    ├── services/
    │   └── ai.js                 # Integração com Claude API
    │
    └── styles/
        └── index.css             # Tailwind directives + base styles
```

## Stack Técnica

- **Frontend**: React 18 + Vite
- **Styling**: Tailwind CSS (utility-first)
- **IA**: Anthropic Claude API (claude-sonnet-4-20250514)
- **Fontes**: Nunito (Google Fonts)
- **Animações**: CSS keyframes + Tailwind utilities

## Configuração para Desenvolvimento

```bash
# Instalar dependências
npm install

# Rodar em modo desenvolvimento
npm run dev

# Build para produção
npm run build
```

## Importação para o Lovable

1. Suba o projeto no GitHub
2. No Lovable, vá em **Import from GitHub**
3. Selecione o repositório
4. O Lovable reconhecerá a estrutura React + Tailwind automaticamente

### Sugestões para evoluir no Lovable:
- **Supabase**: adicionar persistência de progresso (pontuação, medalhas, fases completadas)
- **Autenticação**: login simples para cada criança salvar seu progresso
- **Área dos Pais**: relatórios de desempenho (planejado para versão futura)
- **PWA**: transformar em Progressive Web App para uso offline

## Integração com IA (Claude API)

O app usa a API da Anthropic para 3 funcionalidades:

1. **Geração de exercícios** (`generateExercise` em `services/ai.js`):
   - Recebe: mundo, fase, idioma, dificuldade, tentativa
   - Tem constraints por fase (ex: "ONLY multiplication tables 2-5")
   - Instrução de inclusividade no prompt do sistema
   - Retorna JSON: question, options, correctIndex, hint, explanation

2. **Chat com Zuki** (`chatWithZuki` em `services/ai.js`):
   - Mascote-tutor que responde no idioma selecionado
   - Contexto-aware (sabe em qual mundo a criança está)
   - Limite de 2-3 frases, linguagem infantil

3. **Fallback** (`generateFallbackExercise` em `data/fallbackExercises.js`):
   - Exercícios pré-escritos com explicações didáticas ricas
   - Organizados por mundo → fase → idioma
   - Seleção aleatória dentro do pool de cada fase

### Variáveis de Ambiente (para produção)

Para proteger a API key em produção, configure:
```
VITE_ANTHROPIC_API_KEY=sk-ant-...
```

E atualize `services/ai.js` para usar `import.meta.env.VITE_ANTHROPIC_API_KEY`.

## Princípios de Design

- **Inclusivo**: sem referências a partes do corpo como ferramenta de ensino
- **Didático**: explicações passo a passo com exemplos do cotidiano
- **Encorajador**: linguagem positiva, celebração de persistência
- **Adaptativo**: dificuldade ajusta automaticamente ao desempenho
- **Trilíngue nativo**: não é tradução, cada idioma tem conteúdo próprio

## Licença

Projeto privado. Todos os direitos reservados.