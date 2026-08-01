# Kensei — Product Glossary

> **Document Type:** Product Documentation  
> **Status:** Draft  
> **Product:** Kensei  
> **Purpose:** Define the terminology used consistently across Kensei's product, UI, documentation, database, APIs, and development.

---

## 1. Core Planning Concepts

### Goal

A **Goal** is a high-level outcome that a user wants to achieve within a defined or flexible time period.

Examples:

- Achieve a target GATE score.
- Become job-ready for cybersecurity roles.
- Complete a DSA roadmap.
- Build five portfolio projects.
- Improve physical fitness.

A Goal may contain multiple milestones, tasks, resources, and scheduled sessions.

---

### Milestone

A **Milestone** is a significant checkpoint within a Goal.

It represents measurable progress toward completing the larger Goal.

Example:

**Goal:** Prepare for GATE

**Milestones:**

- Complete Operating Systems theory.
- Complete DBMS theory.
- Finish all subject-wise PYQs.
- Begin full-length mock tests.

---

### Task

A **Task** is the smallest actionable unit of work assigned to a user.

Examples:

- Watch DBMS normalization lecture.
- Solve 10 GATE DBMS PYQs.
- Complete two LeetCode problems.
- Implement login API.
- Complete Cisco networking module.

Tasks may contain:

- Title
- Description
- Due date
- Estimated duration
- Priority
- Status
- Associated goal
- Associated milestone
- Resource links
- XP reward

---

### Subtask

A **Subtask** is a smaller action required to complete a Task.

Example:

**Task:** Build authentication module

**Subtasks:**

1. Create login UI.
2. Create registration UI.
3. Implement authentication API.
4. Connect frontend and backend.
5. Test authentication flow.

---

### Roadmap

A **Roadmap** is a structured sequence of milestones and tasks designed to move a user from their current state toward a Goal.

A roadmap may be:

- AI-generated
- User-created
- AI-generated and manually modified

Kensei should generate roadmaps based on factors such as available time, deadlines, resources, priorities, and user constraints.

---

### Daily Plan

A **Daily Plan** is the set of tasks and sessions scheduled for a particular day.

It represents what Kensei expects the user to work on that day.

---

### Schedule

A **Schedule** determines when tasks and sessions should occur.

Schedules may be generated or modified based on:

- Available hours
- Deadlines
- Goal priority
- Task duration
- Existing commitments
- Missed tasks
- User preferences

---

### Session

A **Session** is a defined period during which a user works on a specific task or activity.

Example:

> Operating Systems — Deadlocks  
> 7:00 PM – 7:45 PM

A session may optionally use the Pomodoro timer.

---

### Pomodoro

A **Pomodoro** is a focused work session managed through Kensei's built-in timer.

A typical structure may include:

- Focus period
- Short break
- Repeated focus cycles
- Long break

Pomodoro completion can contribute to productivity statistics and XP.

---

## 2. Goal Organization

### Workspace

A **Workspace** is a high-level organizational area containing related goals, projects, tasks, and resources.

Examples:

- Academics
- Career
- Projects
- Skills
- Fitness
- Personal

---

### Project

A **Project** is a structured collection of tasks and milestones that produces a concrete output.

Examples:

- Build Kensei
- Create cybersecurity portfolio project
- Build personal portfolio website
- Complete research project

Unlike a general Goal, a Project normally produces a specific deliverable.

---

### Skill

A **Skill** represents a capability the user wants to develop or track.

Examples:

- DSA
- React
- Python
- Cybersecurity
- Machine Learning
- Communication
- System Design

Skills may be connected to Goals, Projects, Resources, and completed Tasks.

---

### Priority

**Priority** represents the relative importance of a Goal, Milestone, or Task.

Possible levels:

- High
- Medium
- Low

Priority influences scheduling and roadmap generation.

---

### Deadline

A **Deadline** is the date or time by which a Goal, Milestone, Project, or Task should be completed.

---

## 3. Resources

### Resource

A **Resource** is external or internal material used to complete a Task, learn a Skill, or progress toward a Goal.

Resource types may include:

- Video
- Course
- Article
- Documentation
- Book
- PDF
- Website
- Coding problem
- GitHub repository
- Practice test

---

### Resource Link

A **Resource Link** is the URL associated with a Resource.

Examples may include links to:

- YouTube lectures
- LeetCode questions
- Codeforces problems
- Documentation
- GitHub repositories
- Learning platforms

---

### Learning Resource

A **Learning Resource** is a Resource specifically intended to teach or explain a concept required for a Task.

---

## 4. AI Concepts

### AI Planner

The **AI Planner** converts user Goals, deadlines, availability, priorities, resources, and constraints into actionable Roadmaps and Daily Plans.

It should assist with planning rather than removing user control.

---

### AI Mentor

The **AI Mentor** provides guidance about progress, priorities, planning decisions, consistency, and execution.

