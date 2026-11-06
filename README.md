# Apex AI

Apex AI is a multi-tenant SaaS for vehicle dealerships in Brazil. It answers leads on WhatsApp with AI, puts every lead in a sales pipeline with a clear owner, and keeps after-sales in the same place.

I'm a co-founder and the tech lead. I'm the only developer today, and I led the developers who worked on the product before.

The product is private, so this repository has no source code. It explains what we built and the engineering decisions behind it.

## The problem

A dealership loses a sale in the first minutes. A customer asks about a bike on WhatsApp at 8 p.m. and nobody answers until morning. Leads come from several channels, sit in salespeople's personal phones, and the manager can't see what happened to them.

## What the product does

1. A lead arrives from WhatsApp, Instagram, Meta Lead Ads, the website or a spreadsheet import.
2. An AI agent greets the customer, works out whether they want sales, service or parts, and qualifies them.
3. The agent can book a visit and collect a financing form.
4. It hands off to a person with a summary of the conversation, and the lead enters the right pipeline stage with an owner.
5. The salesperson continues in the same inbox, on the official WhatsApp API.
6. After the sale, a separate after-sales pipeline keeps the relationship going.

Around that flow there are automations, broadcasts with opt-out, a calendar, a catalog, financing simulations, subscription billing and invoicing.

## Results

At a Yamaha dealership group that runs the AI on WhatsApp:

| Metric | Before | After |
| --- | --- | --- |
| First response time | about 30 minutes | about 3 minutes |
| Lead conversion | 2.4% | 3.6% |
| Token cost per customer served | | R$ 0.11 |

## Size of the system

Counted in November 2026.

| | |
| --- | --- |
| Lambda functions | 80+ |
| CloudFormation stacks | about 30, across 9 CDK apps |
| Database migrations | 200+ |
| Database tables | 100+ |
| Commits on the development branch | 4,700+ |
| Written design specs | 190+ |

## Stack

- **Frontend:** React, TypeScript, Vite, Tailwind CSS
- **Backend:** AWS Lambda, API Gateway (REST and WebSocket), Cognito, EventBridge Scheduler
- **Data:** Aurora PostgreSQL Serverless v2, DynamoDB for socket connections, S3, CloudFront
- **Infrastructure:** AWS CDK only, with a separate AWS account per environment
- **Integrations:** WhatsApp Cloud API, Instagram, Meta Lead Ads, Stripe, Google Calendar, a dealership ERP
- **AI:** OpenAI for the live agents, Claude through MCP for the sales reports
- **Marketing site and video:** Astro, Remotion

## Read more

- [Architecture](docs/architecture.md): how a message travels through the system
- [AI agents](docs/ai-agents.md): what each agent does and how we keep them reliable
- [Engineering decisions](docs/decisions.md): the choices that shaped the product, with their trade-offs

## Contact

[LinkedIn](https://www.linkedin.com/in/bernardo-bergamini-siqueira-0b7843358/) · [GitHub](https://github.com/bndsiqueira)

---

© 2026 Bernardo Bergamini Siqueira. All rights reserved.
