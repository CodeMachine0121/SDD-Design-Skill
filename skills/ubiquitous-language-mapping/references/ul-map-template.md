# 📔 Ubiquitous Language Map (Design)

**Product:** ____________________
**Product Area / Context:** ____________________
**Maintainer:** ____________________
**Last Updated:** YYYY-MM-DD

---

## 1. Concepts & Objects
*The things users talk about, and how each one is named across the design.*

| Domain / User Term | Component or Pattern Name | User-Facing Label | Definition & UX Rules | Status |
| :--- | :--- | :--- | :--- | :--- |
| *(the canonical word a user would use)* | *(name in the design file / component library)* | *(the label actually shown on screen)* | *(what it is, plus any UX constraints — e.g. "max 3 shown, rest collapse")* | *(Archeology / Confirmed / To Be Deleted)* |
| | | | | |
| | | | | |

---

## 2. Actions & Interactions
*Business/user actions, the interaction pattern that carries them, and the experience they produce.*

| User Action | Interaction Pattern | Trigger | Experience Result | Notes |
| :--- | :--- | :--- | :--- | :--- |
| *(action as a user describes it — e.g. "送出訂單")* | *(the pattern — e.g. primary button + confirm dialog)* | *(what the user does to start it)* | *(the feedback / state change / navigation the user perceives)* | |
| | | | | |
| | | | | |

---

## 3. Ambiguities & Conflicts
*Same word meaning different things on different screens, or several labels pointing at one concept.*

| Ambiguous Term / Label | Meaning in Context A | Meaning in Context B | Resolution |
| :--- | :--- | :--- | :--- |
| *(the clashing word or label)* | *(what it means on screen/flow X)* | *(what it means on screen/flow Y)* | *(agreed canonical wording or disambiguation)* |
| | | | |
| | | | |

---

## 4. States & Token Mapping
*Status values and magic strings, and how each one is represented visually.*

| Category | Value / Key | Domain Label | Visual Representation / Token |
| :--- | :--- | :--- | :--- |
| *(e.g. Order Status)* | *(e.g. 1, 2, 3 or "PENDING")* | *(e.g. 待付款, 已完成)* | *(e.g. warning badge / `color.status.pending`)* |
| | | | |
| | | | |

---

## Quick Start Guide
1. **Archeology** — read existing screens/prototypes and the design file; fill `Component or Pattern Name` and `User-Facing Label` with what is actually there.
2. **Mapping** — check the Design Brief and ask the product owner; fill `Domain / User Term` with the correct canonical word users use.
3. **Refine** — add UX rules (e.g. "this list caps at 5, then shows 'view all'"; "this action needs confirmation").
4. **Sync** — this document is the single authoritative dictionary for all future naming of components, labels, states, and tokens across briefs, wireframes, and built designs.
