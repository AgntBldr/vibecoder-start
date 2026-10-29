# Glossary

These words explain how you and your AI assistant work with this starter template. This template uses two assistants: Claude plans, designs and reviews; Codex builds from a written plan.

**.env** A file for settings that a program needs, sometimes including secret access keys. In this template, keep it private and out of the project's saved change history.

**.gitignore** A file listing files and folders that Git, the tool that records project changes, should leave out. It helps keep private settings and temporary files out of shared work, but does not remove anything already recorded.

**~** A shortcut for your own user folder on this computer, such as /Users/you on a Mac or C:\Users\you on Windows.

**AGENTS.md** The rulebook file at the top of your project. Your assistant reads it at the start of every session; section 1 describes your project.

**artifact** A page Claude publishes online on claude.ai, private until you share it. This template uses artifacts such as decision boards; Codex cannot make them.

**backup** A second copy of your notes kept online in a private place, so you do not lose them if this computer breaks.

**board** A private web page Claude publishes as an artifact, listing decisions waiting on you. You answer there instead of in chat and Claude reads your answers; only Claude can make one, and you must never put secrets on it.

**branch** A named line of saved project history that can develop separately from others. This template normally keeps everyone working on the line named main.

**builder** The model that made the change. This template never lets the builder review its own work.

**capability** A feature a published artifact page is granted, such as saving your answers or accepting photos. It lets a page used by this template do more than display information.

**clone** To copy a project and its saved change history onto your computer. This gives you a local copy of the template to work on.

**commit** A saved checkpoint of selected project changes, with a message explaining them. In this template, a commit lets you see what changed and why, and go back to it.

**compact** When the context window fills, Claude Code shortens the conversation into a summary. This template uses /prepare to check everything important is saved first, ending with Safe to compact or Not safe to compact.

**context window** The maximum amount of text and other information an AI can consider at once. This template keeps working notes small so important instructions fit alongside the current task.

**diff** A view showing which lines were added, removed, or changed in files. This template uses it to inspect exactly what a piece of work changed.

**gate** The memory gate, run with `node scripts/memory/memory-check.mjs`, checks that saved notes (memory) are tidy and complete at session start and each time you run /prepare. The session-start hook prints `memory gate: ok` or a FAIL line; the gate must show no FAIL ("gate green") before /prepare can say Safe to compact.

**GitHub** A website that stores projects and their saved history online. Here it holds a private backup of your notes and, if you choose, your project.

**hook** An action that runs automatically at an event, such as starting a session or committing. This template uses a session-start hook to run the memory gate so the assistant sees problems with saved notes before starting work.

**lesson** A saved note explaining what was learned and why it matters. This template keeps lessons so future sessions can avoid repeating mistakes.

**memory** Saved notes that help an assistant carry useful information between conversations, kept in a private folder outside your project and never published with it. This template organizes them so it can find current facts without reading every past conversation.

**model** The AI system that reads your request and produces an answer or proposes actions. This template assigns different kinds of work according to the system's strengths and cost.

**plan** A written sequence of steps for reaching an agreed result. This template has the plan checked before building, then builds one approved step at a time.

**plugin** An add-on that gives an assistant extra tools or instructions. This template expects the superpowers plugin to help plan work before building; other plugins are optional.

**PowerShell** The window on Windows where you type commands. On a Mac the same job is done by the Terminal app.

**prompt** The instructions or question you give an AI assistant. In this template, a clear prompt says what result you want and what the assistant may change.

**pull request** A proposal on a service such as GitHub to bring the changes from one branch into another, with a place for discussion and checking. This template normally saves directly to its main branch, so this extra step is not required by default.

**push** To send saved project checkpoints from your computer to an online copy, making completed work available to others. In this template, check for secrets first with /secret-check, because anything pushed to a public repo can be seen by everyone.

**remote** The online address a Git folder sends its saved changes to, such as a private GitHub repository.

**repo** Short for repository: a project's files together with their recorded change history. This template is a starting repo that you can use for your own project.

**review** An inspection of proposed work to find mistakes and check that it meets the request. This template requires a different AI model from the builder to review changes before committing, with limited exceptions for documentation and bookkeeping (updating project records).

**roadmap** The file `docs/ROADMAP.md`: what has been done and what is next, in dated entries.

**router** The short front page of memory, MEMORY.md, with pointers saying which note to open before each kind of work. It helps this template find relevant notes without reading them all; it has nothing to do with a network router.

**sandbox** A restricted area in which an assistant works. In this template, Codex builds where it can change project files but cannot use the internet; reviewers can only read.

**session** One sitting with an assistant, from opening it to compacting or closing it. This template uses sessions to organize work and save what was learned for next time.

**skill** A reusable set of written instructions for a particular task, such as starting a session or checking changes. This template provides skills for consistent work; you start one by typing its name after a slash, such as /prepare.

**stack** The set of tools a project is built with, such as the programming language and where it is hosted.

**stage** To choose which changed files go into the next commit. This template uses `git add` with file names so only the intended files are included.

**terminal** A window where you type commands for your computer to run. You use it to start the assistants and run this template's checks.

**token** A small piece of text, such as part of a word, that an AI uses to process language; tokens measure how much information the assistant can handle and sometimes what using it costs. Not the same as an access token, which is a secret key.

**verdict** The final result: reviews end with VERDICT: APPROVED (go ahead), REVISE (fix the findings and review again), or BLOCKED (stop; you decide). In this template, /prepare ends with the verdict Safe to compact or Not safe to compact.