The Mentor focuses primarily on:

- What should I do?
- What should I prioritize?
- Why am I falling behind?
- How should my plan change?

---

### AI Teacher

The **AI Teacher** helps the user understand concepts required to complete learning-related Tasks.

The Teacher focuses primarily on:

- Explain this concept.
- Give me an example.
- Help me understand this question.
- Give me a hint.
- What should I study before this?

The Teacher should prefer learning assistance over simply completing academic work for the user.

---

### AI Recommendation

An **AI Recommendation** is a suggested action generated by Kensei.

Examples:

- Reschedule a missed task.
- Reduce today's workload.
- Revise a weak topic.
- Prioritize an approaching deadline.
- Modify a roadmap.

Recommendations should remain understandable and overridable by the user.

---

### Confidence Score

A **Confidence Score** represents Kensei's confidence in an AI-generated plan, prediction, recommendation, or scheduling decision.

It exists to avoid presenting uncertain AI outputs as absolute decisions.

---

### Rescheduling

**Rescheduling** is the process of moving unfinished or affected Tasks to another suitable time.

Kensei may suggest rescheduling when:

- A task is missed.
- Availability changes.
- A deadline changes.
- Progress is slower than expected.
- Higher-priority work appears.

The user should retain control over significant schedule changes.

---

## 5. Execution States

### Backlog

Tasks that exist but are not currently scheduled for execution.

---

### Planned

A Task that has been assigned to a future date or session.

---

### In Progress

A Task that the user has started but not completed.

---

### Completed

A Task that has satisfied its completion requirements.

---

### Missed

A scheduled Task that was not completed within its intended period.

A missed Task may trigger rescheduling.

---

### Skipped

A Task intentionally dismissed by the user for its scheduled occurrence.

Skipped and Missed should not necessarily be treated identically.

---

### Blocked

A Task that cannot currently proceed because another requirement or dependency is unresolved.

---

## 6. Gamification

### XP

**XP (Experience Points)** is Kensei's primary numerical representation of productive progress.

Users may earn XP by completing meaningful actions.

Examples:

- Completing Tasks
- Completing focused Sessions
- Reaching Milestones
- Maintaining consistency
- Completing Projects

XP should reward genuine progress rather than meaningless interaction with the application.

---

### Level

A **Level** represents accumulated progress based primarily on XP.

As users earn XP, they may advance through levels.

---

### Streak

A **Streak** represents consecutive periods of meaningful activity.

The exact qualification rules should be defined separately to prevent users from maintaining streaks through trivial actions.

---

### Streak Freeze

A **Streak Freeze** protects an existing streak under defined circumstances when a user cannot complete the normal streak requirement.

---

### Achievement

An **Achievement** is a recognition awarded when a user satisfies a meaningful condition.

Examples:

- First Goal completed
- First Project completed
- Seven-day consistency milestone
- 100 focused sessions
- Major learning milestone

---

### Rank

A **Rank** is a progression indicator associated with the Kensei experience.

Ranks may provide a more thematic representation of progress than numerical Levels.

The exact relationship between Rank, Level, and XP must be finalized before implementation.

---

## 7. Kensei Theme Concepts

### Kensei

**Kensei** is the product name.

The name is associated with mastery, discipline, deliberate improvement, and long-term progression.

The product should use this identity without compromising usability or turning the interface into excessive decoration.

---

### Theme Engine

The **Theme Engine** controls the visual and atmospheric presentation of Kensei independently from its core productivity functionality.

It may control:

- Colors
- Backgrounds
- Animations
- Sound effects
- Interface atmosphere
- Visual effects

---

### Samurai Theme

The **Samurai Theme** is Kensei's primary immersive visual identity.

Potential elements include:

- Dark visual environment
- Samurai-inspired imagery
- Sakura elements
- Dark-academia influence
- Cinematic transitions
- Subtle environmental animation
- Sword-inspired interaction sounds
- Door or lock interaction sounds

These elements should remain optional or controllable where they affect accessibility or concentration.

---

### Soundscape

A **Soundscape** is the collection of environmental and interaction sounds used to create Kensei's immersive atmosphere.

Sound should never be required for functionality.

---

### Microinteraction

A **Microinteraction** is a small visual, motion, or audio response triggered by a user action.

Examples:

- Completing a Task
- Leveling up
- Opening navigation
- Starting a focus session
- Unlocking an Achievement

---

## 8. Dashboard & Navigation

### Dashboard

The **Dashboard** is the primary overview screen after authentication.

It should surface the most important information required for the user to decide what to do next.

Potential information includes:

- Today's Tasks
- Current Goals
- Upcoming deadlines
- Progress
- Streak
- XP
- Scheduled sessions

---

### Sidebar

The **Sidebar** is Kensei's primary navigation component.

The intended interaction includes a compact icon-based state that can expand to reveal navigation labels.

---

