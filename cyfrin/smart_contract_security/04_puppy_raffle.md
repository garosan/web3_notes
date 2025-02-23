# Section 04. Puppy Raffle

## Introduction

In this audit we will:

- Practice some manual review
- Use tools for static analysis

We'll see the differences between a private audit report and a competitive audit submission and be introduced to the process of competing in a CodeHawks First Flight!

Tools we will be using:

- **[Slither](https://github.com/crytic/slither)** - A pythonic static analysis tool compatible with Solidity and Vyper
- **[Aderyn](https://github.com/Cyfrin/aderyn)** - Built in Rust by _Alex Roan_, Aderyn traverses Abstract Syntax Trees to highlight suspected vulnerabilities.

### Case Studies

We'll dive into real world case studies detailing times these vulnerabilities were exploited in the wild.

At the end of the section, we'll have _even more_ exercises to challenge yourself beyond the course's teachings. These are opportunities to branch out, network and gain additional experience.

This includes participating in a CodeHawks First Flight or a competitive audit! Get ready!

### Prep for Puppy Raffle

If you take a look at the **[repo](https://github.com/Cyfrin/4-puppy-raffle-audit)** associated with this section, you'll see a fairly robust README already supplied. For this review, we're assuming the protocol has already undergone some degree of onboarding and they've provided us a respectable repo.

## Puppy Raffle Primer

1. Do **not** look at the `audit-data` branch of the course **[repo](https://github.com/Cyfrin/4-puppy-raffle-audit)**. This is our `answer key`.

2. Take some time to scope the codebase yourself before proceeding. Try to go through the process we just did with PasswordStore and challenge yourself to find what you can here.

## Phase 1: Scoping
