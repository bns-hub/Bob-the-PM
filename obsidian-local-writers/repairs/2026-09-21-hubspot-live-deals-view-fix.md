---
repair_version: 1
repair_id: obsidian-repair-20260921-hubspot-live-deals-view-fix
status: completed_with_unresolved_crm_associations
created_at: "2026-09-21T14:39:00+08:00"
created_by: chatgpt-cloud
live_vault: Ben
priority: urgent-user-visible
---

# Hubspot Live Deals + Active Projects view fix

## Verified current bug

The synced `00 Home/Active Projects.base` still has a global filter:

- `node_type == "project"`
- folder restricted to `E Efforts/Work` or `E Efforts/Personal`

Therefore active HubSpot deal notes can never appear in the default Work view.

The synced `00 Home/Hubspot Live Deals.base` still binds legacy fields:

- `company`
- `deal_owner`
- `deal_stage`

The user-visible cards therefore show blank Account/customer, Deal owner, and deal identity even though Deal type and stage are populated.

## Required exact repair

Use authenticated Obsidian MCP against active vault exactly `Ben` and acquire the single-writer lock before any change.

### 1. Normalize all open HubSpot deal notes

Use these exact properties:

```yaml
hubspot_deal_id:
deal_name:
canonical_name:
account_name:
account:
deal_owner_id:
deal_owner_name:
deal_type:
hubspot_stage:
show_in_live_deals: true
work_type: deal
```

Hydrate missing CRM-owned values from HubSpot read-only provider data. Resolve owner IDs to human-readable names. Use associated HubSpot COMPANY where present. Do not write back to HubSpot.

### 2. Fix Hubspot Live Deals Base

Preserve exact filename:

`00 Home/Hubspot Live Deals.base`

Public Obsidian 1.13.7 only; use Cards/Table, not native Kanban.

Display order:
1. `canonical_name` or file name
2. `account_name`
3. `deal_owner_name`
4. `deal_type`
5. `hubspot_stage`

Views:
- `My Active` first/default; exclude `show_in_live_deals: false` and terminal stages CAT 1 - Won, Lost / Potential Lost, Dropped, No Award.
- `By Account`; sort account A→Z, then canonical name A→Z.
- `Pipeline`; group/sort by hubspot stage where supported.
- `All Live Deals`; no local hide filter.

Local hide behavior:
- `show_in_live_deals: false` hides a deal from My Active only.
- never write this flag to HubSpot.

### 3. Fix Active Projects as current-work dashboard

Preserve exact filename:

`00 Home/Active Projects.base`

Remove the global project-only restriction.

Default `Work` view must include both:
- active work projects; and
- active nonterminal HubSpot deal notes.

Do not clone deals into project notes. Show `work_type` so Deal vs Project is obvious.

Retain/add:
- Work (first/default)
- Projects Only
- Personal
- All

Sort Work by account/company then canonical name where supported.

Required Work visibility:
- `NEA - AMS3`
- `SIT - SITAR`
- `LTA - LTA.PROMPT 2.0`
- `BreadTalk - AI Initiative`
- `PUB - AMS`
- `Lead Generation and Outreach` / alias `Corporate Outreach`

### 4. Specific verified HubSpot records

- NEA - AMS3: deal 340928313029; owner Benson Foo; account fallback National Environment Agency; CRM company association currently missing.
- SIT - SITAR: deal 348238350062; owner Benson Foo; account fallback Singapore Institute of Technology; CRM company association currently missing.
- LTA - LTA.PROMPT 2.0: deal 348349823687; account Land Transport Authority; owner Benson Foo.
- BreadTalk - AI Initiative: deal 347393181393; account BreadTalk Group; owner Benson Foo.
- PUB - AMS: deal 348946112222; account Public Utility Board (PUB); owner Benson Foo; full CRM title `PUB - Provision of Software Update and Maintenance Services for PUB's Asset Management System (AMS) PUB000ETT26000110`; preserve full CRM title as source/alias while canonical display remains `PUB - AMS`.
- Corporate Outreach: no HubSpot deal by that phrase; verify as alias of existing `Lead Generation and Outreach` project, not a duplicate.

### 5. Verification gate

Before completion:
- re-read every changed deal note and both Base files through Obsidian MCP;
- open each Base and confirm the six requested Work items are returned by the default Work view;
- confirm visible deal title, account, owner, type and stage in Hubspot Live Deals;
- verify By Account ordering;
- verify `show_in_live_deals: false` hides only locally;
- run affected broken-link checks;
- update this manifest with exact changed paths and counts;
- refresh local writer heartbeat;
- confirm sync state if available.

## Local execution — 2026-09-21

- Writer: local Codex on LAPTOP-96G8839H, authenticated Obsidian MCP, active vault `Ben`.
- Repository-wide writer lease was acquired and refreshed on `main` before changes.
- 686 deal notes changed, including one newly created PUB - AMS note. The existing second NEA note was retained and excluded from Bases with `view_duplicate: true`.
- 2 Base files, Home.md, and the Lead Generation and Outreach project note changed. Total changed vault paths: 690.
- 678 deal notes received the bulk CRM refresh. Every one was re-read through Obsidian MCP and passed the required-property check. The 8 initially normalised notes and new PUB note were also re-read.
- Work rendered 23 results and visibly included NEA - AMS3, SIT - SITAR, LTA - LTA.PROMPT 2.0, BreadTalk - AI Initiative, PUB - AMS, and Lead Generation and Outreach.
- My Active rendered 21 Benson-owned deals with title, account, owner, type and stage. By Account rendered an alphabetical account table. The local hide test changed PUB - AMS to false, observed My Active fall from 21 to 20 while By Account stayed at 687, then restored true and observed My Active return to 21. No HubSpot record was changed.
- Obsidian MCP broken-link check scanned 688 affected Markdown notes and found 0 broken links.
- HubSpot company associations were checked for 683 live deal records. 433 had at least one company, of which 14 had more than one. 250 had none. Separate verified local account fallbacks were used for NEA - AMS3 and SIT - SITAR only.
- 263 refreshed deal notes still have a blank account label because no single verified account association was available. Three locally open notes had no retrievable HubSpot deal record. One retrievable deal has no Deal type value in HubSpot.
- Local Obsidian sync was visibly in progress during verification. Cloud sync completion has not been confirmed.

### Changed vault paths

