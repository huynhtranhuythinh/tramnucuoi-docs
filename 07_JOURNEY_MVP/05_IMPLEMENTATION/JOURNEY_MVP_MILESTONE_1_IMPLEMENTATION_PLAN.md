# JOURNEY_MVP_MILESTONE_1_IMPLEMENTATION_PLAN

## Purpose

Define the first implementation milestone for TRẠM NỤ CƯỜI Journey MVP.

Milestone 1 focuses on building the minimum operational foundation:

Journey Creation

-   

Registration

-   

Participant Management

This milestone must create a complete but simple Journey lifecycle
before adding realtime social features.

------------------------------------------------------------------------

# Milestone 1 Goal

Enable TNC to create and operate a real Journey.

The system should allow:

Admin: - create Journey; - publish Journey; - receive registrations; -
manage participants.

Explorer: - discover Journey; - register; - receive confirmation.

------------------------------------------------------------------------

# Scope Included

## 1. Journey Entity

Create the core Journey object.

Required information:

-   title VI;
-   title EN;
-   summary VI;
-   summary EN;
-   cover image;
-   related project;
-   location;
-   start date;
-   end date;
-   capacity;
-   status.

------------------------------------------------------------------------

## 2. Journey CMS

Admin capabilities:

-   create Journey;
-   edit bilingual content;
-   save draft;
-   publish;
-   archive.

Status lifecycle:

Draft

↓

Registration Open

↓

Preparing

↓

Completed

↓

Archived

------------------------------------------------------------------------

# 3. Public Journey Pages

## Journey Listing

Route concept:

/journeys

Display:

-   upcoming journeys;
-   registration status;
-   location;
-   duration;
-   cover image.

------------------------------------------------------------------------

## Journey Detail

Contains:

-   story;
-   schedule;
-   location;
-   participation information;
-   registration button.

------------------------------------------------------------------------

# 4. Registration System

Explorer can submit:

-   personal information;
-   participant type;
-   contact information;
-   notes.

Participant types:

-   Individual Explorer;
-   Family;
-   Contributor;
-   Partner Representative.

------------------------------------------------------------------------

# 5. Admin Participant Management

Admin can:

-   view applications;
-   approve;
-   reject;
-   confirm participants.

Participant status:

Submitted

↓

Reviewing

↓

Accepted

↓

Confirmed

------------------------------------------------------------------------

# 6. Database Scope

Initial tables:

## journeys

Core Journey.

## journey_applications

Registration requests.

## journey_participants

Confirmed participation.

No Live Feed tables in Milestone 1.

------------------------------------------------------------------------

# 7. Security Scope

Apply:

Role + Permission + Ownership

Rules:

Public: - view published journeys.

Explorer: - manage own application.

Admin: - manage all Journey data.

------------------------------------------------------------------------

# 8. Frontend Scope

New pages:

Public:

-   Journey Listing
-   Journey Detail

Admin:

-   Journey Editor
-   Participant Manager

Reuse:

-   existing layout;
-   existing design system;
-   existing bilingual components.

------------------------------------------------------------------------

# 9. Not Included

Move to later milestones:

-   Live Journey feed;
-   comments;
-   reactions;
-   realtime updates;
-   memory archive;
-   contributor submission system.

------------------------------------------------------------------------

# 10. QA Checklist

Before completion:

-   Journey CRUD works;
-   bilingual content works;
-   registration works;
-   participant workflow works;
-   RLS verified;
-   responsive verified;
-   existing website regression checked.

------------------------------------------------------------------------

# Final Principle

Milestone 1 proves:

A real TNC Journey can be created, opened, joined and managed digitally.

After this foundation works, TNC can add:

Live Journey

↓

Memory

↓

Community Network
