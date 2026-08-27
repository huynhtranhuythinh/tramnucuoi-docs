# JOURNEY_MVP_MILESTONE_1_LOVABLE_TASK

## Purpose

This document is the implementation task instruction for Lovable to
build Journey MVP Milestone 1.

Lovable must read all relevant TNC documentation before coding.

------------------------------------------------------------------------

# Required Reading

Read:

-   /tnc-docs/00_INDEX
-   /tnc-docs/01_PRODUCT_VISION
-   /tnc-docs/02_PLATFORM
-   /tnc-docs/03_OPERATIONS
-   /tnc-docs/05_DATABASE_DESIGN
-   /tnc-docs/06_AUDIT
-   /tnc-docs/07_JOURNEY_MVP

Priority files:

1.  JOURNEY_MVP_MASTER_SPEC.md
2.  JOURNEY_MVP_CODEBASE_AUDIT.md
3.  JOURNEY_MVP_MILESTONE_1_IMPLEMENTATION_PLAN.md

------------------------------------------------------------------------

# Mission

Implement Journey MVP Milestone 1 as an extension of the existing TRẠM
NỤ CƯỜI Web App.

Do not create a separate application.

Do not rebuild existing systems.

------------------------------------------------------------------------

# Scope

## Admin Features

Build:

### Journey Management

Admin can:

-   create Journey;
-   edit Journey;
-   save draft;
-   publish;
-   archive.

Fields:

-   title VI;
-   title EN;
-   summary VI;
-   summary EN;
-   cover;
-   related project;
-   location;
-   dates;
-   capacity;
-   status.

------------------------------------------------------------------------

### Registration Management

Admin can:

-   view applications;
-   approve;
-   reject;
-   confirm participants.

------------------------------------------------------------------------

# Public Features

## Journey Listing

Create:

/journeys

Display:

-   upcoming journeys;
-   active journeys;
-   completed journeys.

------------------------------------------------------------------------

## Journey Detail

Create:

/journeys/:slug

Display:

-   story;
-   location;
-   schedule;
-   participation information;
-   registration action.

------------------------------------------------------------------------

# Database Rules

Reuse:

-   existing authentication;
-   existing roles;
-   existing media;
-   existing project system.

Do not create:

-   duplicate user system;
-   duplicate media system.

New tables require review before migration.

------------------------------------------------------------------------

# Design Rules

Follow:

-   existing TNC editorial design system;
-   bilingual architecture;
-   documentary direction.

Avoid:

-   generic travel booking UI;
-   social media clone;
-   unnecessary animations.

------------------------------------------------------------------------

# Security Rules

Maintain:

-   existing RLS principles;
-   role permissions;
-   ownership boundaries.

Do not weaken security for development convenience.

------------------------------------------------------------------------

# Out of Scope

Do not implement:

-   Live Journey Feed;
-   Comments;
-   Reactions;
-   Memory Archive;
-   Contributor submission workflow;
-   Chat;
-   Mobile app.

These belong to later milestones.

------------------------------------------------------------------------

# QA Requirements

Before completion:

Verify:

-   Journey CRUD;
-   bilingual content;
-   registration flow;
-   participant management;
-   responsive design;
-   no regression;
-   build success.

------------------------------------------------------------------------

# Final Requirement

The completed Milestone 1 should allow TNC to run one real Journey:

Create

↓

Publish

↓

Register

↓

Manage Participants

↓

Prepare for Experience

This is the foundation for future Live Journey and Community features.
