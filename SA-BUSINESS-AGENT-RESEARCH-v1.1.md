# South African Business Compliance Agent — Channel Research v1

**Version:** 1.1 (with addendum)  
**Date:** 2026-10-21  
**Scope:** South Africa (national, provincial, municipal archetypes)  
**Purpose:** Define the complete regulatory/ licensing/ approval/ authorisation surface for an AEQUCHAIN/AEFRI business agent, index the forms and portals, and mark the exact point where automation must stop and a human authority must act.  
**Status:** Research complete for v1; not legal advice. Every legal-status claim carries an evidence grade. Counsel must verify before reliance.  
**Companion:** `SA-BUSINESS-AGENT-RESEARCH-v1.1-ADDENDUM.md` (verification register, municipal adapters, counsel packet pointer)

---

## 0. Status, scope, and evidence boundary

### 0.1 What this document is

This is a **regulatory operations map**, not a legal opinion. It answers four questions:

1. What must a business in South Africa register, licence, apply for, declare, pay, or renew — from incorporation to land to sector-specific authorisation?
2. Where is each form, which portal accepts it, what format is it, and what does the applicant need to supply?
3. At which exact step does an agent stop being able to act, who must act instead, and what evidence proves that they did?
4. What tooling exists, what is missing, and what should be built?

### 0.2 Evidence grades

| Grade | Meaning | How to treat it |
|---|---|---|
| **V** | Fetched and read **in this session** on 2026-10-21. URL recorded. | Current at retrieval; still check effective dates. |
| **R** | Retrieved earlier in this engagement, **not re-executed** in this session. | Provisional. Re-check before production use. |
| **S** | Secondary source (law firm, industry body, vendor documentation). | Directional; not regulator text. |
| **U** | Unverified or conflicting. | Do not rely. Listed only to show the gap. |

Only the following were re-executed this session: SARS VAT page; FIC homepage, Directive 12 news item, directives index, RCR page; Deneys Reitz analysis of the Public Procurement Act judgment; the PDALA law-library note (search snippet); the NEAS portal (search snippet and portal description); the CIPC homepage; and the ECTA 25 of 2002 PDF for s13. No consolidated Act text was re-read in full this session. In v1.1 every channel was re-checked against its official source and the per-channel result is recorded in `sa_verification_register.py` and `SA-BUSINESS-AGENT-RESEARCH-v1.1-ADDENDUM.md`.

### 0.3 Known blockers

- Web search returned HTTP 401 throughout this session. Direct fetch and DuckDuckGo HTML were used instead.
- Several official sites returned `000`, `403`, or `500` to `curl` at the time of check (BizPortal, CIDB, DFFE, DMRE, NRCS, PSIRA, Deeds, CSG, NDA, gov.za). This is an **accessibility observation**, not proof that the site is dead. Browser access may still work.
- Municipal portals are not one system. Cape Town, Johannesburg, Tshwane, eThekwini and the smaller municipalities use different vendors, login models, and document requirements. The index treats municipalities as **archetypes** with per-municipality adapter configuration, not as a single channel.

### 0.4 What "complete" means here

"Complete" is not a static list. It is a **versioned graph** with four properties:

1. **Coverage** — every channel that can block an entity, a site, or a payment.
2. **Freshness** — each record has `last_verified`, `sources`, an evidence grade, and a freshness SLA in the maintenance plan (§C.3). `effective_from` is carried only where the commencement date is known; where it is not, the record says so rather than guessing.
3. **Authority** — each record names the human gate and the legal basis for that gate.
4. **Executability** — each record resolves to a form, a portal, a fee, a receipt type, and a status query.

A PDF of forms is not a regulatory graph. A spreadsheet of links is not an index. The JSON companion to this report is the first version of that graph.

---

## 1. Executive answer

**1. The binding constraint is authority, not intelligence.** An agent can read the statute, select the form, fill every field, attach every annexure, calculate the fee, and write the covering letter. It cannot be the director who signs the declaration, the EAP who certifies the report, the conveyancer who lodges the transfer, the auditor who signs the AFS, or the municipal official who exercises discretion. The product is therefore not "an agent that files everything". It is **an agent that drives every process to its authority frontier, then hands a bound, validated, pre-authorised packet to the one person who can cross it**.

**2. South Africa is not one licensing system.** It is at least ten stacks: entity and governance; tax; labour; sector licence; land and planning; environment, water and heritage; building and occupancy; trade and product; data and financial crime; and recurring compliance. A business agent that models "company registration" as the whole problem will miss the 60–80 channels that actually determine whether a site can be used, a product sold, or a fund raised.

**3. The default automation ceiling is C6 — "ready-to-execute".** C6 means every field is filled, every annexure is attached, the fee is calculated, the signature/commissioning requirements are flagged, and the submission instructions are generated. C7 ("authorised") requires a **gate token** bound to the payload hash and issued by a human with verified authority. C8 ("submitted") requires either a portal adapter acting on that token or a human submission. C9 is tracking and closure. Without a human action, C6 is the ceiling. This is not a limitation to be engineered away; it is the legal boundary.

**4. Form-filling tooling exists; the South African orchestration layer does not.** Playwright MCP already handles HTML forms, file uploads, clicks, and network inspection. Public PDF-form MCPs handle AcroForm/XFA. Adobe Acrobat Sign and DocuSign MCPs handle e-signature. What does **not** exist is a South African regulatory operations MCP with a channel registry, form resolver, fee/deadline calculator, portal adapters, gate broker, and evidence ledger. That is the gap worth building.

**5. Land sourcing is the deepest stack and the least automatable.** The paper chain runs: title deed and SG diagram → zoning certificate → land-use/rezoning → environmental screening and authorisation → water use → heritage → subdivision/township → site development plan → building plan → occupation → rates clearance → transfer duty → deed registration. An agent can do perhaps 60–70% of the **paper**, and close to 0% of the **physical inspection, professional certification, public participation, and municipal discretion**. The value is in sequencing, red-flag detection, and preventing a buyer from acquiring land that cannot be used for the intended purpose.

**6. "Current law" is a moving target and two 2026 traps are live.** The Public Procurement Act 28 of 2024 was **struck down in its entirety on 17 September 2026** in *Premier of the Western Cape Government v Speaker of the National Assembly* [2026] ZACC 37, and had never commenced; the old PFMA/MFMA/PPPFA framework remains. The Preservation and Development of Agricultural Land Act 39 of 2024 is **not in force**; the Subdivision of Agricultural Land Act 70 of 1970 remains the operative constraint because its Repeal Act never commenced. An agent that hardcodes "new law" will be confidently wrong. Freshness metadata and change monitoring are not nice-to-haves; they are the product's safety system.

**7. AEFRI/AEQUCHAIN's highest-risk gates are financial, not administrative.** In descending order: (a) FAIS advice/intermediary services and the CASP licence; (b) collective investment scheme / pooled-funds law; (c) National Credit Act lending; (d) FIC accountable-institution duties, including the RMCP; (e) exchange control; (f) the investor portal and offers to the public; (g) payment-system authorisation. These are H5/H4/H1 gates. The agent can prepare, evidence, and route; it cannot be the licensed principal.

**8. The right architecture is a thin regulatory kernel plus adapters, not a monolith.** One `sa-regops` MCP with a versioned channel graph, a gate broker, and an evidence ledger. Portal adapters are plugins with selector hashes and canary tests. Document generation and PDF filling are commodity dependencies. The model never holds raw credentials; it holds scoped, short-lived session tokens and gate tokens.

**9. The evidence ledger is the moat.** Anyone can fill a form. The defensible asset is a hash-chained record that proves: what the agent knew, which version of the rule it applied, what it generated, who authorised it, what was submitted, what the portal returned, and when the obligation next falls due. That record is also the audit trail a regulator, a bank, or an investor will ask for.

**10. Build in 90 days, but measure the boundary, not the demo.** The first release should cover ~70 channels, 8 portal adapters, 12 form templates, 8 gate types, and a replayable test harness. The success metric is not "forms filled". It is: percentage of channels reaching C6 without human rework; gate cycle time; portal rejection rate; stale-law detection rate; and zero submissions without a verified gate token.

---

## 2. The legal boundary: what an agent may do

### 2.1 The agent is an instrument, not a principal

South African law generally permits a person to act through an agent, but the **legal person remains the applicant**. Company filings are made by or on behalf of the company; tax returns are made by the taxpayer; licence applications are made by the licensee; land-use applications are made by the owner or an authorised representative. The agent can prepare and transmit. It cannot acquire the applicant's duties, and it cannot cure a false declaration by pointing to a model.

Practical consequence: every channel needs an **authority record** — a board resolution, a power of attorney, a director's mandate, or a portal delegation — before the agent may submit. The authority record is checked at H2, not assumed at C6.

### 2.2 Electronic transactions and signature

The Electronic Communications and Transactions Act 25 of 2002 (ECTA) gives electronic data and electronic signatures legal effect, subject to exceptions. Section 13(1) provides that where a law requires a signature and does not specify the type, the requirement is met in relation to a data message **only if an advanced electronic signature is used**. Section 13(3) governs signatures required by the parties themselves, and s13(5) preserves expressions of intent that are not signed. Many portals accept an authenticated click plus OTP under their own enabling rules; others require a wet signature, a commissioner of oaths, a notary, or a witness. The classification below is therefore portal-specific, not a general licence to e-sign.

The agent must classify every signature block into one of four types:

| Signature type | Typical channels | Who can satisfy it | Agent's role |
|---|---|---|---|
| **Portal authentication** (login + OTP) | CIPC, SARS eFiling, FIC goAML, most municipal portals | The registered user / director / tax practitioner | Prepare; prompt; never hold the OTP seed |
| **Standard e-signature** | Commercial contracts, some licence applications | Authorised signatory | Prepare; route to e-sign MCP; bind hash |
| **Wet signature / witness** | Affidavits, some municipal forms, resolutions | Director/member + witness/commissioner | Prepare; flag; instruct; verify scan |
| **Notarial / conveyancing** | Property transfer, antenuptial contracts, notarial bonds | Conveyancer/notary | Prepare the pack; cannot execute |

**Rule:** the agent may never generate a signature image, reuse a scanned signature, or enter a password/OTP. It may request a signature through an e-sign provider and verify the returned certificate.

### 2.3 Regulated advice and reserved work

The agent crosses from "preparation" into "regulated advice" when it recommends a course of action in a regulated domain, selects a financial product, or holds itself out as an intermediary. The main boundaries:

- **FAIS Act 37 of 2002.** "Advice" means a recommendation, guidance or proposal on a financial product. "Intermediary service" includes buying, selling, administering or collecting premiums. A person may not render advice or intermediary services without an FSP licence and, where applicable, representative registration. Crypto assets are financial products; a CASP licence is required for crypto-asset services. An agent that says "invest in this structure" is in scope. An agent that says "here are the forms and the disclosed fees, and here is a licensed FSP" is closer to the line but still needs counsel.
- **Legal Practice Act 28 of 2014.** Conveyancing, litigation, and certain notarial acts are reserved. A business agent cannot draft a deed of transfer or appear in court.
- **Auditing Professions Act 26 of 2005.** Audit opinions and certain independent reviews are reserved to registered auditors.
- **Engineering, planning, architecture.** ECSA, SACAP and EAPASA registrations gate the reports that municipalities and environmental authorities accept. The agent can draft; the registered professional must sign.
- **Tax Administration Act 28 of 2011.** A tax practitioner must be registered with a recognised controlling body and with SARS. The agent can prepare; the practitioner signs and accepts responsibility.
- **Health and safety.** OHS Act 85 of 1993 imposes duties on the employer; a consultant's report does not transfer them.

**Rule:** the agent's output must be labelled as **preparation**, not advice. Where a recommendation is unavoidable, it must be framed as a decision prompt with the licensed route named, and it must be reviewed by a human with the relevant licence before it reaches a client.

### 2.4 Tax administration and eFiling

SARS eFiling terms prohibit sharing credentials. The correct model is a **delegated tax practitioner profile** or a **registered representative**, not credential custody. The agent may:

- prepare returns and schedules;
- validate against schema and prior-year data;
- calculate the liability and flag discrepancies;
- request a human to authenticate and submit;
- ingest the receipt and status.

It may not store the taxpayer's password, answer the security questions, or bypass the biometric/OTP step introduced for sensitive transactions.

### 2.5 FIC and AML

Accountable institutions under Schedule 1 of the FIC Act 38 of 2001 must register with the FIC, appoint a compliance officer, adopt and submit a Risk Management and Compliance Programme (RMCP), file risk and compliance returns, and report suspicious and cash-threshold transactions. These obligations attach to the institution, not to its software.

The agent can:

- assemble the RMCP from a template and the institution's actual risk assessment;
- map the institution to the correct Schedule 1 item;
- prepare the RCR and the RMCP submission;
- screen against sanctions lists;
- draft STR/CTR narratives for human review.

It cannot be the compliance officer, cannot sign the RMCP declaration, and cannot decide that a transaction is not suspicious. H2 and H4 apply.

### 2.6 POPIA

The Protection of Personal Information Act 4 of 2013 requires an Information Officer registration, a PAIA manual, lawful processing grounds, security safeguards, and breach notification. An agent processing ID numbers, beneficial-owner data, payroll data, or health data is a **operator** (processor) unless it determines the purpose and means. The responsible party remains liable.

Minimum controls:

- purpose limitation and data minimisation per channel;
- retention schedule per form type (tax 5 years; company records 7 years; FIC 5 years; POPIA as documented);
- cross-border transfer assessment before using an offshore model or storage;
- no special personal information without explicit authority;
- deletion and subject-access handling;
- a record of processing activities.

### 2.7 Payments and exchange control

