# Product Requirements Document (PRD)
## AI-Powered Productivity Operating System for Engineering Students (Codename: "Kensei")

**Document Owner:** Senior PM / Senior Software Architect
**Version:** 2.0 (Finalized — incorporates Head of Product engineering-readiness review)
**Status:** Ready for Engineering Scoping
**Audience:** Engineering, Design, Data Science, Growth, Leadership

**Changelog from v1.0 → v2.0:**
- Resolved terminology inconsistencies (Goal/Domain, Milestone, Task/Block/Session, Habit, Priority, Roadmap scope).
- Added Section 6A: Core Entities (conceptual data model) — the Event, Priority, Goal, Version, Integration, and Consent entities that were previously implied but undefined.
- Replaced flat Section 11 phasing with feature-level MVP/V1/V2 splits for the six features that bundled multiple maturity levels under one heading.
- Added Section 13: Missing User Flows (auth, subscription, lapsed-user return, disconnect, escalation, export/deletion, goal completion).
- Added Section 14: Cross-Cutting Edge Cases (previously scattered or absent — AI-system conflicts, service degradation, non-CS branches, repeat exam cycles, accessibility, locale, integrity-at-scale).
- Added Section 15: Architectural Risks Originating from Product Decisions (distinct from implementation risk — these are consequences of choices made in this document).
- Added Section 16: World-Class Design Principles Addendum.

---

## 1. Executive Summary

Engineering students preparing for competitive exams like GATE face a uniquely fragmented life: they must simultaneously manage academic coursework, GATE preparation, DSA practice, personal projects, research, cybersecurity learning, certifications, portfolio building, resume iteration, and placement prep — typically spread across 8-10 disconnected tools (Notion, Google Calendar, LeetCode, GitHub, WhatsApp groups, YouTube playlists, physical planners, and spreadsheets).

**Kensei** is an AI-native productivity operating system that unifies goal-setting, planning, execution, and reflection into a single adaptive system. Unlike generic productivity tools, Kensei understands the *domain* — it knows what GATE preparation requires, what a strong DSA sheet looks like, what recruiters expect in a resume, and how to sequence a student's finite time across all these competing priorities.

The system is built around a core loop: **Goal → AI-Generated Roadmap → AI-Generated Timetable → Daily Execution (Planner + Pomodoro) → Tracking (XP/Streaks/Habits) → AI Feedback (Mentor/Teacher) → Re-planning (Scheduler)**, with GitHub/LeetCode integrations providing objective, tamper-proof signals of real progress, and Analytics closing the loop with insight.

---

## 2. Problem Statement

Engineering students preparing for GATE and placements are expected to perform well across too many uncorrelated domains at once, with no single system that:
1. Understands the interdependencies between their goals (e.g., DSA practice serves both GATE and placements; a project serves both portfolio and resume).
2. Converts long-term goals into realistic, personalized day-to-day action.
3. Adapts plans when reality diverges from the plan (missed days, harder-than-expected topics, exam date changes).
4. Provides objective evidence of progress rather than relying on self-reported completion.
5. Sustains motivation over the 6-18 month horizons these goals typically require.

The result: over-planning in Notion templates that are abandoned within two weeks, chronic under-preparation in weaker subjects, and burnout from lack of visible progress.

---

## 3. Goals & Objectives

### 3.1 Product Goals
- G1: Reduce the time-to-first-plan for a new user from "hours of manual setup" to under 10 minutes via the Goal Setup Wizard.
- G2: Provide a living roadmap and timetable that adapts weekly based on actual behavior, not just initial intent.
- G3: Make daily execution frictionless (single daily planner view, one-tap Pomodoro, auto-logged progress).
- G4: Drive sustained engagement over 6-18 month exam cycles via XP, streaks, and habit loops without becoming gamification-for-its-own-sake.
- G5: Provide objective, verifiable skill signals via GitHub and LeetCode integration rather than self-reported task completion.
- G6: Give students (and eventually parents/mentors/coaches) clear analytics on trajectory vs. goal.

### 3.2 Business Goals
- Establish category leadership in "AI OS for competitive-exam engineering students," a large underserved market (GATE alone: ~1M+ annual applicants in India, plus adjacent placement-prep audience of every engineering undergrad).
- Build a defensible moat through proprietary progress data (GitHub/LeetCode/study behavior) that improves AI roadmap quality over time.
- Create a natural upsell path: free tier for planning primitives → paid tier for AI roadmap/mentor depth, integrations, and advanced analytics.

### 3.3 Non-Goals (v1)
- Not a course/content platform (no video hosting, no proprietary question bank) — Kensei orchestrates and tracks, it does not replace GATE coaching content providers.
- Not a social network (no public feeds, though light peer accountability may exist).
- Not a full LMS or college ERP replacement.

---

## 4. Target Users & Personas

### Persona 1: "Ananya" — The GATE Aspirant (Primary)
- 3rd/4th-year engineering student or recent graduate, preparing for GATE while still attending college.
- Juggles 6 college subjects, GATE syllabus (partially overlapping), and increasing placement pressure.
- Pain: doesn't know how to allocate hours across overlapping-but-distinct syllabi; anxious about falling behind.

### Persona 2: "Rohit" — The Placement-Focused Builder
- Final-year student targeting product-based company SDE roles.
- Focused on DSA, system design basics, projects, and resume/portfolio polish; GATE is secondary or not a goal.
- Pain: scattered LeetCode grinding without a strategy; project work stalls; resume never feels "ready."

### Persona 3: "Priya" — The Research-Track Student
- Interested in an MTech/PhD path via GATE, also doing an undergrad research project with a professor.
- Pain: research timelines are unstructured and easy to deprioritize under exam pressure; needs to balance deep, unstructured research work with structured GATE study blocks.

### Persona 4: "Karan" — The Security/Certifications Track
- Building toward a cybersecurity career; pursuing certifications (CEH, Security+, etc.) alongside coursework.
- Pain: certification prep, CTF practice, and coursework compete for the same hours with no unified tracker.

### Secondary Users
- **Mentors/Coaches** (v2+): may get a read-only or coaching view into a student's plan and analytics with consent.
- **Parents** (v2+, optional, opt-in): high-level progress visibility for peace of mind, common in the Indian ed-tech context.

---

## 5. Product Principles

1. **One system of record, not another silo.** Kensei should reduce the number of tools a student juggles, not add one more.
2. **AI plans, human decides.** Every AI-generated roadmap/timetable/schedule is editable and explainable — never a black box the student must blindly follow.
3. **Evidence over self-report.** Where possible (code, DSA), verify progress via integrations rather than trusting checkboxes alone.
4. **Adaptive, not rigid.** The system should expect missed days and re-plan gracefully rather than shaming or breaking the streak logic irrecoverably.
5. **Motivation is a feature, not a gimmick.** Gamification (XP/streaks) must be tied to meaningful progress signals, not vanity actions, to avoid Goodhart's Law failure modes.

---

## 6. High-Level System Architecture (Conceptual, No Code)

**Core Layers:**
1. **Data Layer** — unified "Goal Graph": Goals → Domains (GATE, College, Projects, Research, DSA, Cybersecurity, Certifications, Portfolio, Resume, Placements) → Milestones → Tasks → Sessions/Logs. All features read/write against this shared graph rather than maintaining separate silos.
2. **AI Planning Layer** — three cooperating AI services:
   - *Roadmap Generator* (long-horizon, milestone-level, weeks/months)
   - *Timetable Generator* (medium-horizon, weekly recurring structure)
   - *Scheduler* (short-horizon, daily/real-time re-arrangement)
   These share context so they don't contradict each other.
3. **Execution Layer** — Daily Planner, Calendar, Pomodoro Timer, Habit Tracker — the surfaces where students actually do work and log completion.
4. **Motivation Layer** — XP System, Streak System — cross-cutting systems that listen to events from every other feature.
5. **Intelligence/Coaching Layer** — AI Mentor (motivational/strategic coaching) and AI Teacher (subject-matter tutoring), both grounded in the student's actual Goal Graph and performance data.
6. **Integration Layer** — GitHub, LeetCode (and future: Coursera/Udemy, college LMS, job portals).
7. **Insight Layer** — Analytics, Notifications — surface signal back to the student (and re-feed into the AI Planning Layer as adaptation triggers).

**Key Architectural Decision:** All features must emit and consume events from a shared **Progress Event Bus**, backed by a first-class **Event** entity (see Section 6A) — not a narrative concept but a concrete, append-only, versioned log every feature reads and writes against (e.g., "task_completed," "pomodoro_finished," "commit_pushed," "leetcode_solved," "habit_checked_in"). This is what allows XP, Streaks, Analytics, the Scheduler, and the AI Mentor to all stay in sync without duplicated logic. This is the single most important architectural decision in the product — without it, each feature becomes an isolated silo again, recreating the exact problem Kensei is meant to solve.

**Planning-Horizon Arbitration:** Roadmap, Timetable, and Scheduler are three independent AI planners operating on three different time horizons. Because they can legitimately disagree (e.g., Roadmap wants to re-insert a skipped prerequisite the same week Scheduler wants to lighten load after a missed day), the system defines an explicit precedence rule: **Scheduler may only rearrange within a horizon already approved by Timetable/Roadmap; it cannot silently override a Roadmap-level decision.** Any conflict between planners is surfaced to the student as a single, merged suggestion — never as two contradictory nudges. This precedence rule is a product decision, not an implementation detail, and must be treated as such by design and engineering alike.

