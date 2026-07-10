# Retention Insights v1 Design

## Goal

Move the Insights tab closer to a commercial-quality retention surface. The app should not only record daily state and workouts; it should help a beginner understand what the last week means and what to do next.

This version should make the product feel useful after repeated use while preserving the current local-first, no-account prototype.

## Target User

The user is a fitness beginner who has started logging body state and workouts but does not yet know how to interpret patterns. They may wonder:

- Am I training often enough?
- Am I pushing too hard?
- Is poor sleep or pain affecting training?
- What should I adjust next week?

The language should stay plain Chinese, practical, and conservative. Avoid expert-heavy sports science terms unless the UI explains them immediately.

## Product Positioning

This feature turns the Insights tab into:

> A local weekly review center for beginner training, recovery, and next actions.

It is not a paid upgrade page, medical assessment, or advanced analytics dashboard. It should create the foundation for later Pro features such as AI weekly reports, fatigue risk analysis, and richer charts.

## Core Experience

Add a "复盘中心" near the top of the Insights tab, after the readiness panel and before the existing weekly review or summary cards.

The review center should include four sections:

1. 本周摘要
   - Training count.
   - Total completed sets.
   - Average sleep.
   - Hydration target days.
   - Average pain or soreness.

2. 风险提醒
   - Local-rule warnings for high pain, low sleep, dense training, high RPE, or missing data.
   - Each warning should explain the signal and the recommended response.
   - If there are no strong warnings, show a positive but grounded stability state.

3. 下周行动
   - Two or three concrete actions for the next week.
   - Actions should be specific enough to do without planning, such as keeping two beginner sessions, choosing one core lift to repeat, or using a recovery day when sleep is low.

4. 本地报告
   - A button to export or copy a local text/Markdown weekly report.
   - The report should summarize the same review center data and suggestions.
   - No network call is required.

## Recommendation Logic

All logic should be deterministic and derived from local state.

Inputs:

- Daily logs in the last 7 days.
- Workouts in the last 7 days.
- Sleep hours.
- Water amount.
- Energy, soreness, and pain scores.
- Workout session RPE.
- Completed set count.

Core rules:

- If average pain is 2.5 or higher, show a recovery risk and suggest reducing loaded training.
- If any daily pain is 4 or higher, show a stronger caution and recommend avoiding painful movements.
- If average sleep is below 6.5 hours, show a sleep recovery risk.
- If there are 2 or more high-RPE workouts in 7 days, suggest not adding weight next week.
- If there are 2 or more high-RPE workouts within 3 days, show a density warning.
- If there are 0 workouts but at least 3 daily logs, suggest starting with one beginner full-body session.
- If there are fewer than 3 daily logs and fewer than 1 workout, show a data coverage state instead of pretending the review is accurate.
- If training is 2 to 3 sessions, pain is low, and sleep is stable, suggest maintaining rhythm and repeating one core movement.

The system should avoid overconfident phrasing. Use wording such as "更适合", "建议先", and "下周可以".

## Data Flow

Use the existing local state:

- `state.dailyLogs` provides body-state data.
- `state.workouts` provides training data.
- Existing helper functions such as `getRecent`, `average`, `countSets`, `formatMetric`, and `escapeHtml` should be reused where appropriate.

No new persistent schema is required for v1.

The report export can be generated on demand from derived review data. If using a download, create a Blob in the browser and name the file with the current local date.

## Interface Design

The review center should feel like a focused app surface, not a marketing hero.

Desktop:

- Use a compact header with the review period and confidence label.
- Use a metric grid for the summary.
- Show risk and action items in readable panels.
- Keep the export action visible but secondary.

Mobile:

- Stack all sections vertically.
- Keep touch targets full-width where useful.
- Avoid horizontal overflow.
- Keep text concise so the review is scannable.

Visual tone:

- Calm and work-focused.
- Use the existing color system and panel language.
- Avoid alarmist medical styling.

## Empty And Low-Data States

If there is not enough data:

- Show what is missing, such as "还差 3 天状态记录" or "还差 1 次训练记录".
- Provide next actions that help the user generate useful data.
- Still allow report export, but label it as a starter report.

If there is no data at all:

- The review center should not look broken.
- It should explain that weekly insight becomes useful after 3 daily logs and 1 workout.

## Report Format

The local report should be plain text or Markdown and include:

- Title and date range.
- Summary metrics.
- Risk reminders.
- Next-week actions.
- Local-first privacy note.

The report should not include raw full history. It is a readable weekly summary, not a backup file.

## Error Handling

- If report generation fails, show a toast with a clear local error.
- If browser download APIs are unavailable, copy the report text to the clipboard if possible.
- If both download and clipboard fail, show the report in a visible text area or modal-like panel is acceptable only if it follows existing UI patterns.

## Testing And Verification

Implementation should be verified with:

- JavaScript syntax checks.
- Existing smoke test continues passing.
- Empty-data review center state.
- Low-data review center state.
- Enough-data stable week state.
- High-risk week state with high pain or high RPE.
- Report export produces text containing summary, risks, and next actions.
- Desktop and mobile layouts have no horizontal overflow.

## Non-Goals

This version should not add:

- Accounts.
- Cloud sync.
- Payment or subscription UI.
- Online AI dependency.
- Medical diagnosis.
- Advanced exercise progression charts.
- Social sharing.

Those belong to later commercial milestones after the weekly review loop proves valuable.

## Success Criteria

The feature is successful when a returning user can open Insights and understand within one minute:

- What happened this week.
- Whether any recovery or training-load signal needs attention.
- What to do next week.
- How to export a readable local report.

The product should feel less like a logbook and more like a coach that remembers enough to help.