An agent may **calculate** a fee and **request** a payment. It may not hold client funds, initiate an EFT without a payment-service authorisation, or circumvent a portal's payment step. The South African Reserve Bank is opening the national payment system to non-bank payment service providers from around mid-2027; the authorisation framework is still being finalised. Until then, use a licensed PSP or the portal's own payment rail, with a human approval at H3.

Exchange control is a separate gate. Loans to/from non-residents, IP transfers, loop structures, and certain crypto-asset transactions require SARB Financial Surveillance approval or reporting. The agent can prepare the application; it cannot approve the transaction.

### 2.8 Company law and directors' duties

The Companies Act 71 of 2008 imposes duties on directors that cannot be delegated to a model: act in the best interests of the company, with care, skill and diligence; avoid conflicts; ensure accurate records. A filing agent that submits a false beneficial-ownership declaration exposes the declarant and the company. The agent must therefore require a **director-level attestation** for BO, AFS, and any declaration of solvency or accuracy.

### 2.9 Ten rules the agent must enforce

1. No submission without an authority record and a gate token.
2. No signature generation, reuse, or OTP capture.
3. No regulated advice without a licensed human in the loop.
4. No payment without an H3 approval and a licensed rail.
5. No professional attestation substituted by a model-generated document.
6. No assertion of a legal requirement without a source URL, effective date, and confidence.
7. No stale law: if `last_verified` exceeds the channel's freshness SLA, block and escalate.
8. No password/OTP custody: scoped, short-lived session artefacts and gate tokens only.
9. No irreversible action without a receipt and a rollback/retry path.
10. No silent failure: every block, rejection, and timeout is an evidence event.

---

## 3. The ten-layer regulatory stack

| Layer | What it controls | Representative channels | Dominant gates | Failure mode if skipped |
|---|---|---|---|---|
| **L0 Entity & governance** | Legal existence, ownership, control, records | Company registration, BO register, directors, annual return, AFS, trusts/NPOs, IP | H1, H2, H3, H4 | Entity cannot open a bank account, sign a lease, or enforce a contract |
| **L1 Tax & fiscal** | Registration, returns, payments, compliance status | Income tax, VAT, PAYE/SDL/UIF, customs, TCS, practitioner | H1, H2, H3, H4 | Penalties, interest, blocked tenders, VAT input denial |
| **L2 Labour & skills** | Employment, injury cover, equity, skills | UIF, COIDA, EE plan/report, SETA, visas | H2, H3, H4 | Unfunded injury liability, EE fines, illegal employment |
| **L3 Sector licence** | Permission to carry on a regulated activity | FSP, CASP, CIS, NCR, PPRA, liquor, food, gambling, mining, energy, telecoms, aviation, security, construction | H1–H7 | Criminal liability, void contracts, seizure, licence refusal |
| **L4 Land & property** | Rights to use, develop, subdivide, transfer land | Deeds/SG, zoning, land use, subdivision, SDP, building plan, occupation, rates, transfer duty, transfer | H2, H3, H4, H6, H7, H8 | Land acquired but unusable; unlawful development; transfer blocked |
| **L5 Environment, water, heritage** | Environmental impact, water use, waste, air, heritage | Screening, EA, WULA, waste licence, AEL, heritage permit | H4, H6, H7 | Stop-work orders, criminal prosecution, stranded asset |
| **L6 Building & occupancy** | Safe, lawful occupation | Electrical/plumbing/gas CoCs, fire clearance | H4, H6 | Uninsurable building, no occupation, utility refusal |
| **L7 Trade & product** | Import, export, product conformity | ITAC, PPECB, NRCS, SAHPRA, port health | H3, H4, H6 | Goods seized, export rejected, product recall |
| **L8 Data & financial crime** | Lawful data processing, AML/CFT | POPIA IO, PAIA, FIC registration, RMCP, RCR, STR/CTR | H1, H2, H4 | Administrative sanctions, reputational damage, criminal exposure |
| **L9 Recurring compliance** | Keeping the entity alive and licensed | ITR14, VAT201, EMP201, COIDA return, licence renewals | H1, H2, H3 | Deregistration, licence lapse, penalties |

**Sequencing rule:** L0 and L1 must exist before L3 in most cases; L4/L5/L6 must be resolved before construction or occupation; L8 must be live before onboarding customers or processing KYC. An agent that parallelises without dependency checks will create applications that are structurally inadmissible.

---

## 4. Channel index (summary)

The machine-readable index is `sa_business_channels.json`. This section is the human summary. IDs are stable and match the JSON.

**Ceiling legend:** C6 = ready-to-execute; C7 = authorised; C8 = submitted; C9 = tracked/closed. Gates use H1–H8 as defined in §6.

### 4.1 L0 — Entity and governance

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L0-01 | Private company incorporation (with name reservation and MOI) | CIPC / BizPortal | CoR14.1, CoR15.1A–C, CoR9.1 | C6 | H1, H2, H3 | 1 working day via eServices/BizPortal for a standard MOI |
| L0-02 | Beneficial ownership register | CIPC | BO register (eServices) | C6 | H1, H2 | 10 days for new companies; 10 days to update |
| L0-03 | Director / secretary / auditor changes | CIPC | CoR39, CoR44 | C6 | H1, H2, H3 | 3–5 business days |
| L0-04 | Annual return | CIPC | Annual return (eServices) | C6 | H1, H3 | Immediate on payment |
| L0-05 | Annual financial statements / iXBRL | CIPC | iXBRL AFS or FAS | C6 | H2, H4 | Per financial year-end |
| L0-06 | Business name (sole proprietor) | CIPC | Business name application | C6 | H1, H3 | Unverified; no current form/fee page located |
| L0-07 | Trust / NPO / co-operative registration | Master / DSD / CIPC | Trust deed, NPO application, CO-OP forms | C6 | H2, H3, H4 | 5–30 business days |
| L0-08 | Trade mark / patent | CIPC IP | TM1, patent application | C6 | H2, H3, H4 | Months |

### 4.2 L1 — Tax and fiscal

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L1-01 | Income tax registration | SARS | Auto-generated from CIPC; IT77 where needed | C9 | H1 | Automatic |
| L1-02 | VAT registration | SARS | VAT101 | C6 | H1, H2, H3 | 21 business days |
| L1-03 | PAYE / SDL / UIF registration | SARS | EMP101 | C6 | H1, H2, H3 | 21 business days |
| L1-04 | Customs and excise registration | SARS | DA185 | C6 | H1, H2, H3, H4 | 5–21 business days |
| L1-05 | Tax compliance status | SARS | TCS (eFiling) | C8 | H1 | Immediate–5 days |
| L1-06 | Tax practitioner registration / eFiling delegation | SARS + RCB | Practitioner registration, delegation | C6 | H1, H4 | 5–20 business days |

### 4.3 L2 — Labour and skills

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L2-01 | UIF registration | Department of Employment and Labour | UFiling employer registration | C6 | H1, H2, H3 | 1–5 business days |
| L2-02 | COIDA registration and letter of good standing | Compensation Fund | CF registration; LOGS | C6 | H1, H2, H3 | 1–15 business days |
| L2-03 | Employment Equity plan and report | Department of Employment and Labour | Prescribed EEA forms | C6 | H2, H4 | Annual; plan cycle 5 years |
| L2-04 | SETA / skills development | SETA + SARS | SETA registration; WSP/ATR | C6 | H3, H4 | Annual |
| L2-05 | Foreign employee visa / work authorisation | Department of Home Affairs | Visa application; corporate worker permit | C6 | H1, H2, H4, H6 | Weeks–months |

### 4.4 L3 — Sector licences

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L3-01 | Financial services provider (FSP) licence | FSCA | FSCA eLicensing application | C6 | H1–H5 | 3–6 months |
| L3-02 | Crypto asset service provider (CASP) licence | FSCA | CASP application | C6 | H1–H5 | 3–9 months |
| L3-03 | Collective investment scheme manager | FSCA | CIS manager application | C6 | H1–H5 | 6–12 months |
| L3-04 | Credit provider registration | NCR | NCR registration | C6 | H1–H5 | 30–60 days |
| L3-05 | Debt collector registration | NCR | NCR registration | C6 | H4 | 30–60 days |
| L3-06 | Estate agent FFC | PPRA | FFC application | C6 | H4 | 30–60 days |
| L3-07 | Liquor licence | Provincial liquor authority | Provincial application | C6 | H1, H2, H3, H6, H7 | 60–120 days |
| L3-08 | Food premises / health certificate | Municipality | Certificate of acceptability | C6 | H1, H2, H3, H6 | 14–30 days |
| L3-09 | Gambling licence | Provincial gambling board | Provincial application | C6 | H1–H7 | 3–12 months |
| L3-10 | Mining permit / petroleum licence | DMRE | Mining permit, petroleum licence | C6 | H1–H7 | 3–18 months |
| L3-11 | Embedded generation registration | NERSA | Registration | C6 | H1, H2, H4 | 30–90 days |
| L3-12 | Electronic communications service licence | ICASA | ECS licence application | C6 | H1–H5 | 3–9 months |
| L3-13 | Aviation operator certificate (ROC) | SACAA | ROC application | C6 | H1–H6 | 3–12 months |
| L3-14 | Private security registration | PSIRA | Registration | C6 | H1, H2, H4 | 30–60 days |
| L3-15 | Construction / home builder registration | CIDB / NHBRC | CIDB registration; NHBRC enrolment | C6 | H1, H3, H4, H6 | 5–30 days |

### 4.5 L4 — Land and property

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L4-01 | Deeds search and SG diagram | Deeds Office / CSG | Deed search; SG diagram | C8 | H1, H3 | Same day–5 days |
| L4-02 | Zoning certificate | Municipality | Zoning certificate application | C6 | H1, H3, H8 | 10–30 days |
| L4-03 | Land use / rezoning | Municipality | Land use application | C6 | H2, H3, H7, H8 | 3–12 months |
| L4-04 | Subdivision / consolidation / township | Municipality + SG | Subdivision/township application | C6 | H2, H3, H4, H7, H8 | 6–24 months |
| L4-05 | Site development plan | Municipality | SDP application | C6 | H4, H7 | 1–6 months |
| L4-06 | Building plan approval | Municipality | Building plan submission | C6 | H4, H6 | 30–90 days |
| L4-07 | Occupation certificate | Municipality | Occupation certificate | C6 | H2, H6 | 7–30 days |
| L4-08 | Rates clearance certificate | Municipality | Clearance application | C8 | H3 | 5–20 days |
| L4-09 | Transfer duty | SARS | Transfer duty declaration | C8 | H3 | 1–10 days |
| L4-10 | Property transfer | Deeds Office / conveyancer | Deed of transfer lodged by conveyancer | C6 | H2, H4, H8 | 2–6 months |

### 4.6 L5 — Environment, water, heritage

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L5-01 | NEMA screening | DFFE | Screening report (National Web-based Environmental Screening Tool) | C8 | H1 | Same day |
| L5-02 | Environmental authorisation | DFFE / province | EA application via NEAS | C6 | H2, H4, H6, H7 | 3–12 months |
| L5-03 | Water use licence / general authorisation | DWS | WULA via e-WULAAS | C6 | H2, H4, H6, H7 | 90+ days |
| L5-04 | Waste management licence | DFFE / province | Waste licence application | C6 | H2, H4, H6, H7 | 6–18 months |
| L5-05 | Air emission licence | DFFE / municipality | AEL application | C6 | H2, H4, H6 | 6–12 months |
| L5-06 | Heritage permit / HIA | SAHRA / provincial heritage | SAHRIS application | C6 | H2, H4, H6, H7 | 1–6 months |

### 4.7 L6 — Building and occupancy

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L6-01 | Electrical certificate of compliance | Municipality / registered electrician | CoC | C6 | H4, H6 | 1–5 days |
| L6-02 | Plumbing certificate of compliance | Municipality / registered plumber | CoC | C6 | H4, H6 | 1–5 days |
| L6-03 | Gas certificate of compliance | Municipality / registered installer | CoC | C6 | H4, H6 | 1–5 days |
| L6-04 | Fire clearance | Fire department | Fire safety certificate | C6 | H4, H6 | 7–30 days |

### 4.8 L7 — Trade and product

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L7-01 | Import / export permit | ITAC | Permit application | C6 | H3, H4 | 5–30 days |
| L7-02 | PPECB export registration / certificate | PPECB | Exporter registration; export certificate | C6 | H3, H6 | 1–15 days |
| L7-03 | NRCS letter of authority | NRCS | LOA application | C6 | H4 | 30–90 days |
| L7-04 | SAHPRA licence | SAHPRA | Licence application | C6 | H1–H6 | 3–12 months |
| L7-05 | Port health / phytosanitary certificate | Port Health / DALRRD | Certificate application | C6 | H3, H6 | 1–10 days |

### 4.9 L8 — Data and financial crime

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L8-01 | POPIA Information Officer registration | Information Regulator | IO registration | C8 | H1 | Same day |
| L8-02 | PAIA manual | Information Regulator | PAIA manual | C6 | H2 | Before processing |
| L8-03 | FIC registration | FIC | goAML registration | C6 | H1, H2, H4 | 5–20 days |
| L8-04 | RMCP submission (Directive 12) | FIC | RMCP upload | C6 | H2, H4 | Deadline-driven |
| L8-05 | Risk and compliance return (Directive 11) | FIC | RCR questionnaire | C6 | H2, H4 | Annual window |
| L8-06 | STR / CTR reporting | FIC | goAML report | C6 | H4, H5 | 15 days (STR); immediate (CTR) |

### 4.10 L9 — Recurring compliance

