# JOURNEY_MVP_CODEBASE_AUDIT

## Purpose

This document records the implementation audit before building TRẠM NỤ
CƯỜI Journey MVP.

The objective is to understand:

-   what already exists;
-   what can be reused;
-   what must be extended;
-   what must not be rebuilt.

This audit protects the existing platform architecture.

------------------------------------------------------------------------

# 1. Current Platform Position

The current TNC Web App has evolved from a website into a bilingual
digital story platform.

Existing capabilities:

-   Story Platform
-   Project Platform
-   CMS
-   Media Evidence
-   Authentication
-   Role system
-   Partner narrative
-   Bilingual architecture

Journey should extend this foundation.

------------------------------------------------------------------------

# 2. Frontend Audit Direction

## Reuse

Journey should reuse:

-   existing routing patterns;
-   existing layouts;
-   existing design system;
-   existing bilingual utilities;
-   existing CMS UI patterns.

------------------------------------------------------------------------

## New Frontend Areas

Expected additions:

Public:

-   Journey Listing
-   Journey Detail
-   Registration Flow
-   Live Journey Experience
-   Memory Archive

Admin:

-   Journey Management
-   Participant Management
-   Live Update Management

------------------------------------------------------------------------

# 3. CMS Architecture Decision

## Decision

Journey should NOT be implemented as a simple CMS block.

Reason:

Journey has operational behavior:

-   lifecycle;
-   registration;
-   participants;
-   live updates;
-   memories.

Therefore:

Journey = Domain Entity

Not only Content Block.

------------------------------------------------------------------------

# 4. Supabase Architecture Direction

## Reuse Existing

Reuse:

-   auth.users
-   user_roles
-   ecosystem_projects
-   media_assets
-   posts

------------------------------------------------------------------------

## Future Journey Domain

Expected additions:

-   journeys
-   journey_applications
-   journey_participants
-   journey_posts
-   journey_comments
-   journey_media
-   memories

------------------------------------------------------------------------

# 5. Identity Audit

Current principle:

One person = one identity.

Future extension:

Roles:

-   Explorer
-   Contributor
-   Host
-   Partner
-   Admin

Do not create separate accounts.

------------------------------------------------------------------------

# 6. Media Architecture

Existing Media Evidence system should be reused.

Relationship:

Journey

↓

Media Assets

↓

Memory

↓

Evidence Documentation

------------------------------------------------------------------------

# 7. Risk Assessment

## Risk 1

Creating a separate Journey application.

Decision:

Do not do.

Journey must live inside the current TNC platform.

------------------------------------------------------------------------

## Risk 2

Creating duplicate user systems.

Decision:

Do not do.

Reuse existing authentication and roles.

------------------------------------------------------------------------

## Risk 3

Building social features too early.

Decision:

MVP focuses on:

Journey

Experience

Memory

------------------------------------------------------------------------

# 8. Recommended Milestone 1 Scope

## Admin

Build:

-   Journey CRUD
-   Journey status
-   Project relation
-   Registration management

------------------------------------------------------------------------

## Public

Build:

-   Journey listing
-   Journey detail
-   Registration

------------------------------------------------------------------------

## Not Included

Move to later phases:

-   Live Feed
-   Comments
-   Reactions
-   Memory Archive
-   Advanced Community

------------------------------------------------------------------------

# 9. Final Recommendation

Journey MVP should be implemented as an extension layer:

Current Platform

-   

Journey Domain

=

TRẠM NỤ CƯỜI Living Community Operating System
