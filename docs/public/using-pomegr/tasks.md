---
title: "Tasks"
description: "Plan work on a repository's task board, queue it, and have the Pomegr desktop app start Claude Code or Codex sessions for it."
---

# Tasks

A task board holds the work you want coding agents to do in one repository. You start
a session for a task yourself, or turn on the queue and let the desktop app start tasks
one after another. After a start, Pomegr only
observes the session: it never attaches to, answers, or approves anything in a
running session. It ends a session only when its task is done
([Done ends the task's sessions](#done-ends-the-tasks-sessions)), when the session is idle
and its task is in Review ([Review ends idle sessions](#review-ends-idle-sessions)), or when its task
is deleted ([Deleting a task ends its sessions](#deleting-a-task-ends-its-sessions)). Observing sessions
works the same whether or not you use tasks.

Click through the demo below to see a board at work. It is a demo with sample tasks,
not the Pomegr app, and it starts nothing.

```pomegr-demo
tasks-board
```

## The board

Select **Tasks** in the sidebar to open the Tasks home. The header counts the tasks that
are open, in review, and done across all your boards. **Needs you** lists the tasks that
need review, are stalled, or are blocked by their agent. **Boards** shows one card for each
repository that has tasks: its open features and last change, a bar that splits its tasks
between Backlog, Ready, In progress, Review, and Done, the count of each, and what is moving
now. Select **Open board** to open its board. In the desktop app and on a paired device, the
switch on a card turns that board's queue on or off; any other browser only reads whether it
is on. A repository with no task yet is a short row, with **+ New task** where you can
create tasks.

A board has five fixed columns:

| Column | What it holds |
| --- | --- |
| **Backlog** | New tasks, and the plan of the work you have not queued yet. |
| **Ready** | The queue. Queued cards read top to bottom in the order they start. |
| **In progress** | Tasks whose session started and linked to the task. |
| **Review** | Tasks whose agent reported the work complete. Only you mark them done. |
| **Done** | Tasks you accepted with **Mark done**. |

Pomegr moves a card when its task changes state. You can also drag a card to another
column. [Board and task states](tasks-board.md) explains each state and move.

### Done ends the task's sessions

When a task is done, Pomegr ends the sessions it started for the task in its terminal: the
builder session and any review session. This stops the provider process and the helper
processes it started, so finished work does not keep using memory. Pomegr never ends a session
you started outside Pomegr, and it never ends a session for another task.

When you choose **Mark done** or drag a card into **Done** in the desktop app, an idle session
ends at once. If the session is still working, Pomegr asks first: "Its session is still working.
End it?" **End session** ends it. **Keep it running** leaves it running while it works: once it has stayed
idle for two minutes with no command running, Pomegr ends it. Both choices mark the task done. When you mark a task done on your phone, a session that is still working is left
running, and Pomegr ends it later if it goes idle and the task is still done. Pomegr also ends the
idle sessions of tasks that were done before you updated.

### Review ends idle sessions

Aril can start builds and reviews by itself, so idle sessions of tasks in **Review** can
pile up. Each one uses memory, and the terminal holds a limited number of sessions.
Pomegr ends the sessions it started for a task in **Review** after they stay idle for two
minutes. This covers the builder session and any review session, and it needs no setting.

A session is not idle while a command it started still runs. Examples are a watcher on CI,
a dev server, or a command in the background. Pomegr also never ends a session that is working
or whose state it does not know, or one whose terminal you have open in the desktop app. It never ends a session you started outside Pomegr. To work in an ended session again, open it
in the desktop app and select **Continue**. Pomegr never resumes a session by itself.

### Idle sessions end unless their task is In progress

The same two-minute rule covers every session Pomegr started in its terminal whose task is not
**In progress**, for example a task you requeued to **Ready**. It covers a session you brought
back with **Continue** too: that session ends once it has stayed idle for two minutes, unless the
session it continues belongs to a task that is **In progress**. While you have a session's terminal
open in the desktop app, Pomegr waits, and the two minutes start again after you close it. Pomegr
never ends a session that is working, that runs a command, or whose state it does not know.

### Deleting a task ends its sessions

When you delete a task in the desktop app, Pomegr ends its idle sessions with it. If a session is still working,
Pomegr asks first, with the same choices as for Done: **End session** or **Keep it running**. Either choice deletes the task.
A session left running ends later, once it has stayed idle for two minutes with no command running. The same
happens to the sessions of a task deleted on your phone, since only the desktop knows the sessions. Pomegr never
ends a session that is working or whose state it does not know, or one you started outside Pomegr. To work in an ended
session again, open it in the desktop app and select **Continue**.

### Read the task log

In the desktop app or on your paired phone, select **Log** in the strip above the
board. It lists task starts, reports, stalls, CI results, and merges, newest
first. The task log stays on the computer that runs Pomegr. Aril reads entries after its last answer;
the divider shows how far it has read. Opening the log does not move that marker.

In a task window, **Reworks** records earlier work that this task fixes. On the desktop
or a paired device, enter a task reference such as `T-12` or a pull request such as
`#336`, choose a cause, and select **Add**. You can remove a link beside its chip.
Each task can name up to five earlier items; adding a link leaves its state unchanged
and adds **Rework** to the log. Causes are declarations, not automatic detections.

### Aril settings

Open **Settings** and select **Aril** to turn Aril on or off, edit memory, and see the space conversations take.
Each repository has its own memory tab there.

The **Memory** card has a tab for each repository: free text of at most 6,000 characters about how things are done there. Aril keeps it current
and you can edit it; it is context and not permission (see [Aril's settings and memory](agent-chat.md#arils-settings-and-memory)).

Paired devices can read the memory. To let a phone also turn Aril on and change a repository's memory and Aril's settings, turn on the small switch under Aril's switch in the desktop app.
One switch covers every repository, and only the desktop app can change it. Settings has no card for each repository.

### Aril's settings for merging and reviewing

Aril keeps a few settings for a repository. It proposes them after it reads the repository's own rules and
GitHub settings, and you save them with **Apply changes**. In this version nothing acts on them for you:
Pomegr merges no pull request, sends no task back, and marks no task done unless you do it.

- **Merge method.** The repository default, unless you choose **Squash**, **Merge commit**, or **Rebase**.
  If the repository allows several methods, choose one.
- **Merge mode.** **Merge when green** merges once the review passed and the task's Complete when checks
  hold, with any CI green. When the repository's rules block the merge, such as a required approval, it does not try and reports the refusal. **Auto-merge when green** instead waits for the repository's own rules, such as a required approval, and merges once they allow, or adds the pull request to the repository's merge queue.
  Pomegr merges only the commit that was reviewed. It does not turn on GitHub's auto-merge, which would merge a later push with no review.
- **Delete branch after merge.** **Repository default** leaves it to GitHub's own setting. **Delete** deletes the
  task's branch after its pull request is merged, never the default branch or a fork's branch. **Keep** deletes
  nothing.
- **Reviewer.** The provider and model that review a task, and whether the reviewer must differ from the builder.
- **Plans.** Markdown files in the repository that name the work to come.
- **Guards.** How often a failed review may send one task back (3 when unset), and the provider usage percent at
  which Aril starts nothing (the provider's own limit when unset). A task sent back stays linked to its builder's
  session and returns to **In progress**. Aril first sorts the review's findings. It tells the reviewer which
  findings come from this computer and not from the task, and the reviewer decides again. It sends the other findings
  to the builder in one message, and only that message counts as one send-back. Pomegr delivers each message to the
  session while it runs, or continues an ended session with it. Aril's chat shows one line for each message, such as
  **Sent T-12's findings to its builder**. After the builder reports again, Aril asks the same reviewer to check
  again. When Aril does not sort a review in 10 minutes, Pomegr sends every finding to the builder once the builder's
  session has ended. When a message cannot be delivered to the builder, the task shows **Blocked** for you, and
  **Requeue** starts a new session with the findings.
  When the same cause of such findings comes back in a project, such as a dependency that task worktrees lack, Aril
  has Pomegr write one task that proposes the fix on that project's board, with the evidence of each time, where an
  agreement allows **Write tasks**. A later time adds its evidence to that task instead of a second one.

To watch a review you started, open the task's **Terminal** tab in the desktop app. It lists the task's build and
review sessions, and a review carries a **Review** label.

## Before you start

- **Use the desktop app or a paired phone.** You change tasks in the Pomegr desktop app
  or on a [paired phone](phone-access.md). A browser on the same computer shows the
  board read-only.
- **Use Windows to start sessions.** Starting sessions works on Windows only for now.
  Elsewhere a start answers "Starting sessions is available on Windows only."
- **Install the Pomegr plugin 0.9.0 or later in the repository.** The started session
  links to its task, and the agent reports its result, through the plugin; see
  [Reporting plugins](reporting-plugins.md#install-the-plugin).
- **Install Claude Code or Codex** on the computer that runs Pomegr.

## Create a task

1. On the board, hover the **Backlog** column and select **New task** under its last
   card. On a touch screen the action is always shown.
2. Describe the work in **Task**. It is the only required field.
3. Check **Run on**, **Effort**, and **Complete when**. A new task takes these choices from
   the newest task on the board.
4. Select **Create task**. The card lands in **Backlog**, not queued.

The arrow beside **Create task** offers **Create and queue** and **Create and start
session**. [Create and edit tasks](tasks-create.md) explains each field, images, and
the conditions Pomegr checks. To start from an open GitHub issue,
or to create an issue from a task, see [GitHub issues](github-issues.md).

## Start a session

1. Open a card and select **Start session**.
2. Pomegr asks "Start task T-3?" and does nothing until you confirm. T-3 is an example.
3. A terminal window opens on the computer that runs Pomegr. It runs Claude Code or
   Codex with the model and effort of the task.
4. When the plugin in that session reports in, the task links to the session. The card
   moves to **In progress** and shows **Open session**.

A Claude Code session that Pomegr starts for a task accepts messages from your other
Claude Code sessions on this computer. Pomegr sets this for that session only, on its
command line, and changes none of your settings files.

The agent reports with `complete_task` when the work is complete, or with `block_task`
when it cannot continue. A completion puts the task in **Review**, never in **Done**.
A later completion keeps an existing block until you resolve it; see [What agents report](tasks-agents.md#how-a-report-is-checked). A
[start gate](tasks-start-gates.md) can hold a start, for example while a provider has an
incident. In the desktop app you can also ask Aril to start a task; see
[Start a session from the chat](agent-chat.md#start-a-session-from-the-chat).

### Review a task's work

In the desktop app, open a task in **Review** with a linked session and select
**Start review**, before **Requeue task** in the footer. Confirm **Start review**
in the native dialog to start a separate reviewer session in the task's worktree,
or the repository folder when no task worktree is available. No worktree is created.

The reviewer checks the work and runs the repository's checks. It changes no code,
does not move the task, and is instructed to report **pass** or **fail**. The verdict
appears in the task modal's **Review** row, on the desktop and a paired device.
The row reads **Review running…** until the reviewer reports, then **Passed** or
**Rejected** with the time and, for a rejection, its reasons. It shows only verdicts
from the task's current stay in Review. Starting a review is available only in
the Windows desktop app.

Pomegr keeps the ID of every reviewer session with its task, and when it was linked,
until you delete the task. The **Sessions** list shows a reviewer session's task,
marked **Review**, after the review ends and after the task is done or requeued. The
task's **Terminal** tab still finds an ended review.

You can find a review from its task, whether you or Aril started it:

- **On the card.** A task that has had a review shows a line above its model line:
  **Review running**, **Review passed**, **Review rejected**, or **Review, no verdict**.
  After a rejected review goes back to a builder, the line reads **Fixing review findings**
  while a session works on the task, or **Findings to fix** while the task waits to start.
  Select it to open the newest review session.
- **In the task window.** Under **Sessions**, each review session follows the builder
  session, newest first, with its verdict and its state. After a send-back the earlier
  rounds stay listed, such as **Review · round 1 · Rejected** under **Review · round 2 · Passed**.
- **On the review session's page.** The header reads **Review** and the task ID, with
  the verdict, and a **Builder session** link to the session it reviewed. In the desktop
  app and on a paired phone, the task ID opens the task window.

The verdict is what the reviewer reported to Pomegr with `report_review`, never what its
terminal printed. **Running** means the review is the task's current one and has not
reported yet; **No verdict** means that round ended without a report.

## Run the queue

1. Open a task and select **Add to queue**. Its card moves to **Ready**, last.
2. Turn on the **Queue** switch at the top of the board. The queue is **Off** for
   every repository until you turn it on.
3. The queue starts the top waiting card of **Ready**. It starts the next card when that
   task is in **Review** or **Done**.

To change the order, drag a queued card up or down in **Ready**. The line under the
switch says what happens next. The queue works only while Pomegr is open, and turning it off never
touches a running session. [Task queue](tasks-queue.md) explains the wait reasons and
what to do when the queue stops. To run cards at the same time, or to make one task
wait for another, see [Parallel tasks and Runs after](tasks-order.md).

## What Pomegr does not do

- It never attaches to, answers, or approves work in a running session. It ends a
  session only when its task is done, when the session has stayed idle for two minutes
  and its task is in **Review**, or when its task is deleted. It
  merges no pull request, and only you mark a task Done, after its acceptance passes.
- It makes no AI judgment about whether work is complete. It compares the agent's
  report with observed facts. The own condition is the agent's judgment. An unknown
  fact never passes.
- It does not turn silence into success. A session that vanished is **Stalled** only
  after Pomegr establishes its end.
- Task text, task images, and your other task content stay in a private store on this
  computer, apart from session history. Pruning history never deletes a task.

## Learn more

- [Create and edit tasks](tasks-create.md): fields, images, and **Complete when**.
- [Board and task states](tasks-board.md): columns, card moves, search, and states.
- [Features](tasks-features.md): group tasks and give them shared context.
- [Task queue](tasks-queue.md): order, wait reasons, and what to do when the queue stops.
- [Parallel tasks and Runs after](tasks-order.md): groups, worktrees, and dependencies.
- [Start gates and schedules](tasks-start-gates.md): what holds a start, and start times.
- [What agents report](tasks-agents.md): the task tools and how a report is checked.
- [Task sessions](tasks-sessions.md): assign a session, and see a task from **Sessions**.
- [Agent chat](agent-chat.md): have Aril cut a feature into tasks or order the board.
- [Signals and estimates](../concepts/signals-and-estimates.md): which values are agent
  reports.
