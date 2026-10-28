# AI Tutor Matching Domain

## Status

**Confirmed MVP capability:** Authenticated Tutor Seekers receive tutor recommendations through a conversation with AI.

No AI matching implementation, provider, model, prompt, or ranking algorithm is confirmed by this page.

## Confirmed interaction model

- Tutor matching uses a conversational AI experience rather than a fixed recommendation form.
- The Tutor Seeker describes the tutoring need in natural language.
- AI asks follow-up questions when information needed for matching is missing or unclear.
- AI recommends only Tutor profiles in `APPROVED` status.
- Recommendations may use only information the system is authorized to process.
- AI recommendations support the user's decision and do not automatically create a connection request.

## Confirmed matching information

The conversation collects the following information to understand and rank suitable options:

- Subject
- Grade level or current proficiency
- Learning goal
- Online or in-person teaching mode
- Location or teaching area when the mode is in-person
- Tutoring-rate budget range
- Learner availability

AI asks follow-up questions for missing or unclear information when doing so would materially improve the recommendations. Location is not needed when the confirmed teaching mode is online-only.

These values guide ranking and do not require every recommendation to match every preference exactly. AI should optimize for overall suitability and may relax one or more preferences when useful. A preference becomes a strict constraint only when the Tutor Seeker explicitly states that it is mandatory.

## Confirmed recommendation count

- AI targets 5 Tutor recommendations per recommendation turn.
- The 5 results are the most suitable available options based on the full conversation; they do not need to satisfy every preference completely.
- AI may return fewer than 5 only when fewer than 5 eligible, administrator-approved Tutor profiles are available.
- AI must clearly identify material differences between a recommendation and the Tutor Seeker's stated preferences.

## Confirmed recommendation explanation

Each recommended Tutor must include:

- A concise explanation of why the Tutor is suitable
- The main preferences the Tutor satisfies
- Material preferences that are not fully satisfied
- Whether the listed tutoring rate and availability align with the stated need
- A link or action to view the full Tutor profile

The MVP does not display a numeric AI match score. A score may be introduced only after its formula, interpretation, and evaluation method are approved and documented.

## Confirmed connection-history boundary

- AI does not use the Tutor Seeker's connection history by default.
- AI may use that account's connection history only when the Tutor Seeker explicitly requests it in the current conversation.
- A general request for recommendations does not count as permission to use connection history.
- AI must continue to work when no history exists or when history use is not requested.

## Confirmed review usage

- AI may use published ratings and public review content as recommendation signals.
- Ratings and reviews must not be the sole basis for recommending or excluding a Tutor.
- A Tutor with no reviews or few reviews must not be treated as unsuitable solely because review evidence is limited.
- Unpublished, removed, reported, or moderation-only review information must not influence recommendations.

## Confirmed ranking priorities

AI considers recommendation signals in this priority order:

1. Subject, current level, and learning goal
2. Online/in-person teaching mode and location
3. Tutor and learner availability
4. Tutoring-rate budget
5. Teaching experience, education, and certificates
6. Published ratings and reviews

This priority order guides AI reasoning; it is not a fixed numeric scoring formula. AI may balance trade-offs to produce 5 suitable options, but each recommendation must explain material matches and differences.

## Confirmed conversation persistence

- AI conversation history is not persisted in the MVP.
- Conversation context may be held temporarily only while the current interaction is active so AI can ask and understand follow-up questions.
- After the interaction ends, the Tutor Seeker cannot reopen the conversation or its previous recommendation results.
- The MVP does not provide saved AI conversations, conversation search, or recommendation-history screens.
- Tutor connection history remains persisted and is separate from AI conversation history.

## Confirmed insufficient-result behavior

- AI returns the eligible recommendations currently available, even when the result contains fewer than 5 Tutors or no Tutors.
- The MVP does not require an additional explanation, automatic preference-relaxation conversation, or redirect to standard search for an insufficient result.

## To be defined

- Tie-breaking behavior for otherwise comparable Tutors
- Safety rules and prohibited advice
- User feedback and AI-quality evaluation
- Provider, model, prompt versioning, cost controls, and fallback behavior

## Related domains

- [User](user.md)
- [Tutor](tutor.md)
- [Connection request](application.md)
- [Tutor connection and learning history](session.md)

