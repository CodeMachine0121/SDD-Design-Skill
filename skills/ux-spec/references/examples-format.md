# Flow-Scenarios-for-Confirmation Format (Step 3)

These scenarios become the feature's **acceptance criteria** in the Design Brief — the single home for all requirement examples and AC. Get them right here; nothing downstream restates them.

Present the concrete flow scenarios using this structure:

```
**[Scenarios for Confirmation]**

Flow / rule: <the part of the journey or the branch this set of scenarios pins down>

| # | Given (only relevant situation) | When (user action) | Then (experience / outcome) |
|---|---|---|---|
| 1 (happy)     | <situation>  | <what the user does> | <the experience they get> |
| 2 (boundary)  | <situation>  | <what the user does> | <the experience they get> |
| 3 (exception) | <situation>  | <what the user does> | <the experience they get> |

<repeat one block per flow / rule when there are several>
```

Reminders for the content of the table:

- **Data minimality** — each row carries only the situational facts that change the flow or the experience; leave incidental detail out (or mark it `—`).
- **Experience language** — describe the situation, the user's action, and the experience they get as a user would tell it, not as the UI implements it. A `Then` is a felt/observable outcome ("導向確認頁並看到成功訊息", "被擋下並被告知原因"), never a visual spec ("彈出紅色 toast").
- **Cover the branches** — happy path, every boundary (first-time / empty / limit / last valid), every exception (invalid input, no permission, interruption, offline, nothing found).
