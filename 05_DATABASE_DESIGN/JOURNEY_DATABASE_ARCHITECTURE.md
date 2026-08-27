# Journey Database Architecture

## Purpose

This document defines the future database architecture for the TRẠM NỤ
CƯỜI Journey Platform.

It is a conceptual architecture document.

It does not represent immediate database migration.

Any implementation must respect: - Existing Supabase architecture -
Existing RLS security model - Existing CMS structure - Community OS
Blueprint - User Identity Role Model

------------------------------------------------------------------------

# Core Data Philosophy

## One Person --- Multiple Roles

TNC does not create separate accounts.

A person has:

User Identity + Roles + Permissions

------------------------------------------------------------------------

# Core Entity Model

PERSON

-   Roles
-   Journeys
-   Contributions
-   Memories
-   Stories

JOURNEY

-   Project
-   Participants
-   Live Updates
-   Media
-   Discussion
-   Evidence
-   Archive

------------------------------------------------------------------------

# User Identity Layer

## users

Stores the core identity of every person entering the TNC ecosystem.

Possible fields:

-   id
-   full_name
-   avatar
-   email
-   phone
-   location
-   bio
-   created_at

------------------------------------------------------------------------

# Role System

## user_roles

Supports multi-role identity.

Possible roles:

-   Explorer
-   Contributor
-   Host
-   Partner
-   Admin

------------------------------------------------------------------------

# Journey Core Layer

## journeys

The central experience object.

A Journey represents:

-   place
-   people
-   experience
-   story
-   impact

Possible fields:

-   id
-   title
-   title_en
-   slug
-   summary
-   summary_en
-   project_id
-   location
-   start_date
-   end_date
-   status
-   capacity
-   created_by

------------------------------------------------------------------------

# Journey Lifecycle

Draft

↓

Registration Open

↓

Preparing

↓

Live

↓

Completed

↓

Archived

------------------------------------------------------------------------

# Journey Participants

## journey_participants

Relationship:

Person ↔ Journey

Stores participation history.

------------------------------------------------------------------------

# Journey Team

## journey_team_members

Operational assignment:

-   Journey Leader
-   Media Team
-   Contributor Team
-   Host Team

------------------------------------------------------------------------

# Registration

## journey_applications

Handles public applications.

Status:

-   Submitted
-   Reviewing
-   Accepted
-   Rejected

------------------------------------------------------------------------

# Live Journey Layer

## journey_posts

Realtime storytelling:

-   updates
-   photos
-   videos
-   reflections

------------------------------------------------------------------------

## journey_comments

Community conversation.

------------------------------------------------------------------------

## journey_reactions

Simple emotional interaction.

------------------------------------------------------------------------

# Media Relationship

## journey_media

Connects journeys with existing Media Evidence.

------------------------------------------------------------------------

# Memory System

## memories

Personal archive after participation.

Example:

A family can revisit:

-   completed journeys
-   photos
-   videos
-   stories

------------------------------------------------------------------------

# Contribution Tracking

## contributions

Records:

-   volunteer time
-   skills
-   media
-   knowledge
-   resources

------------------------------------------------------------------------

# Evidence Connection

Journey connects:

Journey

↓

Media Evidence

↓

Field Journal

↓

Project Evidence

------------------------------------------------------------------------

# Permission Model

Do not use role-only permission.

Use:

Role + Permission + Ownership

Examples:

Contributor: - submit media - submit stories

Host: - manage assigned journeys

Admin: - full access

------------------------------------------------------------------------

# Implementation Priority

## Phase 1

Journey Foundation: - journeys - participants - applications - roles

## Phase 2

Live Journey: - posts - comments - media

## Phase 3

Memory: - personal archive - participation history

## Phase 4

Impact Network: - partners - CSR dashboard - reporting

------------------------------------------------------------------------

# Final Architecture Principle

The Journey Database should not only store events.

It should store relationships.

A Journey connects:

People + Places + Experiences + Memories + Evidence + Long-term Impact
