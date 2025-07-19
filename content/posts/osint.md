+++
date = 2025-06-28
title = "Google Dorking for Pentesting with a Real Case"
slug = "google-dorking-ethical-hacking-data-exposure"
author = ["Gonci"]
description = "How a basic Google Dorking practice led me to exposed secrets (.env, .git, JWT keys) on a live server. Breaking down the process and why these techniques matter for OSINT and ethical security"
draft = false
tags = [
  "OSINT",
  "Google Dorking",
  "Cybersecurity",
  ".env Exposure",
  ".git Exposure",
  "JWT Secret Exposure",
  "Disclosure Vulnerabilities",
  "Security Misconfiguration",
  "Web Security"
]
+++

## Disclaimer
This post describes techniques used for educational and ethical purposes only. All information discovered was publicly indexed by Google and accessible without any authentication or exploitation. The intention is to raise awareness about common security misconfigurations and help organizations improve their defenses. The issues found were responsibly reported to the company.

## Introduction
Sometimes, the simplest tools reveal the biggest problems. While practicing open-source intelligence (OSINT) techniques, I used a basic Google Dork to find sensitive files exposed on a live web server with **no** exploitation, just careful searching.

In this post, I’ll break down how I found it, what was exposed, and how you can protect your projects from the same mistakes.

## What Is Google Dorking and Why Does It Matter?
Google Dorking is the practice of using advanced search operators to find specific information indexed by search engines, sometimes sensitive data that was never meant to be public. For ethical hackers and security researchers, it’s a powerful tool to check how secure your own assets really are.


## The Dork I Used
For this test, I tried a simple query: `intitle:index.of ".git"`, this dork looks for open directories with exposed `.git` folders.

![Google Dork Serach](/images/osint_search.png)

Surprisingly, the Google dork led me not only to an exposed .git folder but also to a directory listing revealing the entire backend source code and the .env file containing sensitive credentials on a live admin panel.

![Directory Listing and Exposed Data](/images/osint_exposed_data.png)

Here’s what was publicly accessible:
- **`.env` file:** Contained MongoDB credentials and a live JWT secret key.
- **`.git` directory:** The entire source code, commit history, and configs were downloadable.
- **Directory listing:** Anyone could browse files and folders without restriction.
- **Development environment:** The `.env` file showed `ENV=development`, indicating a misconfigured live deployment.
- **Hardcoded Credentials:** The javascript files weren’t being interpreted, so the people where able to read the content showing different MongoDB Password that the `.env` file 

![Env File Exposed](/images/osint_env_certs.png)

**Important:** All of this was possible without any hacking, just a search and public reading public files.

## Reporting It Responsibly
Obviously I'd **not** use any credentials or exploit the system. Instead, I contacted the company with a clear report recommending:
1. Restrict access to `.env` and `.git`.
2. Disable directory listing.
3. Rotate secrets and passwords immediately.
4. Review deployment configs.

![Final Report to Company](/images/osint_report.png)