# JOURNEY_MVP_TECHNICAL_PLAN

## Purpose

Define the technical implementation approach for Journey MVP inside the
existing TRẠM NỤ CƯỜI platform.

This document does not authorize immediate migration.

All implementation must respect:

-   existing Supabase architecture;
-   existing RLS;
-   existing CMS;
-   existing bilingual system.

------------------------------------------------------------------------

# Technical Principles

## Extend Existing Platform

Reuse:

-   Authentication;
-   user roles;
-   CMS patterns;
-   Media system;
-   Project system.

Do not create:

-   separate application;
-   separate users;
-   separate media library.

------------------------------------------------------------------------

# Frontend Scope

## Public Routes

/journeys

/journeys/:slug

Features:

-   Journey listing;
-   Journey detail;
-   registration;
-   live updates;
-   memory archive.

------------------------------------------------------------------------

## Admin Routes

/admin/journeys

Features:

-   create Journey;
-   edit content;
-   manage participants;
-   publish updates.

------------------------------------------------------------------------

# Database Extension

Reuse:

-   auth.users
-   user_roles
-   ecosystem_projects
-   media_assets
-   posts

Add future Journey entities:

-   journeys
-   journey_applications
-   journey_participants
-   journey_posts
-   journey_comments
-   journey_media
-   memories

------------------------------------------------------------------------

# Security Strategy

Use:

Role + Permission + Ownership

Explorer:

-   own registration;
-   own memories.

Contributor:

-   approved contribution access.

Host:

-   assigned Journey access.

Admin:

-   full management.

------------------------------------------------------------------------

# Implementation Milestones

## Milestone 1

Foundation:

-   Journey entity;
-   CMS;
-   registration;
-   participants.

------------------------------------------------------------------------

## Milestone 2

Live Journey:

-   timeline;
-   media;
-   comments.

------------------------------------------------------------------------

## Milestone 3

Memory:

-   archive;
-   participant history.

------------------------------------------------------------------------

## Milestone 4

Community expansion:

-   contribution history;
-   Host network;
-   Partner experience.

------------------------------------------------------------------------

# QA Requirements

Before release:

-   bilingual QA;
-   RLS review;
-   regression testing;
-   Owner approval.
