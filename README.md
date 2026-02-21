<p align="center">
  <img src="https://github.com/MLX-Pilot/.github/raw/main/profile/Visual_Edit.jpg" alt="MLX Pilot" width="420" style="border-radius: 16px; overflow: hidden;" />
</p>

<p align="center">
  <strong>Seu hub pessoal de Inteligência Artificial — local, privado e extensível.</strong>
</p>

<p align="center">
  <a href="#-início-rápido"><img src="https://img.shields.io/badge/-Início_Rápido-5C5CFF?style=for-the-badge" alt="Início Rápido" /></a>&nbsp;
  <a href="#-arquitetura"><img src="https://img.shields.io/badge/-Arquitetura-333?style=for-the-badge" alt="Arquitetura" /></a>&nbsp;
  <a href="README_DEV.md"><img src="https://img.shields.io/badge/-Docs_Dev-444?style=for-the-badge" alt="Dev Docs" /></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Rust-2021_Edition-dea584?logo=rust&logoColor=white" alt="Rust" />
  <img src="https://img.shields.io/badge/Tauri-2.x-24C8D8?logo=tauri&logoColor=white" alt="Tauri" />
  <img src="https://img.shields.io/badge/License-MIT-green" alt="License" />
  <img src="https://img.shields.io/badge/Status-Em_Desenvolvimento-orange" alt="Status" />
</p>

---

## 🧠 O que é o MLX Pilot?

O **MLX Pilot** é uma plataforma desktop **local-first** para execução, gerenciamento e automação de modelos de Inteligência Artificial. Toda a inferência acontece **na sua máquina** — sem enviar dados para nuvem, sem assinaturas mensais, sem limites de uso.

A proposta é reunir em um único ambiente tudo o que você precisa para trabalhar com LLMs locais:

| Capacidade | Descrição |
|---|---|
| 🚀 **Inferência local** | Execute modelos diretamente no seu hardware, com suporte a múltiplos backends. |
| 📦 **Catálogo integrado** | Descubra, pesquise e baixe modelos do Hugging Face sem sair do app. |
| 🔀 **Multi-provider** | Alterne entre **MLX** (Apple Silicon), **llama.cpp** (cross-platform) e **Ollama** de forma transparente. |
| 💬 **Chat nativo** | Converse com modelos locais em uma interface desktop fluida e responsiva. |
| ⚙️ **Controle total** | Gerencie provedores, credenciais e configurações em um painel centralizado. |
| 🤖 **Agente de IA nativo** | Agente inteligente próprio do MLX Pilot (em desenvolvimento) — capaz de executar tarefas, orquestrar workflows e interagir com o ecossistema local de forma autônoma. |
| 🔒 **Privacidade** | Nenhum dado sai da sua máquina. Zero telemetria. |

---

## 🎯 Para quem é?

- **Desenvolvedores** que querem testar e prototipar com LLMs locais sem depender de APIs pagas.
- **Pesquisadores** que precisam de um ambiente controlado para experimentação com modelos.
- **Entusiastas de IA** que valorizam privacidade e querem rodar modelos no próprio hardware.
- **Times** que buscam uma base extensível para construir ferramentas internas com IA.

---

## 🏗️ Arquitetura

O MLX Pilot segue uma arquitetura modular em camadas, construída inteiramente em **Rust** para máxima performance e segurança:

```
┌─────────────────────────────────────────────────────┐
│                  Desktop App (Tauri)                 │
│              Chat · Discover · Settings              │
├─────────────────────────────────────────────────────┤
│                  Daemon HTTP Local                   │
│          REST API · SSE Streaming · Catálogo         │
├──────────┬──────────────┬───────────────────────────┤
│ Provider │   Provider   │        Provider           │
│   MLX    │  llama.cpp   │        Ollama             │
│  (Apple  │  (embutido,  │   (compatibilidade)       │
│  Silicon)│ cross-plat.) │                           │
├──────────┴──────────────┴───────────────────────────┤
│                    Core (Domínio)                    │
│         Traits · Tipos · Contratos · Erros          │
└─────────────────────────────────────────────────────┘
```

