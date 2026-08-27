# JOURNEY_MVP_LOVABLE_BUILD_PROMPT

## Purpose

This document is the official build instruction for implementing TRẠM NỤ
CƯỜI Journey MVP with Lovable.

Before coding, Lovable must read:

-   /tnc-docs/00_INDEX
-   /tnc-docs/01_PRODUCT_VISION
-   /tnc-docs/02_PLATFORM
-   /tnc-docs/03_OPERATIONS
-   /tnc-docs/05_DATABASE_DESIGN
-   /tnc-docs/07_JOURNEY_MVP

------------------------------------------------------------------------

# Mission

Build Journey MVP as an extension of the existing TRẠM NỤ CƯỜI platform.

Do not create a separate application.

Do not rebuild existing systems.

------------------------------------------------------------------------

# Existing Systems To Reuse

Must reuse:

-   Existing React architecture
-   Existing bilingual system
-   Existing CMS patterns
-   Existing Supabase connection
-   Existing Auth
-   Existing RLS principles
-   Existing Media Evidence system
-   Existing Project system

------------------------------------------------------------------------

# Core Product Principle

Journey is not an event calendar.

Journey represents:

People

-   

Places

-   

Experience

-   

Memory

-   

Evidence

------------------------------------------------------------------------

# MVP Scope

## Milestone 1 --- Journey Foundation

Build:

-   Journey data model
-   Journey CMS
-   Public Journey listing
-   Journey detail page
-   Registration flow
-   Participant management

------------------------------------------------------------------------

## Milestone 2 --- Live Journey

Build:

-   timeline updates
-   photo/video updates
-   basic comments

------------------------------------------------------------------------

## Milestone 3 --- Memory

Build:

-   completed Journey archive
-   participant history
-   personal memory access

------------------------------------------------------------------------

# User Roles

## Explorer

Default role.

Can:

-   discover journeys
-   register
-   participate
-   view memories

## Contributor

Additional role.

Can:

-   contribute media
-   support Journey operations

## Host

Local role.

Can:

-   support assigned journeys

## Admin

Can:

-   manage Journey lifecycle

------------------------------------------------------------------------

# Technical Rules

Must not:

-   create second user system
-   create second authentication
-   create second media library
-   change existing RLS without approval
-   apply migrations without review
-   publish without Owner approval

------------------------------------------------------------------------

# Database Rules

Reuse:

-   auth.users
-   user_roles
-   ecosystem_projects
-   media_assets
-   posts

New Journey tables require review before migration.

------------------------------------------------------------------------

# Design Rules

Follow:

-   existing TNC editorial design system
-   documentary visual direction
-   bilingual experience

Avoid:

-   generic travel app design
-   social media clone
-   excessive gamification

------------------------------------------------------------------------

# QA Requirements

Before completion:

-   TypeScript passes
-   Build passes
-   VI/EN verified
-   Responsive verified
-   RLS reviewed
-   No regression to existing platform

------------------------------------------------------------------------

# Final Goal

Create the first working Journey loop:

Discover

↓

Register

↓

Participate

↓

Follow Live Journey

↓

Receive Memory

This is the first operational layer of the TRẠM NỤ CƯỜI Living Community
Operating System.
