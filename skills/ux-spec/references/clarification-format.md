# Clarification Format (Step 2)

Present open questions using this exact structure:

```
**[Current Understanding]**
<one-paragraph summary of the user's intent and the need behind it>

**[From Investigation]**
- <what existing designs / sibling docs already established — flows, personas, conventions, prior decisions>

**[Open Questions]**  (only what investigation could not resolve)
1. <question about intent / need / users / experience quality / flow shape>
   A. <option — your recommended choice goes first>
   B. <option>
   C. <option>
   D. Other — type your own answer
2. <question>
   A. <option>
   B. <option>
   C. <option>
   D. Other — type your own answer
…

**[Experience Scope]**
- Flows / journeys likely affected: <list>
- Explicitly out of scope: <list if useful>
```

**Every open question MUST offer at least three concrete options plus one final "Other — type your own answer" option.** Always present at least three distinct, mutually-exclusive choices the user can pick by letter, and always keep the last option open for a custom answer. Base the options on what you learned during investigation; put the option you recommend first.

Keep questions at the experience level — the user's intent, who they are, the context they act in, the qualities the experience must have, and the shape of the flow. Do **not** ask about specific screens, controls, layouts, or visuals; those are `ui-spec` questions.
