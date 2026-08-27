# Journey MVP Codebase Integration Plan

## Purpose

Define how Journey MVP integrates with the existing TRẠM NỤ CƯỜI
application.

------------------------------------------------------------------------

# Reuse Existing

## Frontend

Reuse:

-   routing patterns;
-   components;
-   design system;
-   bilingual utilities.

------------------------------------------------------------------------

## Backend

Reuse:

-   Supabase connection;
-   authentication;
-   RLS strategy;
-   media storage.

------------------------------------------------------------------------

# New Frontend Areas

Possible routes:

/journeys

/journeys/:slug

/admin/journeys

------------------------------------------------------------------------

# New Components

Public:

-   JourneyCard
-   JourneyDetail
-   JourneyTimeline
-   RegistrationForm

Admin:

-   JourneyEditor
-   ParticipantManager
-   LiveUpdateEditor

------------------------------------------------------------------------

# Integration Principle

Journey is an extension layer.

Do not create a separate application.
