# Free Claude Code + SenseNova Skills

[![Watch the video](https://img.youtube.com/vi/Z-66nCBE7ew/maxresdefault.jpg)](https://youtu.be/Z-66nCBE7ew)

▶️ **Watch the full tutorial on YouTube:** https://youtu.be/Z-66nCBE7ew

Run **Claude Code for $0** using OmniRoute and free AI models. Then turn it into a real-world AI agent with **[SenseNova Skills](https://github.com/OpenSenseNova/SenseNova-Skills)**: deep research, data analysis and infographic generation.

This repo contains everything from the video: the config, every prompt, and the demo spreadsheet.

> 🤝 The video is a paid collaboration with SenseNova.

---

## 📋 Contents

- [How it works](#-how-it-works)
- [Requirements](#-requirements)
- [Part 1: Free Claude Code with OmniRoute](#-part-1-free-claude-code-with-omniroute)
- [Part 2: Install SenseNova Skills](#-part-2-install-sensenova-skills)
- [Part 3: The 3 demos (all prompts)](#-part-3-the-3-demos)
- [Troubleshooting](#-troubleshooting)
- [Resources](#-resources)

---

## 🧠 How it works

| Layer | What it does | Cost |
|---|---|---|
| **Claude Code** | The agent: reads files, runs commands, follows skills | Free tool |
| **OmniRoute** | Local gateway that routes Claude Code's requests to free AI models, with automatic fallback | Free, open source |
| **SenseNova Skills** | Plain-text instructions (`SKILL.md`) that teach the agent real workflows. No API of their own: they run on whatever model your agent uses | Free, open source |
| **SenseNova API** | Specialist models some skills call directly, such as image generation and vision quality checks | Free API key |

---

## ✅ Requirements

- [Node.js](https://nodejs.org) (LTS)
- [Python](https://www.python.org) 3.10+
- [VS Code](https://code.visualstudio.com) (or any terminal)
- **Windows only:** [Git for Windows](https://git-scm.com/download/win), which Claude Code needs for Git Bash
- Free accounts and API keys:
  - [OpenRouter](https://openrouter.ai)
  - [NVIDIA NIM](https://build.nvidia.com)
  - [SenseNova API](https://platform.sensenova.ai)

---

## ⚡ Part 1: Free Claude Code with OmniRoute

### 1. Install and start OmniRoute

Follow the install command on [omniroute.online](https://omniroute.online), then start it:

```bash
omniroute
```

Open the local dashboard it prints, log in, and **change the default password**.

### 2. Connect free providers

In OmniRoute → **Providers**, add:

- **OpenRouter**: paste your API key → *Import only free models*
- **NVIDIA NIM**: paste your API key → *Import only free models*
- **OpenCode (free)**: no key required

Click **Test** on each and ignore models that fail; OmniRoute skips them.

### 3. Create a combo

In **Combos**, create a new combo (e.g. `free-agent`):

- Add your models and put the strongest one at the top
- Strategy: **Priority**, which is more stable than round robin for long agent tasks
- Click **Test combo**

Then create an **OmniRoute API key** and save it somewhere safe.

### 4. Install Claude Code

Install it from the official page: https://claude.com/product/claude-code

### 5. Point Claude Code at OmniRoute

Open `~/.claude/settings.json` (on Windows: `C:\Users\<you>\.claude\settings.json`).

**Back up the file first.** If it already has settings, add the `env` block next to them rather than replacing the whole file.

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "http://localhost:20128",
    "ANTHROPIC_AUTH_TOKEN": "YOUR_OMNIROUTE_KEY",
    "ANTHROPIC_MODEL": "free-agent",
    "DISABLE_AUTOUPDATER": "1",

    "SN_BASE_URL": "https://token.sensenova.ai/v1",
    "SN_API_KEY": "YOUR_SENSENOVA_KEY",
    "SN_CHAT_MODEL": "sensenova-6.8-flash-lite"
  }
}
```

| Key | Purpose |
|---|---|
| `ANTHROPIC_BASE_URL` | Sends Claude Code's requests to your local OmniRoute. Use the port OmniRoute shows you. |
| `ANTHROPIC_AUTH_TOKEN` | Your OmniRoute API key |
| `ANTHROPIC_MODEL` | The name of your combo |
| `DISABLE_AUTOUPDATER` | Keeps Claude Code on a version that works with routers (see [Troubleshooting](#-troubleshooting)) |
| `SN_*` | Lets the SenseNova skills call the SenseNova models for image generation, vision and text |

> ⚠️ Never commit or share this file with real keys in it.

### 6. Test it

```bash
claude
```

Say `hi`, then check OmniRoute's control center. You should see the request arrive.

> 💡 OmniRoute must be running whenever you use Claude Code.

---

## 🧩 Part 2: Install SenseNova Skills

Create a project folder (e.g. `skills-demo`), open it in your terminal, start `claude`, and paste:

```text
Install SenseNova-Skills from https://github.com/OpenSenseNova/SenseNova-Skills. Clone it and copy everything inside its skills/ folder into ~/.claude/skills/. Then install the Python requirements for sn-image-base and all the sn-search-* skills, plus python-dotenv. When you're done, list the installed sn- skills.
```

Then:

1. **Restart Claude Code** (`/exit`, then `claude`). Skills only load at startup.
2. Type `/` to see the `sn-` skills.
3. Check your environment:

```text
Run the sn-image-doctor skill.
```

---

## 🚀 Part 3: The 3 demos

Each prompt saves its output into a folder inside your project so you can open the results directly.

### 🔬 Demo 1: Deep Research

**Skill:** `sn-deep-research`. It plans the research, searches academic, developer and community sources in parallel, cross-validates the evidence, and writes a cited report.

```text
Use sn-deep-research in normal mode on this question: Can free open-weight models realistically replace paid coding agents in 2026? Compare research benchmarks, Hugging Face and GitHub activity, and developer discussion on Hacker News and Stack Overflow.

Requirements:
- Save all files under ./research in this project folder, not /tmp.
- Open the Research Workbench progress page so I can watch.
- Run the full pipeline including the citation step, so the final output is ./research/report.md with numbered citations and citations.json.
- Then use sn-md-to-html-report to turn the report into ./research/report.html.
- At the end, list every file you created.
```

**Output:** `research/report.md`, `research/report.html`, `research/citations.json`, plus the plan and evidence files.

> 💡 Without a GitHub token, GitHub search is limited to about 60 requests per hour. Use **quick** or **normal** mode, and wait about an hour between runs.

---

### 📊 Demo 2: Data Analysis

**Skill:** `sn-da-excel-workflow`. It reads every sheet, cleans and combines the data, joins across sheets, and answers a business question with charts. No API key needed.

Download [`sales_2026_messy.xlsx`](./sales_2026_messy.xlsx) from this repo into your project folder. It's deliberately messy:

| Sheet | Problems |
|---|---|
| North | Merged title block and notes above the real header row |
| South_Sales | Different column names, two date formats, prices as text (`€1.299,00`), discounts as `25%`, messy category spellings, duplicate orders |
| West Q1-Q3 | Columns in a different order, monthly subtotal rows mixed in, blank rows, units like `7 pcs` |
| East (Jan-Sep) | German headers, raw Excel date numbers, `n/a` values, no category column |
| Product Costs | Unit costs (needed for margin) and a note about a cost change |

```text
Use sn-da-excel-workflow on sales_2026_messy.xlsx. Business question: which product category is losing margin, and in which region?

Requirements:
- Clean and combine all sheets first.
- Save everything under ./analysis:
  - cleaning_log.md: every problem you found and fixed, per sheet
  - sales_clean.xlsx: the combined, cleaned data in one sheet
  - charts/: PNG charts, including margin by region and category, and the monthly margin trend for the problem area
  - findings.md: the answer in 5 bullet points, with numbers
- At the end, list every file you created.
```

<details>
<summary>🔍 Spoiler: what the AI should find</summary>

- **Main finding:** **Electronics in the South**. Margin falls from about **53% to about 40%** from June, caused by heavy discounting (20–35%). Sales volume goes up, but profit per sale drops sharply.
- **Secondary finding:** **Apparel in the West** dips from about 64% to 59% from July, from extra discounts plus the supplier cost increase mentioned in the Product Costs sheet.
- About **1,280 orders** after cleaning.

</details>

---

### 🎨 Demo 3: Infographic

**Skill:** `sn-infographic`. It expands one sentence into a full design brief, picks a layout and style, generates several candidates, and uses a vision model to inspect and rank them.

```text
Use sn-infographic with max_rounds 3, prompts_expand_mode force, output_mode verbose, aspect ratio 16:9, on this sentence: "Free open-weight models now match paid coding agents on most everyday tasks, but still lag on long, complex projects."

Requirements:
- Save everything under ./infographic:
  - original_prompt.txt: my sentence, unchanged
  - expanded_prompt.txt: the full expanded prompt
  - candidates/: every generated image
  - ranking.md: the quality review and ranking for each candidate, with scores
  - final.png: a copy of the winning image
- At the end, list every file you created.
```

> 💡 With `max_rounds 1` (the default), the skill skips the quality review and ranking, so keep it at 2 or more.
>
> 💡 Want 4K? Add `"SN_IMAGE_GEN_MODEL": "sensenova-u1.5-lite"` to your `settings.json` and add `image size 4k` to the prompt.

**Tip:** swap the sentence for a key finding from your Demo 1 report to connect all three demos.

---

## 🛠 Troubleshooting

**`400 ... role must be user or assistant`**
Newer Claude Code versions send a role some routers don't support. Roll back and keep auto-update disabled ([source: SenseNova FAQ](https://github.com/OpenSenseNova/SenseNova-Skills/blob/main/docs/faq.md)):

```bash
npm install -g @anthropic-ai/claude-code@2.1.153
```

**`claude` is not recognized (Windows)**
Add the folder containing `claude.exe` (usually `C:\Users\<you>\.local\bin`) to your PATH, then restart your terminal.

**Skills don't show up**
Restart Claude Code. Check that the skill folders are in `~/.claude/skills/` (e.g. `~/.claude/skills/sn-infographic/SKILL.md`).

**Infographic fails or the API key isn't found**
Run `sn-image-doctor`. Check that the `SN_*` values are inside the `env` block of `settings.json`, then restart Claude Code.

**A demo stalls or loops**
Free models vary in quality. Move a stronger model to the top of your OmniRoute combo and try again.

**Requests fail entirely**
Make sure OmniRoute is running and the port in `ANTHROPIC_BASE_URL` matches the dashboard.

---

## 🔗 Resources

- 🧠 SenseNova Skills: https://github.com/OpenSenseNova/SenseNova-Skills
- 🔑 SenseNova API: https://platform.sensenova.ai
- 🔀 OmniRoute: https://omniroute.online
- 🤖 Claude Code: https://claude.com/product/claude-code
- 🌐 OpenRouter: https://openrouter.ai
- ⚡ NVIDIA NIM: https://build.nvidia.com
- 💻 VS Code: https://code.visualstudio.com
- 🟢 Node.js: https://nodejs.org

---

⭐ If this helped, star the repo and [watch the video](https://youtu.be/Z-66nCBE7ew). Tell me in the YouTube comments which SenseNova skill you want covered next.

*Free tiers, models and limits change often. If something here stops working, open an issue.*