**Terminology Standard (binding for all sections below):**
- **Goal** is the single first-class entity a student creates (e.g., "Clear GATE CSE 2027," "Land an SDE-1 role"). **Domain** is a category tag on a Goal (GATE, College, DSA, Cybersecurity, etc.), not a separate entity — a Goal always has exactly one primary Domain and may reference others.
- **Roadmap Milestone** (strategic, weeks-level, lives under a Goal) is distinct from **Project Milestone** (tactical, lives under a Project, see 7.16). They are never referred to interchangeably as "milestone" without a qualifier from this point forward.
- **Time Block** is what the Timetable produces (a scheduled slot on the weekly grid). A Time Block contains one or more **Tasks**. Executing a Task via the Pomodoro Timer produces a **Session** (a timed, loggable unit of work). A **Habit** is a special recurring Task type with its own check-in and streak logic — it is not a parallel structure to Tasks; it renders inside the same Daily Planner list.
- **Priority** is a single canonical ranking set once in the Goal Setup Wizard and stored on the Goal entity. Roadmap, Scheduler, and Mentor all read this same value — none of them maintain an independent notion of priority.
- **"Roadmap"** refers to one Roadmap per student, composed of per-Goal Milestone tracks. There is no such thing as "a roadmap" per domain in isolation — Section 7.2 below has been corrected to reflect this.

---

## 6A. Core Entities (Conceptual Data Model)

This is a conceptual entity list, not a schema — it exists so engineering can scope data modeling work accurately and so every feature section below refers to the same underlying concepts. Fields and relationships are intentionally left to engineering design.

- **User** — account, auth, profile, device list, subscription tier, account lifecycle state (Active / Paused / Goal-Achieved / Dormant / Deleted).
- **Goal** — the first-class student objective (see Terminology Standard above); holds Priority, target date, Domain tag(s), and lifecycle state (active/paused/achieved/archived).
- **Roadmap Milestone** — a strategic checkpoint under a Goal, with a due window, dependency links to other milestones, and a status (on-track/behind/at-risk).
- **Time Block** — a scheduled slot produced by the Timetable, referencing one or more Tasks.
- **Task** — the atomic unit of planned work, appearing in Time Blocks and the Daily Planner; includes Habit as a subtype.
- **Session** — a timed execution record produced by the Pomodoro Timer against a Task.
- **Event** — the append-only record on the Progress Event Bus (source feature, type, timestamp, payload, related entity references). Every XP, Streak, Analytics, and Scheduler computation derives from this log, never from feature-local state.
- **XP Ledger Entry** — a transactional record (not a running total) so awards and clawbacks are auditable; XP totals are a derived view over the ledger.
- **Streak** — per-Domain and global, with an associated **Streak Freeze** inventory (countable, earned/spent resource).
- **Habit** — recurring Task subtype with its own check-in history.
- **Reflection** — end-of-day journal entry, referenced by the AI Mentor as conversational context.
- **Project** — container for Project Milestones, notes/decisions log, and linked Integration activity.
- **Integration / Connection** — one per external service (GitHub, LeetCode), storing auth state, sync status, last-synced timestamp, and granted scopes.
- **Resource** — uploaded/linked study material, tagged to Goal/Domain/Topic.
- **Certification** — exam-based credential pursuit with its own target date and validity/renewal window.
- **Exam** — a distinct entity from generic deadlines (registration status, admit card, attempt number, center) — a Goal may reference one or more Exams (relevant for repeat-attempt cycles).
- **Institution / Academic Calendar** — semester structure, exam windows, holidays; referenced by Timetable/Calendar collision logic.
- **Version / Revision** — applies to Roadmap, Timetable, and Resume; every regeneration creates a new Version rather than overwriting, enabling the Plan Changelog (Section 16).
- **Consent / Permission Grant** — scope, grantee (parent/mentor/coach), grant/revoke timestamps; required before any cross-user visibility (Analytics sharing, Portfolio sharing) is enabled.
- **Notification Preference** — channel, category-level opt-in/out, quiet hours.

---

## 7. Feature Specifications

Each feature below follows: **Purpose → User Flow → Edge Cases → Possible Improvements → Future Scope.**

---

### 7.1 Goal Setup Wizard

**Purpose**
The entry point to the entire system. Converts a student's vague intent ("I want to prepare for GATE and also get placed") into a structured, prioritized, time-bound Goal Graph that every other feature depends on. This is the single highest-leverage feature in the product — a bad goal-capture leads to a bad roadmap, bad timetable, and bad AI mentor advice downstream.

**User Flow**
1. Student signs up and is prompted: "What are you working toward?" — multi-select from Exam Prep (GATE, others), Placements, College, Research, Certifications, Personal Projects, Skill-building (DSA, Cybersecurity), Portfolio/Resume.
2. For each selected domain, a short structured intake: target date (e.g., GATE exam date), current proficiency self-assessment, prior coursework/branch, weekly hours realistically available, and any hard constraints (college exam schedule, internship, part-time job).
3. Wizard asks the student to **rank priorities** when domains conflict (e.g., "If GATE prep and a college deadline collide, which wins by default?").
4. Wizard surfaces overlaps automatically (e.g., "DSA practice supports both GATE and Placements — we'll count it toward both goals to avoid double work").
5. Student reviews a generated **Goal Summary Card** per domain, edits if needed, and confirms.
6. System hands off directly into AI Roadmap Generation (7.2) using this structured data — no manual re-entry.

