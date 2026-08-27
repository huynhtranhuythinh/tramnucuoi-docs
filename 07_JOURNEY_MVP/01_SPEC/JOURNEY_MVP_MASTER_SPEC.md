# JOURNEY_MVP_MASTER_SPEC

## Purpose

This document is the master specification for the first TRẠM NỤ CƯỜI
Journey MVP implementation.

This file summarizes:

-   product vision;
-   user experience;
-   operational model;
-   technical scope;
-   security rules;
-   implementation priorities.

All Journey MVP development must follow this document together with:

-   COMMUNITY_OS_BLUEPRINT.md
-   JOURNEY_PLATFORM_SPEC.md
-   JOURNEY_DATABASE_ARCHITECTURE.md
-   JOURNEY_SUPABASE_IMPLEMENTATION_PLAN.md

------------------------------------------------------------------------

# Product Vision

Journey MVP transforms a simple trip or event into a living experience.

A Journey creates:

-   relationships;
-   memories;
-   stories;
-   evidence;
-   long-term connection.

Core loop:

Person

↓

Journey

↓

Experience

↓

Memory

↓

Community Relationship

------------------------------------------------------------------------

# MVP Goal

Prove one complete Journey lifecycle:

Before Journey

→ During Journey

→ After Journey

The MVP should allow TNC to operate one real Journey successfully.

------------------------------------------------------------------------

# User Model

## Explorer

Default role.

Can:

-   discover journeys;
-   register;
-   participate;
-   follow updates;
-   receive memories.

------------------------------------------------------------------------

## Contributor

Trusted contributor.

Can:

-   support operations;
-   submit media;
-   contribute stories.

------------------------------------------------------------------------

## Host

Local community role.

Can:

-   support assigned journeys;
-   provide local information.

------------------------------------------------------------------------

## Admin

Can:

-   create journeys;
-   manage participants;
-   publish updates;
-   manage content.

------------------------------------------------------------------------

# Journey Object

A Journey contains:

## Story

Why this journey exists.

## Location

Where the journey happens.

## Schedule

What participants experience.

## Participants

Who joins.

## Live Journey

What happens in real time.

## Memory

What remains after completion.

## Evidence

What documents impact.

------------------------------------------------------------------------

# MVP User Flow

## Explorer

Discover

↓

View Journey

↓

Register

↓

Participate

↓

Follow Live Journey

↓

Receive Memory Archive

------------------------------------------------------------------------

## Admin

Create Journey

↓

Open Registration

↓

Review Participants

↓

Operate Journey

↓

Publish Updates

↓

Archive Journey

------------------------------------------------------------------------

# MVP Features

## 1. Journey CMS

Admin can:

-   create journey;
-   edit bilingual content;
-   assign project;
-   manage status.

------------------------------------------------------------------------

## 2. Registration System

Supports:

-   individual participants;
-   family participants;
-   contributors.

------------------------------------------------------------------------

## 3. Participant Management

Admin can:

-   view participants;
-   approve registrations;
-   manage status.

------------------------------------------------------------------------

## 4. Live Journey

Supports:

-   timeline updates;
-   photos;
-   videos;
-   reflections;
-   comments.

------------------------------------------------------------------------

## 5. Memory Archive

After completion:

Users receive:

-   journey history;
-   photos;
-   videos;
-   stories.

------------------------------------------------------------------------

# Technical Scope

## Reuse Existing Systems

Reuse:

-   Supabase Auth;
-   user_roles;
-   ecosystem_projects;
-   media_assets;
-   CMS patterns.

------------------------------------------------------------------------

## New Journey Domain

Initial entities:

-   journeys;
-   journey_applications;
-   journey_participants;
-   journey_posts;
-   journey_comments;
-   journey_media;
-   memories.

------------------------------------------------------------------------

# Security Principles

## Identity

One user identity.

Multiple roles.

------------------------------------------------------------------------

## Permission

Role does not equal permission.

Use:

Role + Permission + Ownership

------------------------------------------------------------------------

## Access Rules

Public:

-   published journey content.

Explorer:

-   own registrations and memories.

Contributor:

-   approved contributions.

Host:

-   assigned journey access.

Admin:

-   full management.

------------------------------------------------------------------------

# Implementation Milestones

## Milestone 1 --- Foundation

Build:

-   Journey entity;
-   registration;
-   participants;
-   admin management.

------------------------------------------------------------------------

## Milestone 2 --- Live Experience

Build:

-   live feed;
-   media;
-   comments.

------------------------------------------------------------------------

## Milestone 3 --- Memory

Build:

-   archive;
-   participation history.

------------------------------------------------------------------------

## Milestone 4 --- Community Expansion

Build:

-   contribution history;
-   host network;
-   partner experience.

------------------------------------------------------------------------

# Explicitly Out of Scope

Do not build in MVP:

-   general social network;
-   follower system;
-   chat;
-   gamification;
-   mobile app;
-   recommendation engine.

------------------------------------------------------------------------

# QA Criteria

Before release:

## Product

-   Explorer can complete journey flow.
-   Admin can operate journey.

## Technical

-   RLS verified.
-   Bilingual content verified.
-   Media permissions verified.
-   No regression to existing platform.

## Experience

A participant should feel:

"I joined a meaningful journey, and TNC preserved that memory."

------------------------------------------------------------------------

# Final Statement

Journey MVP is the first operational layer of the TRẠM NỤ CƯỜI Living
Community Operating System.

It connects:

People

-   

Places

-   

Experiences

-   

Memories

-   

Evidence

-   

Impact
