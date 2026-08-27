# JOURNEY MVP TECHNICAL PLAN

## Purpose

This document translates the Journey MVP Product Specification into an
implementation plan for the existing TRẠM NỤ CƯỜI Web App.

The goal is to define:

-   frontend scope;
-   admin scope;
-   Supabase extension approach;
-   security strategy;
-   implementation milestones.

This document does not authorize immediate coding or migration.

------------------------------------------------------------------------

# 1. Technical Principles

## Extend Existing Platform

Journey must reuse:

-   Existing Authentication
-   Existing User Roles
-   Existing CMS patterns
-   Existing Media Evidence system
-   Existing Project system

Do not create:

-   separate application;
-   separate user database;
-   separate media storage.

------------------------------------------------------------------------

# 2. Frontend Architecture

## Public Journey Area

New route group:

    /journeys

Purpose:

Show upcoming and active journeys.

------------------------------------------------------------------------

## Journey Listing

Features:

-   upcoming journeys;
-   live journeys;
-   completed journeys.

Information:

-   title;
-   cover image;
-   location;
-   duration;
-   registration status.

------------------------------------------------------------------------

## Journey Detail Page

Contains:

### Story

Why this journey exists.

### Schedule

Timeline.

### Information

-   location;
-   dates;
-   capacity;
-   participation type.

### Registration

Call to action.

### Live Feed

Visible when active.

### Memory

Visible after completion.

------------------------------------------------------------------------

# 3. Admin Architecture

## Journey Management

Admin can:

-   create journey;
-   edit bilingual content;
-   assign project;
-   manage status.

------------------------------------------------------------------------

## Registration Management

Admin can:

-   view applications;
-   approve participants;
-   reject applications;
-   export participant list.

------------------------------------------------------------------------

## Live Journey Management

Admin can:

-   create updates;
-   upload photos;
-   publish reflections.

------------------------------------------------------------------------

# 4. Supabase Extension Strategy

## Reuse Existing Tables

Reuse:

### auth.users

Identity.

### user_roles

Permissions.

### ecosystem_projects

Project relationship.

### media_assets

Images and evidence.

### posts

Future story connection.

------------------------------------------------------------------------

# 5. New Tables MVP

## journeys

Core journey object.

------------------------------------------------------------------------

## journey_applications

Registration workflow.

------------------------------------------------------------------------

## journey_participants

Confirmed participants.

------------------------------------------------------------------------

## journey_posts

Live journey timeline.

------------------------------------------------------------------------

## journey_comments

Community interaction.

------------------------------------------------------------------------

## journey_media

Journey-media relation.

------------------------------------------------------------------------

## memories

Personal archive.

------------------------------------------------------------------------

# 6. RLS Strategy

## Public Users

Can:

-   view published journeys;
-   view public updates.

------------------------------------------------------------------------

## Explorer

Can:

-   manage own registration;
-   access joined journeys.

------------------------------------------------------------------------

## Contributor

Can:

-   submit assigned content;
-   upload approved media.

------------------------------------------------------------------------

## Host

Can:

-   manage assigned journey information.

------------------------------------------------------------------------

## Admin

Full management.

------------------------------------------------------------------------

# 7. Migration Strategy

## Phase 1

Create:

-   journeys;
-   applications;
-   participants.

No public social features.

------------------------------------------------------------------------

## Phase 2

Add:

-   live posts;
-   comments;
-   journey media.

------------------------------------------------------------------------

## Phase 3

Add:

-   memories;
-   participation history.

------------------------------------------------------------------------

# 8. First Build Milestone

The first milestone should deliver:

## Admin

Create one Journey.

## Public

View Journey.

## Explorer

Register.

## Admin

Approve participant.

## Journey

Complete one lifecycle.

------------------------------------------------------------------------

# 9. Not Included in MVP

Avoid:

-   chat;
-   follower system;
-   public profiles;
-   recommendation engine;
-   gamification;
-   mobile app.

------------------------------------------------------------------------

# Final Principle

The Journey MVP should prove the complete relationship loop:

Person

↓

Journey

↓

Experience

↓

Memory

↓

Long-term Connection
