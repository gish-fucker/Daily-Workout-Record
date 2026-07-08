# Beginner Daily Coach v1 Design

## Goal

Turn the app from a general habit and workout logger into a beginner-friendly daily fitness coach. The first commercial-quality experience should help a new gym user answer one question quickly:

> What should I do today, based on how my body feels and what I have recently trained?

The feature should keep the current local-first, no-account prototype intact while making the first screen feel more like a product with judgment, guidance, and momentum.

## Target User

The primary user is a fitness beginner who trains mostly in a gym, with occasional home workouts. They know basic goals such as losing fat, building muscle, or getting healthier, but they may not know how to plan sessions, when to rest, or how to interpret soreness and fatigue.

The product should avoid expert-heavy language. It should explain suggestions in plain Chinese, reduce anxiety, and encourage consistent training over maximal performance.

## Product Positioning

The product should become:

> A daily training and body-state coach for fitness beginners.

It is not a pure habit tracker, and it is not trying to beat advanced workout logs on power-user depth. Its edge is combining daily condition, soreness, pain, water, sleep, and recent training into a simple recommendation.

## Core Experience

The Today tab should open with a new "今日建议" decision area above the existing daily form. This area should summarize the user's readiness and recommend one next action.

The card should include:

- Status: "正常练", "轻量练", or "恢复日".
- Suggested workout: a beginner plan such as "全身入门 35 分钟", "上肢入门 30 分钟", or "恢复拉伸 15 分钟".
- Reason: 2 to 3 short evidence points from sleep, energy, soreness, pain, water, and recent workouts.
- Primary action: start the suggested workout.
- Secondary action: only record daily state.

The current daily state form remains available below the coach card. The goal is not to hide recording, but to stop making the form the first emotional impression.

## Recommendation Logic

Version 1 should use local deterministic rules. It should not depend on an online AI call.

Inputs:

- Today's sleep hours.
- Today's energy score.
- Today's soreness score.
- Today's pain score.
- Today's water amount.
- Workouts in the last 7 days.
- High RPE workouts in the last 7 days.
- Last workout date and rough muscle focus if available.

Output:

- Readiness status.
- Suggested workout template.
- Short explanation.
- Caution note when pain or fatigue is high.

Rules:

- If pain is 4 or higher, recommend a recovery day and avoid loaded training.
- If sleep is under 6 hours and soreness is 4 or higher, recommend light training or recovery.
- If energy is 4 or higher, pain is 1 or lower, and there has been no workout in the last 2 days, recommend a normal beginner workout.
- If the user has trained hard 2 or more times in the last 3 days, recommend light training or recovery.
- If water is under 1500 ml, include a hydration reminder but do not make hydration alone block training.
- When data is missing, show a gentle starter recommendation instead of an empty dashboard.

The system should explain each recommendation with concrete reasons. Avoid vague claims such as "AI thinks you should rest".

## Beginner Templates

The app should ship with a small set of default beginner templates. These templates should be available to the recommendation system and visible in the workout/template area.

Initial templates:

- Full Body Beginner: squat pattern, push, pull, hinge or glute, core.
- Upper Body Beginner: push, pull, shoulder, arms or core.
- Lower Body Beginner: squat or leg press, hinge, glute, calf or core.
- Recovery Home Session: mobility, light stretching, walking or bodyweight movement.

Each exercise should include beginner-safe default sets, reps, and target RPE. The product should prefer moderate RPE and consistency over personal records.

## Data Flow

The feature should reuse the existing local app state:

- Daily logs remain the source for body-state inputs.
- Workout history remains the source for recent training and RPE.
- Templates remain the source for suggested workouts.
- Settings can later store preferred training environment, but v1 can default to gym-first.

The recommendation should be derived at render time from local state. It should not create a new stored recommendation object unless the user starts a workout from the suggestion.

When the user starts the suggested workout, the app should prefill the workout editor with the selected template and use a title such as "今日建议 - 全身入门".

## Interface Design

The coach card should feel calm, clear, and action-oriented:

- Use one strong status phrase.
- Keep explanations short.
- Use visual hierarchy instead of dense paragraphs.
- Make the primary action obvious.
- Avoid medical-looking warnings unless pain is high.

Desktop layout can show the recommendation, evidence, and actions in a balanced horizontal layout. Mobile layout should stack into one clear column.

The design should avoid a marketing hero. This is an app surface, not a landing page.

## Error And Empty States

If no daily log exists for today:

- Show a starter state: "先记录今天的睡眠、精力和酸痛，我会给你今天的训练建议。"
- Keep a shortcut to the daily form.

If no workouts exist:

- Recommend a gentle full-body beginner workout.
- Explain that the app will become more accurate after several training logs.

If data is contradictory:

- Pain and safety win over enthusiasm.
- Show the conservative recommendation and explain the limiting factor.

## Testing And Verification

Implementation should be verified with:

- JavaScript syntax checks.
- Browser check on desktop and mobile widths.
- Empty-state scenario with no local data.
- Beginner scenario with low fatigue and no recent training.
- Recovery scenario with high pain or soreness.
- Layout check for no horizontal overflow.

## Non-Goals For This Version

This version should not add:

- Accounts or cloud sync.
- Payment or subscription UI.
- Online AI integration.
- Advanced periodization.
- Social sharing.
- Medical diagnosis or injury treatment advice.

These can become later commercial milestones after the daily coach loop feels valuable.

## Success Criteria

The feature is successful when a first-time beginner can open the Today tab and understand, within a few seconds:

- Whether they should train today.
- What kind of session to do.
- Why the app recommends it.
- How to start without building a plan manually.

This should make the app feel less like a database and more like a beginner coach.
