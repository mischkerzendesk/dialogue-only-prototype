---
type: reference
project: gen3-email
date: 2026-04-24
tags: [dialogue-only, use-cases, intents, scoping, affirmative, negative]
status: draft
---

# Dialogue-Only Use Cases - User Guide

## Overview

**Dialogue-only** is a scoping feature that restricts when certain use cases (intents) can be matched. When a use case is marked as "Dialogue only," it will **only trigger inside dialogue flows** and will **not** be matched during general conversation.

This is essential for use cases like "Affirmative" (yes) and "Negative" (no) that represent customer responses within a specific dialogue context, not standalone requests.

**🎯 [View Interactive Prototype](https://mischkerzendesk.github.io/dialogue-only-prototype/)**

---

## The Problem This Solves

### Without Dialogue-Only Scoping

When use cases like "Affirmative" or "Negative" are active globally:

```
Customer: "Yes"
AI Agent: [Matches "Affirmative" use case]
AI Agent: [No reply configured → falls back to RAG]
AI Agent: [RAG has no relevant response for "yes"]
AI Agent: "I couldn't find an answer to that" [conversation broken]
```

The bot doesn't know what the customer is saying "yes" to, because the context comes from a dialogue flow. Matching it as a standalone intent triggers RAG fallback, which can't provide a meaningful response to context-free "yes/no" statements. This interrupts and breaks the conversation flow.

### With Dialogue-Only Scoping

```
[Inside a dialogue about choosing a product color]
AI Agent: "Would you like the red or blue version?"
Customer: "Yes, the red one"
AI Agent: [Matches "Affirmative" within dialogue context]
AI Agent: [Continues the dialogue flow with proper context]
```

The use case only triggers when it makes sense - inside the dialogue where "yes" has clear meaning.

---

## Common Dialogue-Only Use Cases

### 1. **Affirmative**
- **Purpose:** Matches customer confirmations (yes, sure, correct, that's right, etc.)
- **Reason for request:** "Customer confirms"
- **Why dialogue-only:** Without context, "yes" is meaningless - "Yes" to what?

**Example phrases:**
- "Yes"
- "Correct"
- "That's right"
- "Yeah, that's it"
- "Sure"
- "Yep"

### 2. **Negative**
- **Purpose:** Matches customer rejections or disagreements (no, that's wrong, not that, etc.)
- **Reason for request:** "Customer declines" or "Customer disagrees"
- **Why dialogue-only:** Like affirmative, "no" needs context - "No" to what?

**Example phrases:**
- "No"
- "Nope"
- "That's not right"
- "Not that one"
- "Incorrect"
- "Wrong"

### 3. **Choose Color / Selection Options**
- **Purpose:** Matches when a customer selects from a set of options presented in a dialogue
- **Why dialogue-only:** The available options only exist within the dialogue context

**Example phrases:**
- "Red"
- "The blue one"
- "Option 2"
- "I'll take the first one"

---

## How to Configure

### Marking a Use Case as Dialogue-Only

1. Navigate to **AI agents > Content > Use cases**
2. Click on the use case you want to scope (e.g., "Affirmative")
3. In the use case settings, find the **"Dialogue only"** checkbox
4. Check the box to enable dialogue-only scoping
5. Review the helper text to confirm this is the right setting
6. Click **Save**

### When the System Recommends It

The platform may show a warning banner when it detects potential dialogue-only candidates:

> "3 use cases may need to be marked as **Dialogue only**"

**Indicators that suggest a use case should be dialogue-only:**
- The use case has **0 replies** configured
- The use case is **linked to one or more dialogues**
- The use case would trigger **RAG fallback** with no meaningful response if matched outside a dialogue

Click **"Review"** on the banner to see which use cases are flagged and bulk-apply the setting.

---

## Anatomy of the Setting

### In the Use Case Detail View

**Checkbox label:** "Dialogue only"

**Helper text:**
> "When enabled, this use case will only be matched inside dialogue flows. It will not trigger during general conversation."

**Context hint (appears when relevant):**
> "This use case has no replies and is linked to a dialogue. Without this flag, it will trigger RAG fallback when matched outside a dialogue, which may break conversation flow."

---

## How It Works

### Use Case Matching Flow

#### Dialogue-Only: Disabled (Default)
```
Customer message
  ↓
Task identification runs on ALL use cases
  ↓
If "Affirmative" matches → triggers use case globally
  ↓
No reply configured → falls back to RAG
  ↓
RAG has no response → default reply (conversation broken)
```

#### Dialogue-Only: Enabled
```
Customer message
  ↓
Are we inside a dialogue flow?
  ↓                          ↓
YES                         NO
  ↓                          ↓
Include "Affirmative"     Exclude "Affirmative"
in matching               from matching
  ↓                          ↓
Trigger within dialogue   Match other use cases
with proper context       for general conversation
```

### Reply Method Consideration

When a use case is dialogue-only:
- **Reply method:** Set to "Use dialogue" (not "Use procedure" or "Use reply")
- **Dialogues tab:** Should show at least 1 linked dialogue
- **Procedure/Reply tabs:** Typically empty (not needed for contextual responses)

---

## Best Practices

### 1. Always Mark Yes/No Use Cases as Dialogue-Only

Unless you have a specific reason not to, intents like "Affirmative" and "Negative" should **always** be dialogue-only. There's almost no scenario where a standalone "yes" message should trigger outside a dialogue.

### 2. Review Use Cases with Zero Replies

If a use case has:
- 0 replies configured, AND
- 1+ dialogues linked, AND
- Not marked as dialogue-only

→ It's likely a dead-end risk. Mark it as dialogue-only.

### 3. Be Careful with Selection Intents

Use cases that represent menu selections ("Choose color", "Select size", etc.) should generally be dialogue-only **unless** your customers frequently send these as standalone requests.

**Example exception:**
- If customers commonly say "I want the blue one" as an initial message (not in response to options), you may want the "Choose color" intent to be globally available with a clarifying reply like:
  - "I'd be happy to help! Which product are you referring to?"

### 4. Test Behavior After Marking

After enabling dialogue-only:
1. Test the use case **inside** its linked dialogue → should work
2. Test the use case phrase **outside** any dialogue → should NOT trigger that use case
3. Verify that general conversation still matches appropriate use cases

### 5. Document Scoped Use Cases

In teams with multiple admin users, add a note to the use case's category or description indicating it's dialogue-scoped:
- **Category:** "Dialogue flow - responses"
- **Internal note:** "Scoped to dialogues only - do not make globally available"

---

## Troubleshooting

### "My dialogue isn't triggering the Affirmative use case"

**Check:**
1. Is "Dialogue only" enabled on the Affirmative use case?
2. Is the dialogue correctly linked to the Affirmative use case?
3. Is the Affirmative use case set to "Active" status?
4. Review the dialogue flow to ensure it's properly structured to expect a yes/no response

### "Customers get unhelpful responses when they say 'yes'"

**This is expected if:**
- They're saying "yes" **outside** a dialogue context
- The use case is NOT marked as dialogue-only and has no general reply
- System falls back to RAG, which can't provide a meaningful response to context-free "yes"

**Solutions:**
1. Mark the use case as dialogue-only (recommended)
2. OR add a general reply like: "I'm happy to help! Could you provide more details about what you're confirming?"

### "The system flagged 3 use cases, but I only want to scope 2"

You can selectively apply dialogue-only scoping:
1. In the modal, **uncheck** the use case you want to keep globally available
2. Click "Apply" to scope only the checked items
3. Configure a general reply for the globally-available use case to avoid RAG fallback on context-free responses

### "Can I make a use case available both in dialogues AND general conversation?"

Yes - this is the **default behavior** when dialogue-only is disabled. However:
- You **must** configure a reply or procedure for the general case
- The use case will match everywhere, which may create confusion if the phrase is context-dependent

---

## Advanced: Scoping Multiple Use Cases

### Bulk Apply via Modal

When the system recommends multiple use cases for dialogue-only scoping:

1. Click **"Review"** on the warning banner
2. A modal appears with suggested use cases
3. Each has a checkbox (checked by default)
4. Shows metadata: "0 replies • 1 dialogue"
5. **Uncheck** any you want to exclude
6. Click **"Mark as Dialogue only"** to apply

### Manual Review Process

If you prefer to review each use case individually:
1. Sort use cases by "Replies: 0"
2. Filter by "Active" status
3. For each zero-reply use case:
   - Check if it's linked to dialogues
   - Determine if it's context-dependent (yes/no/selection)
   - Mark as dialogue-only if appropriate

---

## FAQ

**Q: What happens if I mark a use case as dialogue-only but it has replies configured?**
A: The dialogue-only flag will still apply - the use case will only match inside dialogues. The replies will be ignored since the dialogue flow will handle the response.

**Q: Can I have different replies for dialogue context vs. general context?**
A: No. If you need context-aware replies, you must:
- Keep the use case globally available (dialogue-only OFF)
- Use a procedure with conditional logic based on conversation history
- OR create two separate use cases (one dialogue-only, one general)

**Q: Will dialogue-only use cases show up in analytics?**
A: Yes, but they'll only appear for matches that occurred inside dialogues. You won't see them in general conversation metrics.

**Q: Can I use dialogue-only with procedures instead of dialogues?**
A: The "Dialogue only" flag specifically scopes to dialogue flows. Procedures don't have the same bounded context. If you're using procedures, you should:
- Configure a reply/procedure for the general case
- Handle context-awareness within the procedure logic

**Q: What if I'm migrating from dialogues to procedures?**
A: During migration:
1. Keep dialogue-only enabled on contextual use cases
2. Build your procedures to handle the contextual responses
3. Test the procedure flow thoroughly
4. Once procedures are live, you can disable dialogue-only if the procedure handles all contexts

**Q: Is there a "Procedure only" equivalent?**
A: Not currently. Procedures are designed to be general-purpose and context-aware by default.

---

## Related Features

- **Dialogues:** The flow builder where dialogue-only use cases are matched
- **Procedures:** AI-powered flows that can handle context dynamically
- **Task Identification:** The engine that matches customer messages to use cases
- **Clarifying Questions:** Helps the AI gather context when intents are unclear (separate from dialogue-only)

---

## Support

For additional help:
- Contact your Customer Success Manager
- Submit a ticket via the Help Center
- Review the "Dialogues" documentation for flow-building guidance

---

## Resources

- **Interactive Prototype:** https://mischkerzendesk.github.io/dialogue-only-prototype/
- **GitHub Repository:** https://github.com/mischkerzendesk/dialogue-only-prototype

---

*Last updated: 2026-04-24*
*Feature status: Generally Available*
