# JOURNEY MVP PRODUCT SPEC

## Purpose

This document defines the minimum viable product specification for the
first TRẠM NỤ CƯỜI Journey implementation.

The objective is not to build a complete social platform.

The objective is to prove one complete Journey lifecycle:

Before Journey → During Journey → After Journey

while respecting the existing TNC architecture.

------------------------------------------------------------------------

# Product Goal

Create a digital experience where people can:

-   discover meaningful journeys;
-   register to participate;
-   experience real-time updates;
-   receive personal memories after completion.

------------------------------------------------------------------------

# MVP Principle

Build the smallest complete loop.

Do not build:

-   full social network;
-   chat system;
-   follower system;
-   gamification;
-   mobile application.

Focus on:

Journey → Experience → Memory.

------------------------------------------------------------------------

# User Types

## Explorer

Default participant.

Can:

-   view upcoming journeys;
-   register;
-   receive journey updates;
-   view completed memories.

------------------------------------------------------------------------

## Contributor

Additional trusted role.

Can:

-   support journey operations;
-   submit media;
-   contribute stories.

------------------------------------------------------------------------

## Host

Local experience provider.

Can:

-   support assigned journey;
-   provide local information.

------------------------------------------------------------------------

## Admin

Can:

-   create journeys;
-   manage registrations;
-   publish updates;
-   manage participants.

------------------------------------------------------------------------

# Explorer Flow

## Step 1 --- Discover

User sees:

-   Journey story;
-   destination;
-   schedule;
-   cost;
-   availability.

------------------------------------------------------------------------

## Step 2 --- Register

User submits:

-   personal information;
-   participant type;
-   special needs;
-   preferences.

------------------------------------------------------------------------

## Step 3 --- Participate

During Journey:

User can:

-   follow updates;
-   view photos/videos;
-   interact.

------------------------------------------------------------------------

## Step 4 --- Memory

After completion:

User receives:

-   journey history;
-   photos;
-   videos;
-   story archive.

------------------------------------------------------------------------

# Admin Flow

## Create Journey

Admin creates:

-   title;
-   story;
-   location;
-   dates;
-   capacity;
-   project relation.

------------------------------------------------------------------------

## Open Registration

Admin manages:

-   applicants;
-   participant list;
-   confirmation.

------------------------------------------------------------------------

## Live Operation

Admin publishes:

-   updates;
-   photos;
-   videos;
-   reflections.

------------------------------------------------------------------------

## Completion

Admin closes Journey:

-   archive media;
-   create memory;
-   connect story/evidence.

------------------------------------------------------------------------

# MVP Modules

## 1. Journey CMS

Required:

-   create/edit Journey;
-   bilingual content;
-   status management.

------------------------------------------------------------------------

## 2. Registration

Required:

-   application form;
-   participant records;
-   approval workflow.

------------------------------------------------------------------------

## 3. Participant Management

Required:

-   participant list;
-   role/type;
-   status.

------------------------------------------------------------------------

## 4. Live Journey Feed

Required:

-   timeline updates;
-   photos;
-   videos;
-   comments (basic).

------------------------------------------------------------------------

## 5. Memory Archive

Required:

-   completed journey page;
-   participant media collection.

------------------------------------------------------------------------

# MVP Database Scope

Initial tables:

-   journeys
-   journey_applications
-   journey_participants
-   journey_posts
-   journey_media
-   memories

Reuse:

-   auth.users
-   user_roles
-   ecosystem_projects
-   media_assets
-   posts

------------------------------------------------------------------------

# MVP Success Criteria

A successful MVP means:

A family can:

1.  Find a Journey.
2.  Register.
3.  Participate.
4.  Follow live updates.
5.  Receive memories.

TNC can:

1.  Create Journey.
2.  Manage participants.
3.  Publish updates.
4.  Archive the experience.

------------------------------------------------------------------------

# Future Expansion

After MVP:

Phase 2: - reactions; - richer comments; - contributor submissions.

Phase 3: - personal profiles; - contribution history; - recognition.

Phase 4: - partner dashboard; - CSR reporting; - impact network.

------------------------------------------------------------------------

# Final Product Statement

The first TNC Journey MVP should prove one thing:

A journey with TRẠM NỤ CƯỜI is not just a trip.

It is an experience that creates relationships, memories and long-term
connection.
