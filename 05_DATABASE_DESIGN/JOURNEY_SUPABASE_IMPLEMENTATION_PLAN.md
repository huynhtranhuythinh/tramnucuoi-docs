# Journey Supabase Implementation Plan

## Purpose

This document defines the implementation strategy for adding the TRẠM NỤ CƯỜI Journey Platform into the existing Supabase architecture.

This is an implementation planning document.

It does not authorize immediate migration.

All database changes must follow:

- Existing Supabase architecture
- Existing RLS strategy
- Existing CMS structure
- Community OS Blueprint
- Journey Database Architecture

---

# 1. Implementation Philosophy

## Extend, Do Not Rebuild

The current platform already has:

- Authentication
- User roles
- CMS
- Projects
- Posts
- Media Evidence
- Bilingual content system

Journey should extend this foundation.

Do not create:

- a second user system;
- a second authentication system;
- a separate media library;
- a separate community database.

---

# 2. Existing Schema Reuse

## Authentication

Reuse:

- auth.users

Purpose:

Single identity for every person.

---

## User Roles

Reuse:

- user_roles

Extend role possibilities:

- Explorer
- Contributor
- Host
- Partner
- Admin

---

## Ecosystem Projects

Reuse:

- ecosystem_projects

Relationship:

Project
↓
Journey

Example:

Trúc Sào Xin Chào

contains:

- Journey 001
- Journey 002

---

## Media System

Reuse:

- media_assets

Journey media should connect to existing evidence media.

Do not create a second image/video system.

---

## Editorial System

Reuse:

- posts

Journey completion can generate:

- Field Journal
- Stories
- Documentation

---

# 3. New Journey Tables Proposal

## journeys

Main Journey object.

Purpose:

Represent an experience.

Possible fields:

- id
- project_id
- title
- title_en
- slug
- summary
- summary_en
- location
- start_date
- end_date
- capacity
- status
- created_by
- created_at

---

## journey_applications

Registration requests.

Purpose:

Manage people applying for journeys.

Fields:

- id
- journey_id
- user_id
- application_type
- answers
- status
- reviewed_by
- created_at

---

## journey_participants

Confirmed participants.

Relationship:

User ↔ Journey

Fields:

- id
- journey_id
- user_id
- participant_type
- status
- joined_at

---

## journey_team_members

Operational assignment.

Fields:

- id
- journey_id
- user_id
- operation_role

Examples:

- Journey Leader
- Media
- Contributor
- Host

---

## journey_posts

Live Journey timeline.

Fields:

- id
- journey_id
- author_id
- content
- post_type
- visibility
- created_at

---

## journey_comments

Conversation.

Fields:

- id
- post_id
- user_id
- content
- created_at

---

## journey_media

Connect Journey with Media Evidence.

Fields:

- journey_id
- media_asset_id
- uploaded_by

---

## memories

Personal Journey archive.

Fields:

- id
- user_id
- journey_id
- created_at

---

## contributions

Track contribution history.

Fields:

- id
- user_id
- journey_id
- contribution_type
- description

---

# 4. Relationship Mapping

```text
USER

 |
 |
 +---- ROLES
 |
 |
 +---- JOURNEYS
 |
 |
 +---- CONTRIBUTIONS
 |
 |
 +---- MEMORIES


PROJECT

 |
 |
 +---- JOURNEY

 |
 |
 +---- FIELD JOURNAL

 |
 |
 +---- EVIDENCE
```

---

# 5. Role Permission Strategy

## Explorer

Can:

- view public journeys;
- register;
- follow live journeys;
- comment where allowed.

---

## Contributor

Additional:

- submit media;
- submit stories;
- participate in missions.

---

## Host

Additional:

- manage assigned journey information;
- provide local updates.

---

## Partner

Additional:

- access approved partner information.

---

## Admin

Full management.

---

# 6. RLS Strategy

## Public

Can read:

- published journeys;
- public media;
- public updates.

---

## Participants

Can access:

- registered journeys;
- private journey updates.

---

## Contributors

Can:

- submit contributions;
- upload assigned content.

---

## Hosts

Can:

- manage assigned journeys only.

---

## Admin

Can:

- manage all Journey data.

---

# 7. Migration Strategy

## Phase A — Foundation

Create:

- journeys
- applications
- participants

No Live Feed yet.

---

## Phase B — Live Journey

Add:

- journey_posts
- comments
- media relation

---

## Phase C — Memory

Add:

- memories
- participation history

---

## Phase D — Impact Network

Add:

- partners
- CSR reporting
- advanced evidence

---

# 8. MVP Journey Scope

The first implementation should prove one complete cycle:

Before:

- Journey page
- Registration

During:

- Live updates
- Photos
- Comments

After:

- Memory archive
- Story documentation

---

# 9. Avoid Building Too Early

Do not build initially:

- public social network;
- private chat;
- follower system;
- complex recommendation engine;
- gamification;
- mobile app.

Focus on:

Journey → Experience → Memory → Relationship.

---

# Final Principle

The Journey database should not only store activities.

It should store relationships between:

People

+

Places

+

Experiences

+

Memories

+

Evidence

+

Long-term Impact