| ID | Channel | Regulator | Key forms | Ceiling | Gates | Typical time |
|---|---|---|---|---|---|---|
| L9-01 | ITR14 company income tax return | SARS | ITR14 | C6 | H1, H3, H4 | 12 months after year-end |
| L9-02 | VAT201 return | SARS | VAT201 | C6 | H1, H3 | Bi-monthly; manual by the 25th, eFiling by the last business day |
| L9-03 | EMP201 PAYE return | SARS | EMP201 | C6 | H1, H3 | Monthly by the 7th |
| L9-04 | COIDA annual return | Compensation Fund | Annual return | C6 | H1, H3 | Annual |
| L9-05 | Licence renewals (FSP, CASP, liquor, etc.) | Various | Renewal application | C6 | H2, H3, H4 | Per licence cycle |

**Coverage note:** 70 channels are indexed. This is national coverage plus municipal/provincial archetypes. It is not every municipality or every sector sub-licence. The JSON records the coverage boundary and the extension points.

---

## 5. Land sourcing deep dive

### 5.1 Why land is the hardest channel

A company registration is a bounded transaction: one regulator, one form set, one fee, one receipt. Land is a **stack of rights and permissions**, each held by a different authority, each with its own objection window, and each capable of invalidating the others. The buyer can own the land and still be unable to:

- build on it (zoning or land-use restriction);
- subdivide it (SPLUMA approval and SG approval);
- connect water or electricity (service capacity or water-use authorisation);
- clear it (environmental or heritage restriction);
- occupy it (building plan and occupation certificate);
- transfer it (rates clearance, transfer duty, conveyancer);
- finance it (bank's title and planning conditions).

The agent's job is not to "buy land". It is to **assemble the evidence that the intended use is lawful, sequenced, and priced**, and to stop the transaction when a fatal condition appears.

### 5.2 The twelve-stage pipeline

| Stage | Question the buyer is really asking | Channels | Output | Ceiling | Gate | Fatal red flag |
|---|---|---|---|---|---|---|
| 1. Strategy | What use do I need, at what scale, by when? | — | Use brief, space programme, budget | C2 | H5 (business decision) | Use brief inconsistent with the zoning/land capability |
| 2. Desktop search | What is for sale, at what price, with what rights? | L4-01 | Shortlist, title/SG data, listing pack | C6 | H1 | No title deed, disputed boundary, unmortgageable tenure |
| 3. Title and tenure | Who owns it, and what burdens it? | L4-01 | Title report: bonds, servitudes, caveats, leases, labour tenants, restitution | C6 | H1, H4 | Caveat, expropriation, restitution claim, labour tenant, unregistered lease |
| 4. Planning | What may be done on it now? | L4-02 | Zoning certificate, scheme extract, overlay zones | C6 | H1, H3, H8 | Zoning inconsistent with use; pending amendment; unlawful existing use |
| 5. Land use | What must change to permit the use? | L4-03, L4-04, L4-05 | Rezoning/subdivision/SDP application pack | C6 | H2, H3, H7, H8 | Refusal risk; neighbour objection; bulk services unable to support |
| 6. Environment | Does the use trigger NEMA or a specific regime? | L5-01, L5-02, L5-04, L5-05 | Screening report, EA application, EMPr | C6 | H2, H4, H6, H7 | Listed activity without EA; wetland/coastal/biodiversity trigger |
| 7. Water | Can the site lawfully use and discharge water? | L5-03 | WULA or general authorisation | C6 | H2, H4, H6, H7 | No lawful water source; catchment stressed; discharge prohibited |
| 8. Heritage | Is there a heritage or grave constraint? | L5-06 | HIA, SAHRA permit/comment | C6 | H2, H4, H6, H7 | Grave site, protected structure, archaeological sensitivity |
| 9. Agricultural land | Is the land agricultural, and does conversion need consent? | L4-04 (SALA consent embedded) | SALA/PDALA consent analysis | C6 | H2, H4, H8 | Act 70 consent required but not obtained; PDALA assumption wrong |
| 10. Building | Can the intended building be approved and built? | L4-06, L6-01–04 | Building plan approval, CoCs, fire clearance | C6 | H4, H6 | Structural/non-compliant design; fire access; energy compliance |
| 11. Occupation | Can it be lawfully used? | L4-07 | Occupation certificate | C6 | H2, H6 | No occupation; defects; service connection refused |
| 12. Transfer and finance | Can ownership pass, and can the bank lend? | L4-08, L4-09, L4-10 | Rates clearance, transfer duty receipt, registered deed | C6 | H2, H3, H4, H8 | Rates arrears; transfer duty under-declaration; bond condition unmet |

**Critical path:** stages 3–5 usually determine feasibility; stages 6–9 determine whether the project is lawful; stages 10–12 determine whether it can be built, occupied, and financed. Do not let a client sign an unconditional offer before stages 3–7 have produced a **fatal-flag clearance**.

### 5.3 The desktop due-diligence bundle

The agent should assemble these 24 items before any binding offer:

**Title and survey**
1. Deed of transfer and latest title deed.
2. SG diagram and parcel identifier.
3. Bond registration and bondholder consent (if applicable).
4. Servitudes (access, electricity, water, pipeline, right of way).
5. Endorsements, caveats, interdicts, and restrictions.
6. Lease agreements and occupancy rights.
7. Labour tenant claims (ESTA) and occupier register.
8. Restitution claims and land claims commission status.
9. Expropriation notices or intended expropriation.
10. Sectional title rules and exclusive-use areas (if applicable).

**Planning**
11. Zoning certificate and scheme regulations.
12. Land use management scheme / by-law extract.
13. Existing approved building plans and occupancy certificates.
14. Title conditions (e.g., "not to be used for business purposes").
15. Pending or adopted spatial development framework / precinct plan.
16. Services capacity: water, sewer, electricity, roads, stormwater.

**Environment and heritage**
17. NEMA screening report and listed-activity check.
18. Water-use authorisation status and catchment.
19. Waste/air/heritage triggers.
20. Agricultural land capability and Act 70/PDALA status.
21. Flood line, wetland, coastal setback, biodiversity overlay.

**Commercial and municipal**
22. Rates account, arrears, and clearance history.
23. Municipal valuation and property rates category.
24. Development charges / bulk contribution estimates.

### 5.4 Red flags that should stop the transaction

| Red flag | Why it is fatal or near-fatal | Agent action |
|---|---|---|
| No registered title or deed | No lawful transfer; possible fraud | Stop; require deeds search and conveyancer opinion |
| Caveat or interdict | Transfer blocked | Stop; require withdrawal or court order |
| Restitution or labour-tenant claim | Occupiers may have protected rights | Stop; require legal opinion and mediation route |
| Zoning inconsistent with use and no amendment | Unlawful use; no occupation | Stop or reprice; require rezoning feasibility |
| Listed activity without EA | Criminal offence; stop-work | Stop; require EA or written exclusion |
| No lawful water source | Development cannot be serviced | Stop; require WULA/GA or municipal confirmation |
| Heritage site or grave | Permit and consultation delay | Reprice and sequence; require HIA |
| Agricultural land without Act 70 consent | Subdivision/change of use unlawful | Stop; require consent or PDALA analysis |
| Unregistered lease/occupier | Transfer subject to rights | Require disclosure and legal opinion |
| Rates arrears | Clearance refused | Require settlement or municipal arrangement |
| Boundary/survey discrepancy | Building lines, subdivision, finance risk | Require SG re-survey |
| Service capacity refusal | No occupation/connection | Require municipal capacity letter |

### 5.5 Automation boundary on land

| Step | Agent can | Human must | Evidence |
|---|---|---|---|
| Deeds/SG search | Query, parse, summarise, compare | Verify identity and authority; interpret caveats | Search receipt, deed image hash |
| Zoning | Retrieve scheme, draft enquiry | Municipal official confirms zoning; professional interprets | Zoning certificate |
| Land use application | Draft motivation, plans index, public-participation pack | Owner authorises; professional signs; municipality decides | Application receipt, decision |
| Environmental | Screen, draft terms of reference, assemble EAP pack | EAPASA-registered EAP signs; public participation; authority decides | EA, EMPr, comment register |
| Water | Pre-screen, draft WULA, calculate fee | Applicant signs; technical report by qualified person; DWS decides | WULA/GA, receipt |
| Heritage | Screen, draft HIA brief | Heritage practitioner signs; SAHRA/province decides | Permit/comment |
| Building plan | Pre-check against SANS 10400, assemble submission | Registered professional signs; municipality approves | Approved plans |
| Occupation | Assemble CoCs and application | Registered inspector/engineer certifies; municipality issues | Occupation certificate |
| Transfer | Prepare pack, calculate duty, track | Conveyancer lodges; SARS receipts; Deeds registers | Deed, transfer duty receipt |

**Rule:** on land, the agent's maximum is **C6 for applications** and **C8 for searches/status**. It never reaches C7 on a professional certification or a municipal discretion.

---

## 6. Automation boundary model

### 6.1 Capability levels C0–C9

These are **capability** levels, nested under AEFRI's existing autonomy scale L0–L4. They describe what the agent has produced, not what the agent is allowed to decide.

| Level | Name | Definition | Artefact | Human role | AEFRI mapping |
|---|---|---|---|---|---|
| **C0** | Answer | Explains a requirement from the registry | Text answer with sources | None | L0 |
| **C1** | Retrieve | Fetches the current form, fee, portal, and checklist | Form + metadata | None | L0 |
| **C2** | Draft | Produces a draft answer, motivation, or covering letter | Draft document | Reviews | L1 |
| **C3** | Assemble | Builds the complete pack with annexures | Pack manifest | Reviews | L1–L2 |
| **C4** | Validate | Checks completeness, consistency, schema, and known rejection reasons | Validation report | Reviews exceptions | L2 |
| **C5** | Pre-fill | Populates every field it can, flags the rest | Filled form(s) | Completes unknown fields | L2 |
| **C6** | Ready | Produces a submission-ready pack: all fields, annexures, fee, signature flags, portal instructions | Pack + checklist + fee | Signs/authorises | L2 |
| **C7** | Authorised | A human with verified authority issues a gate token bound to the payload | Gate token | **Acts** | L3 |
| **C8** | Submitted | Portal/API accepts the pack and returns a receipt | Receipt + status | May have submitted or approved | L3 |
| **C9** | Tracked/closed | Monitors status, responds to queries, records outcome and next deadline | Case record | Handles exceptions | L2–L4 |

**Default ceiling without human action: C6.** C7 is the first level that requires an authority event. C8 is not automatic even after C7; it depends on whether a portal adapter can submit with the token or whether the human must press the final button.

### 6.2 Human gates H1–H8

| Gate | Name | What it protects | Who can satisfy it | Evidence required | Typical channels |
|---|---|---|---|---|---|
| **H1** | Identity / authentication | The person is who they claim to be; the entity is entitled to act | Registered user, director, practitioner, authorised delegate | Login/OTP receipt, delegation record, session log | All portals |
| **H2** | Signature / declaration | The applicant adopts the statement and accepts liability | Director, member, trustee, authorised signatory; commissioner/notary where required | Signed form, e-sign certificate, commissioner stamp, witness | CIPC, SARS, FIC, municipal, affidavits |
| **H3** | Payment | The fee is authorised and paid from a lawful source | Director/finance officer; licensed PSP | Payment receipt, allocation, bank confirmation | CIPC, SARS, municipal, licence fees |
| **H4** | Professional attestation | A licensed professional certifies a technical or legal matter | EAP, engineer, architect, auditor, conveyancer, tax practitioner, compliance officer | Signed report, registration number, scope, date | EA, WULA, building, AFS, transfer, FIC |
| **H5** | Regulated advice / discretion | A licensed person or official exercises judgement | FSP adviser, legal practitioner, municipal official, regulator | Advice record, decision letter, reasons | FAIS, CIS, NCA, land use, licences |
| **H6** | Physical inspection / test | The thing actually exists and complies | Inspector, registered person, fire officer, health officer | Inspection report, photos, test certificate | Building, occupation, food, fire, CoCs |
| **H7** | Third-party consent / public participation | Affected parties have been heard or have consented | Neighbours, community, landowner, affected parties | Comment register, consent letters, proof of notice | Rezoning, EA, liquor, mining, heritage |
| **H8** | Third-party dependency | Another institution must act before the process can proceed | Municipality, utility, bank, insurer, Deeds Office | SLA, acknowledgement, capacity letter, bond grant | Land, services, transfer, finance |

### 6.3 Gate token semantics

A gate token is the **only** thing that moves a pack from C6 to C7. It is not a checkbox. It binds five things:

```json
{
  "token_id": "gt_01J...",
  "gate": "H2",
  "channel_id": "L0-01",
  "application_id": "app_01J...",
  "actor": {
    "person_id": "per_01J...",
    "capacity": "director",
    "authority_ref": "resolution_2026_04_01",
    "verified_at": "2026-10-21T09:14:00Z"
  },
  "scope": ["CoR14.1", "CoR15.1A", "CoR15.1B", "CoR15.1C"],
  "payload_hash": "sha256:9f2c...",
  "issued_at": "2026-10-21T09:15:00Z",
  "expires_at": "2026-10-21T10:15:00Z",
  "signature": "ed25519:..."
}
```

**Verification rules (fail closed):**

1. Token signature valid against the gate broker's public key.
2. Token not expired.
3. `channel_id` and `scope` match the pack manifest exactly.
4. `payload_hash` matches the current pack hash; any edit after issue burns the token.
5. Actor's capacity is valid for that channel and gate (e.g., a tax practitioner cannot sign a director's solvency declaration).
6. The authority reference is on file and unrevoked.
7. The gate type is permitted for that channel in the registry.
8. No prior token for the same pack has already been used to submit (replay prevention).

If any check fails, the pack returns to C4 with a machine-readable reason code. The agent may not "try again" with a different token without a new human action.

### 6.4 The rejoin protocol

Automation does not end at the gate; it **rejoins** after it.

```
C6 ready
  → gate request (H1..H8)
  → human acts (login / sign / pay / inspect / consent)
  → gate token issued
  → adapter validates token + payload hash
  → C8 submit
      ├─ accepted → receipt + C9 monitor
      ├─ rejected (validation) → C4 fix + new token required
      └─ rejected (discretion) → H5 review + appeal/alternative route
```

