# coinw-skills

coinw-skills is an open skills marketplace that gives AI agents native access to crypto trading and data. Search tokens, execute trades, track portfolios, monitor market signals, and manage orders — all through natural language.

Built by CoinW. Built for everyone.

We're not building this just for CoinW products. coinw-skills is designed for the entire crypto ecosystem: any agent, any framework, any chain. Whether you're building on LangChain, CrewAI, or your own stack, your agents can plug into crypto with a few lines of config.

## About This Repository

Each skill lives in its own folder and contains a `SKILL.md` file with YAML frontmatter and structured instructions.

Browse the existing skills to understand patterns and naming conventions before contributing.

## Installation

Get started with coinw-skills in a single command. Works with various agents such as OpenClaw and Claude Code.

### Prerequisites

Before installing coinw-skills, ensure you have the following prerequisites:

- Node.js (version 22 or higher)

### Install coinw-skills

Run the following command to add coinw-skills to your project:

```bash
npx skills add https://github.com/connectCoinw/coinw-skills
```

## Authentication

For CoinW Skills, certain endpoints require you to provide CoinW API credentials. You can do this by setting environment variables, using a secrets file (such as `.env` or `.openclaw/secrets.env`), or sending them directly to the agent in the chat.

For more details, see the Security section in each skill.

## Contribution

We welcome contributions.

To add a new skill:

1. Fork the repository and create a new branch:
2. `git checkout -b feature/<skill-name>`
3. Create a new folder containing a `SKILL.md` file.
4. Follow the required format:

```yaml
---
title: <Skill Name>
description: A clear description of what the skill does and when to use it.
metadata:
  version: <Skill Version>
  author: <Your Github Username>
license: MIT
---

# <Skill Name>

[Add instructions, examples, and guidelines here]
```

Open a Pull Request to `main` for review. Once approved, the skill will be merged.

## Disclaimer

coinw-skills is an informational tool only. coinw-skills and its outputs are provided to you on an “as is” and “as available” basis, without representation or warranty of any kind. It does not constitute investment, financial, trading or any other form of advice; represent a recommendation to buy, sell or hold any assets; guarantee the accuracy, timeliness or completeness of the data or analysis presented. Your use of coinw-skills and any information provided in connection with this feature is at your own risk, and you are solely responsible for evaluating the information provided and for all trading decisions made by you. CoinW does not endorse or guarantee any AI-generated information. Any AI-generated information or summary should not be solely relied on for decision making. AI-generated content may include or reflect information, views and opinions of third parties, and may also include errors, biases or outdated information. CoinW is not responsible for any losses or damages incurred as a result of your use of or reliance on the coinw-skills feature. CoinW may modify or discontinue the coinw-skills feature at its discretion, and functionality may vary by region or user profile. Digital asset prices are subject to high market risk and price volatility. The value of your investment may go down or up, and you may not get back the amount invested. You are solely responsible for your investment decisions and CoinW is not liable for any losses you may incur. Past performance is not a reliable predictor of future performance. You should only invest in products you are familiar with and where you understand the risks. You should carefully consider your investment experience, financial situation, investment objectives and risk tolerance and consult an independent financial adviser prior to making any investment. This material should not be construed as advice. For more information, please see our Risk Warning and Terms of Use.
