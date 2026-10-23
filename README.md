<div align="center">

# goose

_your native open source AI agent — desktop app, CLI, and API — for code, workflows, and everything in between_

<p align="center">
  <a href="https://opensource.org/licenses/Apache-2.0"
    ><img src="https://img.shields.io/badge/License-Apache_2.0-blue.svg"></a>
  <a href="https://discord.gg/n8R5VaWDAn"
    ><img src="https://img.shields.io/discord/1287729918100246654?logo=discord&logoColor=white&label=Join+Us&color=blueviolet" alt="Discord"></a>
  <a href="https://github.com/aaif-goose/goose/actions/workflows/ci.yml"
     ><img src="https://img.shields.io/github/actions/workflow/status/aaif-goose/goose/ci.yml?branch=main" alt="CI"></a>
  <a href="https://insights.linuxfoundation.org/project/goose"><img src="https://insights.linuxfoundation.org/api/badge/health-score?project=goose"></a>
  <a href="https://repology.org/project/goose-cli/versions"><img src="https://repology.org/badge/tiny-repos/goose-cli.svg" alt="Packaging status"></a>
</p>

<a href="https://trendshift.io/repositories/25298?utm_source=repository-badge&amp;utm_medium=badge&amp;utm_campaign=badge-repository-25298" target="_blank" rel="noopener noreferrer"><img src="https://trendshift.io/api/badge/repositories/25298" alt="aaif-goose%2Fgoose | Trendshift" width="250" height="55"/></a>

</div>


goose is a general-purpose AI agent that runs on your machine. Not just for code — use it for research, writing, automation, data analysis, or anything you need to get done.

A native desktop app for macOS, Linux, and Windows. A full CLI for terminal workflows. An API to embed it anywhere. Built in Rust for performance and portability.