The rejoin is where most "agent" products fail. They can generate a form, but they cannot consume the portal's rejection, map it back to the field, and re-enter the loop without a human starting over. The evidence ledger and the reason-code taxonomy are what make the rejoin reliable.

### 6.5 Reason-code taxonomy

| Code | Meaning | Rejoin path |
|---|---|---|
| `E_AUTH` | Identity/OTP failed | H1 retry |
| `E_AUTHZ` | Actor lacks authority | H2 new authority record |
| `E_SIG` | Signature/commissioning invalid | H2 re-sign |
| `E_PAY` | Payment failed/unallocated | H3 retry |
| `E_SCHEMA` | Portal validation error | C4 fix |
| `E_DOC` | Missing/wrong annexure | C3 rebuild |
| `E_STALE` | Law/form version changed | Registry refresh + C4 |
| `E_QUERY` | Regulator query | C6 response pack |
| `E_DISCRETION` | Refusal/conditions | H5 review |
| `E_DEPENDENCY` | Third party has not acted | H8 chase |
| `E_DUPLICATE` | Already submitted | C9 link to existing case |

### 6.6 What "human intervention required" actually means

Not all human gates are equal. The product should distinguish:

- **Routine gate** (login/OTP, payment): can be designed to take seconds, with a mobile approval.
- **Professional gate** (EAP, engineer, auditor, conveyancer): scheduled, expensive, and on the critical path.
- **Discretionary gate** (municipal approval, regulator decision): uncontrollable; the agent's job is to make the pack refusal-resistant and to track the SLA.
- **Collective gate** (public participation, community consent): time-bound and political; the agent can manage notice, comments, and responses, but not the outcome.

The 90-day plan should measure **gate cycle time by type**, not a single "automation rate".

---

## 7. Agent architecture

### 7.1 Reference architecture

```
┌────────────────────────────────────────────────────────────────────┐
│                        Human Console (web)                         │
│  approvals · exceptions · document review · gate signing · audit   │
└───────────────▲───────────────────────────────▲────────────────────┘
                │                               │
┌───────────────┴───────────────┐   ┌───────────┴────────────────────┐
│        Orchestrator           │   │        Gate Broker             │
│  planner · state machine      │   │  H1–H8 · token issue/verify    │
│  eligibility · dependencies   │   │  authority records · replay    │
└───────▲───────────────▲───────┘   └───────────▲────────────────────┘
        │               │                       │
┌───────┴──────┐ ┌──────┴────────┐   ┌──────────┴────────────────────┐
│ Regulatory   │ │ Document      │   │ Evidence Ledger               │
│ Graph        │ │ Engine        │   │ hash-chained · append-only    │
│ channels ·   │ │ templates ·   │   │ receipts · decisions · gates  │
│ forms · fees │ │ PDF fill ·    │   └──────────▲────────────────────┘
│ deadlines    │ │ OCR · validate│              │
└───────▲──────┘ └──────▲────────┘              │
        │               │                       │
┌───────┴───────────────┴───────────────────────┴────────────────────┐
│                     Portal Adapter Layer                           │
│  CIPC · SARS · FIC · FSCA · NCR · DWS · NEAS · municipal · ...     │
│  detect · login(handoff) · fill · upload · validate · submit ·     │
│  receipt · status · selector hash · canary                         │
└────────────────────────────────────────────────────────────────────┘
```

### 7.2 Components

| Component | Responsibility | Build/buy | Key risk |
|---|---|---|---|
| **Regulatory Graph** | Versioned channel/form/fee/deadline/authority data | Build | Staleness; coverage gaps |
| **Orchestrator** | Eligibility, dependency ordering, state machine, retries | Build | Silent loops; wrong order |
| **Document Engine** | Template rendering, PDF fill, OCR, annexure indexing | Buy + thin build | XFA, scans, signatures |
| **Portal Adapters** | Navigate, fill, upload, submit, read receipt/status | Build per portal | Selector drift; anti-bot; OTP |
| **Gate Broker** | Issue/verify tokens; authority records; replay prevention | Build | Bypass; token theft |
| **Evidence Ledger** | Hash-chained event log; receipts; decisions; retention | Build (VEXL substrate) | Tamper; PII leakage |
| **Human Console** | Approve, sign, pay, inspect, resolve queries | Build | Usability; latency |
| **Monitor** | Deadline calendar, status polling, change detection | Build | False negatives |
| **Policy Guard** | Tool allowlist, jurisdiction, prohibited actions, rate limits | Build | Prompt injection; scope creep |

### 7.3 Data model (minimum)

```
entity(id, type, name, registration_no, tax_no, jurisdiction, status)
person(id, name, id_type, id_hash, capacity, contact, consent)
authority_grant(id, entity_id, person_id, capacity, scope, valid_from, valid_to, document_hash)
channel(id, layer, name, regulator, statute[], portal_id, ceiling, gates[], freshness_sla)
form(id, channel_id, title, format, url, version, effective_from, hash)
application(id, entity_id, channel_id, state, pack_id, current_gate, assignee)
pack(id, application_id, payload_json, payload_hash, annexures[], fee, signature_type)
gate(id, application_id, type, actor_id, authority_grant_id, scope[], payload_hash, issued_at, expires_at, used_at)
submission(id, application_id, adapter_id, receipt_no, receipt_hash, submitted_at, status)
obligation(id, entity_id, channel_id, due_at, recurrence, status, last_filed_at)
evidence(id, seq, event_type, application_id, actor_id, payload_hash, prev_hash, hash, created_at)
source(id, url, publisher, retrieved_at, effective_from, content_hash, confidence)
```

### 7.4 Mapping to the existing VEXL harness

The local `vexl-harness` already provides the primitives that the gate/evidence layer needs:

| Harness tool | Use in this architecture |
|---|---|
| `permission.confirm` | Request a human gate approval; returns a scoped permission |
| `permission.verify` | Verify the gate token before adapter submission |
| `ledger.compact` | Compact the evidence log without losing hash-chain continuity |
| `decision.record` | Record why a route, fee, or exception was chosen |
| `memory.write` | Persist entity facts, prior applications, portal quirks |
| `tool.define` | Register a new portal adapter/tool with a schema and scope |
| `job.kill` | Stop a runaway adapter or repeated submission loop |
| `soft.cancel` | Cancel a queued application before submission |

The gate broker should **not** be a model-callable tool that can issue its own tokens. Token issuance must sit behind a human action in the console, with the model only able to *request*.

### 7.5 Typical sequence (company incorporation)

1. **Intake:** entity type, name, directors, shareholders, address, business description.
2. **Eligibility:** name availability, director disqualification, BO completeness, MOI consistency.
3. **Pack:** CoR14.1 + CoR15.1A–C + CoR9.1 + supporting IDs + address proof.
4. **Validate:** CIPC field rules, name reservation status, fee.
5. **Gate H1/H2/H3:** director authenticates, signs the declaration, pays.
6. **Submit:** CIPC eServices/BizPortal adapter; receipt.
7. **Rejoin:** if rejected, map reason; if accepted, capture registration number.
8. **Downstream:** auto-create L0-02 (BO), L1-01 (income tax), L2-01/L2-02 (UIF/COIDA) tasks.
9. **Ledger:** hash the pack, receipt, and registration certificate.

### 7.6 Security and tenancy

- **No raw credentials.** Passwords and OTP seeds are never stored. Where a portal requires a session, the adapter holds an encrypted, short-lived, per-entity session artefact that the model cannot read; it expires and is never reused across entities.
- **Per-entity isolation.** A pack, ledger, and adapter session never cross tenants.
- **Domain allowlist.** Adapters may only navigate to registered portal domains.
- **No anti-detection.** Government portals are not adversaries; stealth/evasion is prohibited.
- **Rate limits.** Per portal, per entity, per hour; exponential backoff; circuit breaker.
- **Prompt-injection defence.** Portal text, PDFs, and emails are data, never instructions. The policy guard ignores embedded instructions.
- **Retention.** Evidence retained per statutory schedule; PII minimised; deletion logged.
- **Kill switch.** One command stops all adapters and cancels queued jobs.

---

## 8. MCP gap analysis and build specification

### 8.1 What already exists

| Capability | Tool / MCP | What it does | Gap for SA business agent |
|---|---|---|---|
| Browser automation | Playwright MCP | Navigate, click, type, select, fill forms, upload files, screenshot, inspect network | No SA portal knowledge; no receipt parsing; OTP handoff is manual |
| PDF form filling | `pdf-form-filler-mcp` (AcroForm/XFA), `formfill-mcp` | Fill PDF fields, flatten, save | No SA form registry; no version/effective-date tracking; XFA/scanned forms still hard |
| E-signature | Adobe Acrobat Sign MCP, DocuSign MCP | Send, sign, track, audit certificate | Not accepted by all SA portals; wet signature/commissioner still needed |
| OCR | Tesseract / PyMuPDF / cloud OCR | Extract text from scans | No validation against form schema |
| Document generation | DOCX/PDF libraries | Render templates | No legal checklist or annexure logic |
| Human approval | VEXL `permission.confirm/verify` | Scoped permission tokens | Not a full gate broker; needs authority records and replay prevention |
| Ledger | VEXL `ledger.compact` | Hash-chained log | Needs submission receipts, gate events, and retention policy |

### 8.2 What does not exist

There is **no South African government-specific MCP** that:

- resolves a business activity to the correct licences and registrations;
- returns the current form, fee, portal, and effective date;
- knows the difference between CIPC eServices and BizPortal;
- handles SARS eFiling delegation without credential custody;
- navigates FIC goAML, FSCA eLicensing, NCR, NERSA, ICASA, SACAA, SAHPRA, PPECB, SAHRA, DWS e-WULAAS, or NEAS;
- issues and verifies human gate tokens;
- produces a submission receipt and an evidence chain;
- monitors deadlines and regulatory change.

This is the missing layer. It is not a form filler. It is a **regulatory operations kernel**.

### 8.3 Build vs buy

| Layer | Decision | Reason |
|---|---|---|
| Browser automation | **Buy/use** Playwright MCP | Commodity; mature; already installed |
| PDF fill/OCR | **Buy/use** existing MCPs + libraries | Commodity; SA-specific value is the form registry, not the filler |
| E-sign | **Buy/use** Acrobat Sign/DocuSign | Trust, certificates, audit trail |
| Document templates | **Build thin** | SA-specific clauses and annexure logic |
| Regulatory graph | **Build** | The core asset; no vendor has it current |
| Portal adapters | **Build** | Per-portal; selector hashes; canary tests |
| Gate broker | **Build** | Legal boundary; must be outside model control |
| Evidence ledger | **Build on VEXL substrate** | Differentiator; audit-grade |
| Change monitor | **Build** | Freshness is the safety system |

### 8.4 `sa-regops` MCP — tool surface

One MCP server, namespaced tools, adapter plugins. Suggested surface:

```ts
// Registry
channel.search({ query?, layer?, regulator?, ceiling?, limit? }): Channel[]
channel.get({ id }): Channel
form.resolve({ channel_id, entity_type?, variant? }): Form[]
form.fetch({ form_id }): { path, sha256, effective_from, version }
fee.calculate({ channel_id, entity_profile }): { amount, currency, basis, valid_from }
eligibility.check({ channel_id, entity_id }): { eligible, missing[], blockers[] }
dependency.graph({ channel_id }): { requires[], unlocks[] }

// Pack
pack.build({ channel_id, entity_id, answers, annexures[] }): Pack
pack.validate({ pack_id }): { valid, errors[], warnings[], score }
pack.preview({ pack_id }): { html, pdf_path }

// Gates
gate.request({ pack_id, gate, actor_id, authority_ref }): GateRequest
gate.verify({ token, payload_hash }): { valid, actor, scope, reason? }
gate.list({ application_id }): Gate[]
gate.revoke({ token_id, reason }): boolean

// Submission
submit({ pack_id, adapter_id, token }): Receipt
status.get({ application_id }): Status
status.poll({ adapter_id, application_id }): Status
receipt.parse({ adapter_id, raw }): Receipt

// Compliance
deadline.list({ entity_id, horizon_days }): Deadline[]
obligation.create({ entity_id, channel_id, due_at, recurrence }): Obligation
evidence.append({ event }): { seq, hash }
evidence.verify({ from_seq, to_seq }): { valid, broken_at? }
change.scan({ source_id }): Change[]
```

### 8.5 Adapter contract

Every portal adapter implements the same interface:

```ts
interface PortalAdapter {
  id: string;
  detect(): Promise<{ portal: string; version: string; selectors_hash: string }>;
  prepare(pack: Pack): Promise<{ session_requirements: string[]; unresolved_fields: string[] }>;
  fill(pack: Pack): Promise<{ filled: number; unresolved: string[] }>;
  attach(pack: Pack): Promise<{ uploaded: string[]; rejected: string[] }>;
  validate(): Promise<{ errors: PortalError[]; warnings: string[] }>;
  submit(token: GateToken): Promise<Receipt>;
  status(applicationId: string): Promise<Status>;
  canary(): Promise<{ ok: boolean; selector_drift: string[] }>;
}
```

**Adapter rules:**

- Login is always a human handoff; the adapter never stores the password.
- OTP is entered by the human in the console; the adapter waits.
- Payment is an H3 gate; the adapter never stores card data.
- Every selector has a stable `data-testid` fallback and a canary test.
- A selector hash change triggers a regression test before the adapter is used.
- The adapter records the portal's exact receipt text and HTML hash.

### 8.6 Form resolver and PDF pipeline

The form resolver is more important than the filler:

