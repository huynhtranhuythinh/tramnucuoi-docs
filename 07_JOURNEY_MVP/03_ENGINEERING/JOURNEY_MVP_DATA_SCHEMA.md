# Journey MVP Data Schema

## Purpose

Define the first database-level model for Journey MVP.

The schema extends existing TNC systems.

Reuse: - auth.users - user_roles - ecosystem_projects - media_assets -
posts

New Journey domain tables:

## journeys

Core journey entity.

Fields: - id - project_id - title - title_en - slug - summary -
summary_en - location - start_date - end_date - status - capacity -
created_by

## journey_applications

Registration requests.

## journey_participants

Confirmed participants.

## journey_posts

Live Journey updates.

## journey_comments

Conversation.

## journey_media

Journey and media relation.

## memories

Personal archive.

Implementation must preserve existing RLS principles.