goose works with 15+ providers — Anthropic, OpenAI, Google, Ollama, OpenRouter, Azure, Bedrock, and more. Use API keys or your existing Claude, ChatGPT, or Gemini subscriptions via [ACP](https://goose-docs.ai/docs/guides/acp-providers). Connect to 70+ extensions via the [Model Context Protocol](https://modelcontextprotocol.io/) open standard.

goose is part of the [Agentic AI Foundation (AAIF)](https://aaif.io/) at the Linux Foundation.

# Get started

**[Download the desktop app](https://goose-docs.ai/docs/getting-started/installation)** for macOS, Linux, and Windows.

Or install the CLI:

```bash
curl -fsSL https://github.com/aaif-goose/goose/releases/download/stable/download_cli.sh | bash
```

# Architecture

Arrows mean “uses.” Select a component or capability to open its source or documentation. The assembled agent lists the capabilities integrated within goose; MCP servers, skills, and plugins are extension inputs that may come from external projects.

Blue boxes are Goose interfaces and the assembled agent; green marks extracted libraries; purple marks extension inputs; gray with dashed borders marks applications and inference engines outside this repository.

```mermaid
%%{init: {"theme":"dark","themeVariables":{"background":"#101928","edgeLabelBackground":"#101928","lineColor":"#8799b5","fontFamily":"Inter, system-ui, sans-serif"},"flowchart":{"wrappingWidth":460}}}%%
flowchart TB
    subgraph APPS["Applications"]
        Desktop["goose desktop"]:::owned
        Buzz["Buzz"]:::community
        Berd["Berd"]:::community
        Other["Applications …"]:::community
    end

    ACP["Goose ACP server"]:::owned
    CLI["goose CLI"]:::owned
    Bindings["Python / Kotlin bindings<br/><br/>UniFFI"]:::owned
    Desktop --> ACP
    Buzz --> ACP
    Berd --> ACP
    Other --> ACP
    Other --> Agent
    Other --> Bindings
    ACP --> Agent
    CLI --> Agent

    MCP["MCP servers"]:::standard
    SkillFiles["Skills"]:::standard
    PluginPackages["Plugins"]:::standard
    Agent --> MCP
    Agent --> SkillFiles
    Agent --> PluginPackages

    Agent["`<a href='https://github.com/aaif-goose/goose/blob/main/crates/goose/src/agents/agent.rs' target='_blank'>Assembled Goose agent</a><br/><br/><a href='https://github.com/aaif-goose/goose/blob/main/crates/goose/src/agents/state_machine_builder.rs' target='_blank'>Operation pipeline</a> · <a href='https://github.com/aaif-goose/goose/tree/main/crates/goose/src/permission' target='_blank'>approvals</a> · <a href='https://github.com/aaif-goose/goose/tree/main/crates/goose/src/hooks' target='_blank'>hooks</a> · <a href='https://github.com/aaif-goose/goose/blob/main/crates/goose/src/agents/state_machine/ops_retry.rs' target='_blank'>retries</a> · <a href='https://github.com/aaif-goose/goose/blob/main/crates/goose/src/agents/state_machine/ops_steer.rs' target='_blank'>steering</a> · <a href='https://github.com/aaif-goose/goose/tree/main/crates/goose/src/session' target='_blank'>sessions &amp; persistence</a> · <a href='https://github.com/aaif-goose/goose/tree/main/crates/goose/src/agents/extension_manager' target='_blank'>tools, extensions &amp; MCP connections</a> · <a href='https://github.com/aaif-goose/goose/blob/main/crates/goose/src/agents/platform_extensions/code_execution.rs' target='_blank'>Code Mode</a> · <a href='https://github.com/aaif-goose/goose/tree/main/crates/goose/src/skills' target='_blank'>skill integration</a> · <a href='https://github.com/aaif-goose/goose/tree/main/crates/goose/src/plugins' target='_blank'>plugin integration</a> · <a href='https://github.com/aaif-goose/goose/blob/main/crates/goose/src/agents/subagent_handler.rs' target='_blank'>subagent lifecycle</a> · <a href='https://github.com/aaif-goose/goose/blob/main/crates/goose/src/agents/provider_manager.rs' target='_blank'>provider configuration</a> · <a href='https://github.com/aaif-goose/goose/tree/main/crates/goose/src/oauth' target='_blank'>sign-in</a> · <a href='https://github.com/aaif-goose/goose/tree/main/crates/goose/src/live_voice' target='_blank'>live voice</a> · <a href='https://github.com/aaif-goose/goose/blob/main/crates/goose/src/agents/gen_ai_telemetry.rs' target='_blank'>telemetry</a>`"]:::owned

    Loop["Agent loop<br/><br/>State machine · inference / tool machinery<br/>Subagent machinery · skill utilities"]:::extracted
    Compaction["Compaction"]:::extracted
    Providers["Providers<br/><br/>Model implementations · request logging<br/>Realtime / voice provider adapters"]:::extracted
    Local["Local inference engines"]:::extracted
    Eredu["eredu<br/><br/>Local model runtime"]:::community
    Mesh["Mesh LLM<br/><br/>Distributed inference"]:::community

    Agent --> Loop
    Agent --> Compaction
    Agent --> Providers
    Providers --> Local
    Local --> Eredu
    Providers --> Mesh

    Bindings --> Providers
    Bindings --> Compaction

    click Buzz href "https://github.com/block/buzz-app" _blank
    click Berd href "https://github.com/block/berd" _blank
    click Desktop href "https://github.com/aaif-goose/goose/tree/main/ui/desktop" _blank
    click ACP href "https://github.com/aaif-goose/goose/tree/main/crates/goose/src/acp" _blank
    click CLI href "https://github.com/aaif-goose/goose/tree/main/crates/goose-cli" _blank
    click Bindings href "https://github.com/aaif-goose/goose/tree/main/crates/goose-sdk" _blank
    click Loop href "https://github.com/aaif-goose/goose/tree/main/crates/goose-agent" _blank
    click Compaction href "https://github.com/aaif-goose/goose/tree/main/crates/goose-context-management" _blank
    click Providers href "https://github.com/aaif-goose/goose/tree/main/crates/goose-providers" _blank
    click Local href "https://github.com/aaif-goose/goose/tree/main/crates/goose-local-inference" _blank
    click MCP href "https://modelcontextprotocol.io/" _blank
    click SkillFiles href "https://agentskills.io/specification" _blank
    click PluginPackages href "https://github.com/aaif-goose/goose/tree/main/crates/goose/src/plugins" _blank
    click Mesh href "https://github.com/Mesh-LLM/mesh-llm/" _blank
    click Eredu href "https://github.com/jbg/eredu" _blank

    classDef owned fill:#193552,stroke:#6395c4,color:#e2eaf4,stroke-width:1px
    classDef community fill:#30343c,stroke:#9099a7,color:#e3e7ed,stroke-width:1px,stroke-dasharray:5 4
    classDef extracted fill:#203e2d,stroke:#73a889,color:#dfede3,stroke-width:1px
    classDef standard fill:#382449,stroke:#ae83c2,color:#eadff0,stroke-width:1px
    style APPS fill:transparent,stroke:transparent
    linkStyle default stroke:#8799b5,color:#d3dded
```

# Quick links
- [Quickstart](https://goose-docs.ai/docs/quickstart)
- [Installation](https://goose-docs.ai/docs/getting-started/installation)
- [Tutorials](https://goose-docs.ai/docs/category/tutorials)
- [Documentation](https://goose-docs.ai/docs/category/getting-started)
- [Governance](https://github.com/aaif-goose/goose/blob/main/GOVERNANCE.md)
- [Custom Distributions](https://github.com/aaif-goose/goose/blob/main/CUSTOM_DISTROS.md) — build your own goose distro with preconfigured providers, extensions, and branding

## Need help?
- [Diagnostics & Reporting](https://goose-docs.ai/docs/troubleshooting/diagnostics-and-reporting)
- [Known Issues](https://goose-docs.ai/docs/troubleshooting/known-issues)

# a little goose humor 🪿

> Why did the developer choose goose as their AI agent?
> 
> Because it always helps them "migrate" their code to production! 🚀

# goose around with us
- [Discord](https://discord.gg/n8R5VaWDAn)
- [YouTube](https://www.youtube.com/@goose-oss)
- [LinkedIn](https://www.linkedin.com/company/goose-oss)
- [Twitter/X](https://x.com/goose_oss)