1. **Identity:** form ID, title, channel, version, effective date.
2. **Source:** official URL, retrieval time, SHA-256.
3. **Format:** HTML, AcroForm, XFA, scanned PDF, DOCX, online wizard.
4. **Fill map:** field name → canonical entity/person field; transform; validation.
5. **Signature map:** signature type, signatory capacity, witness/commissioner.
6. **Annexure map:** required documents, accepted formats, max size.
7. **Rejection map:** known portal errors and fixes.
8. **Freshness:** expiry date, change-monitor source, last canary.

If a form is scanned (no fields), the pipeline is OCR → schema extraction → human verification → overlay/typed annexure. It must never silently invent a field value.

### 8.7 Security and operations

- **Secrets:** vault, per-entity, short-lived; no secrets in prompts or logs.
- **Network:** egress allowlist; no arbitrary browsing during a submission.
- **Observability:** every adapter step is a span with portal, channel, duration, result; PII redacted.
- **Incident response:** kill switch, token revocation, portal notification, client disclosure.
- **Change management:** registry changes are versioned; adapters are canaried; rollback is one command.
- **Testing:** record/replay fixtures for each portal; synthetic entities; no live client data in test.

### 8.8 Verdict

**Does the LLM need form-download/form-completion tools or MCP servers?** Yes — but those are not the bottleneck, and they already exist in usable form. The LLM needs:

1. a **registry** that tells it which form, fee, portal, and gate apply;
2. a **pack builder/validator** that produces a C6 artefact;
3. a **gate broker** that converts a human action into a scoped token;
4. **adapters** that can submit and read the receipt;
5. an **evidence ledger** that makes the whole chain auditable.

Build 1, 3, 4, and 5. Reuse 2 and the commodity PDF/browser/e-sign tools. A better PDF filler without the registry is a faster way to submit the wrong form.

---

## 9. AEQUCHAIN/AEFRI gate map

### 9.1 The entity and licence stack

A realistic Aequchain/AEFRI structure may need several legal persons, not one:

| Entity | Purpose | Required licence/registration | Hard gate |
|---|---|---|---|
| Holdco | Ownership, IP, group governance | CIPC company; BO; tax | H2, H4 |
| Operating company | Commercial contracts, staff, premises | CIPC; VAT; PAYE; COIDA; municipal | H2, H3 |
| FSP | Advice/intermediary services | FAIS FSP licence; representative registration; compliance officer | H5 |
| CASP | Crypto-asset services, custody, exchange | FSCA CASP licence; FIC item 22; AML/CFT | H1–H5 |
| CIS manager | Pooled investment | CISCA manager licence; trustee; prospectus | H5 |
| Credit provider | Lending/credit | NCR registration; affordability; reckless-lending controls | H4, H5 |
| Payment entity | Collection/payout | SARB/PSP authorisation; no client-fund custody | H3, H8 |
| Property SPV | Land/site holding | CIPC; deeds; municipal; SARS transfer duty | H2, H4, H8 |

### 9.2 Gate map

| Domain | Instrument | What the agent can do | What it cannot do | Gate |
|---|---|---|---|---|
| **FAIS advice** | FAIS Act 37 of 2002 | Prepare the FSP application, rep registers, compliance documentation, client disclosure pack | Give advice or intermediary service without a licence; recommend a product | H5 |
| **CASP** | FSCA CASP licence; FIC item 22 | Prepare the licence pack, AML/CFT programme, custody and wallet controls, travel-rule process | Custody client crypto, operate an exchange, or onboard clients before licensing | H1–H5 |
| **CIS / pooled funds** | CISCA 45 of 2002 | Prepare the manager application, trustee arrangements, offering document | Pool member funds or promise returns without a CIS licence or a lawful alternative | H5 |
| **Credit** | National Credit Act 34 of 2005 | Prepare NCR registration, affordability models, contract templates | Lend without registration; reckless lending; unlawful collection | H4, H5 |
| **FIC** | FIC Act 38 of 2001 | Prepare registration, RMCP, RCR, STR/CTR drafts, sanctions screening | Be the compliance officer; sign the RMCP; decide suspicion | H2, H4 |
| **Exchange control** | Currency and Exchanges Act 9 of 1933; SARB regulations | Prepare applications, loop-structure analysis, foreign loan/IP documentation | Approve or execute a controlled transaction without SARB | H5, H8 |
| **Investor portal** | Companies Act 71 of 2008 (offers to public); FMA 19 of 2012 | Prepare the offer document, risk disclosures, KYC pack, cap table | Make an offer to the public without a prospectus/exemption; operate an exchange without a licence | H5 |
| **Payments** | National Payment System Act 78 of 1998 | Integrate a licensed PSP; prepare reconciliation and merchant records | Hold client funds; initiate EFT without authorisation | H3, H8 |
| **Tax** | Income Tax Act 58 of 1962; VAT Act 89 of 1991; Securities Transfer Tax Act 25 of 2007 | Prepare registrations and returns; calculate VAT/STT/dividends tax | Sign a tax return without a practitioner; conceal a taxable event | H4 |
| **Data** | POPIA 4 of 2013 | Register IO, draft PAIA manual, map processing, assess cross-border transfer | Process special personal information without authority; transfer data unlawfully | H1, H7 |

### 9.3 What AEFRI can do without a licence

AEFRI can safely:

- map a member's business idea to the required registrations and licences;
- prepare the application packs and evidence;
- route the member to a licensed FSP, attorney, EAP, engineer, auditor, or conveyancer;
- maintain the regulatory graph and the evidence ledger;
- run deadline monitoring and status tracking;
- provide general, non-recommendatory information with sources;
- operate a sandbox/demo that does not take real funds or onboard real clients.

AEFRI cannot safely:

- recommend a specific financial product or structure;
- accept deposits, custody assets, or pool funds;
- act as an intermediary for a financial product;
- sign a declaration on behalf of a director;
- certify environmental, engineering, audit, or conveyancing work;
- submit a licence application as the licensee;
- approve a transaction that requires SARB or FIC approval.

### 9.4 Investor portal red lines

The portal is the highest-risk surface because it combines securities law, AML, payments, data protection, and consumer law. Red lines:

1. **No offer to the public** without a prospectus or a valid exemption, and counsel sign-off.
2. **No pooled returns** or profit-sharing arrangement that looks like a CIS without a licence.
3. **No custody of investor funds** outside a licensed PSP/bank trust account.
4. **No token sale** that constitutes a security, derivative, or CIS without the correct licence.
5. **No onboarding** without KYC, sanctions screening, and POPIA-compliant consent.
6. **No "guaranteed" or "risk-free" language.**
7. **No cross-border solicitation** without exchange-control and local-law analysis.
8. **No automated investment advice** without an FSP licence and representative.

### 9.5 Token/ledger design constraints

If Aequchain uses a token or on-chain ledger, the design must not accidentally create a regulated instrument. Counsel must classify:

- Is the token a security, a derivative, a CIS participatory interest, or a utility token?
- Does it confer a right to profit, income, or a pool?
- Is it transferable/tradable? Does that require an exchange licence?
- Does the issuer custody assets or promise redemption?
- Does the structure create a loop structure or exchange-control issue?
- Is the token marketed to the public?

The agent can prepare the classification memo and the licence map. It cannot make the legal determination.

### 9.6 Counsel triggers

Escalate to counsel before any of the following:

- taking real money from anyone;
- onboarding a non-resident investor;
- issuing a token or membership interest;
- pooling funds;
- lending or offering credit;
- custodying crypto or securities;
- making a public offer;
- signing a lease or land purchase;
- starting construction;
- submitting an environmental or water application;
- changing the entity structure or control.

---

## 10. Risk register

| # | Risk | Likelihood | Impact | Mitigation | Residual |
|---|---|---|---|---|---|
| 1 | **Stale law** — agent applies a repealed or uncommenced Act | High | Critical | Versioned graph; `effective_from`; freshness SLA; change monitor; block on stale | Medium |
| 2 | **Portal drift** — selectors/validation change without notice | High | High | Selector hashes; canary tests; record/replay; adapter rollback | Medium |
| 3 | **Credential compromise** — session/password theft | Medium | Critical | No raw credentials; vault; scoped tokens; per-entity isolation; MFA handoff | Low–Medium |
| 4 | **Unauthorised practice** — agent gives regulated advice | Medium | Critical | Scope guard; "preparation not advice" labelling; H5 routing; counsel review | Medium |
| 5 | **Signature repudiation** — applicant denies authorising | Medium | High | Gate tokens bound to payload hash; e-sign certificates; authority records; audit log | Low |
| 6 | **Payment fraud / misallocation** — fee paid to wrong account | Low–Medium | High | H3 approval; licensed PSP; verified beneficiary; receipt reconciliation | Low |
| 7 | **Data breach / PII leakage** | Medium | Critical | Minimisation; encryption; redaction; retention; access control; DPIA | Medium |
| 8 | **Hallucinated requirement** — agent invents a form or fee | Medium | High | Source-bound answers; registry-only form resolution; confidence threshold; refusal | Low–Medium |
| 9 | **Over-automation** — agent submits without authority | Low | Critical | Fail-closed gate verification; replay prevention; no self-issued tokens; kill switch | Low |
| 10 | **Municipal variance** — one adapter assumed to fit all | High | High | Per-municipality configuration; archetype tests; manual fallback | Medium |
| 11 | **Third-party delay** — municipality/utility/bank stalls | High | Medium | SLA tracking; escalation; alternative route; client expectation setting | High |
| 12 | **Sanctions / PEP false negative** | Low–Medium | Critical | Multiple lists; fuzzy matching; human review; periodic re-screen | Low–Medium |
| 13 | **Record retention breach** — evidence deleted too early/late | Medium | Medium | Statutory retention matrix; legal hold; deletion audit | Low |
| 14 | **Model injection via portal/PDF/email** | Medium | High | Treat all inbound as data; tool allowlist; no instruction following from content; sandbox | Low–Medium |
| 15 | **Scope creep** — agent becomes a de facto adviser/broker | Medium | High | Charter guard; licence map; counsel triggers; product boundaries | Medium |

**Top three to instrument first:** stale law, portal drift, and gate bypass. These are the failure modes that make every other control irrelevant.

---

## 11. 90-day plan

### Phase 1 (Days 0–30): Kernel and three anchors

**Goal:** a working registry, gate broker, ledger, and three portal adapters that can take a synthetic company from incorporation to tax registration without a human re-keying data.

| Workstream | Deliverable | Exit test |
|---|---|---|
| Registry | 70 channel records with source URL, evidence grade, confidence, gates, forms | JSON validates; every record has ≥1 source; no record older than its SLA |
| Gate broker | H1–H8 token issue/verify; authority records; replay prevention | Bypass test fails closed; expired token rejected; payload edit burns token |
| Evidence ledger | Hash-chained append/verify; receipt storage | Tamper test detected at exact sequence; replay produces identical hash |
| Adapters | CIPC eServices/BizPortal, SARS eFiling (delegated), FIC goAML | Canary passes; receipt parsed; no credential stored |
| Document engine | CoR14.1, CoR15.1A–C, VAT101, EMP101, BO register | Field coverage ≥80% on synthetic entity; validation catches missing annexures |
| Console | Approve/sign/pay/exception queue | A human can complete H1–H3 in <10 minutes |

**Phase 1 exit criterion:** a synthetic private company reaches C9 (registration number + tax number + BO filed) with a complete evidence chain and zero gate bypasses.

### Phase 2 (Days 31–60): Sector and land

**Goal:** add the high-risk financial and land channels, and prove the C6 boundary on professional attestations.

| Workstream | Deliverable | Exit test |
|---|---|---|
| Financial channels | FSP, CASP, CIS, NCR credit, FIC RMCP/RCR | Eligibility rules correctly block unlicensed activity; counsel review of outputs |
| Land channels | Deeds/SG, zoning, land use, building plan, occupation | Fatal-flag engine catches 10/10 synthetic red flags |
| Environmental/water | NEAS, e-WULAAS, heritage | EAP/WULA packs reach C6; H4 gate enforced |
| Municipal archetypes | Cape Town, Johannesburg, Tshwane | Three adapters pass canary; document requirements differ correctly |
| Change monitor | Source watchers, diff alerts, stale-block | Injected change to a statute date blocks the channel |

**Phase 2 exit criterion:** a synthetic site acquisition reaches C6 on land use and environmental, with every professional gate identified and no submission attempted.

### Phase 3 (Days 61–90): Production hardening and measurement

**Goal:** run 20 real channels with consenting users, measure the boundary, and publish v1.1.

| Workstream | Deliverable | Exit test |
|---|---|---|
| Live pilot | 20 channels, 10 entities, consent and DPIA | Zero unauthorised submissions; all receipts in ledger |
| Rejection loop | Reason-code taxonomy; rejoin workflow | 90% of portal rejections mapped to a code and a fix |
| Metrics | C6 rate, gate cycle time, rejection rate, stale detection, cost per filing | Dashboard reproducible from ledger |
| Security | Pen test, secrets audit, egress review | No critical findings; no credentials in logs |
| Counsel review | Licence map, disclaimers, investor portal red lines | Sign-off or written conditions |
| v1.1 | Updated registry, adapters, runbook | Diff from v1 is documented and tested |

**Phase 3 exit criterion:** measured C6 rate ≥70% on the pilot channels; zero gate bypasses; median routine gate <10 minutes; all high-risk channels routed to a licensed human.

### Staffing and roles

| Role | Responsibility |
|---|---|
| Regulatory lead | Registry accuracy, source verification, counsel liaison |
| Agent engineer | Orchestrator, gate broker, ledger |
| Adapter engineer | Portal automation, canary tests, receipt parsing |
| Document engineer | Templates, PDF/OCR, validation |
| Compliance/MLRO | FIC, POPIA, FAIS boundary, incident response |
| Counsel | Legal status, licence map, investor portal, terms |

### What not to build in 90 days