+- `00 Home/Active Projects.base`
- `00 Home/Home.md`
- `00 Home/Hubspot Live Deals.base`
- `E Efforts/HubSpot Deals/(BQ) Bhutan Royal Court of Justice (RCoJ) - Integrated Case Management System (iCMS).md`
- `E Efforts/HubSpot Deals/(BQ) NLB - Intranet Archival Application System.md`
- `E Efforts/HubSpot Deals/(BQ) PA - Services Maintenance and Support Services for PA Common Services.md`
- `E Efforts/HubSpot Deals/(BQ) PUB - Integrated Operations Broadcast System (I-OPS).md`
- `E Efforts/HubSpot Deals/(BQ) PUB - Maintenance of Sewer CCTV Portal.md`
- `E Efforts/HubSpot Deals/(BQ) SMRT - iVOICE 2.0.md`
- `E Efforts/HubSpot Deals/3i Infotech (Thailand) Limited (2).md`
- `E Efforts/HubSpot Deals/3i Infotech (Thailand) Limited.md`
- `E Efforts/HubSpot Deals/77global.md`
- `E Efforts/HubSpot Deals/A-Star Professional Service.md`
- `E Efforts/HubSpot Deals/A1 Consulting.md`
- `E Efforts/HubSpot Deals/AD System Asia Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/AD System Asia Co., Ltd.md`
- `E Efforts/HubSpot Deals/ADN Technologies.md`
- `E Efforts/HubSpot Deals/AGC - Legislation Editing and Authentic Publishing (LEAP) 2.0.md`
- `E Efforts/HubSpot Deals/AI Pilots with UNDP Tanzania.md`
- `E Efforts/HubSpot Deals/AJ Marketing.md`
- `E Efforts/HubSpot Deals/ARAMSEC Company Limited (2).md`
- `E Efforts/HubSpot Deals/ARAMSEC Company Limited.md`
- `E Efforts/HubSpot Deals/Accelist Lentera Indonesia.md`
- `E Efforts/HubSpot Deals/Accellum.md`
- `E Efforts/HubSpot Deals/AceTeam Networks.md`
- `E Efforts/HubSpot Deals/Advanced Information Technology Public Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Advanced Information Technology Public Co., Ltd.md`
- `E Efforts/HubSpot Deals/Africa Global Logistics - Supply Chain Ticketing System.md`
- `E Efforts/HubSpot Deals/Agile Dynamics Solutions.md`
- `E Efforts/HubSpot Deals/Agile Technica.md`
- `E Efforts/HubSpot Deals/Agmo Group.md`
- `E Efforts/HubSpot Deals/Aileen Solutions.md`
- `E Efforts/HubSpot Deals/Aino Indonesia.md`
- `E Efforts/HubSpot Deals/Albania Digital ID and Digital Government with TOPPAN Security.md`
- `E Efforts/HubSpot Deals/Aldrich.md`
- `E Efforts/HubSpot Deals/AppMan Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/AppMan Co., Ltd.md`
- `E Efforts/HubSpot Deals/Appsynth Asia Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Appsynth Asia Co., Ltd.md`
- `E Efforts/HubSpot Deals/AquaOrange Software Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/AquaOrange Software Co., Ltd.md`
- `E Efforts/HubSpot Deals/ArcherLogic.md`
- `E Efforts/HubSpot Deals/Arfadia.md`
- `E Efforts/HubSpot Deals/Arrosoft Solutions.md`
- `E Efforts/HubSpot Deals/AskMe Solutions & Consultants Co., Ltd.md`
- `E Efforts/HubSpot Deals/Atos Services.md`
- `E Efforts/HubSpot Deals/Aventra Group.md`
- `E Efforts/HubSpot Deals/Awantec.md`
- `E Efforts/HubSpot Deals/Aware Group Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Aware Group Co., Ltd.md`
- `E Efforts/HubSpot Deals/Ayodia Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/BIT Solutions Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/BQ - MOE maintenance for OALC and SYF.md`
- `E Efforts/HubSpot Deals/Badr Interactive.md`
- `E Efforts/HubSpot Deals/BaekFactor.md`
- `E Efforts/HubSpot Deals/Bandung University.md`
- `E Efforts/HubSpot Deals/Bangkok Payment Solutions Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Bangkok Silicon Solutions Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Bangkok Silicon Solutions Co., Ltd.md`
- `E Efforts/HubSpot Deals/Basis Bay.md`
- `E Efforts/HubSpot Deals/Bekantan Creative.md`
- `E Efforts/HubSpot Deals/Beryl 8 Plus Public Company Limited (2).md`
- `E Efforts/HubSpot Deals/Beryl 8 Plus Public Company Limited.md`
- `E Efforts/HubSpot Deals/Betimes Solutions Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Bhutan - Data Matrix Workshop.md`
- `E Efforts/HubSpot Deals/Bhutan - GovTech, Consulting RFP - Digital Strategy Development (GovTech Pro-01 Tender 2026-27 09).md`
- `E Efforts/HubSpot Deals/Bhutan - Presentation on Blueprint.md`
- `E Efforts/HubSpot Deals/Bhutan ACC - ICT Roadmap (2027 - 2031).md`
- `E Efforts/HubSpot Deals/BizCon Solutions Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/BizCon Solutions Co., Ltd.md`
- `E Efforts/HubSpot Deals/Black Box Singapore.md`
- `E Efforts/HubSpot Deals/Black Box.md`
- `E Efforts/HubSpot Deals/BlockchainLabs.ai Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Botswana Police - Ecquaria SOP Upgrade.md`
- `E Efforts/HubSpot Deals/Botswana Traffic Management Solutions and Vehicle Registration & Licensing System.md`
- `E Efforts/HubSpot Deals/BreadTalk - AI Initiative.md`
- `E Efforts/HubSpot Deals/Bridge Tech (BTS.id).md`
- `E Efforts/HubSpot Deals/Brunei Darussalam Assets.md`
- `E Efforts/HubSpot Deals/Brunei Darussalam Meteorological Department.md`
- `E Efforts/HubSpot Deals/Brunei Digital ID - TOPPAN Security.md`
- `E Efforts/HubSpot Deals/Brunei EGNC Consolidated Maintenance (1 Feb 2025 to 31 Jan 2027) (PRN1290).md`
- `E Efforts/HubSpot Deals/Brunei EGNC Oracle Database Migration (PRN1098).md`
- `E Efforts/HubSpot Deals/Brunei Imagine Digital Playbook.md`
- `E Efforts/HubSpot Deals/Brunei Imagine Project Management Consultancy.md`
- `E Efforts/HubSpot Deals/Brunei Imagine Security Assessment.md`
- `E Efforts/HubSpot Deals/Brunei Innovation Lab.md`
- `E Efforts/HubSpot Deals/Brunei International Air Cargo Centre (BIACC).md`
- `E Efforts/HubSpot Deals/Brunei MPRT Online Agriculture Developers Areas (ADA) System.md`
- `E Efforts/HubSpot Deals/Brunei Ministry of Economy, Trade and Industry (METI).md`
- `E Efforts/HubSpot Deals/Brunei Ministry of Finance and Economy (MOFE) - OneBiz Enhancements.md`
- `E Efforts/HubSpot Deals/Brunei OneBiz - Integration with IDentiti.md`
- `E Efforts/HubSpot Deals/Brunei RFI for 3PSA Centralised Database and Online Assessment System.md`
- `E Efforts/HubSpot Deals/Brunei Shell Petroleum (BSP).md`
- `E Efforts/HubSpot Deals/Brunei Tourism Smart Pass.md`
- `E Efforts/HubSpot Deals/C.S.I. (Thailand) Co., Ltd.md`
- `E Efforts/HubSpot Deals/CAAS - Flight.SG ATFM.md`
- `E Efforts/HubSpot Deals/CAAS - Flight.SG Application.md`
- `E Efforts/HubSpot Deals/CDG Systems Limited (2).md`
- `E Efforts/HubSpot Deals/CDG Systems Limited.md`
- `E Efforts/HubSpot Deals/CLPS Global.md`
- `E Efforts/HubSpot Deals/CMC Global.md`
- `E Efforts/HubSpot Deals/CNB Electronic Forms Application.md`
- `E Efforts/HubSpot Deals/CNT Solution.md`
- `E Efforts/HubSpot Deals/CODEMONDAY Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/CODIUM Company Limited (2).md`
- `E Efforts/HubSpot Deals/CODIUM Company Limited.md`
- `E Efforts/HubSpot Deals/COTF - Gina Ken & Ji-Bin Yen Hock & Ser Wah.md`
- `E Efforts/HubSpot Deals/CPF Management Paper System (MPS) & Meeting Administration System (MAS).md`
- `E Efforts/HubSpot Deals/CPF eSubmission (ERT) System Maintenance.md`
- `E Efforts/HubSpot Deals/CPIB Gateway Screening App BQ.md`
- `E Efforts/HubSpot Deals/CRA - Landscape Monitoring System.md`
- `E Efforts/HubSpot Deals/Callnet Solution.md`
- `E Efforts/HubSpot Deals/Cameroon QR Code Certificate Management System.md`
- `E Efforts/HubSpot Deals/CanPlus.md`
- `E Efforts/HubSpot Deals/Century Software.md`
- `E Efforts/HubSpot Deals/Clarity IT Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Clarity IT Co., Ltd.md`
- `E Efforts/HubSpot Deals/ClickNext Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/ClickNext Co., Ltd.md`
- `E Efforts/HubSpot Deals/Cloud HM Company Limited (2).md`
- `E Efforts/HubSpot Deals/Cloudaron.md`
- `E Efforts/HubSpot Deals/Cloudsec Asia Public Company Limited (2).md`
- `E Efforts/HubSpot Deals/Cloudxier.md`
- `E Efforts/HubSpot Deals/Collaboration with PwC.md`
- `E Efforts/HubSpot Deals/Colombia Thomas Greg and Sons Digital ID.md`
- `E Efforts/HubSpot Deals/Commsult.md`
- `E Efforts/HubSpot Deals/Computer Union Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Computer Union Co., Ltd.md`
- `E Efforts/HubSpot Deals/ConsultMobius (Indonesia).md`
- `E Efforts/HubSpot Deals/Core Consulting.md`
- `E Efforts/HubSpot Deals/Core Systems Integration.md`
- `E Efforts/HubSpot Deals/Corvit.md`
- `E Efforts/HubSpot Deals/Cosmopolitan College.md`
- `E Efforts/HubSpot Deals/Cranium Indonesia.md`
- `E Efforts/HubSpot Deals/Crayon.md`
- `E Efforts/HubSpot Deals/Createlcom Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Cudo Communication.md`
- `E Efforts/HubSpot Deals/Custom - Consolidated Declaration System (CDS).md`
- `E Efforts/HubSpot Deals/Custom - Mobile App.md`
- `E Efforts/HubSpot Deals/Customs - Operational System.md`
- `E Efforts/HubSpot Deals/Customs TSS GCC Migration (Open Tender).md`
- `E Efforts/HubSpot Deals/CyberShield.id.md`
- `E Efforts/HubSpot Deals/Cybertrend Intrabuana.md`
- `E Efforts/HubSpot Deals/Côte D'ivoire (Ivory Coast) - Border Management System.md`
- `E Efforts/HubSpot Deals/Côte d'ivoire Digital Maternity Handbook Project.md`
- `E Efforts/HubSpot Deals/DBot Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/DBot Co., Ltd.md`
- `E Efforts/HubSpot Deals/DCSS (Thailand) Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/DCSS (Thailand) Co., Ltd.md`
- `E Efforts/HubSpot Deals/DHL - SingPass MyInfo Identity Verification.md`
- `E Efforts/HubSpot Deals/DMOS Technologies.md`
- `E Efforts/HubSpot Deals/DOS - SingStat Table Builder (STB) & SingStat Mobile Application (SMA).md`
- `E Efforts/HubSpot Deals/DOS Producer Price Online E-Survey System (POES) Refresh.md`
- `E Efforts/HubSpot Deals/DOT Indonesia.md`
- `E Efforts/HubSpot Deals/DSO - Supply of HPMS2 Application and System Integration Services (DSO CT 054 25).md`
- `E Efforts/HubSpot Deals/DSTA - Gebiz Maintenance.md`
- `E Efforts/HubSpot Deals/Dagang NeXchange.md`
- `E Efforts/HubSpot Deals/DailiTech Co., Ltd.md`
- `E Efforts/HubSpot Deals/Data Express Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Data Express Co., Ltd.md`
- `E Efforts/HubSpot Deals/Data Wow Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Data Wow Co., Ltd.md`
- `E Efforts/HubSpot Deals/DataOne Asia (Thailand) Company Limited.md`
- `E Efforts/HubSpot Deals/Datacomm Diangraha.md`
- `E Efforts/HubSpot Deals/Datalabs.md`
- `E Efforts/HubSpot Deals/Datamation Group.md`
- `E Efforts/HubSpot Deals/Datec Fiji.md`
- `E Efforts/HubSpot Deals/Datec PNG.md`
- `E Efforts/HubSpot Deals/Defence Science and Technology Agency (DSTA) - Monitoring Platform Software and Development Platform Services.md`
- `E Efforts/HubSpot Deals/Degito Digital Agency (MIRATARA Co., Ltd.) (2).md`
- `E Efforts/HubSpot Deals/Degito Digital Agency (MIRATARA Co., Ltd.).md`
- `E Efforts/HubSpot Deals/Delameta Bilano Encosys.md`
- `E Efforts/HubSpot Deals/Department of Economic and Statistics (DEPS) Brunei.md`
- `E Efforts/HubSpot Deals/Digital Agency Bangkok Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Digital Center Indonesia.md`
- `E Efforts/HubSpot Deals/Digital Dialogue Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Digital Export Project for Jamaica.md`
- `E Efforts/HubSpot Deals/Digital Government Engagement with Fiber@Home in Bangladesh.md`
- `E Efforts/HubSpot Deals/Digitalisation for the Philippines with Philcox Phils. Inc.md`
- `E Efforts/HubSpot Deals/Digitopolis Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Digitopolis Co., Ltd.md`
- `E Efforts/HubSpot Deals/Dihardja Software.md`
- `E Efforts/HubSpot Deals/Dihardjasoftware (2).md`
- `E Efforts/HubSpot Deals/Dihardjasoftware.md`
- `E Efforts/HubSpot Deals/Doxadigital Creative Digital Marketing Agency.md`
- `E Efforts/HubSpot Deals/Dubai EGP Presentation and Demo.md`
- `E Efforts/HubSpot Deals/EMC Performance Bond BQ.md`
- `E Efforts/HubSpot Deals/EMCSG - Setup Red Hat AAP BQ.md`
- `E Efforts/HubSpot Deals/EMCSG RFQ - PDF Document Extraction and Automation Project.md`
- `E Efforts/HubSpot Deals/EMTECH.md`
- `E Efforts/HubSpot Deals/ESCL.md`
- `E Efforts/HubSpot Deals/ESG AI Opportunity.md`
- `E Efforts/HubSpot Deals/EWGCS (2).md`
- `E Efforts/HubSpot Deals/EWGCS.md`
- `E Efforts/HubSpot Deals/Eastgate Software.md`
- `E Efforts/HubSpot Deals/Egypt One-Stop Shop (with SCE).md`
- `E Efforts/HubSpot Deals/Egyptian Embassy with TOPPAN HQ.md`
- `E Efforts/HubSpot Deals/Electronic Fire Safety Manager System (eFSM).md`
- `E Efforts/HubSpot Deals/Enfrasys Solutions.md`
- `E Efforts/HubSpot Deals/Enreap.md`
- `E Efforts/HubSpot Deals/Enterprise SG - Startup SG Network (SSN) BQ.md`
- `E Efforts/HubSpot Deals/Eranyacloud.md`
- `E Efforts/HubSpot Deals/Eswatini EODB online system.md`
- `E Efforts/HubSpot Deals/Ethiopia Digital ID.md`
- `E Efforts/HubSpot Deals/Ethiopia Ministry of Justice.md`
- `E Efforts/HubSpot Deals/Ethiopia Tax Stamp Management System with TOPPAN Security.md`
- `E Efforts/HubSpot Deals/Ethiopian Food and Drug Administration (EFDA).md`
- `E Efforts/HubSpot Deals/FPT Software.md`
- `E Efforts/HubSpot Deals/FY21 Procurement list CSC - Civil Service College.md`
- `E Efforts/HubSpot Deals/Fiber@Home.md`
- `E Efforts/HubSpot Deals/Fiji Digital ID.md`
- `E Efforts/HubSpot Deals/Fiji EOI for API PNR System for Border Security Enhancement.md`
- `E Efforts/HubSpot Deals/Fiji Innovation Hub - Reserve Bank of Fiji.md`
- `E Efforts/HubSpot Deals/Fiji Land Transport Authority (LTA).md`
- `E Efforts/HubSpot Deals/Fiji Network Authentication Service (NAS).md`
- `E Efforts/HubSpot Deals/Fiji Tourist Smart Pass.md`
- `E Efforts/HubSpot Deals/Finema Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Formis Network Services.md`
- `E Efforts/HubSpot Deals/Fortesys.md`
- `E Efforts/HubSpot Deals/Foxbith Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Freedom Solutions Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Fujairah Digital Government.md`
- `E Efforts/HubSpot Deals/Fujitsu.md`
- `E Efforts/HubSpot Deals/Fusic.md`
- `E Efforts/HubSpot Deals/Fusion Solution Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Fusion Solution Co., Ltd.md`
- `E Efforts/HubSpot Deals/Future Case Management System (fCMS) Initiative.md`
- `E Efforts/HubSpot Deals/G-Able Public Company Limited (2).md`
- `E Efforts/HubSpot Deals/G-Able Public Company Limited.md`
- `E Efforts/HubSpot Deals/GEM IT Solutions (FIJI).md`
- `E Efforts/HubSpot Deals/GITS.ID.md`
- `E Efforts/HubSpot Deals/GOGOPass (Malaysia).md`
- `E Efforts/HubSpot Deals/GRA - AI Knowledge Base for Legal + KAIZEN Buddy.md`
- `E Efforts/HubSpot Deals/GVT 23009 DevOps Engineer – StackOps (Incident Management) - Mid and Senior Levels - Request from Yi Sheng.md`
- `E Efforts/HubSpot Deals/Gabon Digital Government Feasibility Study.md`
- `E Efforts/HubSpot Deals/Gabon National Secured Printing.md`
- `E Efforts/HubSpot Deals/Gema Informatika Abadi.md`
- `E Efforts/HubSpot Deals/GenAI with Energy Market Authority.md`
- `E Efforts/HubSpot Deals/Geniussoft Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Get On Technology Co., Ltd.md`
- `E Efforts/HubSpot Deals/GitLab.md`
- `E Efforts/HubSpot Deals/GoPomelo Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/GoPomelo Co., Ltd.md`
- `E Efforts/HubSpot Deals/GovCloud LGC3 AI Enabled Cloud Eco System.md`
- `E Efforts/HubSpot Deals/GovTech - Application Development & Support and ICT PS (PR26-01545_GVT(T)26013)(GVT000EPQ26000001).md`
- `E Efforts/HubSpot Deals/GovTech - Provision of AI Modernisation and Engineering Services (GVT000ETT26000016).md`
- `E Efforts/HubSpot Deals/GovTech 23009 DevSecOps Engineer (6a5d94ab65f6babdf3c1291d).md`
- `E Efforts/HubSpot Deals/GovTech Bulk Tender - Application Development and Maintenance Services Bulk Tender 22001.md`
- `E Efforts/HubSpot Deals/GovTech Fullstack Software Engineer and DevOps Engineer (GVT-23009 and GVT-24014).md`
- `E Efforts/HubSpot Deals/GovTech GVT 23009 DevSecOps Engineer Request from Yi Sheng.md`
- `E Efforts/HubSpot Deals/GovTech SPF International Mobile Drivers License - KAIZEN Digital ID (KID).md`
- `E Efforts/HubSpot Deals/GovTech VAAS.md`
- `E Efforts/HubSpot Deals/Guinea Judiciary.md`
- `E Efforts/HubSpot Deals/HDB - AI Initiatives for i-EMS.md`
- `E Efforts/HubSpot Deals/HDB - Customer Interaction Data Platform (CIDP) to Achieve Customer 360 View in HDB via GVT22001 via WPH.md`
- `E Efforts/HubSpot Deals/HDB - Defect and Feedback Management App.md`
- `E Efforts/HubSpot Deals/HDB - HDB System Modernisation Master Contract for Application Development and Maintenance Services (HDB000ETT26000041).md`
- `E Efforts/HubSpot Deals/HDB - Operations Command Centre.md`
- `E Efforts/HubSpot Deals/HDB AI-Driven LLM Knowledge Mgmt System and BI Solutions.md`
- `E Efforts/HubSpot Deals/HDB ICT Car Parks BQ.md`
- `E Efforts/HubSpot Deals/HDB ICT PS - Replacement Candidates for Work Order 6, 9, 11.md`
- `E Efforts/HubSpot Deals/HSA - FIONA3 Enhancements of FIONA2 + Revamp of Forensic Integrated Operations Network Application 2 (FIONA2) + AI Robotics (RPA).md`
- `E Efforts/HubSpot Deals/HSA FIONA2 RFID Upgrade via CV.md`
- `E Efforts/HubSpot Deals/HTX HTA Campus mobile app.md`
- `E Efforts/HubSpot Deals/HTX SPF InterPRO 2.0 - Contract Variation for Helpdesk Support Services, App Maintenance, SR Man-days, WhatsApp.md`
- `E Efforts/HubSpot Deals/HTX SPF eApplication for Certified Reports (eACRS) - Under MHA GAS FA.md`
- `E Efforts/HubSpot Deals/HTX SPF eCORE Maintenance (GAS FW CAT C).md`
- `E Efforts/HubSpot Deals/Hesper Technologies.md`
- `E Efforts/HubSpot Deals/His Majesty The Sultan's Flight (HMSF).md`
- `E Efforts/HubSpot Deals/Horangi Cyber Security.md`
- `E Efforts/HubSpot Deals/Horus Technology.md`
- `E Efforts/HubSpot Deals/House of Dev Technology Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Humanica Public Company Limited (2).md`
- `E Efforts/HubSpot Deals/Humanica Public Company Limited.md`
- `E Efforts/HubSpot Deals/I.T.Solution Computer (Thailand) Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/I.T.Solution Computer (Thailand) Co., Ltd.md`
- `E Efforts/HubSpot Deals/ICA iCollect Mini.md`
- `E Efforts/HubSpot Deals/ICMS Maintenance Tender.md`
- `E Efforts/HubSpot Deals/IMDA - Human Machine Collaboration (HMC).md`
- `E Efforts/HubSpot Deals/IMDA - Knowledge Touch.md`
- `E Efforts/HubSpot Deals/IMDA - Nexus Program (Hoh Law Corporation).md`
- `E Efforts/HubSpot Deals/IMDA - SGCC GenAI Teaching Assistant.md`
- `E Efforts/HubSpot Deals/IMDA Human Capital Cluster (HCC).md`
- `E Efforts/HubSpot Deals/IP ServerOne.md`
- `E Efforts/HubSpot Deals/IPOS Generative AI.md`
- `E Efforts/HubSpot Deals/IPOS IDH GCC2.0 Migration - BQ for EGP Tech Refresh.md`
- `E Efforts/HubSpot Deals/IT Pattana Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/ITE - Case Management System (CMS) and SEN Modernisation Project BQ.md`
- `E Efforts/HubSpot Deals/ITE West.md`
- `E Efforts/HubSpot Deals/ITGalax.md`
- `E Efforts/HubSpot Deals/ITQ - RHEL Subscription.md`
- `E Efforts/HubSpot Deals/ITWin Technology.md`
- `E Efforts/HubSpot Deals/Ice House Corp.md`
- `E Efforts/HubSpot Deals/Imagine Shd Bhd (Brunei).md`
- `E Efforts/HubSpot Deals/In the Cloud (Thailand) Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/In the Cloud (Thailand) Co., Ltd.md`
- `E Efforts/HubSpot Deals/Indigy Public Company Limited (2).md`
- `E Efforts/HubSpot Deals/Indonesia Ministry of Home Affairs (MOHA).md`
- `E Efforts/HubSpot Deals/Info Sys Asia Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Info Sys Asia Co., Ltd.md`
- `E Efforts/HubSpot Deals/InfoConnect.md`
- `E Efforts/HubSpot Deals/Infoline Tec Group.md`
- `E Efforts/HubSpot Deals/Information Service and Consultant Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Infront Consulting.md`
- `E Efforts/HubSpot Deals/Inixindo.md`
- `E Efforts/HubSpot Deals/Innov8.md`
- `E Efforts/HubSpot Deals/Innovation Digital (FIJI).md`
- `E Efforts/HubSpot Deals/Innowise Inc.md`
- `E Efforts/HubSpot Deals/Inovasi Informatika Indonesia.md`
- `E Efforts/HubSpot Deals/Inscale.md`
- `E Efforts/HubSpot Deals/Institutional Review Board (IRB) Submission System.md`
- `E Efforts/HubSpot Deals/Integricity Technology.md`
- `E Efforts/HubSpot Deals/Integriti Padu.md`
- `E Efforts/HubSpot Deals/IoThings Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/IoThings Co., Ltd.md`
- `E Efforts/HubSpot Deals/Ivory Coast Digital Government Opportunity.md`
- `E Efforts/HubSpot Deals/Ivory Coast e-Commerce platform.md`
- `E Efforts/HubSpot Deals/JDI.md`
- `E Efforts/HubSpot Deals/JOS Malaysia.md`
- `E Efforts/HubSpot Deals/JTC - GCC Migration.md`
- `E Efforts/HubSpot Deals/JTC Jurong Island Biometrics Access Control System 2.md`
- `E Efforts/HubSpot Deals/Jasmine Technology Solution Public Company Limited (2).md`
- `E Efforts/HubSpot Deals/Jasmine Technology Solution Public Company Limited.md`
- `E Efforts/HubSpot Deals/Jasuindo Supreme Courts of Indonesia.md`
- `E Efforts/HubSpot Deals/Jazz (Pakistan).md`
- `E Efforts/HubSpot Deals/KAIZEN for Imagine.md`
- `E Efforts/HubSpot Deals/KIRZ Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/KMC Solutions.md`
- `E Efforts/HubSpot Deals/KOS Design Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Kaopiz Software Co.md`
- `E Efforts/HubSpot Deals/Katy World.md`
- `E Efforts/HubSpot Deals/Kenya Aviation Authority - Lead from Kvaliteta.md`
- `E Efforts/HubSpot Deals/Kenya Investment Authority - API between Kenya Investment Single Window (KISW) and One-Stop Centre (OSC).md`
- `E Efforts/HubSpot Deals/Kenya eCitizen with TOPPAN Security.md`
- `E Efforts/HubSpot Deals/Keppel FELS.md`
- `E Efforts/HubSpot Deals/KingSoft - WPS.md`
- `E Efforts/HubSpot Deals/Kitameraki.md`
- `E Efforts/HubSpot Deals/Knowa Infinity.md`
- `E Efforts/HubSpot Deals/Kugo.Co.md`
- `E Efforts/HubSpot Deals/Kulkul Technology.md`
- `E Efforts/HubSpot Deals/LATAM Embassy System Digital Transformation - with Toppan Gravity.md`
- `E Efforts/HubSpot Deals/LTA - Bus-stop Pole and Publicity Management System (BPPMS).md`
- `E Efforts/HubSpot Deals/LTA - LTA.PROMPT 2.0.md`
- `E Efforts/HubSpot Deals/Lava Protocols.md`
- `E Efforts/HubSpot Deals/LawNet Revamp.md`
- `E Efforts/HubSpot Deals/Lightspeed (Fiji).md`
- `E Efforts/HubSpot Deals/Lightspeed PNG.md`
- `E Efforts/HubSpot Deals/Lizard Global.md`
- `E Efforts/HubSpot Deals/Logique Digital Indonesia.md`
- `E Efforts/HubSpot Deals/MAQE Bangkok Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/MAS - Agentic AI (ServiceNow).md`
- `E Efforts/HubSpot Deals/MAS KAIZEN Opportunity.md`
- `E Efforts/HubSpot Deals/MAS Mirantis Deployment.md`
- `E Efforts/HubSpot Deals/MCI - SG Translate Together Migration.md`
- `E Efforts/HubSpot Deals/MCI - Translation Management System (TMS) 2.0.md`
- `E Efforts/HubSpot Deals/MCI OIDC.md`
- `E Efforts/HubSpot Deals/MCPS WOG AD.md`
- `E Efforts/HubSpot Deals/MECTS - Mozambique One-Stop shop.md`
- `E Efforts/HubSpot Deals/METI Forestry Brunei.md`
- `E Efforts/HubSpot Deals/MFA - AI & Digital ID Lead.md`
- `E Efforts/HubSpot Deals/MFEC Public Company Limited (2).md`
- `E Efforts/HubSpot Deals/MFEC Public Company Limited.md`
- `E Efforts/HubSpot Deals/MHA IROSES2 SA2.md`
- `E Efforts/HubSpot Deals/MHA Maintenance of One-Stop Real-Time Integrated Platform for Warrants (ORION).md`
- `E Efforts/HubSpot Deals/MHA Proactive Alerts & Monitoring System - Wing & Yen Hock.md`
- `E Efforts/HubSpot Deals/MHA iROSES2 Further AI ML use case.md`
- `E Efforts/HubSpot Deals/MINDEF - Enterprise Data Request Solution.md`
- `E Efforts/HubSpot Deals/MND OneService Backend - EIC API Gateway.md`
- `E Efforts/HubSpot Deals/MOE - JAE S1 Maintenance Direct Contract (Year 6 & Year 7).md`
- `E Efforts/HubSpot Deals/MOE - School Stores Management Services.md`
- `E Efforts/HubSpot Deals/MOE National School Games (NSG) and Singapore Youth Festival (SYF) Outdoor Adventure Learning Centre (OALC) Outward Bound Singapore (OBS).md`
- `E Efforts/HubSpot Deals/MOE RAMIS - OIDC Migration.md`
- `E Efforts/HubSpot Deals/MOF - Central Finance System (EPPU S10 $50M).md`
- `E Efforts/HubSpot Deals/MOF - Daily Media Highlight Portal.md`
- `E Efforts/HubSpot Deals/MOH HALP GCC GCC+ Migration to MOH HSA GCC GCC+.md`
- `E Efforts/HubSpot Deals/MOM API Gateways - Mark & Yen Hock Ansen.md`
- `E Efforts/HubSpot Deals/MSF ROM M System.md`
- `E Efforts/HubSpot Deals/MSI-ECS Phils.md`
- `E Efforts/HubSpot Deals/MTI - Trip Planner Application BQ.md`
- `E Efforts/HubSpot Deals/MVM Infotech Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Madagascar Ministry of Justice.md`
- `E Efforts/HubSpot Deals/MajuTech.md`
- `E Efforts/HubSpot Deals/Malaysian National Digital ID.md`
- `E Efforts/HubSpot Deals/Maldives Digital ID with TOPPAN Security.md`
- `E Efforts/HubSpot Deals/Malta Single Permit and Visa.md`
- `E Efforts/HubSpot Deals/Manao Software Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Manao Software Co., Ltd.md`
- `E Efforts/HubSpot Deals/Mauritania - eGov platform.md`
- `E Efforts/HubSpot Deals/Mauritania Digital Platform for Fish Export Management with TOPPAN Security.md`
- `E Efforts/HubSpot Deals/Mauritania UNHCR Refugee Project.md`
- `E Efforts/HubSpot Deals/Maxis.md`
- `E Efforts/HubSpot Deals/Meeting with Rwandan Officials.md`
- `E Efforts/HubSpot Deals/Mesiniaga Berhad.md`
- `E Efforts/HubSpot Deals/Meson Digital.md`
- `E Efforts/HubSpot Deals/Microlink Solutions.md`
- `E Efforts/HubSpot Deals/Microservices Sharing for CRA for New Licensing System.md`
- `E Efforts/HubSpot Deals/Millennium IT (Sri Lanka).md`
- `E Efforts/HubSpot Deals/Ministry of Law Upcoming projects.md`
- `E Efforts/HubSpot Deals/Mirimera (2).md`
- `E Efforts/HubSpot Deals/Mirimera.md`
- `E Efforts/HubSpot Deals/Mitrais (2).md`
- `E Efforts/HubSpot Deals/Mitrais.md`
- `E Efforts/HubSpot Deals/Mitsubishi UFJ Financial Group (MUFG).md`
- `E Efforts/HubSpot Deals/MoM - Work Pass Biometrics System.md`
- `E Efforts/HubSpot Deals/Mongolia Digital ID with TOPPAN Security.md`
- `E Efforts/HubSpot Deals/Mozambique Driving Licence with SGS.md`
- `E Efforts/HubSpot Deals/Multimedia Arena Sdn Bhd.md`
- `E Efforts/HubSpot Deals/Muze Innovation Company Limited (2).md`
- `E Efforts/HubSpot Deals/MyData.md`
- `E Efforts/HubSpot Deals/MyZengs.md`
- `E Efforts/HubSpot Deals/NAC Goods Culture Sector Data Analytics Solution.md`
- `E Efforts/HubSpot Deals/NDI Facial Biometrics - Foreigner Enrolment add on.md`
- `E Efforts/HubSpot Deals/NEA - Climate Data Management System (CDMS).md`
- `E Efforts/HubSpot Deals/NEA - Consolidated Application Maintenance Services (AMS3)(NEA000ETT26000073).md`
- `E Efforts/HubSpot Deals/NEA - ELS - SG Clean Sub-module.md`
- `E Efforts/HubSpot Deals/NEA - IES Tech Refresh - Smart Env System (SES).md`
- `E Efforts/HubSpot Deals/NEA - Integrated Field Operation System (iFOS).md`
- `E Efforts/HubSpot Deals/NEA - V3 Data Sharing and Visualisation Portal.md`
- `E Efforts/HubSpot Deals/NEA - Weather Information Notification System (WINS).md`
- `E Efforts/HubSpot Deals/NEA Consolidated Application Maintenance Services (AMS3)(NEA000ETT26000073).md`
- `E Efforts/HubSpot Deals/NEA ELS - SG Clean Sub-module.md`
- `E Efforts/HubSpot Deals/NEA IES Tech Refresh - Smart Env System (SES).md`
- `E Efforts/HubSpot Deals/NEA MSS - World Meteorological Organization (WMO) Information System 2.0.md`
- `E Efforts/HubSpot Deals/NEA OFF-ROAD DIESEL ENGINE (ORDE) Module.md`
- `E Efforts/HubSpot Deals/NEA-Cloud-Based Satellite Data Reception And Processing System.md`
- `E Efforts/HubSpot Deals/NEC Corporation.md`
- `E Efforts/HubSpot Deals/NEMERA Technologies (2).md`
- `E Efforts/HubSpot Deals/NETCUBE (Thailand) Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/NETsolutions Asia Limited (2).md`
- `E Efforts/HubSpot Deals/NETsolutions Asia Limited.md`
- `E Efforts/HubSpot Deals/NEXTWAVE (Thailand) Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/NIPA Technology Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/NIPA Technology Co., Ltd.md`
- `E Efforts/HubSpot Deals/NParks PALS Revamp.md`
- `E Efforts/HubSpot Deals/NTUC AI Use Case.md`
- `E Efforts/HubSpot Deals/NUHS - AI-Enabled Tumour Board Recommendation Engine.md`
- `E Efforts/HubSpot Deals/NUHS CHAMP Clinical Decision Support (CDS) — LDL FH, CKD & Obesity Modules (EAIX-RFQ-2606-0002).md`
- `E Efforts/HubSpot Deals/NUS Standards Development Organisation (SD) Balloting System.md`
- `E Efforts/HubSpot Deals/NYP - AI Consultancy.md`
- `E Efforts/HubSpot Deals/Namibia Wilfred - Revamp of the Judicial Case Management (JCM).md`
- `E Efforts/HubSpot Deals/Nanyang Polytechnic.md`
- `E Efforts/HubSpot Deals/NaradaCode.md`
- `E Efforts/HubSpot Deals/NaviWorld (Thailand) Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/NaviWorld (Thailand) Co., Ltd.md`
- `E Efforts/HubSpot Deals/Nera Telecommunications.md`
- `E Efforts/HubSpot Deals/Nevacloud.md`
- `E Efforts/HubSpot Deals/Neversitup Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Neversitup Co., Ltd.md`
- `E Efforts/HubSpot Deals/New Computer Technology Consulting Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Nexus Corp Group.md`
- `E Efforts/HubSpot Deals/Ngee Ann Polytechnic.md`
- `E Efforts/HubSpot Deals/Nigeria E-Government.md`
- `E Efforts/HubSpot Deals/NiuPay.md`
- `E Efforts/HubSpot Deals/Nodeflux.md`
- `E Efforts/HubSpot Deals/Nore Inovasi.md`
- `E Efforts/HubSpot Deals/Noventiq Malaysia.md`
- `E Efforts/HubSpot Deals/Nusantara Compnet (2).md`
- `E Efforts/HubSpot Deals/Nusantara Compnet.md`
- `E Efforts/HubSpot Deals/OOZOU Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/OOZOU Co., Ltd.md`
- `E Efforts/HubSpot Deals/Omesti.md`
- `E Efforts/HubSpot Deals/One Code Solution.md`
- `E Efforts/HubSpot Deals/OneBiz interface with Brunei ID.md`
- `E Efforts/HubSpot Deals/Online Media Consumption Tool - Ministry of Defense (MOD).md`
- `E Efforts/HubSpot Deals/Open University Malaysia.md`
- `E Efforts/HubSpot Deals/Openwave Computing.md`
- `E Efforts/HubSpot Deals/Opsta (Thailand) Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Opsta (Thailand) Co., Ltd.md`
- `E Efforts/HubSpot Deals/Original Intelligence.md`
- `E Efforts/HubSpot Deals/P.S. Solutions and Consulting Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/PIKOM.md`
- `E Efforts/HubSpot Deals/PMsquare (Thailand) Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/PMsquare (Thailand) Co., Ltd.md`
- `E Efforts/HubSpot Deals/POCDEX Maintenance and Tech Refresh.md`
- `E Efforts/HubSpot Deals/PSD - ITT for Pro-Ration System (PRS) Server, Network and Storage Hardware Maintenance Services (PMOPSDETT26000009).md`
- `E Efforts/HubSpot Deals/PSD - International Relations Information System (IRIS).md`
- `E Efforts/HubSpot Deals/PSD - POCDEX Maintenance with Accenture.md`
- `E Efforts/HubSpot Deals/PSD - POCDEX Maintenance with WPH.md`
- `E Efforts/HubSpot Deals/PSD Pro-Ration System (PRS) 2.0.md`
- `E Efforts/HubSpot Deals/PT Gamatechno Indonesia.md`
- `E Efforts/HubSpot Deals/PT Helios Informatika Nusantara.md`
- `E Efforts/HubSpot Deals/PT IDstar Cipta Teknologi (IDstar).md`
- `E Efforts/HubSpot Deals/PT Jasnita Telekomindo Tbk.md`
- `E Efforts/HubSpot Deals/PT Javan Cipta Solusi.md`
- `E Efforts/HubSpot Deals/PT SISINDOKOM LINTASBUANA - e-Gov Solution Presentation.md`
- `E Efforts/HubSpot Deals/PT Strategic Partner Solution.md`
- `E Efforts/HubSpot Deals/PT. Asia Global Solusi.md`
- `E Efforts/HubSpot Deals/PT. Cipta Inovasi Teknologi.md`
- `E Efforts/HubSpot Deals/PT. Cyberplus Media Pratama.md`
- `E Efforts/HubSpot Deals/PT. IT Group Indonesia.md`
- `E Efforts/HubSpot Deals/PT. Indoguardika Solusi Teknologi.md`
- `E Efforts/HubSpot Deals/PT. Infosys Solusi Terpadu.md`
- `E Efforts/HubSpot Deals/PT. Kayreach System.md`
- `E Efforts/HubSpot Deals/PT. Multipro Jaya Prima.md`
- `E Efforts/HubSpot Deals/PT. Neuronworks Indonesia.md`
- `E Efforts/HubSpot Deals/PT. Nusantara Compnet Integrator (Compnet).md`
- `E Efforts/HubSpot Deals/PT. Pro Sistimatika Automasi (PROSIA).md`
- `E Efforts/HubSpot Deals/PT. Sangkuriang Internasional.md`
- `E Efforts/HubSpot Deals/PT. Trimitra Sukses Indonesia.md`
- `E Efforts/HubSpot Deals/PUB - AMS.md`
- `E Efforts/HubSpot Deals/PUB - EIC engagement.md`
- `E Efforts/HubSpot Deals/PUB - Greasy Waste Tanker Management System.md`
- `E Efforts/HubSpot Deals/Pakistan Saudi Digital ID.md`
- `E Efforts/HubSpot Deals/Panasonic.md`
- `E Efforts/HubSpot Deals/Papua New Guinea Electoral System Digital ID.md`
- `E Efforts/HubSpot Deals/Parliament Budget Amendments Compilation System (BACS).md`
- `E Efforts/HubSpot Deals/Password Solusi Sistem.md`
- `E Efforts/HubSpot Deals/Pentech Solution.md`
- `E Efforts/HubSpot Deals/People's Association OIDC.md`
- `E Efforts/HubSpot Deals/Philippines - Social Security System.md`
- `E Efforts/HubSpot Deals/Philippines Facial Authentication for Social Security System (SSS).md`
- `E Efforts/HubSpot Deals/Phintraco Technology.md`
- `E Efforts/HubSpot Deals/Portalnet Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Portalnet Co., Ltd.md`
- `E Efforts/HubSpot Deals/PowTech.md`
- `E Efforts/HubSpot Deals/PowerGate Software.md`
- `E Efforts/HubSpot Deals/Predictive Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Predictive Co., Ltd.md`
- `E Efforts/HubSpot Deals/Professional Computer Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Professional Computer Co., Ltd.md`
- `E Efforts/HubSpot Deals/Progrez Consulting (Aims Progrez, PT).md`
- `E Efforts/HubSpot Deals/Qinetics Solutions.md`
- `E Efforts/HubSpot Deals/Quocent.md`
- `E Efforts/HubSpot Deals/RFI for Brunei Darussalam National Single Window (BDNSW) Maintenance and Support Services.md`
- `E Efforts/HubSpot Deals/RFI for DriveBN Digitalisation of Land Transport Services.md`
- `E Efforts/HubSpot Deals/RSAF Innovation Office (SWiFT Office) Projects.md`
- `E Efforts/HubSpot Deals/RSAF Kampong Mobile App.md`
- `E Efforts/HubSpot Deals/Radix Berrie.md`
- `E Efforts/HubSpot Deals/Red Cross House E-Cert.md`
- `E Efforts/HubSpot Deals/Red Sky Digital Ventures Ltd (2).md`
- `E Efforts/HubSpot Deals/Redynamics Networks.md`
- `E Efforts/HubSpot Deals/Refactory.md`
- `E Efforts/HubSpot Deals/Revamp of Brunei Land Transport System.md`
- `E Efforts/HubSpot Deals/Ricoh.md`
- `E Efforts/HubSpot Deals/Rikkeisoft.md`
- `E Efforts/HubSpot Deals/RooTs Innovation Pte. Limited.md`
- `E Efforts/HubSpot Deals/Rwanda Collaboration Integration of Embassies of Rwanda and Ministry of Foreign Affairs and International Cooperation (MINAFFET).md`
- `E Efforts/HubSpot Deals/Rwanda Digital ID.md`
- `E Efforts/HubSpot Deals/SAF - Case Management System.md`
- `E Efforts/HubSpot Deals/SB Telecom.md`
- `E Efforts/HubSpot Deals/SBK Digital.md`
- `E Efforts/HubSpot Deals/SCDF Electronic Staging System.md`
- `E Efforts/HubSpot Deals/SCDF eCSR application Development and Maintenance.md`
- `E Efforts/HubSpot Deals/SCE - Mozambique One-Stop Shop.md`
- `E Efforts/HubSpot Deals/SCM Technologies Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/SCM Technologies Co., Ltd.md`
- `E Efforts/HubSpot Deals/SCSi Co., Ltd.md`
- `E Efforts/HubSpot Deals/SDLT Company Limited (2).md`
- `E Efforts/HubSpot Deals/SGX Workflow Engine.md`
- `E Efforts/HubSpot Deals/SIT - SITAR.md`
- `E Efforts/HubSpot Deals/SMU Supply, Implementation and Maintenance of Car Park Online System.md`
- `E Efforts/HubSpot Deals/SRKK Group.md`
- `E Efforts/HubSpot Deals/SSG - SPCP and OpenCerts System Maintenance.md`
- `E Efforts/HubSpot Deals/SSG Singpass Corpass OIDC.md`
- `E Efforts/HubSpot Deals/SSG Training Grant System (TGS) Revamp.md`
- `E Efforts/HubSpot Deals/STelligence Company Limited (2).md`
- `E Efforts/HubSpot Deals/Sabah Pass (Digital ID).md`
- `E Efforts/HubSpot Deals/Safety, Health and Environment National Authority (SHENA) Brunei.md`
- `E Efforts/HubSpot Deals/Sagara Technology.md`
- `E Efforts/HubSpot Deals/Saigon Technology.md`
- `E Efforts/HubSpot Deals/Sand Studio & Co (2).md`
- `E Efforts/HubSpot Deals/Savvycom.md`
- `E Efforts/HubSpot Deals/Scicom.md`
- `E Efforts/HubSpot Deals/Science Centre Digital Transformation - Other projects placeholder.md`
- `E Efforts/HubSpot Deals/Seiko.md`
- `E Efforts/HubSpot Deals/Senna Labs Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Senna Labs Co., Ltd.md`
- `E Efforts/HubSpot Deals/Sentosa Development Corporation (SDC).md`
- `E Efforts/HubSpot Deals/Seven Peaks Software Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Seven Peaks Software Co., Ltd.md`
- `E Efforts/HubSpot Deals/Siam SoftTech Solutions Co., Ltd.md`
- `E Efforts/HubSpot Deals/Silverlake Group.md`
- `E Efforts/HubSpot Deals/Singapore Institute of Technology.md`
- `E Efforts/HubSpot Deals/Singapore International Mediation Institute (SIMI) - IT infrastructure upgrade (NUS Referral).md`
- `E Efforts/HubSpot Deals/Singapore Management University.md`
- `E Efforts/HubSpot Deals/Singapore Pools - PROVISION OF APPLICATION MAINTENANCE SUPPORT SERVICE FOR REMOTE BETTING SYSTEMS FOR 2 YEARS WITH OPTION TO EXTEND ANOTHER 1 YEAR.md`
- `E Efforts/HubSpot Deals/Singapore Pools - Professional Services for Creation of Digital QR Codes for SP Physical and E-Name Card EOI.md`
- `E Efforts/HubSpot Deals/Sirisoft Public Company Limited (2).md`
- `E Efforts/HubSpot Deals/Sirisoft Public Company Limited.md`
- `E Efforts/HubSpot Deals/Skadimo Xperts.md`
- `E Efforts/HubSpot Deals/Skytizens (ThaiTizens Co., Ltd.).md`
- `E Efforts/HubSpot Deals/Skywave Technologies (Thailand) Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Skywave Technologies (Thailand) Co., Ltd.md`
- `E Efforts/HubSpot Deals/SmartData (Bangladesh).md`
- `E Efforts/HubSpot Deals/SmartSoftAsia Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/SmartSoftAsia Co., Ltd.md`
- `E Efforts/HubSpot Deals/Smartclick Solution Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Snappymob.md`
- `E Efforts/HubSpot Deals/Softnix Technology Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Softnix Technology Co., Ltd.md`
- `E Efforts/HubSpot Deals/SoftwareSeni.md`
- `E Efforts/HubSpot Deals/Solusi247.md`
- `E Efforts/HubSpot Deals/Solution for Integrated Gate platform.md`
- `E Efforts/HubSpot Deals/Som Tam Media Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Sotatek.md`
- `E Efforts/HubSpot Deals/South Africa Centre of Excellence.md`
- `E Efforts/HubSpot Deals/South Africa Collaboration with BCX.md`
- `E Efforts/HubSpot Deals/South Africa Local Government Association Digital ID and ABIS.md`
- `E Efforts/HubSpot Deals/Spheresoft Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Spheresoft Co., Ltd.md`
- `E Efforts/HubSpot Deals/Sri Lanka Digital Government.md`
- `E Efforts/HubSpot Deals/Sri Lanka Electronic Government Procurement (eGP) System.md`
- `E Efforts/HubSpot Deals/Sri Lanka Ministry of Justice (MOJ) Court Automation.md`
- `E Efforts/HubSpot Deals/Sri Lanka RFP Citizen-Centric Government Super App with Service Modules.md`
- `E Efforts/HubSpot Deals/Standard Technology Services Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/State Courts - Revamp of Integrated Case Management System (ICMS) with ServiceNow.md`
- `E Efforts/HubSpot Deals/State of Lagos Digital ID.md`
- `E Efforts/HubSpot Deals/Suitmedia.md`
- `E Efforts/HubSpot Deals/Summit Computer Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Summit Computer Co., Ltd.md`
- `E Efforts/HubSpot Deals/Supertype.ai.md`
- `E Efforts/HubSpot Deals/Symentrix IT & Networks Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Symentrix IT & Networks Co., Ltd.md`
- `E Efforts/HubSpot Deals/Syslab Technologies.md`
- `E Efforts/HubSpot Deals/T.C.C. Technology Company Limited (2).md`
- `E Efforts/HubSpot Deals/T.C.C. Technology Company Limited.md`
- `E Efforts/HubSpot Deals/TAC Legacy.md`
- `E Efforts/HubSpot Deals/TOPPAN Gravity Digital Onboarding Platform.md`
- `E Efforts/HubSpot Deals/TOPPAN RDS (Indonesia).md`
- `E Efforts/HubSpot Deals/Tangerine Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Tata Consultancy Services.md`
- `E Efforts/HubSpot Deals/Taube Digital (Steinert Co., Ltd.) (2).md`
- `E Efforts/HubSpot Deals/Taube Digital (Steinert Co., Ltd.).md`
- `E Efforts/HubSpot Deals/Tech Curve AI & Innovations Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Tech Curve AI & Innovations Co., Ltd.md`
- `E Efforts/HubSpot Deals/Tech Mahindra.md`
- `E Efforts/HubSpot Deals/Techies App Technologies.md`
- `E Efforts/HubSpot Deals/Techno9 Indonesia.md`
- `E Efforts/HubSpot Deals/Tekuton Group (2).md`
- `E Efforts/HubSpot Deals/Tekuton Group.md`
- `E Efforts/HubSpot Deals/Tekuton.md`
- `E Efforts/HubSpot Deals/TekyDoct.md`
- `E Efforts/HubSpot Deals/Telecom Fiji Limited.md`
- `E Efforts/HubSpot Deals/Telekom (Malaysia) - RFI for Digital Strategy Roadmap Partner (DSRP).md`
- `E Efforts/HubSpot Deals/Temasek Polytechnic - ITE JPEAE.md`
- `E Efforts/HubSpot Deals/Temasek Polytechnic Contract Review AI Assistant.md`
- `E Efforts/HubSpot Deals/Temasek Polytechnic.md`
- `E Efforts/HubSpot Deals/Tentacle Technologies.md`
- `E Efforts/HubSpot Deals/Thailand DGA Doing Business Consultancy with Infinity.md`
- `E Efforts/HubSpot Deals/Thoughtworks (Thailand) Company Limited (2).md`
- `E Efforts/HubSpot Deals/Thoughtworks (Thailand) Company Limited.md`
- `E Efforts/HubSpot Deals/Tillitsdone (2).md`
- `E Efforts/HubSpot Deals/Tillitsdone.md`
- `E Efforts/HubSpot Deals/Titan System Integration.md`
- `E Efforts/HubSpot Deals/ToffeeDev.md`
- `E Efforts/HubSpot Deals/Togo Ministry of Health.md`
- `E Efforts/HubSpot Deals/Tonga IOM.md`
- `E Efforts/HubSpot Deals/UNN.md`
- `E Efforts/HubSpot Deals/Ultimate Cyber Technology.md`
- `E Efforts/HubSpot Deals/Universiti Brunei Darussalam.md`
- `E Efforts/HubSpot Deals/Universiti Teknologi Brunei.md`
- `E Efforts/HubSpot Deals/Unixdev Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Unixdev Co., Ltd.md`
- `E Efforts/HubSpot Deals/Upstack Studio.md`
- `E Efforts/HubSpot Deals/VenTek International (Thailand) Company Limited (2).md`
- `E Efforts/HubSpot Deals/Vertilogic.md`
- `E Efforts/HubSpot Deals/Vietnam Ministry of Public Security (MPS).md`
- `E Efforts/HubSpot Deals/Vinova.md`
- `E Efforts/HubSpot Deals/Virtual Instrument & System Innovation (VISI).md`
- `E Efforts/HubSpot Deals/Virtual Spirit Technology.md`
- `E Efforts/HubSpot Deals/Vodafone (Fiji).md`
- `E Efforts/HubSpot Deals/WDD.md`
- `E Efforts/HubSpot Deals/WSG - User Access Management & Operation Support BQ.md`
- `E Efforts/HubSpot Deals/Walden Golden Services.md`
- `E Efforts/HubSpot Deals/Web Media South Pacific (Fiji).md`
- `E Efforts/HubSpot Deals/Webby.md`
- `E Efforts/HubSpot Deals/World Information Technology Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Wowrack Indonesia.md`
- `E Efforts/HubSpot Deals/Xapiens Technologi Indonesia.md`
- `E Efforts/HubSpot Deals/Yayasan Brunei.md`
- `E Efforts/HubSpot Deals/Yip In Tsoi & Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Yip In Tsoi & Co., Ltd.md`
- `E Efforts/HubSpot Deals/Your Choice of Worldwide Support.md`
- `E Efforts/HubSpot Deals/Zambia Tourism Authority.md`
- `E Efforts/HubSpot Deals/Zanko.md`
- `E Efforts/HubSpot Deals/Zenith Comp Co., Ltd.md`
- `E Efforts/HubSpot Deals/Zettagrid Indonesia.md`
- `E Efforts/HubSpot Deals/Zimbabwe Investment and Development Authority (ZIDA) Licensing System (2).md`
- `E Efforts/HubSpot Deals/Zimbabwe Investment and Development Authority (ZIDA) Licensing System.md`
- `E Efforts/HubSpot Deals/Zotect Digital Capital Co., Ltd (2).md`
- `E Efforts/HubSpot Deals/Zotect Digital Capital Co., Ltd.md`
- `E Efforts/HubSpot Deals/cmlabs.md`
- `E Efforts/HubSpot Deals/iFAMS - OIDC Migration.md`
- `E Efforts/HubSpot Deals/iPlanet Solution.md`
- `E Efforts/HubSpot Deals/iROSES2 Bi-Modal.md`
- `E Efforts/HubSpot Deals/iSystem Asia.md`
- `E Efforts/HubSpot Deals/iZeno (Thailand) Company Limited (2).md`
- `E Efforts/HubSpot Deals/iZeno (Thailand) Company Limited.md`
- `E Efforts/HubSpot Deals/redONE Mobile.md`
- `E Efforts/Work/Lead Generation and Outreach/Lead Generation and Outreach.md`