### Componentes principais

| Componente | Caminho | Papel |
|---|---|---|
| **Core** | `crates/core` | Contratos de domínio — tipos, erros, trait `ModelProvider`. |
| **Provider MLX** | `crates/providers/mlx` | Inferência via Apple MLX framework (Apple Silicon). |
| **Provider llama.cpp** | `crates/providers/llamacpp` | Backend embutido cross-platform com `llama-server` gerenciado automaticamente. |
| **Provider Ollama** | `crates/providers/ollama` | Integração com Ollama para compatibilidade com modelos existentes. |
| **Daemon** | `crates/daemon` | Servidor HTTP local (padrão `127.0.0.1:11435`) — ponto central de todas as operações. |
| **Desktop UI** | `apps/desktop-ui` | App nativo Tauri com frontend (HTML/CSS/JS) — abas de Chat, Discover e Settings. |

---

## 🚀 Início Rápido

### Pré-requisitos

- **Rust** (toolchain estável via [`rustup`](https://rustup.rs))
- **Tauri CLI** — `cargo install tauri-cli --locked`
- **Windows**: Visual Studio Build Tools com C++ e Windows SDK
- **macOS/Linux**: ferramentas de build nativas do sistema

### Rodando em desenvolvimento

**1. Inicie o Daemon (API)**

```bash
cargo run -p mlx-ollama-daemon
```

**2. Inicie o Desktop App** *(em outro terminal)*

```bash
cd apps/desktop-ui/src-tauri
cargo tauri dev
```

> 💡 **macOS/Linux**: use `./scripts/run-desktop.sh` para iniciar tudo de uma vez.

### Testando a API

```bash
# Health check
curl http://127.0.0.1:11435/health

# Listar modelos disponíveis
curl http://127.0.0.1:11435/models

# Chat com um modelo
curl -X POST http://127.0.0.1:11435/chat \
  -H 'Content-Type: application/json' \
  -d '{
    "model_id": "seu-modelo-aqui",
    "messages": [{"role":"user", "content":"Olá, como você funciona?"}],
    "options": {"temperature": 0.7, "max_tokens": 256}
  }'
```

---

## 📁 Estrutura do Repositório

```
mlx-ollama-pilot/
├── Cargo.toml              # Workspace raiz
├── crates/
│   ├── core/               # Domínio: tipos, traits, erros
│   ├── providers/
│   │   ├── mlx/            # Provider Apple MLX
│   │   ├── llamacpp/       # Provider llama.cpp
│   │   └── ollama/         # Provider Ollama
│   └── daemon/             # Servidor HTTP principal
├── apps/
│   └── desktop-ui/
│       ├── ui/             # Frontend (HTML, CSS, JS)
│       └── src-tauri/      # Backend Tauri (Rust)
└── scripts/                # Scripts de conveniência
```

---

## 🛣️ Roadmap

- [x] Daemon HTTP com endpoints REST completos
- [x] Provider MLX (Apple Silicon)
- [x] Provider llama.cpp embutido e gerenciado
- [x] Provider Ollama (compatibilidade)
- [x] UI Desktop com Chat e Discover Models
- [x] Catálogo e download de modelos do Hugging Face
- [ ] **Agente de IA nativo do MLX Pilot** — agente próprio com capacidade de executar tarefas, raciocinar sobre contexto local e orquestrar ações de forma autônoma
- [ ] Automações locais e workflows de IA
- [ ] Sistema de plugins e extensões
- [ ] Suporte a mais provedores (OpenAI, Anthropic, etc.)

---

## 🤝 Contribuindo

Contribuições são bem-vindas! Consulte o [README_DEV.md](README_DEV.md) para detalhes completos sobre setup de desenvolvimento, variáveis de ambiente, endpoints da API e build de release.

---

## 📄 Licença

Este projeto está licenciado sob a [MIT License](LICENSE).

---

<p align="center">
  Feito com 🧡 e Rust &nbsp;·&nbsp; <strong>MLX Pilot</strong>
</p>