- A general-purpose "file anything" agent.
- A replacement for CIPC/SARS portals.
- A payment institution.
- An investment advice engine.
- A conveyancing or EAP service.
- A cryptocurrency exchange or custody service.

---

## 12. Open questions for counsel

These extend, and should be read with, `COUNSEL-BRIEF-v1.md`. They are questions, not conclusions.

1. **Authority to submit.** What form of mandate (board resolution, power of attorney, portal delegation) is sufficient for an agent to submit each of the 70 channels on behalf of a company? Which require an original signature or a commission?
2. **ECTA signature classification.** For each portal that accepts an authenticated click, is that a valid signature for the underlying statute? Which forms require an advanced electronic signature?
3. **FAIS boundary.** At what point does a preparation tool that ranks or recommends registrations become "advice" or an "intermediary service"? Does a fee-based routing service to a licensed FSP require an FSP licence?
4. **CASP boundary.** If AEFRI never custodies assets but prepares the licence pack and operates a demo, is it providing a crypto-asset service? Does a membership token with no profit right still trigger FSCA jurisdiction?
5. **CIS boundary.** Does a members' pooled treasury, a revenue-share arrangement, or a tokenised participation constitute a collective investment scheme? What is the lawful non-CIS alternative?
6. **NCA boundary.** Does member-to-member lending, deferred payment, or a subscription credit facility require NCR registration? What are the affordability and reckless-lending obligations?
7. **FIC ownership.** Who is the accountable institution for a platform that facilitates but does not hold funds? Can the platform rely on a licensed partner's RMCP, or does it need its own?
8. **RMCP/RCR.** What level of detail must the RMCP contain for a startup with no historical transaction data? Can a template-based RMCP satisfy Directive 12?
9. **Exchange control.** What approvals are required for non-resident membership, foreign loans, IP transfers, and crypto-asset transactions? Does a loop structure arise in the proposed holdco?
10. **Investor portal.** Is the membership/participation a security, a CIS interest, or a utility right? Does the portal require a prospectus, an exemption, or a licensed exchange?
11. **Payments.** Can the platform use a licensed PSP's trust account for member subscriptions? What are the client-fund and reconciliation obligations before the non-bank PSP regime takes effect?
12. **POPIA.** Is AEFRI a responsible party or an operator for each data flow? What is the lawful basis for processing KYC and beneficial-ownership data? What cross-border transfer mechanism applies to offshore model providers?
13. **Professional liability.** If an agent prepares a pack that an EAP/engineer/auditor signs, who bears liability for a defective pack? How should the engagement terms allocate it?
14. **Municipal submissions.** Do municipalities accept agent-submitted applications, and under what delegation? Are there by-laws that require the owner's original signature?
15. **Record retention.** What retention period applies to each channel, and can the evidence ledger satisfy it? What happens to records on tenant termination?

---

## 13. Source and verification log

### 13.1 Re-executed this session (V)

| Source | URL | Status | Used for |
|---|---|---|---|
| SARS VAT page | https://www.sars.gov.za/types-of-tax/value-added-tax/ | 200 | VAT threshold R2.3m / R120k from 1 Apr 2026; VAT101 link |
| FIC homepage | https://www.fic.gov.za/ | 200 | RMCP 9 Oct 2026 notice; FIC annual report |
| FIC Directive 12 news | https://www.fic.gov.za/2026/10/09/important-information-on-submission-of-rmcp-submissions-9-october-2026/ | 200 | Directive 12; items 1,2,9,11; 9 Oct 2026 |
| FIC directives index | https://www.fic.gov.za/compliance/compliance-guidance/directives/ | 200 | Directive 11/12 context |
| FIC RCR page | https://www.fic.gov.za/risk-and-compliance-return-submissions/ | 200 | Directive 11; 30 Jun / 31 Jul 2026 deadlines |
| Deneys Reitz PPA analysis | https://www.deneys.co.za/thinking/news/public-procurement-act-2024-invalid-says-constitutional-court-what-it-means-south | 200 | PPA struck down 17 Sep 2026; never commenced; old framework remains |
| NEAS portal | https://neas.environment.gov.za/ | 200 | NEAS = National Environmental Authorisation System; EIA Reg 5(4) |
| CIPC homepage | https://www.cipc.co.za/ | 200 | Forms & fees, eServices, BO, XBRL |
| ECTA 25 of 2002 (original PDF) | https://www.gov.za/sites/default/files/gcis_document/201409/a25-02.pdf | 200 | s13 signature rules: advanced e-signature where law requires a signature and does not specify type |

### 13.2 Retrieved earlier in the engagement (R)

| Source | URL | Status at check | Used for |
|---|---|---|---|
| CIPC eServices | https://eservices.cipc.co.za/ | 200 | Company transactions |
| BizPortal | https://bizportal.gov.za/ | 000 (curl) | CIPC registration front-end |
| SARS eFiling | https://www.sarsefiling.co.za/ | 200 | Returns, TCS |
| FSCA | https://www.fsca.co.za/ | 200 | FSP/CASP/CIS |
| NCR | https://www.ncr.org.za/ | 200 | Credit/debt |
| NERSA | https://www.nersa.org.za/ | 200 | Embedded generation |
| ICASA | https://www.icasa.org.za/ | 200 | ECS licence |
| SACAA | https://www.caa.co.za/ | 200 | Aviation |
| NHBRC | https://www.nhbrc.org.za/ | 200 | Home builder enrolment |
| CIDB | https://www.cidb.org.za/ | 000 (curl) | Construction registration |
| SAHPRA | https://www.sahpra.org.za/ | 200 | Health products |
| PPECB | https://www.ppecb.com/ | 200 | Export certification |
| SAHRA | https://www.sahra.org.za/ | 200 | Heritage |
| DWS | https://www.dws.gov.za/ | 200 | Water use |
| DFFE | https://www.dffe.gov.za/ | 000 (curl) | Environment |
| DMRE | https://www.dmre.gov.za/ | 000 (curl) | Mining/petroleum |
| ITAC | https://www.itac.org.za/ | 200 | Import/export |
| NRCS | https://www.nrcs.org.za/ | 000 (curl) | Product conformity |
| PSIRA | https://www.psira.co.za/ | 000 (curl) | Private security |
| PPRA | https://theppra.org.za/ | 403 (curl) | Estate agents; www.ppra.org.za is a commercial redirect |
| Labour | https://www.labour.gov.za/ | 200 | UIF/COIDA/EE |
| uFiling | https://ufiling.labour.gov.za/ | 200 | UIF |
| DHA | https://www.dha.gov.za/ | 200 | Visas |
| Treasury | https://www.treasury.gov.za/ | 200 | PFMA/PPA |
| Information Regulator | https://inforegulator.org.za/ | 200 | POPIA/PAIA |
| Deeds | https://www.deeds.gov.za/ | 000 (curl) | Property transfer |
| CSG | https://csg.dla.gov.za/ | 000 (curl) | SG diagrams |
| NDA | https://www.nda.gov.za/ | 000 (curl) | Agricultural land |
| Parliament | https://www.parliament.gov.za/ | 200 | Bills |
| SAFLII | https://www.saflii.org/ | 403 (curl) | Case law |

### 13.3 Secondary (S)

| Source | Used for | Caveat |
|---|---|---|
| Deneys Reitz PPA article | PPA judgment | Law firm summary; judgment link to SAFLII |
| Lawlibrary PDALA note | PDALA not in force as at 25 Sep 2026 | Secondary consolidator; verify Gazette |
| EAPASA LinkedIn notice | NEAS rollout from 1 Oct 2026 | Industry notice; verify DFFE notice |
| MCP vendor docs | Playwright, PDF fill, e-sign capabilities | Not installed/tested locally in this session |
| FSCA CASP statistics (reported 31 Mar 2026: 533 applications, 310 approved, 17 declined) | Market context | R/S; verify current FSCA publication |

### 13.4 Legal status register

| Instrument | Status | Evidence | Implication |
|---|---|---|---|
| Companies Act 71 of 2008 | In force | R | Core entity law |
| Companies Amendment Act 16 of 2024 | Staged commencement | R | Check which sections are in force |
| Companies Amendment Act 17 of 2024 | In force 27 Dec 2024 | R | Check remuneration/other changes |
| Public Procurement Act 28 of 2024 | **Struck down 17 Sep 2026 in *Premier of the Western Cape Government v Speaker of the National Assembly* [2026] ZACC 37; never commenced** | V/S | Old PFMA/MFMA/PPPFA framework remains |
| Preservation and Development of Agricultural Land Act 39 of 2024 | **Not in force** | S | SALA 70 of 1970 remains operative |
| Subdivision of Agricultural Land Act 70 of 1970 | In force; Repeal Act 64 of 1998 never commenced | R | Act 70 consent still required |
| Expropriation Act 13 of 2024 | Signed 23 Jan 2025; not operational | R | Old Expropriation Act 63 of 1975 applies until commencement |
| NEMA 107 of 1998 / EIA Regulations 2014 | In force | R | NEAS from 1 Oct 2026 for new applications |
| National Water Act 36 of 1998 | In force | R | WULA/GA required |
| FIC Act 38 of 2001 | In force | V/R | Directive 11/12 obligations |
| ECTA 25 of 2002 | In force | V | s13 signature classification |
| FAIS Act 37 of 2002 | In force | R | FSP/CASP licensing |
| CISCA 45 of 2002 | In force | R | CIS manager licence |
| National Credit Act 34 of 2005 | In force | R | NCR registration |
| POPIA 4 of 2013 | In force | R | IO/PAIA |
| Electricity Regulation Amendment Act 38 of 2024 | In force 1 Jan 2025 | R | NERSA registration |
| Startup Act | Not enacted | R | No startup-specific statute |
| National AI Policy | Draft 10 Apr 2026; reports of revision | R/S | No AI Act; POPIA governs |

---

## Appendix A — C-level to AEFRI L-level mapping

| C-level | AEFRI L-level | Why |
|---|---|---|
| C0–C2 | L0–L1 | Observe, answer, propose |
| C3–C5 | L2 | Act on digital artefacts in a sandbox |
| C6 | L2 | Produce a ready pack; no external effect |
| C7–C8 | L3 | Per-action human approval; external submission |
| C9 | L2–L4 | Tracking is L2; standing pre-approved monitoring can be L4 only after a written envelope and audit |

**Rule:** C8 does not imply L4. L4 requires a standing, written, pre-approved envelope with limits, not a single token.

---

## Appendix B — Form index (selected, with locations)

Deep links are given only where the form URL was verified or is a stable regulator page. Where the form is portal-generated, the portal is listed instead of a fabricated PDF link.

| Form | Title | Channel | Format | Location |
|---|---|---|---|---|
| CoR14.1 | Application to incorporate a company | L0-01 | PDF/online | CIPC Forms & Fees: https://www.cipc.co.za/?page_id=3786 |
| CoR15.1A | MOI (private company) | L0-01 | PDF | CIPC Forms & Fees |
| CoR15.1B | MOI (public company) | L0-01 | PDF | CIPC Forms & Fees |
| CoR15.1C | MOI (non-profit company) | L0-01 | PDF | CIPC Forms & Fees |
| CoR9.1 | Name reservation | L0-01 | Online | CIPC eServices: https://eservices.cipc.co.za/ |
| BO register | Beneficial ownership register | L0-02 | Online | CIPC eServices / BO portal |
| CoR39 | Change of directors | L0-03 | PDF/online | CIPC Forms & Fees |
| CoR44 | Change of auditor/secretary | L0-03 | PDF | CIPC Forms & Fees |
| Annual return | Annual return | L0-04 | Online | CIPC eServices |
| iXBRL AFS | Annual financial statements | L0-05 | XBRL | CIPC XBRL programme |
| VAT101 | VAT registration application | L1-02 | PDF | https://www.sars.gov.za/wp-content/uploads/Ops/Forms/SARS_VAT101_Registration-Threshold_LookFeel_v2026.00.03.pdf |
| EMP101 | PAYE/SDL/UIF registration | L1-03 | Online/PDF | SARS eFiling |
| DA185 | Customs and excise registration | L1-04 | PDF | SARS Customs |
| IT77 | Income tax registration (where required) | L1-01 | PDF | SARS |
| TCS | Tax compliance status | L1-05 | Online | SARS eFiling |
| UFiling employer | UIF registration | L2-01 | Online | https://ufiling.labour.gov.za/ |
| CF registration | COIDA registration | L2-02 | Online/PDF | Compensation Fund |
| LOGS | Letter of good standing | L2-02 | Online | Compensation Fund |
| EEA forms | Employment Equity plan/report | L2-03 | Online/PDF | Department of Employment and Labour |
| FSCA eLicensing | FSP/CASP/CIS application | L3-01–03 | Online | https://www.fsca.co.za/ |
| NCR registration | Credit provider/debt collector | L3-04–05 | Online | https://www.ncr.org.za/ |
| FFC | Estate agent fidelity fund certificate | L3-06 | Online | https://theppra.org.za/ |
| Liquor application | Provincial liquor licence | L3-07 | Provincial | Provincial liquor authority |
| Certificate of acceptability | Food premises | L3-08 | Municipal | Municipality |
| NERSA registration | Embedded generation | L3-11 | Online | https://www.nersa.org.za/ |
| ICASA ECS | Electronic communications service licence | L3-12 | Online | https://www.icasa.org.za/ |
| SACAA ROC | Aviation operator certificate | L3-13 | Online | https://www.caa.co.za/ |
| PSIRA registration | Private security | L3-14 | Online | https://www.psira.co.za/ |
| CIDB registration | Construction contractor | L3-15 | Online | https://www.cidb.org.za/ |
| NHBRC enrolment | Home builder | L3-15 | Online | https://www.nhbrc.org.za/ |
| Deeds search | Title deed search | L4-01 | Portal | https://www.deeds.gov.za/ |
| SG diagram | Surveyor-General diagram | L4-01 | Portal | https://csg.dla.gov.za/ |
| Zoning certificate | Zoning certificate | L4-02 | Municipal | Municipality |
| Land use application | Rezoning/consent use | L4-03 | Municipal | Municipality |
| Subdivision/township | Subdivision/township establishment | L4-04 | Municipal | Municipality |
| SDP | Site development plan | L4-05 | Municipal | Municipality |
| Building plan | Building plan approval | L4-06 | Municipal | Municipality |
| Occupation certificate | Occupation certificate | L4-07 | Municipal | Municipality |
| Rates clearance | Rates clearance certificate | L4-08 | Municipal | Municipality |
| Transfer duty | Transfer duty declaration | L4-09 | Online | SARS eFiling |
| Deed of transfer | Property transfer | L4-10 | Conveyancer | Deeds Office |
| Screening report | NEMA screening | L5-01 | Online | https://neas.environment.gov.za/ |
| EA application | Environmental authorisation | L5-02 | Online | https://neas.environment.gov.za/ |
| WULA | Water use licence | L5-03 | Online | https://www.dws.gov.za/ |
| Waste licence | Waste management licence | L5-04 | Online/PDF | DFFE/province |
| AEL | Air emission licence | L5-05 | Municipal/province | Municipality/province |
| SAHRIS | Heritage permit/HIA | L5-06 | Online | https://www.sahra.org.za/ |
| CoC electrical | Electrical certificate | L6-01 | Certificate | Registered electrician |
| CoC plumbing | Plumbing certificate | L6-02 | Certificate | Registered plumber |
| CoC gas | Gas certificate | L6-03 | Certificate | Registered installer |
| Fire clearance | Fire safety certificate | L6-04 | Municipal | Fire department |
| ITAC permit | Import/export permit | L7-01 | Online | https://www.itac.org.za/ |
| PPECB registration | Export registration/certificate | L7-02 | Online | https://www.ppecb.com/ |
| NRCS LOA | Letter of authority | L7-03 | Online | https://www.nrcs.org.za/ |
| SAHPRA licence | Health product licence | L7-04 | Online | https://www.sahpra.org.za/ |
| Port health/phytosanitary | Export/import certificate | L7-05 | Portal | Port Health/DALRRD |
| IO registration | POPIA Information Officer | L8-01 | Online | https://inforegulator.org.za/ |
| PAIA manual | PAIA manual | L8-02 | Document | Information Regulator |
| goAML registration | FIC registration | L8-03 | Online | https://goweb.fic.gov.za/goAMLWEb_PRD/Home |
| RMCP | Risk management and compliance programme | L8-04 | Upload | FIC |
| RCR | Risk and compliance return | L8-05 | Online | https://rcr.fic.gov.za/ |
| STR/CTR | Suspicious/cash threshold report | L8-06 | Online | goAML |
| ITR14 | Company income tax return | L9-01 | Online | SARS eFiling |
| VAT201 | VAT return | L9-02 | Online | SARS eFiling |
| EMP201 | PAYE return | L9-03 | Online | SARS eFiling |
| COIDA return | Annual return | L9-04 | Online | Compensation Fund |
| Renewal | Licence renewal | L9-05 | Various | Regulator portal |