### Calendar

The **Calendar** provides a time-based representation of Tasks, Sessions, deadlines, and other scheduled activities.

---

### Daily Planner

The **Daily Planner** is the execution-oriented interface showing what the user should work on during a particular day.

---

### Goal Setup Wizard

The **Goal Setup Wizard** is the guided onboarding flow used to collect the information required to create a Goal and generate its initial Roadmap.

Potential inputs include:

- Goal
- Deadline
- Current level
- Available time
- Resources
- Constraints
- Desired completion timeline

---

## 9. Progress & Analytics

### Progress

**Progress** represents measurable advancement toward a Goal, Milestone, Skill, or Project.

Progress should preferably be based on meaningful completion data rather than application usage alone.

---

### Completion Rate

**Completion Rate** is the proportion of planned Tasks successfully completed during a defined period.

---

### Productivity Analytics

**Productivity Analytics** summarizes execution patterns over time.

Potential measurements include:

- Task completion
- Focus time
- Streak consistency
- Goal progress
- Planned versus completed work

---

### Activity History

**Activity History** records relevant user actions and progress events over time.

---

## 10. Integrations

### Integration

An **Integration** connects Kensei with an external service to exchange or verify useful data.

---

### GitHub Integration

The **GitHub Integration** may use repository and contribution information to support development-related progress tracking.

---

### LeetCode Integration

The **LeetCode Integration** refers to Kensei's ability to associate coding-practice activity with DSA Goals and Tasks.

Because third-party API availability may be limited, the exact implementation must be determined during architecture design.

---

### Manual Verification

**Manual Verification** allows users to record progress when automatic integration is unavailable or inappropriate.

---

## 11. Notifications

### Notification

A **Notification** informs the user about something requiring awareness or action.

Examples:

- Upcoming Task
- Missed Task
- Deadline approaching
- Roadmap adjustment
- Achievement unlocked

---

### Reminder

A **Reminder** is a time-based notification associated with a Task, Session, Goal, or deadline.

---

## 12. User & System Concepts

### User

A **User** is a person with a Kensei account.

---

### Profile

A **Profile** stores user-specific information and preferences required to personalize the Kensei experience.

---

### Preference

A **Preference** is a configurable user setting.

Examples:

- Theme
- Sound
- Notifications
- Pomodoro duration
- Scheduling preferences

---

### Constraint

A **Constraint** is a limitation that the planning system must consider.

Examples:

- College hours
- Work schedule
- Maximum available study hours
- Fixed commitments
- Unavailable days

---

### Availability

**Availability** represents the periods during which a user can realistically perform Tasks or Sessions.

---

## 13. Product Version Terminology

### MVP

**MVP (Minimum Viable Product)** is the smallest usable version of Kensei that proves the core product concept.

It should contain only the functionality necessary to test whether users can:

> Define a Goal → receive a realistic plan → execute Tasks → track progress → adapt the plan.

---

### V1

**V1** is the first broader production version after the MVP.

It may introduce functionality intentionally excluded from the MVP.

---

### V2

**V2** represents a later expansion of Kensei after the core product has been validated.

Features should not be assigned to V2 simply because they are difficult; version placement should reflect product priority.

---

## 14. Terminology Rules

The following rules should be followed throughout the Kensei project:

1. Use **Goal** for high-level outcomes.
2. Use **Milestone** for significant checkpoints inside Goals.
3. Use **Task** for actionable units of work.
4. Use **Subtask** only for actions contained within a Task.
5. Use **Project** when work produces a defined deliverable.
6. Use **Session** for a scheduled period of focused work.
7. Use **Resource** for material associated with learning or execution.
8. Use **XP** consistently for experience points.
9. Do not use multiple names for the same product entity without an explicit UI/theme reason.
10. The Samurai theme must not change the underlying product/data terminology unless a documented mapping exists.

---

## 15. Terms Requiring Product Decisions

The following concepts require further discussion before architecture is finalized:

- Relationship between Goal and Project
- Relationship between Level and Rank
- Exact XP calculation
- Streak qualification rules
- Streak Freeze rules
- Task completion verification
- AI confidence scoring
- Automatic versus manual rescheduling
- Workspace structure
- Goal dependencies
- Task dependencies
- Recurring Tasks
- Calendar event behavior
- GitHub progress verification
- LeetCode progress verification
- AI Mentor memory
- AI Teacher conversation history
- Notification rules
- Achievement system
- Theme terminology mappings

These decisions should be recorded in `DECISIONS.md` before their implementation affects the database or API design.

---

## Document Maintenance

This glossary is a **living product document**.

When a new core concept is introduced into Kensei:

1. Define it here.
2. Confirm that it does not duplicate an existing concept.
3. Record significant product decisions in `DECISIONS.md`.
4. Use the same terminology in the PRD, architecture, database, APIs, UI, and code.

---

**End of Product Glossary**