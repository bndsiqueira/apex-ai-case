# Engineering decisions

Each entry is a real decision, with what it cost us.

## Stay relational when moving to AWS

The first version ran on Supabase. Moving to AWS raised the question of DynamoDB. The data is deeply relational, and dealership questions are ad hoc: pipeline by stage, leads by source, performance by salesperson.

**Decision:** Aurora PostgreSQL Serverless v2.

**Cost:** connections need care in a Lambda world, and every schema change goes through a migration.

## Send the signal, not the content

Supabase gave us realtime for free. On AWS we had to build it. The WebSocket could carry the whole message, but that creates a second way out for customer data, with its own authorization to maintain.

**Decision:** the socket only says "this conversation changed". The screen then fetches the data through the normal API.

**Cost:** one extra round trip per update.

## Replace database cron with EventBridge Scheduler

The old jobs ran inside the database and carried credentials in their commands.

**Decision:** each job became a schedule that calls a Lambda through IAM.

**Cost:** more infrastructure, and a deploy order to respect.

## One official WhatsApp channel

We once ran three WhatsApp providers at the same time, because Meta's app review was pending and sales couldn't wait. Once Meta approved the app, we removed the others, code first and server after.

**Decision:** one channel, the official Cloud API, with coexistence, so stores keep using the WhatsApp app on their phones.

**Cost:** no fallback. Outside the 24-hour window, a message without an approved template doesn't go out.

## Promote to production by cherry-pick

`dev` is not a superset of `main`. Configuration differs, and merging all of `dev` once came close to overwriting live production settings.

**Decision:** features reach `main` one at a time, by cherry-pick. We audit the difference by content, with `git cherry`, because cherry-picks change commit hashes.

**Cost:** more work per release, and some features need manual adaptation.

## A channel is not a source

Most leads were recorded with "WhatsApp" as their source. That says how they arrived, not where they came from, so marketing reports were misleading.

**Decision:** leads start as "not identified" until something proves the source, such as an ad or a registered phrase.

**Cost:** "not identified" became the largest group at some stores. It was the honest number.

## Make forbidden data impossible to express

The blog publishes results from real stores, and the product handles personal data of end customers.

**Decision:** the blog's content schema has no field for forbidden data, such as money values or raw production screenshots. Automated checks fail closed and block the deploy.

**Cost:** many posts can't be written. A post that fails a check holds back the whole deploy.

## Ship big integrations dark

A large partner integration needed external accreditation before it could run for real.

**Decision:** build it now against a simulator, keep it switched off in production, and send everything through an idempotent outbox with backoff.

**Cost:** a large module in production with no real traffic, and assumptions that only homologation can confirm.

## Put the development environment to sleep

The database minimum capacity was billed around the clock, even in development. Auto-pause alone did nothing, because scheduled engines kept the cluster awake.

**Decision:** in development, engines only run during working hours, and the database can pause when idle. Production never sleeps.

**Cost:** the first request of the morning waits for the cluster to resume.

## Tests block the deploy

For a long time our tests only ran on developers' machines.

**Decision:** tests run in CI and block the deploys that depend on them. Merge request pipelines have no cloud credentials.

**Cost:** one failing test can hold back an unrelated part of the same push.