---

## Appendix C — Coverage boundary and maintenance

### C.1 What is covered

- 70 national/provincial/municipal-archetype channels across 10 layers.
- The main entity types: private company, sole proprietor, trust, NPO, co-operative.
- The main sector licences relevant to AEFRI/AEQUCHAIN and a general SME.
- The land pipeline from title to transfer.
- The environmental/water/heritage stack.
- FIC, POPIA, FAIS, CASP, CIS, NCA, exchange control, payments.

### C.2 What is not covered (v1)

- Every municipality's by-laws and tariffs (only archetypes).
- A standalone agricultural-land consent adapter (Act 70 consent is embedded in L4-04, not modelled as its own portal).
- Every sector sub-licence (e.g., all 9 provinces' liquor variants, all gambling modes, all health professions).
- Mining right (as opposed to mining permit) and petroleum exploration/ production rights in full.
- Financial market infrastructure (exchange, central securities depository, clearing).
- Cross-border recognition of foreign entities and branches.
- Industry-specific bargaining councils and collective agreements.
- Tax rulings, transfer pricing, and customs tariff classification.
- Litigation, insolvency, business rescue, and liquidation.
- Data localisation and sectoral data codes beyond POPIA.
- AI-specific regulation (none enacted as at 2026-10-21).

### C.3 Freshness SLAs

| Channel class | SLA | Trigger |
|---|---|---|
| Fees and forms | 30 days | Regulator page change; Gazette |
| Statutes and commencement | 7 days | Gazette; Parliament; SAFLII |
| Portal selectors | 7 days (canary) | Canary failure; login page change |
| Municipal tariffs/by-laws | 90 days | Municipal budget/notice |
| Case law and judgments | 30 days | SAFLII/regulator update |

### C.4 Change-monitor sources

- Government Gazette (commencement notices, regulations, fees).
- Parliament (Bills, committee reports).
- SAFLII / LawLibrary (consolidated Acts, judgments).
- Regulator newsrooms: CIPC, SARS, FIC, FSCA, NCR, NERSA, ICASA, SACAA, SAHPRA, DFFE, DWS, SAHRA.
- Municipal notices and budget documents.
- Portal canaries (selector hashes, validation messages).

### C.5 Next versions

| Version | Scope |
|---|---|
| v1.1 | Re-verify all R-grade facts (done: 34 A / 23 B / 13 C); add 10 municipal adapters (4 metros done); add provincial liquor variants (not done) |
| v1.2 | Add mining right, financial market infrastructure, cross-border entities |
| v2.0 | Live registry with change monitor, gate broker, and evidence ledger in production |

---

## Document control

| Field | Value |
|---|---|
| Author | Research agent (AEFRI) |
| Date | 2026-10-21 |
| Version | 1.1 |
| Companion | `sa_business_channels.json`; `SA-BUSINESS-AGENT-RESEARCH-v1.1-ADDENDUM.md`; `sa_verification_register.py`; `municipal_adapters.json`; `COUNSEL-PACKET-v1.md` |
| Review required | Licensed attorney; FIC/FAIS compliance officer; EAP; conveyancer |
| Change policy | Any legal-status change requires a version bump and a source hash |
| Licence | Internal research; not for redistribution as legal advice |

**Final disclosure:** This document was produced under a phase-12 gate review. Three first-pass statements were corrected during review: the Public Procurement Act was assumed to be pending commencement and is in fact invalid; NEAS was assumed to mean "National Environmental Assessment System" and is in fact the "National Environmental Authorisation System"; and ECTA s13 was first summarised as requiring an advanced electronic signature only where a statute specifies one, when s13(1) in fact requires it where a law requires a signature and does not specify the type. All three corrections are recorded above. In v1.1 every channel was re-checked against its official source and the result is recorded in `sa_verification_register.py` and `SA-BUSINESS-AGENT-RESEARCH-v1.1-ADDENDUM.md`. A/B/C are observations of what was checked, not legal opinions. B and C must not be treated as verified. No claim of completeness or legal correctness is made beyond the evidence grades shown.

# SA business compliance agent research — v1.1 addendum

**Date:** 2026-10-21  
**Companion to:** `SA-BUSINESS-AGENT-RESEARCH-v1.md` (base report, now v1.1)  
**Companion artifacts:** `sa_business_channels.json` (schema 1.2), `municipal_adapters.json`, `MUNICIPAL-ADAPTERS-v1.md`, `COUNSEL-PACKET-v1.md`, `sa_verification_register.py`, `sa_channel_verification.json`  
**Status:** research and adapter configuration. **No live submission path is enabled.** Not legal advice.

---

## 1. What changed in v1.1

1. **Per-channel verification register.** Every channel in the 70-channel index was re-checked against its official source on 2026-10-21. The register is in `sa_verification_register.py` and is merged into `sa_business_channels.json` under a `verification` block.
2. **Real municipal adapter configurations** for Cape Town, Johannesburg, eThekwini, and Tshwane, with systems modelled separately (`planning_submission` vs `account_and_revenue`). See `municipal_adapters.json` and `MUNICIPAL-ADAPTERS-v1.md`.
3. **Counsel packet** consolidating the licence map and investor-portal red lines, marked **UNSIGNED — external counsel required**. See `COUNSEL-PACKET-v1.md`.
4. **Corrections** to the base report:
   - `L9-02 VAT201` timeline corrected to distinguish manual (25th) from eFiling (last business day).
   - `L3-06 PPRA` source corrected from the commercial redirect `www.ppra.org.za` to the official domain `theppra.org.za`, which returned HTTP 403 to curl.
   - `L0-06 business name` marked **unverified**; no current form or fee page was located.
   - `L0-01` incorporation timeline corrected to 1 working day via eServices/BizPortal for a standard MOI.
5. **Verification boundary** updated with a link-check summary (42 URLs: 33 HTTP 2xx, 1 HTTP 5xx, 8 errors) and an explicit A/B/C method note.

---

## 2. Verification register method and summary

**Method**

- Every channel was re-checked on 2026-10-21 against its official source.
- **A** means route and form were confirmed on an official source read this session.
- **B** means the regulator/portal was reachable and the function identified, but the form, fee, deadline, or post-login route was **not** confirmed.
- **C** means the official source was blocked, unreachable, or errored. The original record remains at R-grade and **must not** be treated as verified.
- **A/B/C do not certify the legal status of the underlying law.** They are observations of what was checked, not opinions.

**Summary**

| Level | Channels | Meaning |
|---|---|---|
| A | 34 | Route and form confirmed on an official source read this session. |
| B | 23 | Regulator/portal reachable and function identified; form/fee/deadline not confirmed. |
| C | 13 | Official source blocked, unreachable, or errored. |

**Route / form / detail breakdown**

| Component | A | B | C |
|---|---|---|---|
| Route | 43 | 14 | 13 |
| Form | 34 | 22 | 14 |
| Detail (fee/deadline) | 12 | 44 | 14 |

**Link check**

- 42 URLs checked.
- 33 returned HTTP 2xx.
- 1 returned HTTP 5xx (`uFiling`, 503).
- 8 returned errors (`bizportal`, `csg`, `dalrrd`, `deeds`, `dffe`, `dmre`, `nrcs`, `psira`).

**Boundary.** A/B/C are **accessibility and route observations**, not legal opinions. They do not certify the legal status of any statute, regulation, fee, or deadline.

---

## 3. Per-channel verification register

The authoritative register is `sa_verification_register.py`. The table below is the human-readable summary. `gaps` are the things the adapter **cannot do yet** without a human in the loop.

| ID | Channel | Regulator | Level | Route | Form | Detail | Evidence note |
|---|---|---|---|---|---|---|---|
| L0-01 | Private company incorporation (standard MOI) | CIPC | A | A | A | A | CIPC Forms & Fees page: private company R125; name reservation R50 online / R75 manual; CoR14.1, CoR15.1A-C and CoR9.1 listed; eServices standard private company turnaround 1 working day. |
| L0-02 | Beneficial ownership register | CIPC | A | A | A | A | CIPC beneficial ownership page: new companies must file the BO register within 10 business days; changes must be filed within 10 business days. |
| L0-03 | Director, secretary and auditor changes | CIPC | A | A | A | B | CIPC Forms & Fees page lists CoR39 for initial directors and CoR44 for auditor/secretary changes; eServices transaction route identified. |
| L0-04 | Annual return | CIPC | A | A | A | A | CIPC annual return page: file within 30 business days after the anniversary of incorporation; immediate processing on payment. |
| L0-05 | Annual financial statements / iXBRL | CIPC | A | A | A | B | CIPC XBRL pages, taxonomy files, and iXBRL manual are present; the iXBRL AFS filing route is confirmed. |
| L0-06 | Business name registration (sole proprietor) | CIPC | B | B | C | C | CIPC Legislation page still lists 'Register business names', but a CIPC site search and the Forms & Fees page produced no current business-name application or fee. No official form was found. |
| L0-07 | Trust, NPO and co-operative registration | Master / DSD / CIPC | A | A | A | B | Master's Trusts page: Trust Property Control Act 57/1988; trust deed and fee required. DSD page carries NPO online registration; CIPC forms page carries CO-OP1 and CO-OP6.1. |
| L0-08 | Trade mark and patent | CIPC | A | A | A | A | CIPC Trade Marks Forms and Fees page: TM1 fee R590; TM2 and related forms listed; online/IP route identified. |
| L1-01 | Income tax registration | SARS | B | B | B | B | SARS income tax guide page is reachable. The claimed automatic CIPC-to-SARS income tax registration was not confirmed on a specific registration page. |
| L1-02 | VAT registration | SARS | A | A | A | A | SARS VAT registration page: compulsory registration at R2.3 million taxable supplies; voluntary registration at R120,000 from 1 April 2026; application within 21 business days. |
| L1-03 | PAYE, SDL and UIF registration | SARS | A | A | A | A | SARS PAYE registration page: register within 21 business days; EMP101 route. |
| L1-04 | Customs and excise registration | SARS | A | A | A | B | SARS Registration, Licensing and Accreditation page: RLA route, DA185, clearing agent, warehouse, deferment, AEO and related registrations listed. |
| L1-05 | Tax compliance status (TCS) | SARS | A | A | A | B | SARS TCS guide: Tax Clearance Certificate process modernised into Tax Compliance Status; eFiling functionality confirmed. |
| L1-06 | Tax practitioner registration and eFiling delegation | SARS + RCB | A | A | A | B | SARS Tax Practitioners page: register as a tax practitioner, RCB route, and eFiling delegation process listed. |
| L2-01 | UIF registration | Department of Employment and Labour | C | C | C | C | uFiling returned HTTP 503 on 2026-10-21; no registration or form route could be confirmed. |
| L2-02 | COIDA registration and letter of good standing | Compensation Fund | B | A | B | B | Department of Employment and Labour homepage is reachable and identifies the Compensation Fund. The CF registration and letter-of-good-standing forms were not separately opened. |
| L2-03 | Employment Equity plan and report | Department of Employment and Labour | B | A | B | B | Labour homepage is reachable. Employment Equity plan/report forms were not separately opened. |
| L2-04 | SETA registration and skills reporting | SETA + SARS | B | A | B | B | DHET SETA links page is reachable and lists the SETAs. SETA registration and WSP/ATR routes were not separately confirmed. |
| L2-05 | Foreign employee visa and work authorisation | Department of Home Affairs | A | A | A | B | DHA Immigration Services page: business, general work, critical skills, intra-company transfer, corporate, study, exchange and retired visa categories; VFS route identified. Online application shown is for passports/IDs, not visas. |
| L3-01 | Financial services provider (FSP) licence | FSCA | A | A | A | B | FSCA Regulated Entities page: Financial Advisory and Intermediary Services licensing category and a New Licence Application link were confirmed. |
| L3-02 | Crypto asset service provider (CASP) licence | FSCA | B | A | B | B | FSCA site is reachable and the licensing section is present. The CASP application form and crypto-asset licensing page were not separately opened. |
| L3-03 | Collective investment scheme manager licence | FSCA | A | A | A | B | FSCA Collective Investment Schemes licensing page and CIS New Applications link were confirmed. |
| L3-04 | Credit provider registration | NCR | A | A | A | B | NCR Registrations and Compliance page: credit provider registration route and application forms were confirmed. |
| L3-05 | Debt collector registration | NCR | A | A | A | B | NCR site is reachable and the registrations section covers debt collectors and debt counsellors. The debt-collector form was not separately opened. |
| L3-06 | Estate agent fidelity fund certificate (FFC) | PPRA | C | C | C | C | Official PPRA domain theppra.org.za returned HTTP 403 to curl on 2026-10-21. www.ppra.org.za is a commercial redirect to theppra.org and states it is 'not affiliated with or endorsed by the PPRA'. No official FFC page was read. |
| L3-07 | Liquor licence | Provincial liquor authority | C | C | C | C | Only the generic gov.za entry point was available; no provincial liquor authority portal or form was confirmed. |
| L3-08 | Food premises certificate of acceptability | Municipality | C | C | C | C | No municipal food-premises certificate portal or form was confirmed across the four metropolitan adapters. |
| L3-09 | Gambling licence | Provincial gambling board | C | C | C | C | Only the generic gov.za entry point was available; no provincial gambling board portal or form was confirmed. |
| L3-10 | Mining permit / petroleum licence | DMRE | C | C | C | C | DMRE returned HTTP 000 to curl on 2026-10-21; no mining or petroleum route was confirmed. |
| L3-11 | Embedded generation registration | NERSA | B | A | B | B | NERSA homepage is reachable and exposes an online portal. The embedded-generation registration form and threshold were not separately confirmed. |
| L3-12 | Electronic communications service licence | ICASA | A | A | A | B | ICASA site: licensing section, ECS licence applications, fees and renewals were confirmed from the licensing pages. |
| L3-13 | Aviation operator certificate (ROC) | SACAA | B | A | B | B | SACAA site is reachable and personnel licensing is present. The air operator certificate (ROC) application route was not separately confirmed. |
| L3-14 | Private security registration | PSIRA | C | C | C | C | PSIRA returned HTTP 000 to curl on 2026-10-21; no registration route was confirmed. |
| L3-15 | Construction and home builder registration | CIDB / NHBRC | A | A | A | B | CIDB site exposes online registration for contractors; NHBRC site exposes home builder registration/enrolment. Both routes were identified. |
| L4-01 | Deeds search and SG diagram | Deeds Office / Chief Surveyor-General | C | C | C | C | Deeds Office and Chief Surveyor-General returned HTTP 000 to curl on 2026-10-21; no search or diagram route was confirmed. |
| L4-02 | Zoning certificate | Municipality | B | B | B | B | Cape Town DAMS offers a zoning extract request; Johannesburg publishes land-use forms; eThekwini planning portal was not identified; Tshwane was unreachable. Coverage is partial, not a single municipal API. |
| L4-03 | Land use / rezoning application | Municipality | B | B | B | B | Cape Town requires land-use applications online via DAMS; Johannesburg publishes land-use forms (Forms 4-12) and a 2026/27 fee schedule but submission channel is not confirmed; eThekwini and Tshwane are blocked. |
| L4-04 | Subdivision, consolidation and township establishment | Municipality + SG | B | B | B | B | Cape Town DAMS lists subdivision/consolidation exemption and land-use manuals; Johannesburg publishes Form 5 subdivision/consolidation. Township establishment is not fully covered. |
| L4-05 | Site development plan approval | Municipality | B | B | B | B | Johannesburg publishes Form 12 Site Development Plan; Cape Town DAMS lists land-use applications. eThekwini and Tshwane remain blocked. |
| L4-06 | Building plan approval | Municipality | B | B | B | B | Cape Town: building plans accepted online only via DAMS after eServices registration. Johannesburg: application form downloadable, submission at Metro Centre counters, 158 Loveday Street, or by form; eServices provides tracking, not submission. eThekwini and Tshwane are blocked. |
| L4-07 | Occupation certificate | Municipality | B | B | B | B | Cape Town DAMS lists an occupancy-certificate request manual; Johannesburg has a certificate-of-occupancy page. eThekwini and Tshwane are blocked. |
| L4-08 | Rates clearance certificate | Municipality | B | B | B | B | Cape Town, Johannesburg and eThekwini eServices accounts expose rates/statements and payment functions. The specific rates-clearance workflow was not confirmed. |
| L4-09 | Transfer duty | SARS | A | A | A | B | SARS Transfer Duty page: TDC01 declaration, eFiling/payment route and supporting documents were confirmed. |
| L4-10 | Property transfer registration | Deeds Office / conveyancer | C | C | C | C | Deeds Office returned HTTP 000 to curl on 2026-10-21; conveyancer lodgement and registration route not verified. |
| L5-01 | NEMA screening | DFFE | A | A | A | B | NEAS portal returned HTTP 200. The National Web-based Environmental Screening Tool and NEMA screening route were identified. |
| L5-02 | Environmental authorisation | DFFE / province | A | A | A | B | NEAS portal returned HTTP 200; environmental authorisation applications are routed through NEAS under NEMA/EIA Regulations 2014. |
| L5-03 | Water use licence / general authorisation | DWS | B | A | B | B | DWS homepage is reachable. The e-WULAAS/WULA application route was not separately confirmed. |
| L5-04 | Waste management licence | DFFE / province | C | C | C | C | DFFE returned HTTP 000 to curl on 2026-10-21; no waste licence route was confirmed. |
| L5-05 | Air emission licence | DFFE / municipality | C | C | C | C | DFFE returned HTTP 000 to curl on 2026-10-21; no air emission licence route was confirmed. |
| L5-06 | Heritage permit / impact assessment | SAHRA / province | A | A | A | B | SAHRA site is reachable and SAHRIS is identified as the heritage application system; development applications and permits are hosted there. |
| L6-01 | Electrical certificate of compliance | Registered electrician | B | B | B | B | Electrical CoC regime is known (registered electrician, test and certificate), but no single official registry or issuer-verification portal was confirmed. |
| L6-02 | Plumbing certificate of compliance | Registered plumber | B | B | B | B | Plumbing CoC regime is known, but no single official registry or issuer-verification portal was confirmed. |
| L6-03 | Gas certificate of compliance | Registered gas installer | B | B | B | B | Gas CoC regime is known, but no single official registry or issuer-verification portal was confirmed. |
| L6-04 | Fire clearance | Fire department / municipality | B | B | B | B | Fire clearance is a municipal fire-department function; no single portal was confirmed across the four metropolitan adapters. |
| L7-01 | Import / export permit | ITAC | A | A | A | B | ITAC site: application forms and import/export permit routes were confirmed. |
| L7-02 | PPECB export registration and certificate | PPECB | A | A | A | B | PPECB site: eCert/TITAN exporter and export-certificate routes were confirmed. |
| L7-03 | NRCS letter of authority | NRCS | C | C | C | C | NRCS returned HTTP 000 to curl on 2026-10-21; no letter-of-authority route was confirmed. |
| L7-04 | SAHPRA licence | SAHPRA | A | A | A | B | SAHPRA site: licences, inspections and compliance, and an application portal were confirmed. |
| L7-05 | Port health and phytosanitary certificate | Port Health / DALRRD | C | C | C | C | DALRRD returned HTTP 000 to curl on 2026-10-21; no port-health or phytosanitary route was confirmed. |
| L8-01 | POPIA Information Officer registration | Information Regulator | A | A | A | A | Information Regulator POPIA page: section 55(2) requires Information Officers to register with the Regulator before taking up duties; registration is via the Regulator's E-Services Portal. |
| L8-02 | PAIA manual | Information Regulator | B | A | B | B | Information Regulator PAIA page is reachable. The PAIA manual template and submission requirements were not separately confirmed. |
| L8-03 | FIC registration | FIC | A | A | A | B | FIC goAML portal returned HTTP 200 and identifies registration and reporting functions. |
| L8-04 | RMCP submission (Directive 12) | FIC | A | A | A | A | FIC notice dated 9 October 2026: Directive 12 requires specified accountable institutions to submit an RMCP; Schedule 1 items 1, 2, 9 and 11 are in scope. |
| L8-05 | Risk and compliance return (Directive 11) | FIC | A | A | A | A | FIC RCR page: 2026 risk and compliance return window 4 May to 30 June, extended to 31 July 2026; goAML credentials required. |
| L8-06 | Suspicious and cash threshold reporting | FIC | A | A | A | B | FIC goAML portal returned HTTP 200 and identifies reporting functions for STR/CTR. |
| L9-01 | ITR14 company income tax return | SARS | A | A | A | A | SARS company income tax guidance: ITR14 is due within 12 months of the financial year-end. |
| L9-02 | VAT201 return | SARS | A | A | A | A | SARS VAT201 guide: manual submissions are due by the 25th of the month; eFiling submissions are due by the last business day of the month. |
| L9-03 | EMP201 PAYE return | SARS | A | A | A | A | SARS PAYE guidance: EMP201 is due monthly by the 7th. |
| L9-04 | COIDA annual return | Compensation Fund | B | A | B | B | Labour/Compensation Fund homepage is reachable. The COIDA annual return form and submission route were not separately opened. |
| L9-05 | Licence renewals | Various regulators | B | B | B | B | Renewal windows vary by regulator. No single renewal register or consolidated calendar was verified. |

---

## 4. Municipal adapters

The authoritative configuration is `municipal_adapters.json`. The human-readable summary is `MUNICIPAL-ADAPTERS-v1.md`.

**Key findings**

- **Cape Town** has a real online planning submission channel: **DAMS via e-Services**. Building plans and land use applications are accepted **online only** via DAMS; `BDM02` and `LUM01` forms are no longer downloadable. A revenue/eServices account is a **separate system** from a planning submission account.
- **Johannesburg** has a **physical** building-plan submission channel at Metro Centre counters, 158 Loveday Street, Braamfontein. The application form is downloadable; eServices provides **tracking**, not submission. Land-use forms 1–13 and the 2026/27 fee schedule are downloadable.
- **eThekwini**'s eServices² covers municipal accounts, statements, meter readings, complaints, and trade licensing. It is **not** a planning submission portal. No planning submission portal was identified in the public page.
- **Tshwane** is unreachable from this environment (`HTTP 000` from curl for both `eservices.tshwane.gov.za` and `www.tshwane.gov.za`). No adapter can be built from public evidence without further browser-side discovery.

**Why this matters**

- A revenue account is **not** a planning submission account. Treating it as one produces the classic failure: the agent can log in, but it cannot submit a plan, or worse, it submits to the wrong system.
- `automation_ceiling` is **per system**, not per municipality. A municipality can be C8 (submitted) for revenue and C6 (ready) for planning.
- `discovery_required` is not decoration: it is the list of things the adapter **cannot do yet** without a human in the loop.

---

## 5. Counsel packet

`COUNSEL-PACKET-v1.md` consolidates:

- the **licence map** (18 activity rows, each with a counsel question and a blocking flag);
- the **investor-portal red lines** (12 red lines the platform must enforce in code);
- the **open questions** counsel must answer;
- the **sign-off matrix**; and
- an explicit **no live submission path** status.

The packet is marked **UNSIGNED — external counsel required**. No live submission path may be enabled until it is signed.

---

## 6. Remaining gaps

1. **Official source access.** Several official sites are unreachable or JavaScript/login-gated from this environment. This is an accessibility observation, not proof that the site is dead. Browser-side discovery is required.
2. **Selectors and post-login workflows.** No municipal selector hash, document schema, or fee schedule has been verified. `discovery_required` is explicitly set where evidence is incomplete.
3. **Provincial and municipal variants.** Provincial liquor, gambling, and food-premises adapters are outside v1 coverage.
4. **Tshwane.** No public evidence of portal availability. Discovery is required.
5. **eThekwini planning.** No planning submission portal identified. Discovery is required.
6. **Live submission path.** Not enabled anywhere. `C7` requires a gate token bound to the payload; `C8` requires a portal receipt. Neither is implemented here.

---

## 7. How to read this addendum

- **Do not** treat A/B/C as legal verification. They are observations of what was checked.
- **Do not** treat the municipal adapters as live. `no_live_submission_path` is `true`.
- **Do not** enable any live submission path until counsel signs `COUNSEL-PACKET-v1.md`.
- The base report `SA-BUSINESS-AGENT-RESEARCH-v1.md` remains the legal boundary document. This addendum is the verification and operations layer.