**Edge Cases**
- Student has no fixed exam date yet (GATE registration not open) → allow provisional/placeholder dates with a reminder to update later.
- Student selects domains that are mutually exclusive under their stated timeline (e.g., 10 hrs/week total but 8 domains selected) → wizard should flag over-commitment before proceeding, not silently generate an impossible plan.
- Returning student changing goals mid-year (e.g., dropping GATE to focus on placements) → wizard must support **goal editing/archival**, not just first-time creation, and must gracefully re-flow existing progress data (don't discard historical XP/streaks).
- Student is a first-year with vague, exploratory goals rather than concrete ones → wizard should offer a lighter-weight "Explorer" mode that doesn't force premature specificity.
- Conflicting priority rankings (student says "everything is top priority") → wizard should force a relative ranking via pairwise comparison or forced sequencing rather than accepting ties.

**Possible Improvements**
- Auto-suggest realistic weekly-hour budgets based on anonymized cohort data ("Students with your branch and target typically study 22-28 hrs/week").
- Detect syllabus overlap intelligently (e.g., GATE CS syllabus vs. college DSA course) and pre-merge redundant study items.
- Allow import of an existing plan (screenshot, Notion export, spreadsheet) that AI parses into the Goal Graph instead of starting from zero.

**Future Scope**
- Branch/college-specific templates (e.g., pre-filled GATE CSE roadmap vs. GATE ECE).
- Mentor/parent co-setup mode where a coach reviews and adjusts goals before finalizing.
- Integration with actual GATE application data (exam date, admit card) to auto-populate and auto-update target dates.

---

### 7.2 AI Roadmap Generation

**Purpose**
Translates the Goal Graph from the Wizard into a long-horizon (weeks-to-months) milestone roadmap per domain — e.g., "GATE: complete Theory of Computation by Week 6, first full mock test by Week 10." The roadmap is the strategic layer; it does not assign specific days/times (that's the Timetable's job) but defines *what must be true by when*.

**User Flow**
1. Triggered automatically after Wizard completion (or manually via "Regenerate Roadmap").
2. AI generates a single Roadmap composed of one Roadmap Milestone track per active Goal, sequenced by prerequisite logic (e.g., "Data Structures" before "Graph Algorithms") and weighted by the student's stated proficiency and available hours. There is one Roadmap per student, not one per Goal — this avoids the "N independent roadmaps" ambiguity flagged in review and lets the Scheduler escalate at the single-Roadmap level (7.12) instead of per-Goal.
3. Roadmap is presented visually (timeline/Gantt-style view) with milestones grouped by domain and color-coded by priority.
4. Student can drag milestones, mark topics as "already know this — skip," or request the AI to explain *why* a milestone is placed where it is ("Why is Operating Systems in Week 4 and not Week 2?").
5. Student approves the roadmap, which then feeds the AI Timetable Generator (7.3).
6. Roadmap is **living**: it re-runs automatically on major triggers (missed milestones, goal edits, exam date change) and notifies the student of proposed changes for approval rather than silently rewriting history.

**Edge Cases**
- Student is significantly behind an existing milestone → roadmap must re-baseline realistically rather than compressing remaining time into an impossible sprint; should surface trade-offs explicitly ("To stay on track, we recommend deprioritizing X").
- Two domains have genuinely competing critical-path deadlines in the same week (college exams + GATE mock test) → roadmap should flag the collision at generation time, not let the student discover it later.
- Student skips a prerequisite topic as "already known" but later performance data (mock scores, AI Teacher interactions) suggests otherwise → roadmap should be able to *propose* re-inserting it, framed as a suggestion, not a silent forced change.
- Very long horizon (e.g., 18 months to GATE) → risk of AI over-specifying distant milestones with false precision; roadmap should be coarse-grained far out and only fine-grained near-term (rolling-wave planning).

**Possible Improvements**
- Confidence scoring per milestone ("high confidence" vs. "aggressive stretch") so students understand plan risk, not just plan content.
- Scenario comparison ("Show me the roadmap if I only had 15 hrs/week vs. 25 hrs/week").
- Explainability panel showing the reasoning/data behind each milestone's placement.

**Future Scope**
- Roadmap benchmarking against anonymized outcomes of prior successful students with similar starting points.
- Multi-goal optimization solver that treats roadmap generation as a constrained scheduling problem across all domains simultaneously, rather than domain-by-domain.
- Integration with official syllabus updates (GATE syllabus changes year to year) to auto-refresh roadmap templates.

---

### 7.3 AI Timetable Generation

**Purpose**
Converts the approved Roadmap's milestones into a recurring **weekly structure** — which days/time-blocks are allocated to which domain and activity type. This bridges strategy (roadmap) and day-to-day execution (planner).

**User Flow**
1. Triggered after roadmap approval, or on-demand ("Regenerate my week").
2. AI proposes a weekly template respecting: student's declared available hours, college class schedule (imported or manually entered), stated energy patterns (e.g., "I focus best in mornings"), and roadmap milestones due that week.
3. Timetable is shown as a weekly grid; each block is labeled with domain + task type (e.g., "DSA — Graph practice," "GATE — Mock Test," "Research — Literature review").
4. Student can lock blocks (fixed college classes), drag/resize others, and mark preferred "protected" time (e.g., no study Sunday evenings).
5. Once adjusted, student saves the template as their "default week," which the Daily Planner and Scheduler use as the baseline before daily adjustments.

**Edge Cases**
- Student's college timetable changes mid-semester → timetable must support quick re-import/edit without a full regeneration cycle.
- Fixed commitments overflow available hours (e.g., labs + internship leave no study time) → system should surface the shortfall and suggest roadmap-level trade-offs rather than cramming impossibly.
- Student wants irregular scheduling (e.g., project sprints on weekends only, distributed study weekdays) → template must support non-uniform daily structures, not force identical days.
- Time zone or semester-break shifts (e.g., summer break with radically more free time) → should detect and prompt "Regenerate for break schedule?" rather than keeping a semester-time template.

**Possible Improvements**
- Energy-aware scheduling: use logged focus-quality data (from Pomodoro sessions) to learn actual best-focus hours over time, rather than relying only on self-report at setup.
- Auto-buffer time between demanding blocks to reduce burnout risk.
- "What-if" simulation before committing to a new weekly template.

**Future Scope**
- Calendar-of-calendars: auto-merge with college's official academic calendar feed if institutions expose one.
- Adaptive multi-week rotation templates (e.g., alternating "heavy DSA week" / "heavy theory week") for variety and spaced repetition.

---

### 7.4 Daily Planner

**Purpose**
The single surface a student opens every day. Converts that day's slice of the Timetable + any Scheduler adjustments + ad-hoc tasks into a concrete, ordered, checkable list of what to do today.

**User Flow**
1. Student opens the app; Daily Planner is the default landing view showing "Today."
2. List is pre-populated from the weekly Timetable, adjusted for any Scheduler re-arrangements (e.g., yesterday's missed task auto-inserted).
3. Student can reorder tasks, add ad-hoc items (e.g., "reply to professor's email"), mark tasks done/skipped/partially done, or push a task to tomorrow.
4. Each task can launch directly into a Pomodoro session with pre-filled context (domain, task name).
5. Completing tasks emits events to XP/Streaks/Analytics automatically — no separate logging step required.
6. End-of-day: lightweight reflection prompt ("How did today go?" — quick mood/energy tag) feeds the Scheduler's next-day adjustment logic.

**Edge Cases**
- Student opens the app very late at night or the next day without having opened it → planner should clearly distinguish "missed" vs. "not yet attempted" and avoid guilt-inducing framing, offering a one-tap "re-plan my day" option.
- Task estimated duration is wildly inaccurate (planned 30 min, actually needs 3 hours) → planner should let students adjust remaining-day plan on the fly, cascading changes to later tasks rather than leaving a broken schedule.
- Multiple tasks marked "in progress" simultaneously (context-switching) → UI should support this without breaking Pomodoro/XP attribution logic.
- Fully empty day (holiday, no plan generated) → should not show a blank/broken state; offer suggested optional light-touch activities (e.g., revision) or explicit rest-day framing.

**Possible Improvements**
- Smart task ordering suggestions based on historical focus patterns (e.g., "You tend to finish DSA faster in the morning — want to reorder?").
- Inline quick-capture for random tasks/ideas that don't yet belong to a domain, triaged later.
- Visual "day shape" preview (how demanding is today at a glance) before diving in.

**Future Scope**
- Voice-based daily planning check-in ("Tell me how today went") transcribed into reflection data.
- Cross-device sync with a physical/paper planner via photo import (OCR) for students who still prefer handwriting.

---

### 7.5 Calendar

**Purpose**
The macro, date-anchored view of everything — deadlines, exams, milestones, fixed college events, and the timetable — giving students a bird's-eye view that the Daily Planner intentionally doesn't provide.

**User Flow**
1. Student switches to Calendar view (day/week/month) from the Planner.
2. Calendar overlays multiple layers: Roadmap milestones, Timetable recurring blocks, one-off deadlines (assignment due dates, GATE exam date, certification exam date), and personal events.
3. Student can toggle layers on/off by domain (e.g., hide "College" to focus on "GATE" view only) for clarity.
4. Tapping any event opens its detail and related tasks; deadlines can be added manually or imported.
5. Calendar supports drag-to-reschedule for flexible items, with fixed items (exam dates) locked and visually distinct.

**Edge Cases**
- Two deadlines fall on the same day from different domains → visually surface conflict with a warning rather than silent overlap.
- Recurring event exceptions (e.g., one week's class is cancelled) → must support single-instance edits without breaking the recurring series.
- Imported external calendar (Google Calendar) conflicts with internally generated timetable → need a clear merge/precedence strategy (external fixed events always win over flexible study blocks).
- Long-term view (viewing months ahead) showing sparse/uncertain future roadmap data → should visually differentiate "confirmed" vs. "projected" milestones.

**Possible Improvements**
- Heatmap view showing workload density per day/week to help students spot upcoming crunch periods early.
- One-click "protect this week" (e.g., during college exams) that pauses non-critical study blocks automatically.
- Two-way sync with Google/Apple Calendar rather than one-way import.

**Future Scope**
- Shared calendar view for study groups/accountability partners (opt-in, privacy-respecting).
- Predictive conflict warnings weeks in advance based on roadmap trajectory ("Your GATE mock and mid-sems collide in 3 weeks — plan ahead").

---

### 7.6 Pomodoro Timer

**Purpose**
The focused-execution unit of the system. Converts a planned task into a timed, distraction-resistant work session, and is the primary mechanism by which "time actually spent" data is captured for Analytics, XP, and the Scheduler's learning loop.

**User Flow**
1. Student taps "Start Focus Session" on a Daily Planner task (or starts a freeform session).
2. Chooses/confirms session length (default Pomodoro 25/5, but customizable) and task context.
3. Timer runs with optional focus-mode features (notification silencing, simple ambient sound).
4. On completion, prompts a quick self-rating (focus quality, e.g., 1-5) and auto-logs session duration against the task and domain.
5. Break timer follows automatically; after configured cycles, suggests a longer break.
6. Session data feeds XP, Streaks, Habit Tracker (if tied to a habit), and Analytics in real time.

**Edge Cases**
- App is backgrounded/phone locked mid-session (mobile) → timer must continue accurately in the background and reconcile on return, not silently lose the session.
- Student abandons a session early → should log partial time honestly rather than either discarding it or falsely counting a full session (both undermine trust in the data).
- Back-to-back sessions with no break taken → system should gently flag burnout risk after a threshold rather than silently allowing endless chaining.
- Session started but not tied to any planner task (ad-hoc focus work) → must still be logged and later attributable to a domain, even if retroactively categorized.

**Possible Improvements**
- Adaptive session length suggestions based on task type and historical focus-quality ratings (e.g., DSA problems may warrant longer uninterrupted blocks than flashcard review).
- Optional "body doubling" mode showing an anonymized count of other students focusing right now for ambient accountability.
- Integration with device Do Not Disturb / app-blocking during sessions.

**Future Scope**
- Passive focus-quality detection (e.g., via typing/activity patterns, with explicit consent) to reduce reliance on self-rating.
- Group Pomodoro sessions for study partners/accountability pods with synchronized timers.

---

### 7.7 XP System

**Purpose**
A unified progress currency that makes otherwise invisible effort (hours studied, problems solved, commits pushed, habits maintained) visible and rewarding, while staying tightly coupled to real, verifiable progress rather than empty gamification.

**User Flow**
1. XP accrues automatically from Progress Event Bus events: completed Pomodoro sessions, verified LeetCode solves, verified GitHub commits/PRs, completed roadmap milestones, habit check-ins, and daily planner completion rate.
2. XP is domain-tagged (GATE XP, DSA XP, Projects XP, etc.) as well as totaled, so students can see balance/imbalance across their goals, not just a single vanity number.
3. XP contributes to Levels (broad progress tiers) shown on a profile/progress view, alongside milestone-based badges (e.g., "Completed Operating Systems module").
4. Weekly XP summary surfaces in Analytics and can optionally be shared (opt-in) for accountability.

**Edge Cases**
- Student games the system with trivial repeated actions (e.g., starting/stopping many tiny Pomodoro sessions) → XP weighting and anti-gaming thresholds (minimum session length, diminishing returns on repeated low-effort actions) must be designed in from the start.
- XP imbalance causing anxiety (e.g., low "GATE XP" visibly next to high "DSA XP") → framing must avoid shame; consider showing progress-to-goal rather than raw comparative numbers by default.
- Retroactive corrections (e.g., a GitHub commit later reverted, or a mock test score voided) → XP system needs a defined policy for adjusting/clawing back XP without confusing the student.
- New domains added mid-journey (goal edited) → XP history should not be lost or inconsistently re-bucketed.

**Possible Improvements**
- Weighted XP by task difficulty/importance (a hard DP problem should be worth more than a trivial one) rather than flat per-action XP.
- "XP decay" or freshness indicators for stale skills, encouraging periodic revision rather than one-and-done grinding.
- Personal XP goals/challenges (e.g., "Earn 500 GATE XP this week") set by the student, not just system-imposed targets.

**Future Scope**
- Cohort-relative XP benchmarking (opt-in, anonymized) — "You're in the top 20% of GATE CSE XP this month."
- XP-to-real-world mapping research (correlating XP patterns with actual exam outcomes) to continuously recalibrate weighting for genuine predictive value, not just engagement.

---

### 7.8 Streak System

**Purpose**
Encourages the consistency that long exam-prep and skill-building horizons require, by rewarding showing-up-daily behavior — while being explicitly designed to avoid the classic streak-anxiety failure mode where a single missed day causes total disengagement.

**Note on granularity:** To stay consistent with XP (7.7), which is Domain-tagged by default, Streaks are **also Domain-level by default, with a computed global streak as a rollup** — not the other way around. This is a deliberate v1.0 correction: treating global streak as primary and domain streak as a "future improvement" (as in the original draft) created a granularity mismatch against XP that would have confused both users and the Analytics layer built on top of both systems.

**User Flow**
1. Streak increments per-Domain when a student meets that Domain's defined daily "minimum viable engagement" (configurable — e.g., completing at least one planned Task or one Habit check-in), not an unrealistic full-day-completion bar.
2. A computed global streak (rollup of Domain streaks) and each Domain's current/best streak are visible on the home/profile view.
3. Streak-risk notification triggers late in the day if a Domain's daily minimum hasn't been met yet (see Notifications, 7.20).
4. Missed-day handling: system offers a limited number of "streak freezes" (earned via consistency or available by default), drawn from the Streak Freeze inventory (6A), so an occasional missed day doesn't erase months of consistency.
5. Streak milestones (7-day, 30-day, 100-day) are celebrated distinctly from routine daily upkeep, both per-Domain and globally.

**Edge Cases**
- Student has a legitimate reason to be fully offline (illness, family emergency, exam leave) → should support explicit "pause streak" mode rather than forcing either a broken streak or dishonest self-logging.
- Time zone travel or very late-night sessions crossing midnight → "day" boundary logic must be robust and based on the student's local day, not naive UTC cutoffs.
- Student with multiple domains, some with strong streaks and some neglected → since streaks are Domain-level by default, this is now directly visible rather than needing separate "consistency indicators" — the global rollup should never mask a Domain-level collapse.
- Long-broken streak causing motivation collapse ("what's the point of restarting") → messaging on streak reset must be encouraging and reframe restart positively, not just show "0."

**Possible Improvements**
- Flexible streak definitions the student can tune to their real capacity rather than one-size-fits-all, per Domain.
- "Streak insurance" earned through consistent behavior rather than purchasable, to keep the mechanic meaningful rather than pay-to-win.

**Future Scope**
- Adaptive streak difficulty that scales the "minimum viable engagement" bar as the student's capacity and habits mature.
- Social/accountability-pod shared streaks (opt-in) for peer-supported consistency.

---

### 7.9 Habit Tracker

**Purpose**
Tracks recurring, non-task-specific behaviors that support long-term success but don't fit neatly into the roadmap/timetable structure — e.g., "daily revision review," "sleep by 12," "no phone during study block," "read one research paper a week." Complements the Daily Planner (which handles discrete tasks) with sustained behavioral consistency.

**User Flow**
1. Student defines habits during or after the Wizard (some suggested by AI based on selected goals — e.g., "Daily DSA warm-up" suggested for placement-focused students).
2. Per the Terminology Standard (Section 6), a Habit is a recurring **Task subtype**, not a parallel structure — it renders inline in the same Daily Planner list as other Tasks, visually distinguished (recurrence indicator, streak chip) rather than living in a separate checklist.
3. Student checks in daily; habit streaks and completion rate are tracked independently per habit.
4. Habits can be linked to domains for XP attribution and to the Progress Event Bus for Analytics.
5. Weekly/monthly habit consistency view shows patterns (e.g., "You maintain habits well on weekdays, drop off on weekends").

**Edge Cases**
- Habit becomes irrelevant mid-journey (e.g., "revise for mid-sems" after mid-sems end) → needs easy archival without losing historical data, and shouldn't keep nagging for a stale habit.
- Habit that's inherently hard to self-verify (e.g., "sleep by 12") → system should be honest that this is self-reported and not conflate it with verified-progress habits in framing/trust level.
- Too many habits defined at once (student over-commits) → system should gently flag habit overload (e.g., more than 5-6 active habits) as a known cause of tracker abandonment.
- Habit check-in forgotten but genuinely done → allow retroactive same-day or previous-day check-ins within a reasonable window, rather than being punitively rigid.

**Possible Improvements**
- AI-suggested habit adjustments based on completion patterns ("You've missed 'read 1 paper/week' 3 weeks running — reduce to biweekly?").
- Habit stacking suggestions (attaching a new habit to an already-consistent one).
- Visual habit calendar (GitHub-contribution-graph style) for at-a-glance consistency history.

**Future Scope**
- Correlation insights between habit consistency and downstream outcomes (XP growth, mock scores) surfaced back to the student as motivation.
- Community habit templates shared/rated by other students in similar tracks.

---

### 7.10 AI Mentor

**Purpose**
Provides strategic, motivational, and planning-level coaching grounded in the student's actual Goal Graph and behavioral data — the "big picture" voice that helps with things like prioritization, burnout, motivation dips, and course-correction conversations. Distinct from the AI Teacher (7.11), which handles subject-matter tutoring.

**User Flow**
1. Accessible via a persistent chat entry point, and proactively surfaces at key moments (e.g., after a missed milestone, before a major exam, after a streak break).
2. Mentor has context on the student's roadmap progress, XP trends, streak history, and recent reflections — conversations are grounded, not generic ("I noticed you've fallen behind on Operating Systems for two weeks — want to talk through why?").
3. Student can ask open-ended questions ("Should I drop cybersecurity certs to focus on GATE this month?") and receive reasoned, trade-off-aware advice rather than one-line platitudes.
4. Mentor can propose concrete actions (e.g., "Adjust roadmap," "Take a rest day," "Talk to a human counselor") which the student can accept, triggering the relevant feature (Roadmap regeneration, etc.).
5. Conversation history is retained so mentor advice builds over time rather than resetting each session.

**Edge Cases**
- Student expresses signs of serious burnout, anxiety, or distress beyond productivity coaching scope → mentor must recognize its limits and gently direct toward appropriate human support (college counseling, helplines) rather than attempting therapy-style intervention.
- Student asks mentor to justify an unrealistic plan (e.g., "tell me I can learn all of DSA in 3 days") → mentor should give honest, evidence-grounded pushback rather than sycophantic agreement, while remaining supportive in tone.
- Mentor's advice conflicts with what the AI Roadmap/Scheduler independently decided → advice and system actions must stay consistent; mentor should be the "voice" of the same underlying planning intelligence, not a separate opinion.
- Repeated identical questions/frustration loops (student stuck in indecision) → mentor should recognize the pattern and shift tactics (e.g., suggest a decision framework) rather than repeating similar advice.

**Possible Improvements**
- Personality/tone customization (e.g., "tough coach" vs. "gentle encourager") to match what motivates the individual student.
- Proactive weekly check-in ritual initiated by the mentor rather than always waiting for the student to initiate.
- Explicit "what changed and why" recaps after mentor-triggered plan adjustments.

**Future Scope**
- Optional escalation path connecting students to real human mentors/coaches/alumni for higher-stakes decisions (career choices, GATE vs. placement trade-offs).
- Long-horizon narrative memory — mentor references the student's entire multi-month journey meaningfully in later conversations, not just recent events.

---

### 7.11 AI Teacher

**Purpose**
Subject-matter tutoring — explaining concepts, walking through DSA problems, clarifying GATE topics, and answering "how/why" academic questions. Distinct from the Mentor: the Teacher is about *understanding content*, not planning or motivation.

**User Flow**
1. Student opens AI Teacher from a specific task/topic in the Daily Planner ("Explain: B-Trees") or starts a freeform question session.
2. Teacher engages Socratically where appropriate (especially for DSA — guiding toward a solution rather than just stating it) and directly where appropriate (e.g., factual GATE concept clarification).
3. Sessions can include worked examples, follow-up practice question suggestions, and links to the student's Resource Management library (7.15) for deeper material.
4. Teacher can flag topics the student is visibly struggling with (repeated confused follow-ups) back to the Roadmap as a "needs reinforcement" signal.
5. Session summaries are saved and searchable, building a personal knowledge log over time.

**Edge Cases**
- Student uses the Teacher to get direct answers to graded assignments/exams (academic integrity risk) → Teacher should default to explanatory/Socratic mode for anything resembling a graded submission and be transparent that it won't simply provide copyable answers.
- Student asks about a topic outside the declared syllabus/domain scope → Teacher should still help (learning shouldn't be artificially blocked) but shouldn't silently expand the roadmap without the student's awareness.
- Incorrect or outdated explanation risk (AI hallucination) on technical topics → especially critical for GATE-level correctness; needs strong grounding/verification approach and clear flagging when the Teacher is uncertain.
- Student becomes overly reliant on the Teacher instead of independent problem-solving (common failure mode with AI tutors) → interaction design should nudge toward productive struggle (hints before full solutions) rather than instant answers by default.

**Possible Improvements**
- Difficulty-adaptive explanations based on the student's demonstrated proficiency (from LeetCode/mock data), avoiding both over-simplification and over-complication.
- Multi-modal explanations (diagrams for DSA/OS concepts) where visual aids meaningfully help.
- Spaced-repetition follow-up questions auto-generated from past Teacher sessions.

**Future Scope**
- Mock oral-exam / viva-style practice mode for research and certification tracks.
- Peer-teaching mode where the AI Teacher helps a student prepare to explain a topic to a study group, reinforcing mastery through the "protégé effect."

---

### 7.12 AI Scheduler

**Purpose**
The short-horizon, real-time re-arrangement engine. Where the Timetable defines the "default week" and the Roadmap defines long-term milestones, the Scheduler handles the constant small reality of daily life: a missed session, a task that ran long, an unplanned college assignment — and re-arranges the *near-term* plan (today/this week) accordingly without requiring a full roadmap/timetable regeneration.

**User Flow**
1. Runs continuously in the background, listening to Progress Event Bus signals (task completed late, task skipped, new deadline added, Pomodoro session overran).
2. When a disruption occurs, Scheduler proposes a specific, minimal re-arrangement (e.g., "You skipped this morning's DSA block — move it to this evening and push tonight's revision to tomorrow?") rather than a full re-plan.
3. Student can accept, modify, or dismiss each suggestion; dismissed suggestions don't repeat immediately for the same event.
4. Scheduler respects locked/fixed items (exams, classes) and priority rankings from the Goal Setup Wizard when deciding what to compress/defer/drop under time pressure.
5. Escalates to Roadmap-level regeneration only when disruptions are severe/repeated enough that near-term patching is no longer sufficient (e.g., a full week lost to illness), clearly explaining why it's escalating.

**Edge Cases**
- Cascading disruptions (missing several days in a row) → Scheduler must avoid infinite compounding compression that creates an impossible catch-up day; should trigger Roadmap-level re-baseline instead past a defined threshold.
- Student repeatedly overrides/ignores Scheduler suggestions → system should learn and reduce suggestion frequency/intrusiveness rather than nagging with the same rejected pattern.
- Conflicting simultaneous disruptions (missed task + new urgent deadline arriving at once) → needs clear precedence logic (priority ranking from Wizard) to resolve deterministically rather than producing contradictory suggestions.
- Scheduler suggestion involves dropping something emotionally significant to the student (e.g., a research task) → should flag high-priority-domain impacts distinctly rather than treating all deferrals as equivalent.

**Possible Improvements**
- Batch "week re-plan" mode giving the student full visibility/control when disruptions accumulate, instead of only granular one-off nudges.
- Transparency log ("Why did my Tuesday change?") showing the disruption→decision chain in plain language.
- Configurable Scheduler aggressiveness (some students want tight auto-management, others want to be asked before any change).

**Future Scope**
- Predictive disruption modeling (e.g., recognizing patterns like "mid-sem weeks historically cause 40% task drop-off" and pre-emptively lightening load).
- Cross-student anonymized disruption-recovery pattern learning to improve re-planning quality over time.

---

### 7.13 GitHub Integration

**Purpose**
Provides objective, tamper-resistant evidence of coding/project progress — critical for Projects, Portfolio, Placements, and (for CS-adjacent research) Research domains — reducing reliance on self-reported "I worked on my project today" checkboxes.

**User Flow**
1. Student connects GitHub account via OAuth during onboarding or later from Settings/Integrations.
2. Student selects which repos map to which Kensei project/domain entries (e.g., "resume-website" repo → Portfolio domain).
3. Commits, PRs, and issue activity on linked repos are pulled in and matched against active Project tasks, auto-marking related planner tasks as progressed/complete where confidence is high, or suggesting a match for student confirmation otherwise.
4. Commit/contribution activity feeds XP, Streaks, and a "Project Health" indicator in Analytics (e.g., stale vs. active repos).
5. Portfolio/Resume features (7.16, 7.17) can pull verified project stats (languages used, commit history, README quality) directly from linked repos.

**Edge Cases**
- Private repos / sensitive code (e.g., proprietary internship work) → integration must support selective repo visibility and never require exposing repo contents, only metadata (commit counts/timestamps), respecting student privacy and any employer NDAs.
- Group projects where commit volume doesn't reflect individual contribution → should surface nuance (e.g., lines-of-code isn't effort) and avoid naive XP-per-commit gaming (trivial commits inflating score).
- Student has pre-existing repos unrelated to current goals (old coursework, forks) → onboarding matching should not force-link irrelevant history and clutter Portfolio automatically.
- GitHub API rate limits or auth token expiry → integration should degrade gracefully (clear "reconnect GitHub" prompt) rather than silently failing and showing stale/misleading data.

**Possible Improvements**
- Commit-quality signals beyond raw count (meaningful commit messages, PR review activity, README/documentation completeness) contributing to Portfolio strength scoring.
- Auto-suggested project task creation from open issues/TODOs in linked repos.
- Contribution graph embedded directly in Analytics alongside study-time data for a unified effort view.

**Future Scope**
- GitHub Classroom integration for college coursework assignments.
- Auto-generated project case-study drafts for Portfolio/Resume based on actual commit history and README content (student-reviewed before publishing, never auto-published).

---

### 7.14 LeetCode Integration

**Purpose**
Provides objective, verified DSA practice data — problem-solving volume, difficulty distribution, topic coverage, and streaks — which is central evidence for both GATE algorithmic-thinking prep and Placement readiness.

**User Flow**
1. Student connects LeetCode account/profile during onboarding or later.
2. Solved problems sync automatically, tagged by topic/difficulty, and matched against DSA roadmap milestones (e.g., "Graphs" milestone progress updates as graph problems are solved).
3. Daily/weekly solve activity feeds XP, Streaks, and Analytics; a "topic coverage map" shows strengths/gaps (e.g., strong in Arrays, weak in Dynamic Programming) directly informing Roadmap/Scheduler adjustments.
4. AI Teacher can reference specific attempted-but-unsolved problems when the student asks for help, using actual struggle data rather than generic prompts.
5. Placement-readiness view (feeding Analytics/Portfolio) summarizes solve stats in a recruiter-relevant format.

**Edge Cases**
- LeetCode has no official public API for arbitrary third-party sync → integration likely depends on unofficial methods or user-provided data exports; must handle sync failures/rate limits gracefully and be transparent with students about data freshness/reliability.
- Student solves problems on other platforms too (Codeforces, GeeksforGeeks, HackerRank) → single-platform integration risks under-representing real practice; should allow manual logging for non-integrated platforms to avoid penalizing students who use other tools.
- Problem solved with help (copied solution, looked up approach) counted identically to independently solved → integration can't fully verify genuine understanding; framing in Analytics/XP should acknowledge this limitation rather than overstate confidence.
- Sudden large backlog import (student connects account with 500 historical solves) → shouldn't cause a jarring one-time XP flood that breaks streak/leveling logic; needs sensible historical-data handling (e.g., counted for coverage stats, not retroactive XP).

**Possible Improvements**
- Difficulty-weighted and recency-weighted scoring (recent Hard problems matter more than old Easy ones for readiness signal).
- Topic-gap-driven problem recommendations feeding directly back into the Daily Planner.
- Support for additional platforms (Codeforces, GFG, HackerRank) under a unified "DSA practice" signal rather than LeetCode-only.

**Future Scope**
- Mock technical interview mode combining LeetCode problem-solving with AI Teacher-led verbal explanation practice, closer to real interview conditions.
- Company-specific problem-set alignment (e.g., "problems commonly asked by Company X") for targeted placement prep.

---

### 7.15 Resource Management

**Purpose**
A unified library for the study materials students inevitably accumulate — PDFs, video links, lecture notes, cheat sheets, past papers, research papers — tagged to the domains/topics they support, so materials are discoverable exactly when needed (e.g., surfaced automatically alongside a related Daily Planner task).

**User Flow**
1. Student uploads/links resources (PDF upload, YouTube/article URL, notes) manually, or saves them via a browser extension/share-sheet while browsing elsewhere.
2. AI auto-tags resources by domain/topic (e.g., a PDF titled "Operating_Systems_Ch4" auto-tagged to GATE → Operating Systems) with student confirmation/correction.
3. Resources surface contextually: opening a related Daily Planner task or AI Teacher session about a topic shows linked resources alongside it.
4. Student can organize resources into custom collections (e.g., "GATE OS Master List," "Cert Prep — Security+") independent of auto-tagging.
5. Search across all resources by keyword, tag, or domain.

**Edge Cases**
- Duplicate resources saved from multiple sources (same PDF uploaded twice, or linked both as a URL and a downloaded file) → should detect and offer to merge/dedupe rather than cluttering the library.
- Large volume of resources overwhelming organization (hundreds of saved links) → auto-tagging and search must remain reliable at scale; stale/unused resources should be identifiable (e.g., "never opened") for cleanup prompts.
- Copyrighted/paid course material uploads → system must respect copyright and not enable redistribution; storage should be treated as strictly personal, non-shareable by default.
- Broken links (video removed, article paywalled after saving) → periodic link-health checks should flag broken resources rather than leaving silent dead ends.

**Possible Improvements**
- AI-generated summaries/flashcards from uploaded resources (e.g., auto-summarize a long PDF chapter) to accelerate review.
- Collaborative shared collections for study groups (opt-in, with clear ownership/attribution).
- Reading/watching progress tracking for long resources (e.g., "60% through this lecture series").

**Future Scope**
- Direct integration with common platforms (Coursera, NPTEL, YouTube playlists) for automatic progress sync rather than manual logging.
- AI-curated resource recommendations based on identified skill gaps (from LeetCode/Roadmap data), suggesting specific existing library resources or new ones to add.

---

### 7.16 Project Management

**Purpose**
Structured tracking for personal/academic/research projects — distinct from generic tasks because projects have their own sub-structure (milestones, scope, technical decisions) and directly feed Portfolio and Resume content.

**User Flow**
1. Student creates a project entry (manually, or auto-suggested from a newly linked GitHub repo) with a goal, target completion date, and domain tag(s) (e.g., "Portfolio," "Research," "Cybersecurity").
2. Project is broken into milestones/tasks, which integrate with the Daily Planner and Timetable like any other task, but stay grouped under the project view for focused tracking.
3. Project view shows a consolidated status: task completion, linked GitHub activity (commits/PRs), and a freeform "notes/decisions log" for capturing technical decisions or research findings over time.
4. On project completion (or at any point), student can generate a "Project Summary" — a structured writeup (problem, approach, tech stack, outcome) usable directly for Portfolio/Resume.
5. Stale project detection: projects with no activity for a defined period are flagged for a check-in ("Still working on this? Archive or re-prioritize?").

**Edge Cases**
- Open-ended/exploratory projects without clear milestones (common in early research) → should support a lighter-weight "journal-style" tracking mode rather than forcing rigid milestone structure prematurely.
- Group projects with multiple contributors → v1 is single-student-focused; must be clear about scope (tracking the *student's* view/contribution) rather than implying full team project management capability it doesn't have.
- Project abandoned partway → archiving should preserve learnings/history for potential resume mention ("attempted X, learned Y") rather than treating incomplete as valueless and deleting.
- Project scope creep (a small project balloons far beyond original plan) → system should surface scope growth relative to original estimate, prompting a check-in rather than silently absorbing it.

**Possible Improvements**
- Templates for common project types (web app, ML project, CTF writeup, research paper) with pre-suggested milestone structures.
- Tech-stack tagging feeding directly into Resume's skills section.
- Time-investment-vs-outcome view to help students judge which projects are worth deepening vs. wrapping up.

**Future Scope**
- Lightweight collaboration mode for genuinely joint projects (shared milestone board) while keeping individual XP/tracking separate.
- AI-assisted project ideation aligned to identified skill/portfolio gaps ("You have no cybersecurity project yet — here are 3 scoped ideas matching your current skill level").

---

### 7.17 Portfolio & Resume (Portfolio, Resume)

**Purpose**
Converts the verified work already tracked elsewhere in the system (projects, GitHub activity, certifications, DSA stats) into polished, recruiter-ready Portfolio and Resume artifacts — minimizing the usual painful, disconnected process of manually reconstructing "what have I even done" at application time.

**User Flow**
1. Student opens Portfolio/Resume builder; system pre-populates draft sections from existing data: completed Projects (with summaries from 7.16), verified GitHub stats, Certifications (7.9-adjacent tracking, see below), and relevant DSA/skills stats.
2. Student reviews, edits tone/wording, and selects which items to include per target (e.g., a resume tailored for a specific company vs. a general one) — supporting multiple resume variants from one underlying data source.
3. AI assists with phrasing (impact-oriented bullet points, quantification suggestions) while keeping factual claims traceable back to real tracked data (avoiding AI-fabricated achievements).
4. Portfolio (web-facing) and Resume (document) can be exported/published; portfolio can optionally link live GitHub/LeetCode stats for real-time freshness.
5. System flags staleness ("Your resume hasn't reflected your last 2 completed projects — update?") tied to Project Management completions.

**Edge Cases**
- AI-suggested resume bullet overstates/fabricates impact beyond what tracked data supports → strict guardrail: AI phrasing must stay grounded in verifiable tracked facts, and the student must explicitly confirm any claim before it's included, given real career/integrity stakes.
- Student's actual experience doesn't map neatly to generic resume templates (e.g., strong research background, thin project portfolio) → builder should adapt structure/emphasis rather than forcing a one-size-fits-all format.
- Multiple resume versions drifting out of sync with underlying project updates → need clear versioning so students know which resume is current/tailored-to-what.
- Sensitive information handling (contact details, internship company NDAs) → strict data handling and no unintended public exposure via portfolio publishing defaults.

**Possible Improvements**
- ATS (Applicant Tracking System) compatibility checks before export.
- Company/role-specific tailoring suggestions (emphasize different projects/skills per target role).
- Side-by-side resume diffing across versions to track how it's evolved.

**Future Scope**
- Direct integration with job application flows (auto-fill applications from resume data on supported platforms).
- Recruiter-view analytics (with consent) showing which portfolio sections get attention, if published/shared externally.

---

### 7.18 Certifications Tracking

**Purpose**
Manages the certification pursuit lifecycle (e.g., CEH, Security+, AWS, Coursera specializations) as first-class goals with their own timelines, distinct from generic Projects, since certifications have exam dates, prerequisite study material, and renewal/validity considerations.

**User Flow**
1. Student adds a certification goal (from a curated list or manual entry) with target exam date and current prep status.
2. Certification gets its own mini-roadmap (syllabus breakdown) integrated into the main Roadmap/Timetable alongside other domains.
3. Study resources (7.15) and relevant Daily Planner tasks are tagged to the certification; AI Teacher can help with cert-specific content.
4. On completion, certification is logged with proof (e.g., certificate upload/link) and automatically flows into Portfolio/Resume as a verified credential.
5. Expiring certifications (where applicable) trigger renewal reminders ahead of expiry.

**Edge Cases**
- Certification exam gets postponed/rescheduled (common with third-party providers) → roadmap must support target-date edits without a full reset.
- Certification abandoned partway (student decides it's not worth continuing) → should archive gracefully, preserving partial-study value (e.g., resource notes remain useful) rather than treating it as pure loss.
- Overlapping certification and GATE/placement crunch periods → Scheduler/Roadmap conflict detection (7.2, 7.12) must treat certification deadlines with the same seriousness as other domain deadlines.
- Certification catalog doesn't include a niche/new certification the student wants → must support fully custom certification entries, not just curated-list selection.

**Possible Improvements**
- Curated prep-time estimates and prerequisite guidance per popular certification, informed by aggregate student data over time.
- Cost/ROI notes (many certifications have exam fees) to help students prioritize which are worth pursuing given limited time/budget.

**Future Scope**
- Direct proctoring/exam-provider integrations for automatic completion verification (where providers expose this).
- Employer-relevance signals ("This certification appears in X% of job postings you're targeting") to guide prioritization.

---

### 7.19 Analytics

**Purpose**
Closes the loop by turning the Progress Event Bus's raw data (study time, XP, streaks, habits, GitHub/LeetCode activity, roadmap adherence) into clear, actionable insight — helping students understand *how they're actually doing* against their goals, not just what they've logged.

**User Flow**
1. Analytics dashboard accessible from a dedicated tab, with domain-level and cross-domain views.
2. Core views: time allocation (planned vs. actual, by domain), roadmap adherence (on-track/behind per milestone), XP/streak trends, skill coverage maps (from LeetCode/GitHub/Certifications), and a weekly/monthly digest summarizing the period.
3. Insight callouts are surfaced proactively, not just raw charts (e.g., "You've spent 60% of study time on DSA and only 5% on Cybersecurity this month, despite both being active goals").
4. Student can drill into any metric's underlying data (e.g., which specific sessions contributed to a time-allocation number) for trust/transparency.
5. Analytics insights feed back into Roadmap/Scheduler suggestions (closing the loop) and into Mentor conversations.

**Edge Cases**
- Data gaps from failed integrations (GitHub token expired, LeetCode sync failure) → dashboard must clearly flag incomplete data periods rather than silently understating actual progress.
- Small sample sizes early in the journey (first week) → should avoid over-confident trend claims ("Your pace suggests X") before there's enough data, showing appropriate uncertainty instead.
- Metric overwhelm (too many charts, unclear what matters) → dashboard needs a clear "what matters most right now" default view rather than dumping all metrics equally.
- Comparison/benchmark data (future scope, cohort-relative) risking unhealthy comparison anxiety → must be opt-in and carefully framed if/when introduced.

**Possible Improvements**
- Custom date-range comparisons (e.g., "this month vs. last month").
- Exportable reports (e.g., for students who want to share progress with a mentor, parent, or coach).
- Predictive readiness indicators (e.g., estimated GATE-readiness percentage based on syllabus coverage and mock performance trends) — clearly labeled as estimates, not guarantees.

**Future Scope**
- Outcome correlation research (which behavior patterns actually correlate with strong GATE/placement outcomes) feeding back into product-wide recommendation quality.
- Mentor/coach-facing analytics views (opt-in sharing) for those using human coaching alongside Kensei.

---

### 7.20 Notifications

**Purpose**
The system's proactive nudge layer — timely reminders and alerts that keep students engaged with their plan without becoming noisy, anxiety-inducing, or ignorable background clutter (a common failure mode of productivity apps).

**User Flow**
1. Notification preferences are configured during/after onboarding: channels (push/email), quiet hours, and category-level opt-in/out (e.g., "streak risk" on, "marketing/tips" off).
2. Contextual, event-driven notifications fire based on Progress Event Bus signals: streak-at-risk (late in the day if minimum engagement unmet), upcoming deadline, Scheduler-proposed re-arrangement, roadmap milestone check-in, Mentor proactive check-in trigger.
3. Notifications are batched/prioritized to avoid flooding — the system enforces a sensible daily cap and groups related nudges rather than firing one-by-one.
4. Each notification deep-links directly to the relevant action (e.g., streak-risk notification opens straight to today's remaining tasks), minimizing friction between alert and action.
5. Student can adjust notification intensity at any time from Settings, and the system self-tunes down frequency for notification types the student consistently dismisses/ignores.

**Edge Cases**
- Student in a genuinely offline period (exam leave, travel) receiving guilt-inducing streak/deadline pings → should respect explicitly set "pause" states (from Streak System, 7.8) and suppress non-essential nudges accordingly.
- Time-zone-sensitive delivery (notification meant for "evening" arriving at the wrong local time) → must be based on accurate local time, especially relevant if the student travels.
- Notification fatigue from too many domains firing independently (GATE reminder + DSA reminder + habit reminder all at once) → cross-feature batching/prioritization logic is essential, not optional.
- Critical vs. non-critical conflation (an actual upcoming exam deadline notification looking identical to a routine streak nudge) → visual/priority differentiation must be clear so important alerts aren't lost in routine noise.

**Possible Improvements**
- Smart timing based on the student's historical app-open patterns (send nudges when they're actually likely to act, not just at a fixed default time).
- Rich, actionable notifications (e.g., inline "mark done" / "reschedule" actions without opening the app).
- Digest mode (daily/weekly summary notification) as an alternative to real-time pings for students who prefer less frequent interruption.

**Future Scope**
- Cross-device intelligent delivery (e.g., don't push-notify on phone if the student is actively working in the desktop app).
- Notification-effectiveness analytics feeding back into the system's own tuning (which nudges actually drive follow-through vs. get ignored/dismissed).

---

## 8. Cross-Feature Dependency Map (Summary)

| Feature | Primary Inputs From | Primary Outputs To |
|---|---|---|
| Goal Setup Wizard | Student (manual entry) | AI Roadmap Generation |
| AI Roadmap Generation | Wizard, Scheduler (re-baseline triggers), Analytics | AI Timetable Generation, Calendar |
| AI Timetable Generation | Roadmap, Calendar (fixed events) | Daily Planner, Scheduler |
| Daily Planner | Timetable, Scheduler, Habit Tracker | Pomodoro Timer, XP, Streaks, Analytics |
| Calendar | Timetable, Roadmap, external imports | Daily Planner, Scheduler |
| Pomodoro Timer | Daily Planner task selection | XP, Streaks, Analytics, Scheduler (focus-quality data) |
| XP System | Progress Event Bus (all features) | Analytics, Mentor context |
| Streak System | Progress Event Bus (all features) | Notifications, Analytics, Mentor context |
| Habit Tracker | Wizard suggestions, student definition | Daily Planner, XP, Analytics |
| AI Mentor | Roadmap, XP, Streaks, Analytics, Reflections | Roadmap regeneration triggers, Scheduler triggers |
| AI Teacher | Daily Planner tasks, LeetCode struggle data, Resources | Resource Management, Roadmap ("needs reinforcement" signal) |
| AI Scheduler | Progress Event Bus (disruption signals) | Daily Planner, Roadmap (escalation), Notifications |
| GitHub Integration | Student OAuth, repo activity | Project Management, XP, Portfolio/Resume, Analytics |
| LeetCode Integration | Student account/profile | Roadmap (DSA milestones), XP, Analytics, AI Teacher context |
| Resource Management | Manual/auto-tagged uploads | Daily Planner (contextual surfacing), AI Teacher |
| Project Management | GitHub Integration, Daily Planner | Portfolio/Resume, XP, Analytics |
| Portfolio & Resume | Project Management, GitHub, Certifications, LeetCode stats | External export/publish |
| Certifications Tracking | Wizard, Roadmap | Portfolio/Resume, Scheduler (deadline conflicts) |
| Analytics | Progress Event Bus (all features) | Mentor context, Roadmap/Scheduler adaptation triggers |
| Notifications | Progress Event Bus, Streaks, Scheduler, Mentor | Deep links back into Daily Planner/relevant feature |

The **Progress Event Bus** (Section 6) is the load-bearing architectural concept enabling this — every feature both emits and consumes shared events rather than maintaining isolated state.

---

## 9. Success Metrics (North Star & Supporting)

**North Star Metric:** Weekly Active Roadmap Adherence Rate — the % of planned roadmap milestones a student actually completes on schedule, tracked cohort-wide. This is chosen over simpler engagement metrics (DAU, session count) because it directly reflects whether Kensei is achieving its core promise: helping students actually execute their plan, not just open the app.

**Supporting Metrics:**
- Activation: % of new users completing the Goal Setup Wizard and receiving an approved Roadmap within first session.
- Engagement: Weekly streak retention rate; average Daily Planner completion rate.
- Integration depth: % of active users with GitHub and/or LeetCode connected (proxy for objective-progress trust and stickiness).
- Feature health: AI Mentor/Teacher session frequency and follow-through rate (did a Mentor-suggested action get accepted?).
- Outcome (longer-term, harder to measure): self-reported and, where obtainable, verified exam/placement outcomes correlated with Kensei usage patterns.
- Retention: Month-over-month retention across the typical 6-18 month exam-prep horizon (not just 30-day retention, which under-represents this product's natural usage cycle).

---

## 10. Risks & Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| AI-generated roadmaps/timetables are unrealistic, causing early abandonment | High | Confidence scoring, explainability, easy manual override, conservative defaults erring toward achievable rather than aggressive plans |
| Gamification (XP/Streaks) gets gamed or causes anxiety instead of motivation | Medium-High | Anti-gaming weighting, streak freezes, domain-level nuance, careful UX framing avoiding shame |
| Over-reliance on AI Teacher undermines genuine learning / raises academic-integrity concerns | Medium-High | Default Socratic/hint-first mode, transparency about graded-work boundaries |
| Third-party integration fragility (LeetCode has no official API, GitHub rate limits) | Medium | Graceful degradation, manual fallback logging, clear sync-status indicators |
| Feature sprawl overwhelming new users (18 major features at once) | High | Progressive disclosure — Wizard scopes initial feature exposure to selected goals; not all 18 features surfaced day one |
| Student mental health signals surfacing in Mentor conversations mishandled | High | Clear scope boundaries for AI Mentor, escalation paths to human support, no attempt at therapeutic intervention |
| Data privacy concerns (academic performance, GitHub private repo metadata, personal reflections) | High | Strict data minimization, granular integration permissions, clear data ownership/export/deletion controls |

---

## 11. Phasing — Feature-Level MVP / V1 / V2

Phase-level sequencing (which of the 18 features ships first) is necessary but not sufficient — several individual features bundle multiple maturity levels under one heading. This section splits those features internally so engineering doesn't default to building the V2 version of everything the prose describes.

**11.1 Release Phases (feature-level, supersedes any single "Phase 1/2/3" grouping)**

**MVP — prove the core loop:**
Goal Setup Wizard, AI Roadmap Generation (single-Goal, non-adaptive regeneration), AI Timetable Generation (initial generation + manual edit, no learning), Daily Planner, Calendar, Pomodoro Timer, XP (Domain-tagged, flat weighting), Streaks (Domain-level + rollup, manual pause only), Habit Tracker (basic recurring Task subtype), Portfolio & Resume (static template pulling from tracked data, no AI phrasing), Analytics (time-allocation and adherence charts only, no insight callouts, no predictive readiness).

**V1 — adaptivity and objective signal:**
AI Scheduler (minimal re-arrangement proposals only — no override-learning, no aggressiveness tuning), AI Mentor (reactive Q&A grounded in Goal Graph + proactive check-ins on missed milestones — no personality customization, no human-coach escalation network), AI Teacher (Socratic/hint-first tutoring, session summaries), GitHub Integration (OAuth sync, commit/PR counts, basic repo→Project mapping — no commit-quality scoring), LeetCode Integration (sync via best-available method, topic/difficulty tagging — no difficulty-weighted recency scoring), Resource Management, Project Management, Certifications Tracking (folded into generic Goal/Domain rather than a fully separate lifecycle at this stage), Analytics (insight callouts added), Notifications (event-driven + batching/prioritization).

**V2 — compounding intelligence and ecosystem:**
AI Scheduler (override-learning, configurable aggressiveness, predictive disruption modeling), AI Mentor (personality customization, human-coach escalation partnerships), GitHub Integration (commit-quality signals, auto-suggested tasks from open issues), LeetCode Integration (difficulty-weighted/recency-weighted scoring, multi-platform support, mock-interview mode), Portfolio & Resume (AI-assisted phrasing under the integrity guardrail defined in 7.17, ATS scoring, job-application autofill), Analytics (predictive readiness percentage — intentionally deferred until there is enough cohort/longitudinal data to earn the confidence such a number implies), Certifications Tracking (fully independent lifecycle, employer-relevance signals), templates/community layer (Section 16).

**11.2 Sequencing Rationale**
The MVP list is deliberately conservative on AI surface area: it proves that Goal → Roadmap → Timetable → Daily execution → Tracking is sticky *before* investing in Scheduler learning loops, Mentor escalation infrastructure, or predictive analytics that depend on data the MVP hasn't generated yet. V1 is where the product's actual differentiation (objective evidence over self-report, adaptive re-planning) starts to show up. V2 is where compounding/ecosystem effects (community templates, predictive modeling, coach partnerships) become viable because there's now a data and user base to compound on.

---

## 12. Success Metrics, Risks & Open Questions

*(Sections 9 and 10 above — Success Metrics and Risks & Mitigations — remain as previously defined and are not restated here.)*

**Open Questions for Stakeholder Review**
1. What is the primary monetization model (freemium feature gating, subscription tiers, institutional/coaching-center licensing)? This affects which features (AI Mentor depth, integrations) are gated, and directly informs Section 13.2 (Subscription/Paywall Flow) below.
2. Should Kensei support non-GATE competitive exams (e.g., ESE, PSU exams, GRE) in v1 scope or remain GATE-specific initially with a clear extension path?
3. What is the data retention and privacy policy for sensitive integrations (GitHub private repo access, academic performance data), especially given the primarily student/minor-adjacent-age user base?
4. Is a parent/mentor-facing view in scope for v1 given common expectations in the Indian ed-tech market, or explicitly deferred? (See 13.9, Sharing Flow.)
5. What human-support escalation partnerships (college counseling, helplines) should be pre-established before AI Mentor ships, given the burnout/mental-health edge cases identified in Section 14?
6. Given that non-CS branches (mechanical, civil, electrical GATE aspirants) are a stated target persona but poorly served by DSA/GitHub/LeetCode-centric features as scoped, should MVP explicitly narrow to CS/software-adjacent branches, or should generic (non-code) skill-tracking be pulled forward into MVP?

---

## 13. Missing User Flows (Added in Review)

These flows were absent from v1.0 despite being load-bearing for a product with a stated 6-18 month usage horizon and sensitive personal data.

**13.1 Sign-Up / Authentication**
Student creates an account (email or OAuth) before ever reaching the Goal Setup Wizard. Includes standard verification, and — given downstream GitHub/LeetCode OAuth — a decision on whether "Sign in with GitHub" doubles as both auth and integration-connection in one step, or is kept deliberately separate to avoid conflating identity with data access.

**13.2 Subscription / Paywall**
A free user reaches a gated feature (per Section 11's MVP/V1/V2 tiering, likely Mentor depth, integrations, or advanced Analytics). Flow must define: what the gate looks like in-place (not a dead end), what a time-boxed trial (if any) looks like, and what happens to data/progress generated during a trial if the student doesn't convert.

**13.3 Lapsed-User Return**
A student who disappears for weeks or months (common around exam crunches or burnout, and structurally likely given the product's own horizon) returns. Flow must define: does the Roadmap silently regenerate or prompt the student first; does the Streak simply show zero with no context; is there a "welcome back, here's what changed" recap rather than dropping the student into a stale or jarring state.

**13.4 Partial Wizard Drop-Off / Resume**
Student abandons the Goal Setup Wizard partway through. Flow must define whether partial state is saved and resumable, or discarded, and how re-entry is surfaced (e.g., "finish setting up your goals" prompt) rather than silently losing input.

**13.5 Multi-Device Conflict Resolution**
The same Daily Planner or Timetable is edited on two devices (e.g., mobile Pomodoro session running while desktop Planner is edited). Flow must define reconciliation behavior — not just the Pomodoro-specific "backgrounded on mobile" edge case already covered, but general offline/conflicting-edit resolution.

**13.6 Integration Disconnect**
Student disconnects GitHub or LeetCode (revokes the Connection). Flow must define what happens to XP already earned and Task attributions already derived from that data — retained as historical record, or flagged as now-unverifiable — and must be the explicit inverse of the existing connect flow (7.13/7.14), not an afterthought.

**13.7 Mentor-to-Human Escalation**
When the AI Mentor recognizes it should "gently direct toward appropriate human support" (7.10 edge case), this flow defines what the student actually sees: a resource directory (college counseling, helplines), whether anything is logged, and whether there's any follow-up — not just a single deflecting message.

**13.8 Data Export / Account Deletion**
Given the volume of sensitive academic, behavioral, and integration data this product holds, a concrete flow (not just a Section 10 risk-table bullet) for requesting a full data export and for account/data deletion, including what happens to data already shared with a parent/mentor via Consent grants (6A).

**13.9 Sharing Analytics / Portfolio with a Parent or Mentor**
The actual grant/revoke/view flow behind the "opt-in" language used throughout Sections 7.19 and elsewhere: how a share is initiated, what the recipient sees, how it's revoked, and what the recipient is told when access is revoked.

**13.10 Goal Completion / "Graduation"**
A student actually clears GATE or gets placed — a first-class moment for a product this identity-invested in the student's journey, and currently entirely unaddressed. Flow must define: how the Goal is marked achieved, what happens to associated Roadmap/Timetable/streak state, whether the account prompts a new goal cycle (e.g., MTech prep, on-the-job upskilling) or simply archives, and how this moment is celebrated rather than left as a silent state transition.

---

## 14. Cross-Cutting Edge Cases (Added in Review)

These sit above any single feature and were previously absent even though the per-feature edge cases were otherwise thorough.

- **AI planning systems disagreeing in the same moment** — e.g., Roadmap re-inserts a skipped prerequisite the same week Scheduler proposes deferring that Domain after a missed day. Resolved architecturally by the Planning-Horizon Arbitration rule (Section 6); must also be resolved at the UX level as a single merged suggestion, never two contradictory nudges.
- **AI service degradation or outage** — Roadmap/Timetable/Scheduler generation itself becomes unavailable. Product must define a fallback (last-known-good plan continues to apply; manual-entry mode remains available) rather than leaving the student without a plan.
- **Non-CS GATE branches** — mechanical, civil, and electrical engineering students are a stated target persona, but DSA, GitHub, and LeetCode integration assume a CS/software-adjacent student. This is a scope gap, not just an edge case, and is reflected as Open Question 6 above.
- **Repeat exam cycles** — GATE has no attempt cap; many students retake it. The Roadmap/Goal model must support re-cycling a completed or lapsed Goal for a second attempt without treating prior history as noise.
- **AI usage limits under real demand** — if Mentor/Teacher calls are rate-limited or cost-gated, there must be a defined, honest degradation experience for a student who hits that limit during exam week, rather than a silent failure.
- **Integration token compromise** — GitHub OAuth tokens are a realistic target; beyond the general privacy language in Section 10, there must be a defined incident-response path (forced re-auth, notification to affected student) for token leakage or misuse.
- **Accessibility** — not addressed anywhere in the original 18-feature scope. Given heavy reliance on timers, calendars, and dense dashboards, baseline accessibility (screen-reader support, color-contrast, keyboard navigation) must be a stated requirement, not an omission.
- **Language / locale** — the target market is heavily India-centric; regional-language content and non-English Resource tagging are unaddressed and should be scoped explicitly in or out of MVP.
- **Academic-integrity risk at scale** — the existing AI Teacher edge case (7.11) covers one student getting direct answers; it does not cover multiple students independently querying the Teacher about the same assignment and converging on similar-enough guidance to constitute a shared-answer problem in aggregate. Worth a policy decision, not just a per-session guardrail.

---

## 15. Architectural Risks Originating from Product Decisions

Distinct from Section 10's general risk table — these are consequences of specific choices made in this PRD, not implementation risk, and should be owned at the leadership level.

- **Universal coupling via the Progress Event Bus.** Making every feature both a producer and consumer of one shared Event log (6A) means no feature in this system is truly isolated. This is the right architecture, but it means every future feature addition is an integration project, not an additive one — a cost worth acknowledging explicitly rather than discovering mid-roadmap.
- **Three independent AI planners, now with a stated but unproven arbitration rule.** Section 6's Planning-Horizon Arbitration rule resolves the *documented* ambiguity, but it is a product decision made on paper — it needs to be validated against real conflicting-plan scenarios during MVP, not assumed correct because it's now written down.
- **Continuous re-planning vs. long-horizon trust.** The product promise is a plan that's both "living" (adapts via Scheduler potentially daily) and a stable 6-18 month commitment. Nothing in this document bounds how often the plan is allowed to visibly change before it stops feeling like a plan the student can trust. This is a risk of the always-adaptive positioning itself, not a bug — worth a stated internal threshold (e.g., a soft cap on visible Scheduler-driven changes per week) before launch.
- **Foundational dependency on an unofficial LeetCode integration.** LeetCode has no public API (7.14); the product's core differentiator — objective evidence over self-report — partially rests on a data source outside Kensei's control, with real reliability and ToS risk. This is a strategic bet that deserves explicit executive risk ownership, not just a technical footnote, and argues for treating the multi-platform DSA-tracking future scope (Codeforces, GFG, HackerRank) as risk diversification, not just a nice-to-have.
- **XP/Streaks as deeply entangled cross-cutting infrastructure.** Because gamification reads from and writes to nearly every feature via the Event log, any future decision to soften, redesign, or regulate it (a realistic possibility for a student-facing product) carries large blast radius. Section 16 proposes treating this as a detachable layer specifically to de-risk this.
- **AI Mentor's structurally opposed mandates.** It is asked to drive engagement/motivation (retention-aligned) and to recognize burnout and tell students to do less (protective, anti-engagement). Both jobs living in one conversational surface is a product decision with an inherent incentive tension — it should be named explicitly in Mentor's design brief rather than left to emerge as an ad hoc UX judgment call later.

---

## 16. World-Class Design Principles Addendum

Concrete product commitments to make this a category-defining product rather than a feature-complete one.

1. **The Event Log is core infrastructure, not an implementation detail.** It is formalized in Section 6A precisely so every team building against it treats it as a stable contract.
2. **One canonical Priority, always visible.** Priority lives on the Goal entity and is read identically by Roadmap, Scheduler, and Mentor (Section 6). Students should be able to see "your priorities as of [date]" as a first-class, editable surface — the same way a well-designed project tool surfaces priority transparently rather than burying it in settings.
3. **A single, unified quick-capture entry point** for Tasks, Habits, Resources, and Project notes, triaged afterward — one fast "add" primitive instead of four feature-specific ones.
4. **A shared cadence primitive across Roadmap, Timetable, and Scheduler** so the three planning horizons operate on one common rhythm (e.g., a lightweight "cycle" concept) instead of three independently-invented time models — this also gives the Planning-Horizon Arbitration rule (Section 6) something concrete to arbitrate over.
5. **A first-class Plan Changelog, always visible, not a debug feature.** Every AI-driven change to Roadmap, Timetable, or a daily plan is logged against the Version entity (6A) and surfaced as a readable "what changed and why" trail — this is a trust feature given how much autonomy the system has over a student's plan, not a nice-to-have buried under Scheduler's improvements list.
6. **"Verified" vs. "self-reported" as one consistent visual language**, applied uniformly across XP, Analytics, and Portfolio — this distinction (objective GitHub/LeetCode evidence vs. self-logged completion) is the actual product thesis and deserves one design treatment, not six different ad hoc explanations across features.
7. **Explicit account lifecycle states** (Active, Paused, Goal-Achieved, Dormant, Deleted) as a first-class part of the User entity (6A), because this is a multi-month-to-multi-year relationship, not a session-based tool.
8. **Gamification as a detachable layer, with a "quiet mode."** Structuring XP/Streaks so they can be fully hidden without breaking the core plan-and-execute loop, both to de-risk the coupling problem (Section 15) and to serve the real segment of students (and increasingly, ed-tech critics) who find streak/XP mechanics counterproductive to focus.
9. **An integration abstraction from day one**, even with only GitHub and LeetCode live at MVP, so that Codeforces/GFG/HackerRank and future platforms are configuration additions, not re-architecture events.
10. **One owned templates/community surface**, unifying what v1.0 scattered as three separate footnotes (branch-specific roadmap templates, community habit templates, curated certification guidance) into a single layer — this is a genuine network-effect and growth lever if built intentionally, not three disconnected "future scope" bullets.

---

*End of PRD (v2.0 — Finalized).*