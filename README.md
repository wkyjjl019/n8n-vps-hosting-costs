# n8n VPS hosting: how much RAM do you actually need, what does "pre-installed" include, and which plan is still affordable at renewal? (plans compared, real cost math)

n8n runs on 2 vCPU and 4 GB of RAM. A small team is comfortable on 4 vCPU and 8 GB. Anything running queue mode, heavy AI agent chains or a local model on the same box wants 8 vCPU and 16 GB. Those numbers are the easy part.

The harder part is what you actually pay. The VPS market sells cheap first terms that double at renewal, and the cheapest headline price usually requires handing over 24 or 36 months of cash upfront. AvaHost takes the opposite approach for n8n: a €5 app stack with n8n and PostgreSQL provisioned together, billed as a plan rather than a multi-year contract. That makes it the lowest-commitment way to get a working n8n instance, and the right starting point if you're not yet sure how big your workflows will get.

> **Quick decision:** if you want n8n and its database running today without a two-year contract, AvaHost's n8n stack starts at €5 and includes one-click deployment, automatic n8n updates, scheduled backups, custom domain and automated HTTPS. Confirm the vCPU and RAM figures in the order form before paying, since those aren't published on the app page.
> 👉 [See AvaHost's n8n hosting plan and current price](https://my.ava.hosting/aff.php?aff=15562)

## What AvaHost sells for n8n, and what €5 covers

AvaHost treats n8n as one of its app stacks, which means you buy a plan sized for the application plus its database rather than a bare server you configure from scratch. n8n is mapped to Plan 2 at €5, and that price covers the n8n application and the PostgreSQL stack together.

| AvaHost plan | What's included | Price | Billing | Order |
| --- | --- | --- | --- | --- |
| Plan 2 (n8n app stack) | n8n plus PostgreSQL provisioned and connected automatically, one-click deployment, automatic n8n updates, scheduled backups, custom domain support, automated HTTPS. PostgreSQL is included but unmanaged by AvaHost. | €5 | Monthly (confirm term at checkout) | [Get AvaHost's n8n plan](https://my.ava.hosting/aff.php?aff=15562) |

Two things worth knowing before you order. First, AvaHost publishes the price and inclusions on its app page but not the vCPU, RAM or storage figures, so those only appear in the order form. Second, AvaHost itself flags that busy webhook endpoints, large file payloads or several concurrent long-running workflows will need a larger plan than the €5 entry point. If you already know you're pushing big payloads through webhooks, size up rather than migrating later.

The database detail matters more than it looks. PostgreSQL holds your workflow definitions, users, credential metadata and execution records, so it isn't an add-on you can ignore. AvaHost provisions it and connects it, but leaves it unmanaged, which means nobody at the company is tuning it, restoring it or fixing a corrupted table on your behalf. Scheduled backups are included, so the practical job on your side is confirming that a restore actually works before you depend on it.

## The real cost question: renewal price and prepay length

AvaHost's €5 has no multi-year lock attached to it in what the company publishes, which puts the risk on the small side: if the plan turns out too small, you move up or move out having spent €5 a month. Compare that with how the larger VPS brands structure their n8n offers, using their own published numbers.

Hostinger's KVM 1 (2 vCPU, 8 GB RAM, 100 GB NVMe, 8 TB traffic) is advertised at $6.49/mo against a $19.49 regular price, but that rate requires paying **$155.76 today for 24 months**. The same-term renewal price is **$11.99/mo**, so the second 24 months cost $287.76. Over four years that plan costs $443.52, not the $155.76 the page leads with. KVM 4 (4 vCPU, 16 GB, 200 GB NVMe) runs $311.76 upfront at $12.99/mo, then $28.99/mo on renewal, which is $1,007.52 across four years. KVM 8 (8 vCPU, 32 GB RAM, 400 GB NVMe) is $623.76 upfront at $25.99/mo and renews at $49.99/mo.

OBHost shows the same mechanics in a harsher form. Its n8n VPS starts at $6.89/mo with a 14-day money-back window, but the 16 vCPU / 64 GB configuration at $239.97/mo is only available on a 36-month term, which means **$8,638.92 in a single payment**. That is a real number from their pricing page, not a typo.

None of this makes those plans bad. Hostinger's KVM 1 gives you a documented 8 GB of RAM at a first-term price AvaHost doesn't publish a RAM figure for, and if you need that much memory on day one, it's the more concrete buy. The point is what you're committing before you know whether the box fits: $155.76 and 24 months, versus €5 and a month.

👉 [Check AvaHost's €5 n8n stack before you commit to a 24-month term](https://my.ava.hosting/aff.php?aff=15562)

## "Pre-installed" means three different things in this market

This is where n8n VPS listings diverge most, and where the €5 is easiest to justify or dismiss.

**Manual install.** OBHost doesn't preinstall n8n at all. You SSH in as root, install Node.js 18 or newer, run `npm install -g n8n`, then either keep it alive with PM2 (`pm2 start n8n`) or run the Docker image with `docker run --name n8n -p 5678:5678 docker.n8n.io/n8nio/n8n`. Production means adding Nginx as a reverse proxy and a Let's Encrypt certificate. Budget an afternoon, plus the ongoing job of applying updates yourself.

**Image or template.** Hostinger ships an "Ubuntu 24.04 with n8n" template that runs n8n inside Docker, plus a separate template for queue mode. One click gives you a working instance, and you still own the domain, SSL and upgrade path.

**App stack.** AvaHost's one-click deployment provisions n8n and PostgreSQL together, attaches a custom domain with automated HTTPS and applies n8n updates automatically. One.com does something similar with n8n pre-installed alongside Docker and PostgreSQL, and IONOS preinstalls n8n with a Let's Encrypt certificate on both its VPS+ and dedicated lines. This is the least work at handover, and the least control over what version you're on.

Whichever route you take, the first-run security picture is the same. n8n comes up on `your-server-IP:5678`, and that port has to be opened in the provider's firewall before the editor loads. IONOS documents the default protection clearly: the web UI sits behind HTTP Basic Auth with the username `admin` and your server's root password, which you can disable over SSH once a domain and HTTPS are in place. Production use means domain plus SSL, not the raw IP and port.

AvaHost makes one operational point that's easy to skip. External services store your webhook URLs, so the domain should be final before you wire up critical integrations, and every connected service needs retesting after a URL change to confirm triggers, callbacks and authentication still work. If you're migrating from a temporary setup, do the domain move first.

## How much server n8n actually needs

Three independent sources land in the same place. Hostinger's help documentation puts the minimum at 1 vCPU, 2 GB RAM and 20 GB SSD, and recommends 2+ vCPU and 4+ GB. OBHost calls 2 GB a solid start for personal use and small teams, with 4 to 8 GB for production, concurrent workflows and AI agent pipelines. One.com maps light workflows to 2 vCPU / 4 GB, moderate workloads to 4 vCPU / 8 GB, and heavy production to 8 vCPU / 16 GB or above.

| Workload | Sizing | What it covers |
| --- | --- | --- |
| Testing, a handful of webhooks, one person | 2 vCPU / 4 GB | The realistic floor. Below this, node installs and updates get tight. |
| Small team, several scheduled workflows, moderate payloads | 4 vCPU / 8 GB | The comfortable default for most small businesses. |
| Queue mode, many concurrent executions, local AI models | 8 vCPU / 16 GB and up | Queue mode workers eat CPU cores, and a local model competes for the same RAM. |

Two sizing details get lost in spec sheets. PostgreSQL needs headroom beyond the application-only RAM figure, which is why a stack that bundles the database needs more memory than the n8n process alone suggests. And self-hosting removes execution limits but not the ceiling: your cap becomes CPU and RAM, and a workflow that saturates 4 GB will still time out no matter how many executions your plan allows.

On integrations, treat vendor numbers as marketing. Published figures range from 400+ (one.com) through 1,000+ (IONOS) to roughly 1,900 in Hostinger's own FAQ, and Zapier is quoted above 5,000. The template counts are just as inconsistent, with 100+ at IONOS and Hostinger against 9,500+ and 10,000+ on two different parts of one.com's page. If a specific connector decides your purchase, check it exists rather than trusting a total.

## How the n8n VPS options compare on paper

| Provider | Plan | vCPU / RAM / NVMe | Price | Catch |
| --- | --- | --- | --- | --- |
| AvaHost | Plan 2 (n8n app stack) | Not published on the app page | €5/mo | PostgreSQL unmanaged; larger plan needed for heavy webhook load |
| Hostinger | KVM 1 | 2 / 8 GB / 100 GB | $6.49/mo on 24-month prepay ($155.76 today), renews $11.99/mo | Support covers the VPS and that n8n is running, not your workflows |
| Hostinger | KVM 4 | 4 / 16 GB / 200 GB | $12.99/mo prepaid ($311.76), renews $28.99/mo | Same support boundary |
| One.com | Cloud Server S | 2 / 4 GB | Not shown on its n8n page | EU data, GDPR-ready, ISO 27001 |
| OVHcloud | VPS-4 | 8 / 24 GB / 200 GB | From ₹2,240/mo ex GST (India pricing) | You own OS patching, firewall, n8n version and credentials |
| OBHost | Entry n8n VPS | 1 / 2 GB / 60 GB | From $6.89/mo, 14-day refund | No preinstall; manual SSH setup |

## Where AvaHost fits, and who it suits

AvaHost is a Moldova-based provider (Chisinau) that has been selling hosting since 2001, and a 2026 third-party review describes its data center as a hardened underground facility with crypto billing accepted. That combination, a non-EU jurisdiction plus private payment, is the specific reason some people pick it over Hostinger or one.com, both of which put your data in EU data centers. If your reason for self-hosting is jurisdiction rather than price, this is the differentiator that matters.

Reviews are mixed, and worth reading before you buy. Trustpilot carries praise from long-term users about support and uptime alongside complaints about inconsistent network performance and disputes over account suspensions. The same third-party review notes 24/7 chat, ticket and phone support with replies in the 15 to 30 minute range and managed options on higher plans. Nothing in that is unusual for an offshore host, but "budget offshore provider" and "hands-off managed platform" are different products.

The €5 stack is a good fit if you want n8n and PostgreSQL running without a contract, you're comfortable owning your database, and you'd rather move up a plan than argue about a renewal price. It's a poor fit if you need a documented RAM guarantee before paying, if you want someone else to tune PostgreSQL, or if you need a support engineer to debug a broken workflow.

## Who shouldn't buy a VPS for n8n at all

Self-hosting swaps a subscription for maintenance. OVHcloud states the split plainly: it secures hardware and network, while you handle OS updates, firewall rules, HTTPS, n8n updates and credential management. Hostinger's support scope is narrower still, covering the VPS environment and confirming the n8n application is running, with no troubleshooting for internal workflows, configuration or custom code.

If you don't want to patch an operating system or debug a failing credential at 11pm, a managed automation platform is the honest choice. The same applies if your workflows handle unattended critical processes with nobody watching executions, which is a point AvaHost makes itself: n8n works best when each workflow has an owner, documented credentials, test data and a failure plan.

## FAQ

**Can I move existing workflows in?** Yes. n8n exports and imports workflows as JSON, and Hostinger explicitly documents migrating from n8n Cloud, Render and DigitalOcean with workflows preserved. On AvaHost's stack, PostgreSQL is already in place, so you're importing into a working database rather than setting one up.

**Will my AI data leave the server?** It depends which nodes you use. Calling OpenAI, Anthropic or Gemini sends the data to those providers over an encrypted connection, and IONOS spells out that the entered data is transmitted to their cloud for processing. Running open-source models locally through Ollama or Llama keeps everything on your own hardware, at the cost of RAM and CPU.

**SQLite or PostgreSQL?** SQLite is the default and fine for most single-user setups. High-volume production benefits from PostgreSQL or MySQL, configured through environment variables such as `DB_TYPE` and `DB_POSTGRESDB_HOST`. AvaHost ships PostgreSQL from the start, which removes that migration later.

**What's the refund window?** Hostinger offers 30 days and OBHost 14 days. AvaHost's n8n app page doesn't state a refund period, so ask support in writing before you pay, and keep the answer.

## Before you order

Match the plan to the workload, not to the discount: 2 vCPU / 4 GB to start, 4 vCPU / 8 GB for a small team, 8 vCPU / 16 GB once queue mode or a local model is in the picture. Then confirm two things in the order form, because neither is on the marketing page: the actual vCPU and RAM you're getting for €5, and the refund terms if the plan turns out too small.

If a monthly €5 with n8n and PostgreSQL provisioned together beats $155.76 and a two-year commitment for your situation, that's your answer.

👉 [Open AvaHost's n8n hosting plan and start with the €5 stack](https://my.ava.hosting/aff.php?aff=15562)
