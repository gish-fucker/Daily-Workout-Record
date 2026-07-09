# Beginner Onboarding v1 Design

## Goal

Make the first session feel guided, useful, and low-pressure. A new fitness beginner should understand what the product does, record enough body-state data for the daily coach, and reach a useful recommendation without reading documentation.

The goal is activation, not account setup. The first session should help the user reach:

> I know what to do today, and the app feels like it understands my current state.

## Why This Matters

The current app already has a daily coach, starter guide, workout templates, and execution mode. The remaining gap is first-use clarity. A new user can see many useful surfaces, but the app does not yet clearly lead them through the first valuable loop.

Commercial-quality onboarding should reduce hesitation, not collect every preference upfront.

## Target User

The onboarding is for a fitness beginner who may be uncertain about:

- What information matters.
- Whether soreness or pain means they should rest.
- How much they need to fill in before the app can help.
- Whether they need to create a plan manually.

The tone should be calm and practical. Avoid hype, expert jargon, or long setup forms.

## Core Activation Path

The first useful loop is:

1. Record today's body state.
2. Receive today's training recommendation.
3. Start a beginner workout or recovery session.
4. Save at least one real completed set.
5. See a completion summary.

Onboarding v1 should focus on steps 1 to 3. Step 4 and 5 are already supported by workout execution mode.

## Proposed Experience

Replace the current generic starter guide with a more direct activation panel shown only while the user has no daily logs and no workouts.

The panel should sit near the top of the Today tab, below the daily coach or visually connected to it.

It should include:

- A short promise: "先记录 4 个状态，我会给你今天怎么练的建议。"
- A progress checklist:
  - 睡眠
  - 精力
  - 酸痛
  - 疼痛
- A primary action: "开始 60 秒记录"
- A secondary action: "直接看今日建议"
- A privacy note: "数据先保存在本机，可随时导出。"

The daily form should remain the source of truth. Onboarding should guide the user to the right fields, not create a separate data model.

## Interaction Details

### Start 60-Second Record

Clicking the primary action should:

- Scroll to the daily form.
- Highlight the sleep, energy, soreness, and pain controls.
- Keep the user in the Today tab.
- Not open a modal.

The user can still edit water, mood, habits, and note, but the onboarding should make clear that sleep, energy, soreness, and pain are enough to get started.

### Progress Checklist

The checklist should update live:

- Sleep is complete when `sleepHours` has a value.
- Energy is always available because the range has a default, but it should count as confirmed only after the user changes it or saves the daily log.
- Soreness is confirmed after change or save.
- Pain is confirmed after change or save.

If tracking "changed" state is too much for v1, the first implementation can use saved daily log presence as the completion state and keep the checklist informational.

### Directly See Today's Suggestion

The secondary action should keep the user at the daily coach and explain that the suggestion is starter-level until body-state data is saved.

Do not block the user from starting a workout before onboarding is complete.

## Copy Direction

Good copy:

- "先记录今天身体状态，我会给你今天怎么练的建议。"
- "不知道怎么填也没关系，先按现在的感觉。"
- "疼痛高时，我会优先建议恢复。"
- "数据先保存在本机。"

Avoid:

- "完成个人档案"
- "训练算法初始化"
- "最大化你的表现"
- "马上变强"

## Data Flow

Use existing local state:

- `dailyLogs` determines whether onboarding is complete.
- Current form values can drive live checklist UI.
- No new persistent onboarding schema is required for v1.

If a future version needs richer progress, it can add a small settings key such as `settings.onboardingDismissedAt`, but v1 should avoid this unless implementation clearly needs it.

## Interface Design

The onboarding panel should feel like a compact product guide, not a marketing hero:

- Use the existing card radius and restrained visual language.
- Keep the panel compact enough that the daily coach remains visible above the fold on desktop.
- On mobile, put the action buttons before long explanatory text.
- Use checklist rows rather than large decorative cards.

## Empty And Returning States

Show onboarding when:

- `dailyLogs.length === 0`
- `workouts.length === 0`

Hide onboarding when:

- The user has saved at least one daily log, or
- The user has saved at least one workout.

If the user only starts a suggested workout but does not save anything, onboarding can remain visible. The product should still encourage a real first record.

## Testing And Verification

Implementation should verify:

- Empty state shows onboarding.
- Saving a daily log hides onboarding.
- Saving a workout hides onboarding.
- Primary action scrolls to the daily form.
- Daily coach still appears and works.
- Existing smoke test continues passing.
- Mobile width has no horizontal overflow.

## Non-Goals

This version should not add:

- Account registration.
- Goal-setting questionnaire.
- Body measurement setup.
- Notification permission prompts.
- Subscription prompts.
- Long tutorial slides.

These can come later after the first value loop is strong.

## Success Criteria

The feature is successful when a new user can open the app and quickly understand:

- What to record first.
- Why those fields matter.
- That the app will give a training recommendation after basic state data.
- That their data is local-first.

The first-use experience should feel like a helpful coach opening the door, not a form asking for homework.
