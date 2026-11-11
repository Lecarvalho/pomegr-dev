---
title: "Agent chat"
description: "Ask Aril, the Pomegr task-board agent, to cut work into tasks, order the board, advise on models, propose starts, and turn the queue on or off. You apply its proposals."
---

# Agent chat

**Aril** is the agent built into Pomegr that helps you organize a repository's task
board. You ask in a chat panel on the right side of the window. Aril reads the board,
the repository's files, and what Pomegr observed, then answers. Aril proposes board
changes, and you apply them.

Aril records links when the evidence shows that one task or pull request fixes earlier work, without **Apply changes**. Each link names a fixed cause, appears in the answer and task log, and changes no task. Aril weighs reworked results when choosing models and adds a memory check when a cause repeats.

Aril has one switch, **On** or **Off**, at the top of the **Aril** section of Settings. Aril is On when you first use Pomegr. While it is Off there is no Aril at all: no chat panel, no **Aril** button, and no message box on Home, and Aril takes no action of its own (see [Turn Aril on or off](#turn-aril-on-or-off)).

Apart from those links and the memory updates described below, Aril changes nothing by
itself: it proposes, and you apply. The exception is an agreement you settle with Aril in the
**Aril** section of Settings. In your words, it says which actions Aril may take without asking,
each **Allowed** or **Never**. Aril takes them in a project only while that project's
**Autonomy** switch is on. Each project has its own switch, so turning it on or off in one
project changes only that one. Autonomy starts Off in every project, only you turn it on, and
turning Aril off turns it off in every project.

Sessions that Aril starts, reviews, resumes, or ends for you run with your own provider
account, under that provider's terms. Check that your plan allows unattended use. A provider
may require its API key for automated use.

## Before you start

- **Use the desktop app or a paired device.** The **Aril** button shows in the Pomegr
  desktop app and on a phone you paired with it (see
  [Phone access](phone-access.md)), while Aril is On. A browser on the same computer has no chat.
- **Choose a billing route.** Subscription runs need the current Claude Code or Codex tool installed and signed in.
  Direct API runs need a provider API key and budget, but do not need the CLI.
  Subscription fallback also needs the installed, signed-in tool.
- **Have a repository.** Aril works on one repository's board. Pomegr lists a repository
  once it observed a session there.

> **Note:** Each message starts one read-only provider run. Applying or dismissing a
> proposal also starts a follow-up run. It can read files and Pomegr's observations.
> It cannot write files or use your other tools and plugins.

When a task's text was shortened in the board snapshot, Aril can read the whole task on demand with `get_task`. It reads only the selected repository's board, including the task's conditions and attention or block line; it changes nothing.

Subscription runs use the coding tool's native read permissions. The tool starts in the selected repository, but permissions may allow reading files elsewhere.
Pomegr adds no separate file filter to a subscription run. Aril remains configured without write access, and you apply its board proposals.
A Codex subscription run needs a supported Windows sandbox: native MXC, or a provisioned elevated sandbox.
Some read commands can still be refused by the coding tool's policy. Direct API runs use bounded repository reads without a shell. Aril reports missing evidence instead of guessing.

On large boards, Aril keeps task titles first, then details for In progress, Review, Ready, Done, and Backlog.
Ready follows queue order; recent Done cards come first. If titles alone exceed the limit, each keeps a shortened prefix.
Aril sees how many texts were shortened and can retrieve needed full tasks with `get_task`.

## Ask Aril

Aril sees board changes since its last completed answer: task starts, reports, stalls, CI results, and merges.
You can ask what happened while you were away. For these changes, Aril receives only task IDs, event kinds, times, and pull-request numbers.

1. Select **Aril** at the bottom right. The panel opens on the right side of the window, on your
   conversation with Aril in that repository. It lays over the page, which dims behind it. Select **Expand** to widen it over the page,
   and **Dock** to bring it back. Select the dimmed page, or **Close Aril**, to close it.
2. Choose the repository in the **Repository** menu beside the attach button when Pomegr lists more than one.
3. Choose the **Model** and the **Effort** beside the send button. The list shows named models, grouped
   by provider. A repository's first message starts on the newest model Pomegr knows and on **Medium**,
   and later messages start on what the last one used. The selected route uses the model and the effort you chose.
4. Type your message and press Enter. Shift+Enter starts a new line.
5. Select **Stop** to end an answer early. The text written so far is kept.

On **Home**, the message field replaces the **Aril** button. Select the field, or a chip above
it, and the rest of Home fades out while the field moves to the middle of the page. Type and
press Enter: Pomegr opens the expanded panel on that repository's conversation and sends your
message with the model and the effort the field showed. The chips put a question in the field
and send nothing. Press Esc, or select outside the field while it is empty, to go back to Home.
Close the panel to return to Home.

Things you can ask:

- "I have this feature. Create the tasks for it." Aril cuts the feature into parts and
  proposes one feature with its tasks in the order they should start. If the repository has an `acos` skill,
  Aril uses its sizing rules for the cut.
- "Create this task", "Update T-12", or "Remove T-12".
- "Add a task for this under T-12." Aril proposes the new task with its place, so one
  **Apply changes** creates the card right under T-12. A card placed in Ready is not
  queued unless you also ask for the queue.
- "T-12 looks too big. How do I cut it?"
- "Which task must run first, and why?"
- "Which model fits this kind of task?" Aril compares the tasks already on the board:
  the planned and observed models, the outcome, and each checked condition. It can also
  read a session's report.

When Aril proposes a task, it chooses the provider, the model, and the effort for that
task. It weighs the kind and size of the work, the risk, how tasks of the same kind did
on your board, and the usage of both providers. It can choose any model that the **Run
on** menu of the task form offers, for Claude Code or Codex, and it says in one line
why it chose it. If you name a provider, a model, or an effort, Aril uses it.

### Quick replies and questions

Aril keeps its answers short: the answer first, and more detail only when you ask. When it needs something from you, it asks under the answer:

- **Quick replies** are small buttons for the likeliest answers, such as **Squash** or **Tell me more**.
- **A multiple-choice question** is a short list. Each option has a label and one line of trade-off, and Aril's recommendation comes first, marked **Recommended**.

Select one to send its label as your own message. It sends a message and nothing else: it applies nothing, starts nothing, and changes nothing on the board. You can always answer in your own words in the message field instead.

Only the choices under Aril's newest answer can be selected, and not while Aril is answering. Once you send anything after the answer, the choices turn into plain text, and the one you picked is marked **Chosen**. Use Tab to reach a choice and Enter to send it. A choice that cannot be shown is left out, and the answer stays.

### Send an image

You can show Aril a screenshot or a mock-up.

1. Select **Attach image**, paste an image into the message field, or drop one on it.
   The image shows under the message field. **Remove image** takes it out again.
2. Type what you want Aril to do with it, and press Enter.

A message holds up to four images: PNG, JPEG, GIF, or WebP, up to 5 MB each. A message
with an image still needs text. Aril sees the images of the part of the conversation it answers in, and a part holds up to 40.

Ask "create a task for this" or "attach this to T-12". Aril then proposes the task with
the image, or the image for a task that has not started. The proposal shows each image,
and the session that runs the task gets the image. A task holds up to four images.

**Close Aril** closes the panel and keeps the conversation. Each message shows the time it was sent.

## One conversation for each repository

Each repository has one continuous conversation with Aril. There is no list of conversations to
choose from, and no button to start a new one: you keep writing in the same place, and Aril keeps
the context of what you said before.

- **Scroll up for older messages.** The panel shows the newest messages first. When you reach the
  top, it loads the messages before them and keeps your place.
- **Search.** Select **Search** in the panel's header, type, and read the matches: who wrote the
  message, when, and a short excerpt with your text in bold. The search ignores case and reads your
  text as plain letters, not as a pattern. Select a match to jump to that message, which stays
  marked for a moment. Press Escape to close the search.
- **A long conversation continues by itself.** Pomegr stores a conversation in parts of up to 200
  messages and starts a new part when one is full, however many parts there are. You see one
  thread, and Aril reads the newest messages of all the parts. A message a notification links to opens in the same thread.

When the conversation gets longer than what Aril can read at once, Pomegr summarizes the older part
in the background and gives Aril the summary with the newest messages. A quiet line, "Earlier
messages summarized for Aril", shows where. Nothing is removed: you can still read and search
every message, and your settings and repository memory are not part of the summary. The summary
keeps what Aril concluded (decisions, your preferences, open questions, what became of its
proposals), so Aril can lose a detail that was only in a file it read. If you send a message while
Pomegr is summarizing, Pomegr stops the summary and tries again after Aril's next answer.

Hover the time of an answer to see how long it took: when the process started, when the first
words came, and when it finished. Pomegr measures this on your computer and keeps only the numbers.

With Claude Code, an answer starts faster when Pomegr has a Claude Code process ready. When you
click into the message field, and after Aril answers, Pomegr starts that process and leaves it
waiting. It sends nothing to Claude and spends none of your allowance until you send a message.
It closes the process after five minutes without a message, when a message needs another model
or effort, and when you quit Pomegr; closing the panel does not close it. The time of an answer
that used a ready process reads "Started 0.1s (warm)". Codex starts only when you send a message.

### Choose how Aril is billed

In the desktop app, open **Settings › Aril › Billing** to choose how each provider
pays for Aril. **Subscription** uses the signed-in coding tool's plan allowance.
Either provider can also use **API · metered** with your own provider API key and a monthly
budget in US dollars. API access and subscription access are separate; see
[OpenAI authentication](https://learn.chatgpt.com/docs/auth).

Add or replace a provider key in its card. For Claude, use a workspace-scoped Console key; Default workspace is supported. Organization-wide Claude keys are not supported. Pomegr stores each key encrypted on this computer; a
paired phone cannot view or change it. Enter a nonnegative budget; it saves when you
leave the field. The default is $0, which permits no API requests. The card shows the
configured budget for each provider even when subscription billing is selected.

The meter estimates only Pomegr's API requests, not your invoice or remaining credits.
A request already running can exceed the budget. If the estimate reaches the budget,
or usage becomes unknown, later runs use the same provider's subscription when it is
available; otherwise Aril stops for that provider. The desktop
chat explains the current route and offers a link back to Billing. If the key is missing, add one or manually choose Subscription; Aril does not fall back automatically. Received HTTP errors, including rate-limit and service errors, record no spend and allow a later attempt without changing your key. A dropped connection or missing usage can still leave the cost unknown. A reply you stop, or one that times out, after the request was sent counts at the most it could have cost, so the spend shows around that amount and API use stays available. Unknown usage continues to block API requests across month boundaries, and changing the budget does not clear it. After reviewing unknown usage in the provider account, save your key again, even unchanged, to clear the earlier usage block while preserving recorded spend, the budget, and the unknown history. This is your acknowledgment, not a provider-confirmed cost adjustment; the estimate can omit earlier unknown charges. A request still running is not cleared. A known estimate starts a new month at zero. API runs for answers, follow-ups, task writing, and
conversation summaries all count toward the estimate.

Eligible Claude Max plans include monthly API credits for a linked Console
organization, separate from the Claude Code allowance. Other purchased credits or
auto-reload may also pay for API use. Review your balance and billing settings in the
Claude Console. Eligible ChatGPT Pro or Business plans can link an API organization for credits
that cover Responses API requests. They do not increase signed-in Codex limits, and API-key
Codex CLI use is excluded. Check eligibility and balance in your account; see
[OpenAI API credits](https://developers.openai.com/api/docs/guides/chatgpt-api-credits).
Codex API requests use models with known rates; an unsupported model is refused before a request.

### Optional beta message threads

Claude's **API · metered** panel has a separate **Message threads** switch with a **Beta** label. It defaults to Off.
Selecting API billing does not enable it. The option never applies to Claude Code CLI runs or Codex.
When enabled but API is not the active route, the setting shows **Not in use**.

Threads can reduce repeated uploads during tool follow-ups within one reply. They do not change pricing or automatically enable prompt caching.
Pomegr keeps the complete replay data only for that active run. Later messages still use your local conversation.
If a thread expires or cannot continue, Pomegr uses ordinary Messages requests.
A recognized thread rejection permits one fallback request, with a fresh budget check and separate accounting.

Anthropic holds the conversation, system prompt, and tools in encrypted memory. The thread expires after five idle minutes.
Anthropic deletes it within one hour of last use; Pomegr cannot request earlier deletion.
Turning the option Off, deleting local messages, or closing Pomegr does not delete that remote copy.
Threads are not eligible for zero data retention or HIPAA use.
These beta rules were checked on 2026-11-05; see [Anthropic's message thread documentation](https://platform.claude.com/docs/en/build-with-claude/message-threads).

### Let Jev choose a lower effort (optional)

Replies like "thanks" or a yes or no do not need much thinking. If you have a Jev key, enter it
under **Effort routing** in the **Aril** section of **Settings** in the desktop app. Then, for each message, Pomegr
asks Jev how much effort the reply needs, while it prepares the answer. The **Effort** you chose
stays the highest: Jev can lower it for a simple reply and never raises it, and it never changes
the provider or the model. If Jev does not answer within 1.5 seconds, or fails, your effort is used.

Pomegr stores the key encrypted by your operating system, on your computer only. A paired phone
cannot set, see, or read it. Jev receives your message and the last 2,000 characters of Aril's
previous answer, and nothing else; it receives no board, task text, image, or file path. A message
with an image is not sent to Jev. When you click into the message field, Pomegr also connects to Jev ahead of time so
the answer is quick; that connection carries no message, no key, and no text. Hover the time of an answer to see the effort it used, such as
"Effort low, chosen by Jev in 0.4s". Select **Remove key** to turn this off.

## Apply a proposal

When you ask for a change, or when Aril recommends one, it ends its answer with a
proposal: a short summary and a list of the changes, one line each. Nothing changes until
you select **Apply changes**. Aril proposes a delete, a session start, or a queue change
only when you ask for it.

1. Read every line. A delete is marked, because it cannot be undone.
2. Select **Apply changes** to run the list in order, or **Dismiss** to drop it.
3. Read the result beside each line: **Done**, the ID of a new task, or why it failed.

Pomegr stops at the first change that fails and does not run the ones after it. Ask Aril
for the rest once you know why it failed. A new task that could not be put in its place
is still created: the result names it, and the card stays at the end of Backlog.

### Read Aril's follow-up

After you apply or dismiss a proposal, Aril answers once more by itself. You do not
need to send a message.

- **After you apply.** Aril says what changed, what failed, and what Pomegr did not run.
  It reads the board again first.
- **After you dismiss.** Aril acknowledges it in a line. Nothing was changed, and Aril
  does not propose the same thing again unless you ask.
- **After a start or a queue change.** Aril says what Pomegr confirmed. A task in the
  queue, or a queue that is on, is not a running session. Check **Sessions** or the
  Board for that.

A follow-up never applies a proposal or retries an operation. After an applied or partial
outcome, it may offer one board-change proposal that continues your earlier
request. Read it, then select **Apply changes** or **Dismiss** yourself. Each choice runs
one more read-only follow-up; Pomegr never applies a proposal or continues the chain by
itself. After a dismissed, refused, or failed proposal, the follow-up has no proposal. A
follow-up cannot propose a session start, a queue change, or a worktree removal; send Aril
a new message to ask for one. Select **Stop** to end a follow-up early.

The result beside each line of the proposal is Pomegr's own record. Trust it over the
follow-up when they differ.

A proposal can create a feature, create a task in a place you name, update, delete, or
move a task, add a task to the queue or remove it, put queued tasks in a parallel group
or take them out of one, and put an image that you sent on a task. It can also rename a
feature, mark it done, reopen it, or delete a feature that has no task. You apply a feature
edit in the desktop app only. Marking a feature done or reopening it changes none of its
tasks, and deleting a feature cannot be undone. It cannot mark a task done,
requeue a task, or change the queue's threshold or schedule. Those stay with you, as
described in [Tasks](tasks.md). Starting a session, turning the queue on or off, and removing
task worktrees are proposals of their own, described below.

### When Aril writes without being asked

While Aril and a project's **Autonomy** are both on, Pomegr wakes Aril by itself when a task of that project may need a
decision: a task reaches Review, a review verdict arrives, CI finishes on a task's pull
request, a builder blocks, a session stalls, or a merge Aril made lands. Events that arrive
close together are answered in one run. Aril reads the CI results and the sessions, then
writes a short note in the conversation, under a quiet line that names the events, for
example "Pomegr woke Aril: CI failed on T-12." It may end with one proposal for you to apply.

- It never applies anything, starts anything, or changes its memory. What your agreements
  allow still happens through Pomegr, as before.
- At most four of these runs start in an hour, none while another answer is running, and
  none while the provider's usage is at the limit that holds new starts.
- Each one is a provider run like an answer, so it counts in your usage or API budget. While
  it runs, Aril cannot take a new message: send yours when the answer is in.
- Turn the project's Autonomy off to stop them there. Events that happen while it is off do not wake Aril later.

## Start a session from the chat

Ask Aril to start one task by its ID, for example "start T-63". Aril answers with a
start proposal for that task. A start is always a proposal of its own.

1. In the Pomegr desktop app, select **Start session** on the proposal, or **Dismiss**
   to drop it. This approves the proposal.
2. Confirm the named task in the desktop dialog. Pomegr uses the same checks as [Start a session](tasks.md#start-a-session).
3. Read the result on the proposal.

| Result | What it means |
| --- | --- |
| **Session started in a new terminal window.** | Pomegr opened the session. The task links to it when the plugin in that session reports in. |
| **Not started.** and a reason | Nothing was started. A held start names what holds it, such as usage above the threshold or uncommitted changes. |

A proposal that did not start stays open. Select **Start session** again when the
reason is gone. Pomegr never tries again by itself, and a proposal that started a
session cannot start a second one.

- Aril proposes a start only when you ask for one, and only for a task that is **Not
  queued**, **Queued**, or **Scheduled** with no session.
- A start from the chat does not use the queue and changes nothing in it.
- On a paired phone, the proposal has no **Start session**. Apply it in the desktop app,
  or open the task on the phone and select **Start session** there.
- Pomegr picks the folder of the start, as for every start (see
  [Where a task starts](tasks-start-gates.md#where-a-task-starts)).

## Turn the queue on or off from the chat

Ask Aril to turn this repository's queue on or off, for example "turn the queue on".
Aril answers with a queue proposal that names the state you asked for. A queue change is
always a proposal of its own.

1. Select **Turn queue on** or **Turn queue off** on the proposal, or **Dismiss** to
   drop it. Nothing changes before that. You can do this in the desktop app or on a
   paired phone.
2. Pomegr changes the queue as the **Queue** switch of the Tasks header does. See
   [Run the queue](tasks.md#run-the-queue).
3. Read the result on the proposal.

| Result | What it means |
| --- | --- |
| **Queue turned on.** | The queue is on. Pomegr starts the queued tasks by itself, as described below. |
| **Queue turned on.** and a line about a stalled or blocked task | The queue is on, but a task holds it. Resolve that task on the Board. |
| **Queue turned off.** | The queue is off. No new task starts. Running sessions continue. |
| **Queue not changed.** and a reason | Nothing changed. Select the same button again to try once more. |

> **Caution:** Turning the queue on lets Pomegr start sessions without asking again. It
> starts the queued tasks on the computer that runs Pomegr, in Ready order, while
> Pomegr is open there. Read the **Ready** column before you turn the queue on.

- Aril proposes a queue change only when you ask for one.
- A proposal names **On** or **Off**. It never switches the queue to the opposite of
  what it is, so applying it again cannot reverse the queue.
- Every start still passes the same checks as on the Tasks page: the Ready order and parallel
  groups of the queue, each task's own start time, the queue's schedule, and the
  [start gates](tasks-start-gates.md#start-gates).
- Aril does not start the tasks and cannot tell you which ones will start. The Board
  shows that.
- Aril cannot change the usage threshold or the schedule of the queue. Set them on the
  Tasks page.

> **Caution:** Aril can be wrong. Its advice on models is a reading of the tasks
> recorded on your board, not a measurement. Check a proposal before you apply it.

## Free disk space from the chat

Task worktrees take disk space; see [Worktrees](tasks-order.md#worktrees). Ask Aril which
worktrees remain, or to clean up. Aril sees each worktree's task, whether it has uncommitted
changes or a commit that exists nowhere else, and about how large it is. It sees no path.
These facts can be a few minutes old, and a size is unknown until Pomegr has measured it.
Just after Pomegr starts, Aril may say it is still reading them; ask again in a moment.
Pomegr checks a worktree again when you apply its removal.

Aril proposes a removal when you ask about disk space or cleanup, or when a done or deleted
task still has a worktree. It only tells you about a worktree that holds uncommitted changes
or a commit that exists nowhere else. It proposes none for a task in progress.

1. In the Pomegr desktop app, select **Remove worktree** or **Remove worktrees** on the
   proposal, or **Dismiss** to drop it. A paired phone cannot apply it.
2. Read the result beside each row. “Worktree removed” means Pomegr removed it.
   “Worktree kept: uncommitted changes”, “Worktree kept: commits exist nowhere else”, and
   “Worktree kept: task or review is running” mean Pomegr kept it.

A kept worktree does not stop the next row. Pomegr never forces a removal.

## Turn Aril on or off

The first thing in the **Aril** section of Settings is one switch.

- **Aril is On when you first use Pomegr.** Only you turn it off and on again. Aril never turns itself on, and it is Off again only when you turn it off. A restart keeps what you chose. If Pomegr cannot read the file that holds your choice, it treats Aril as Off, because that file may hold your Off.
- **Off means no Aril at all.** It takes effect at once, in the desktop app and on a paired phone. The chat panel, the **Aril** button, and the expanded Aril page are gone, and Home has no message box and no suggestions: the rest of Home is as it was. A link to the chat opens Home instead. Nothing is drawn before Pomegr knows whether Aril is On, so the chat never shows up and then goes away.
- **What Aril was doing stops.** An answer that Aril was writing is stopped, as if you had selected **Stop**, and it shows as stopped. Pomegr starts no provider run for Aril, and it refuses a message, a proposal's **Apply changes** or **Dismiss**, and an image for Aril while Aril is Off. A session that is already running goes on.
- **Your conversations are kept.** Turning Aril off deletes nothing. When you turn it on again, the conversations are there as you left them. Aril saves nothing to memory while it is Off.
- **Settings turns read only.** Everything under the switch in **Settings**, under **Aril**, is gray and read only while Aril is Off: your memory and each repository's, the conversation space and its limit, billing, and the effort routing. Pomegr refuses those changes too, and any change to Aril's settings, on the computer and from a paired phone. A notice says so. Turn Aril on to change them. Pomegr still flushes a conversation that passes your limit, because that is housekeeping on your own files, not something Aril does.
- **A phone** can always turn Aril off. It can also turn Aril on, and change Aril's settings and each repository's memory, only after you allow that in the desktop app. Turn on the small switch under Aril's switch: "A paired phone can always turn Aril off. Let it also turn Aril on and change Aril's repository memory and settings." One switch covers every repository, and a phone never edits your own memory.
- **If you allowed paired devices before.** Earlier versions had a **Paired devices** switch for each repository. If you turned it on for any repository, Pomegr turns the one switch on for you, once. You can turn it off, and Pomegr does not turn it on again.

## Aril's settings and memory

Ask Aril to settle this repository's merge settings or reviewer choice. Aril proposes each
change, and you select **Apply changes** to save it. Aril never changes settings by itself. Only
you can allow paired devices to manage these settings, with the switch under Aril's switch in the desktop app. They are in the
**Aril** section of Settings (see [Aril settings](tasks.md#aril-settings)).

Aril does not ask again what a setting or the memory already answers. It asks again when
you say that a rule changed, or when a workflow file of the repository changed after you
settled the rule. These files are `AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, a pull request
template, `.pomegr/signals.md`, and the files in `.github/workflows/`. Aril reads the changed
file, then proposes an update or asks only about what the change left unclear.

### Memory

**Memory** in the **Aril** section holds what Aril should know about how you like things done. It has one tab for you, for every repository, and one for each repository. Each memory is free text of at most 6,000 characters, such as "Reply in the language I write in." Edit it in the field; it saves when you leave the field. A paired phone reads your memory and never edits it. It edits a repository's memory only when you allowed paired devices in the desktop app.

Memory is context for Aril. It is not permission: a line in it lets Aril do nothing new.

Aril keeps your memory and the repository's up to date by itself. When you settle a lasting preference in the chat, or say that a line is wrong, Aril updates the memory and tells you in one line under its answer. It saves only what you said in the chat, never text it read in a file, a task, or a pull request, and only while Aril is **On**. Under the field, Settings says who changed the memory last and when. **Show change** shows the text before the change beside the text now. Aril never changes more than 6,000 characters, and nothing you edit stays hidden from you. Edit or empty the text at any time.

Do not keep secrets in a memory.

### Free space from a thread

A repository's conversation grows with use. **Conversation space** in the **Aril** section of Settings, in the desktop app, shows what each repository's conversation takes on disk, with its images, as a bar against a limit, and its number of messages. The bar turns coral at 80% of the limit.

**Flush oldest at** sets the size at which Pomegr flushes a conversation by itself: 2 MB, 5 MB, 10 MB, 25 MB, or **Never**. The default is 5 MB. When a conversation passes it and Aril is not answering or summarizing in that repository, Pomegr removes its oldest messages and their images until it is at 75% of the limit. It keeps the newest message and its summary of the conversation. **Messages that no summary covers yet are gone when they are removed**, so choose **Never** if you want to keep every message.

**Flush oldest** removes the oldest half of that repository's messages, and the images they carry, when you ask. It asks first, and tells you what it removed and how much space it freed. Aril keeps its summary of the conversation and the newest half. Pomegr will not flush a conversation while Aril is answering in that repository, and no phone can flush or change the limit.

Settings and repository memory are sent with each chat prompt to your chosen provider, and so is your own memory.

## Understand what is kept and sent

- **Kept on this computer.** Pomegr saves your conversation with Aril in its private data folder,
  in parts of up to 200 messages, with the images you sent and, once a long conversation is summarized, its
  summary. The summary never leaves your computer except in the prompts to your provider, and no
  phone or browser receives its text. A repository has no limit of
  parts. An image that Aril puts on a task is a copy of the image. The search and the older
  pages are answered from these files on your computer.
- **Sent to the provider.** Each message sends the summary of the older messages, when there is one,
  the newest earlier messages of the repository's conversation (as many as fit in 24,000 characters)
  and the board to the
  provider you chose, through your signed-in coding tool or, when selected, its direct API. The search sends nothing. After an answer, Pomegr may also run your provider once, read-only and told to use no
  tool, to write or update the summary of a long conversation. It sends only the older messages and the
  earlier summary, and it spends your provider allowance like an answer does. Applying or dismissing a proposal
  sends them once more, with the result of the proposal, for Aril's follow-up. That run
  uses the provider, model, and effort of the last message. A run Pomegr starts on task events sends
  the same, with your agreements, the task log, and the events. The board includes the text of your
  tasks. Aril may also send file content and session evidence that it reads. The images
  you sent in the conversation go to the provider too, and the prompt names the folder
  where Pomegr keeps them. Repository settings include the selected merge method, merge mode, and branch deletion choice. Aril also gets the repository's GitHub merge
  settings (whether squash, merge commit, rebase, auto-merge, and delete-branch-on-merge are on) and its default branch name.
  It gets the names and dates of the changed workflow files, but not their content, and the facts of the task worktrees that remain, but not their paths.
  Merge and branch results appear as fixed task-log labels.
- **Your memory and the switch.** Your memory is kept in a private file on this computer, a repository's memory in Pomegr's task store, and the switch and the flush limit in files of their own. Each memory goes to the provider with each prompt, within 6,000 characters. The switch and the limit go nowhere.
- **Sent to Jev, only with your key.** If you set a Jev key, each message without an image also sends Jev your message and the last
  2,000 characters of Aril's previous answer, so it can suggest a lower effort. Nothing else goes to Jev.
- **Not observed.** A chat run leaves no session, so it does not show in **Sessions**.
  Subscription usage counts in your account's usage windows; direct API requests are billed to your provider API account. The same holds for a Claude Code process that
  waits for your next message: it holds no message, board, or conversation text until you send.
- **On a paired device.** The conversation crosses your local network without
  encryption, like the rest of phone access. Anyone who holds a paired phone can read
  and search the conversation and its images, send a message or an image, and apply a proposal
  of board changes or a queue change. Applying or dismissing a proposal there also runs
  Aril's follow-up on your computer. Clicking into the message field there also starts a waiting Claude Code process on your computer. Turning the queue on lets Pomegr start queued tasks on your computer. A start
  that Aril proposed can be applied only in the desktop app.

## If the result is missing

| What you see | What to do |
| --- | --- |
| No **Aril** button | Open the Pomegr desktop app, or pair the phone again. |
| The tool is not installed on this computer | Install Claude Code or Codex, or choose a model of the other provider in **Model**. |
| Aril could not finish this answer | Open the coding tool once in a terminal. Check that you are signed in and that the tool is up to date. |
| This answer took too long and was stopped | Ask a narrower question. A run may take ten minutes. |
| Aril is already answering | Wait for the running answer, or select **Stop**. Aril answers one message at a time for the whole app. |
| Aril could not be reached | Check that Pomegr shows **Local monitor**, then send again. |
| No follow-up after you applied or dismissed a proposal | Aril was answering another message, so the follow-up waits for it. If Pomegr closed meanwhile, ask Aril what happened: the result is saved. |
| Search could not be reached | Check that Pomegr shows **Local monitor**, then type the search again. |
| The follow-up says that Aril could not finish | The proposal's result is saved and shown on its lines. Check the coding tool as for any answer. |
| Aril wrote a proposal that Pomegr could not read | Ask for the proposal again. Nothing was changed. A rejected follow-up proposal has no card. |
| A line says **No longer possible** or **Not found** | The board changed after Aril wrote the proposal. Ask again. |
| A line says **Not attached.** | Read the reason on the line. A task that has started takes no image, and a task holds up to four. |
| A line says a task was created, but its images could not be attached | Open the task and put the image in again, if the task has not started. |
| The image could not be attached | Check that Pomegr shows **Local monitor**, then add the image again. |
| A start proposal says **Not started.** | Read the reason on the proposal. [Start gates](tasks-start-gates.md#start-gates) explains a held start. Select **Start session** again when it is gone. |
| A queue proposal says **Queue not changed.** | Check that Pomegr shows **Local monitor**, then select the same button again. |
| The queue is on and nothing starts | Open **Tasks**. A banner, or the line under the queue switch, names what holds the queue. See [Run the queue](tasks.md#run-the-queue). |
| This proposal can be applied only in the Pomegr desktop app | Apply the proposal in the desktop app. To start a task from this device, open the task and select **Start session**. |
