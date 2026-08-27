# TNC Phase 3 Current System Audit

## Purpose

This document records the current state of the TRẠM NỤ CƯỜI Web App
before expanding into Community & Journey OS.

The purpose is to identify:

-   what already exists;
-   what can be reused;
-   what needs extension;
-   what should not be rebuilt.

This audit prevents duplicate systems and protects the current
architecture.

------------------------------------------------------------------------

# 1. Current Platform Status

## Overall Assessment

The TNC Web App has moved beyond a traditional website.

Current position:

A bilingual digital story platform with:

-   CMS foundation;
-   editorial storytelling;
-   ecosystem presentation;
-   project documentation;
-   media evidence architecture;
-   partner-facing narrative.

Future direction:

Living Community Operating System.

------------------------------------------------------------------------

# 2. Existing Completed Systems

## Story Platform

Status:

DONE

Includes:

-   About / Về Trạm
-   Ecosystem
-   Project Stories
-   Field Journal
-   Partner narrative

Purpose:

Communicate TNC philosophy and ecosystem.

------------------------------------------------------------------------

## Bilingual System

Status:

DONE

Supports:

-   Vietnamese
-   English

All future modules must maintain bilingual architecture.

------------------------------------------------------------------------

## CMS System

Status:

DONE

Existing capabilities:

-   editable content;
-   bilingual fields;
-   project management;
-   journal management;
-   media management.

Do not create a second CMS.

------------------------------------------------------------------------

## Project System

Status:

DONE

Current entity:

ecosystem_projects

Used for:

-   Trúc Sào Xin Chào
-   Làng Rong Chơi
-   Trạm Nông Sản
-   Wander Bamboo

Future Journey must connect to projects.

------------------------------------------------------------------------

## Media Evidence System

Status:

DONE

Existing:

media_assets

Supports:

-   captions;
-   credits;
-   evidence classification;
-   project relation.

Do not create a second media library.

------------------------------------------------------------------------

## Trust & Security Foundation

Status:

DONE

Includes:

-   Auth;
-   RLS;
-   private security helpers;
-   production safety rules.

Future modules must follow existing security architecture.

------------------------------------------------------------------------

# 3. Existing Systems To Reuse

## User Identity

Current:

Supabase Auth

Future:

Extend into:

-   Explorer
-   Contributor
-   Host
-   Partner
-   Admin

Do not create separate accounts.

------------------------------------------------------------------------

## Roles

Current:

user_roles

Future:

Extend role model.

Principle:

One person + multiple roles.

------------------------------------------------------------------------

## Media

Current:

media_assets

Future Journey:

Reuse for:

-   journey photos;
-   videos;
-   memories;
-   evidence.

------------------------------------------------------------------------

## Stories

Current:

posts

Future:

Journey completion may generate:

-   Field Journal
-   Stories
-   Documentation.

------------------------------------------------------------------------

# 4. Missing Future Modules

## Journey Platform

Status:

NOT BUILT

Needed:

-   journeys;
-   registration;
-   participants;
-   live updates;
-   memory archive.

------------------------------------------------------------------------

## Community Platform

Status:

NOT BUILT

Needed:

-   member identity;
-   contribution history;
-   community interaction.

------------------------------------------------------------------------

## Memory System

Status:

NOT BUILT

Needed:

-   personal journey history;
-   photos;
-   videos;
-   participation archive.

------------------------------------------------------------------------

## Contribution System

Status:

NOT BUILT

Needed:

-   volunteer contribution;
-   skills;
-   recognition.

------------------------------------------------------------------------

# 5. Architecture Decision

## Do Not Build

Avoid creating:

-   second user system;
-   second authentication;
-   separate media storage;
-   separate social network;
-   separate project database.

------------------------------------------------------------------------

# 6. Future Extension Direction

The future architecture:

``` text
Current Platform

Story
Project
Media
CMS

        +

Future Platform

Journey
Community
Memory
Contribution

        ↓

Living Community Operating System
```

------------------------------------------------------------------------

# 7. Journey Module Integration

Journey should connect with:

## Project

Why the Journey exists.

## People

Who participates.

## Media

What happened.

## Story

What was learned.

## Evidence

What can be documented.

------------------------------------------------------------------------

# 8. Development Priority

Recommended order:

## Phase A

Journey Foundation

-   Journey entity
-   Registration
-   Participants

------------------------------------------------------------------------

## Phase B

Live Journey

-   timeline;
-   photos;
-   videos;
-   comments.

------------------------------------------------------------------------

## Phase C

Community Identity

-   profiles;
-   contributions;
-   memory.

------------------------------------------------------------------------

## Phase D

Impact Network

-   partners;
-   CSR;
-   reporting.

------------------------------------------------------------------------

# Final Assessment

The current TNC Web App foundation is strong enough to evolve into a
Living Community Operating System.

The next expansion should not rebuild the platform.

It should add relationship layers:

People

-   

Journeys

-   

Communities

-   

Memories

-   

Evidence
