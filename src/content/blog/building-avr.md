---
title: "Inside the Application Vulnerability Report"
description: "How I built AVR — a runtime vulnerability scanner for Cloud Foundry applications on SAP BTP that combines open-source and proprietary scanners to catch what shift-left pipelines miss."
date: 2026-09-22
tags: ["security", "cloud-foundry", "sap-btp", "open-source"]
---

Most security tooling lives in the CI pipeline. You scan during the build, catch what you can, and hope nothing slips through. But what about the applications that are already deployed — the ones running in production right now, maybe with dependencies that were clean six months ago but have a CVE today?

That's the gap the Application Vulnerability Report (AVR) was built to close.

## The problem

Open-source vulnerabilities are a constant. New CVEs drop daily, and any application with third-party dependencies is a potential target. The shift-left approach — scanning in your pipeline — is necessary but not sufficient. It only catches issues at deploy time. If a vulnerability is disclosed *after* your last deployment, your running application is exposed and nobody knows.

We needed something that watches what's already deployed, continuously, without requiring developers to opt in or change their workflow.

## What AVR does

AVR scans custom Cloud Foundry applications at runtime on SAP BTP. Instead of relying on a single scanner, it uses a proprietary scanning layer that combines multiple engines:

- **[OSV](https://osv.dev/)** (Open Source Vulnerabilities) — Google's open-source vulnerability database
- **[Trivy](https://trivy.dev/)** — Aqua Security's comprehensive scanner for containers and filesystems
- **Custom SAP BTP-specific scanners** — targeting platform-specific dependency patterns
- **0-day exploit targeted scanners** — focused on actively exploited vulnerabilities

This layered approach gives us broad and up-to-date coverage. No single scanner catches everything, but the combination covers a lot of ground.

## How it works

At the time of writing this, scans run twice per week — Mondays and Thursdays. The service inspects what's actually running in your Cloud Foundry spaces, resolves dependency trees, and matches them against known CVEs from multiple vulnerability databases.

The results surface in two ways:

**A dashboard in the SAP BTP cockpit.** The findings view lets you visualize, prioritize, and act on vulnerabilities without building custom reporting. A separate scanning history dashboard shows which applications were scanned and when — useful for tracking whether fixes were actually applied and for providing audit evidence that applications are regularly scanned.

**A CAP-based OData API.** This is where it gets interesting for automation. You can pipe AVR findings directly into your ITSM workflows or ticketing systems. Security team gets a new finding? It becomes a Jira ticket automatically. No manual triage step.

## Why runtime matters

The key insight behind AVR is that the deployed state is the one that matters. Your pipeline might scan at build time, but:

- Dependencies get updated without redeploying
- Transitive dependencies pull in vulnerable versions you never explicitly chose
- Applications deployed months ago accumulate new CVEs silently
- Some applications were deployed before your scanning pipeline existed

AVR targets the applications you've developed and deployed on Cloud Foundry — the ones you're responsible for. It doesn't scan SAP's own platform services, just your custom code.

## Availability

At the time of writing this, AVR runs in the Cloud Foundry environment on SAP BTP with an Always Free service plan. Subscriptions are available in `cf-eu10` and `cf-us10`, though scanning covers all supported global landscapes. It's currently in beta, available for both trial and enterprise accounts.

## What I took away from building this

Working on AVR reinforced a few things I believe about security tooling:

**Meet developers where they are.** If using the tool requires changing a workflow, adoption will be low. AVR scans automatically — there's nothing to configure per-application.

**Layer your scanners.** A single vulnerability database has blind spots. Combining OSV, Trivy, and custom scanners means a CVE that one misses, another catches.

**Runtime visibility is non-negotiable.** Shift-left is great for catching issues early. But production is where the risk actually lives, and you need eyes on it continuously.
