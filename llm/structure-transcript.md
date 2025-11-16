### LLM Workflow: Turn a Raw Speech Transcript into a Structured Blog Post

Use this when you have a messy voice transcript (like the one in the prompt below) and want to turn it into a clean, long-form blog post in the style of your existing posts (e.g., `00-WIP-Cascade.md`, `01-WIP-granular.md`, `02-WIP-evolutionary-content-generation.md`).

You can copy‑paste the templates below into an LLM, replacing the ALL_CAPS placeholders and pasting your transcript where indicated.

---

### High-level workflow

1. **Dump the raw transcript**
   - Get the full transcript from your speech tool with minimal editing.
   - Do not try to write; just capture everything you said.

2. **Clarify the core thesis and scope**
   - Ask the LLM to extract the main thesis, target audience, and constraints (what this post is / is not about).

3. **Extract and cluster ideas from the transcript**
   - Have the LLM pull out all distinct ideas, examples, constraints, and meta-comments.
   - Group them into coherent clusters (sections / phases).

4. **Design the post structure**
   - Turn the clusters into a structured outline with section headings, a narrative arc, and an example that runs throughout the post when appropriate (e.g., retirement-home recreation programming).

5. **Draft section-by-section**
   - For each section, feed the LLM the outline + relevant transcript chunks and ask it to draft prose in your preferred style.

6. **Global refinement pass**
   - Ask the LLM to do a full-article pass to tighten the narrative, remove repetition, and clarify definitions—*without* dumbing things down.

7. **Labeling, ethics, and meta sections**
   - Ensure the post clearly distinguishes:
     - **Fully human-created**
     - **Human + AI enhanced**
     - **Fully AI-generated**
   - Add any revenue-sharing or ethics sections if relevant.

8. **Final polish for publication**
   - Add title, subtitle, intro hook, conclusion, and (optionally) an implementation roadmap.

---

### Prompt 1: Extract thesis, audience, and constraints from transcript

**Goal**: Turn a messy transcript into a clear statement of what the post is about and who it’s for.

Copy‑paste this, then add your transcript at the end:

---

**Prompt**

You are helping me turn a raw speech transcript into a long-form blog post.

I will give you a messy, spoken transcript. From it, extract and write the following, concisely:

1. **Working title options** (3–5 variants).
2. **One-sentence thesis**: what the post is fundamentally arguing.
3. **Target audience**: who this is for (and who it’s explicitly not for).
4. **Scope and boundaries**: what this post will cover vs. what is out of scope.
5. **Key example(s)** to use throughout (e.g., recreation programming for retirement homes).
6. **List of major themes** you hear in the transcript (bullets only, no prose yet).

Do **not** clean up the transcript itself yet. Just interpret it.

Here is the transcript:

TRANSCRIPT_HERE

---

### Prompt 2: Extract and cluster ideas into candidate sections

**Goal**: Turn raw, overlapping ideas into coherent sections / phases.

Use the thesis + themes from Prompt 1, then run:

---

**Prompt**

Using the thesis, target audience, and themes we already identified, analyze the transcript again and produce:

1. **Idea inventory**
   - A bullet list of all distinct ideas you can find in the transcript.
   - For each idea, add a short tag like `[judge calibration]`, `[prompt discovery]`, `[raw idea seeding]`, `[real-world validation]`, `[revenue sharing]`, etc.

2. **Idea clustering**
   - Group related ideas into **candidate sections** for a blog post.
   - For each section, give:
     - A short working heading.
     - A 1–2 sentence summary of what that section should argue or explain.
     - The list of idea-tags that belong in that section.

3. **Gaps and clarifications**
   - List any questions where the transcript seems incomplete or ambiguous.
   - Mark which sections might need more examples, data, or definitions.

Keep everything in bullet / outline form. Do **not** write full paragraphs yet.

Here is the transcript again for reference:

TRANSCRIPT_HERE

---

### Prompt 3: Design a full outline that fits the transcript (framework, essay, or research-notes style)

**Goal**: Turn clusters into a post outline whose structure matches what the transcript is trying to be (framework/how-to, opinionated essay, or research/notes synthesis).

---

**Prompt**

You are designing a detailed outline for a blog post based on my transcript.

First, infer which **shape** best fits this material (and tell me your choice at the top of your answer):
- **Framework / phased system** (similar to `02-WIP-evolutionary-content-generation.md`): numbered phases, implementation roadmap, clear example that runs through the post.
- **Opinionated comparative essay** (similar to `00-WIP-Cascade.md`): clear biases section, narrative argument, comparison of approaches, lots of subheadings and anecdotes/examples.
- **Research / notes synthesis** (similar to `01-WIP-granular.md`): definitions, external references, quotes, and structured notes that build toward an argument.

If I provide a preferred shape in the placeholder `PREFERRED_SHAPE`, use that; otherwise, pick the one that best matches the transcript and thesis.

Using:
- The thesis and themes we identified.
- The idea clusters and candidate sections you produced earlier.
- The inferred or preferred shape: PREFERRED_SHAPE (or your best guess if left blank).

Produce:

1. **Final outline**
   - Top-level sections (H2) and subsections (H3) for the full post.
   - For each section, write 2–4 bullet points describing the specific arguments and examples that should appear there.

2. **Running example plan (if applicable)**
   - If the material lends itself to a running example (e.g., retirement-home recreation programming), specify how that example will be used in each major section so the example threads through the piece.

3. **Labeling and ethics integration (if relevant to the transcript)**
   - If the transcript discusses AI assistance, data sourcing, or economics, show where in the outline we explicitly:
     - Distinguish fully human / human+AI / fully AI content.
     - Describe revenue sharing and content economics.
     - Talk about clearly labeling AI-generated content so users know what they are reading.

Return only the outline; do not write the full prose yet.

---

### Prompt 4: Draft a single section from outline + transcript

**Goal**: Work section-by-section so you keep control, while the LLM does the heavy lifting.

You’ll repeat this prompt for each section (Introduction, Phase 1, Phase 2, …).

---

**Prompt**

We are now going to draft **one section** of the blog post.

I will give you:
- The **overall thesis and audience**.
- The **post outline**, including where this section fits.
- The **raw transcript excerpts** that are most relevant to this section.

Your job:
1. Write this section in **clear, structured prose** with markdown headings that match the outline.
2. Preserve my **voice and style** from the transcript: opinionated, concrete examples, explicit about slop vs. quality, and comfortable with technical details (LLMs, embeddings, cosine similarity, batch providers like Groq, etc.).
3. Integrate the **retirement-home recreation programming** example where appropriate.
4. Make sure any technical ideas are **explained once, clearly**, but don’t over-explain for a non-technical audience; assume readers are technically literate.
5. Avoid generic AI content clichés.

Inputs:
- Thesis + audience: THESIS_AND_AUDIENCE_SUMMARY
- Outline (relevant excerpt):

OUTLINE_SNIPPET_FOR_THIS_SECTION

- Transcript excerpts for this section:

TRANSCRIPT_EXCERPTS_FOR_THIS_SECTION

Now, draft this section only. Do not rewrite other sections or the title.

---

### Prompt 5: Global refinement pass (consistency + de-slop)

**Goal**: Once all sections are drafted, get a single pass that tightens the whole article and enforces “no slop” standards.

---

**Prompt**

Here is a full draft of a blog post created section-by-section.

Your job now is to do a **global refinement pass** with these goals:

1. **Enforce narrative coherence**
   - Ensure the introduction sets up all the later sections.
   - Make sure each phase logically leads into the next.
   - Add or adjust short bridging sentences between sections when needed.

2. **Maintain anti-slop standards**
   - Remove generic AI-writing fluff (vague statements, overused metaphors, empty claims).
   - Tighten sentences while keeping nuance.
   - Prefer specific, domain-grounded examples (retirement-home recreation) over abstract generalities.

3. **Clarify technical ideas once**
   - Ensure that key concepts (judge calibration, prompt discovery, genetic mutation, embedding similarity, popularity scoring, seasonality, model routing, batch providers like Groq, etc.) are each defined clearly once, then referenced consistently.

4. **Find missing or weak sections and ambiguous reasoning**
   - Identify any parts of the draft where:
     - A concept is mentioned but never really explained.
     - A step in the workflow is implied but not described concretely.
     - The argument jumps or feels hand-wavy or ambiguous.
   - Propose **specific fixes**, such as:
     - Adding a new subsection.
     - Expanding an existing paragraph with a clearer explanation or example.
     - Reordering a couple of paragraphs to make the logic flow better.
   - At the end of your response, include a short "**Suggested structural and clarity fixes**" list summarizing the main missing sections or ambiguous points you found and how you addressed (or would address) them.

5. **Preserve labeling and ethics**
   - Make sure the sections about:
     - Fully human vs. human+AI vs. fully AI content.
     - Revenue sharing and content economics.
     - Transparent labeling of AI-generated content.
   - Are prominent, clear, and consistent in terminology.

6. **Output format**
   - Return the **full, refined markdown** for the post, ready to save as a `.md` file.

Here is the current draft:

FULL_DRAFT_HERE

---

### Prompt 6: Short meta-review for future you

**Goal**: After a post is drafted, capture quick notes to improve your next speech-to-post run.

---

**Prompt**

You have the final refined blog post and (optionally) access to the original transcript.

Write a **short meta-review for my future self** with:

1. **What worked well** in the speech → outline → draft workflow.
2. **Where the transcript was confusing or wasteful**, and how I could speak differently next time (e.g., clearer section markers, fewer digressions, more concrete examples).
3. **What prompts could be improved** in this workflow (e.g., additional constraints, missing steps, better ways to surface edge cases).
4. **A 5-bullet checklist** I can glance at before recording the next batch of voice notes, so that the raw transcript is easier to structure.

Keep it concise and actionable.

If you reference the transcript, don’t quote it at length—just summarize patterns.

---

### How to use this in practice

- **Step 0**: Record voice notes whenever you have ideas (while walking, cooking, etc.).
- **Step 1**: Transcribe and paste into Prompt 1.
- **Step 2**: Run Prompts 2 and 3 to get a strong outline.
- **Step 3**: Iterate through Prompt 4 for each section.
- **Step 4**: Run Prompt 5 for a global refinement.
- **Step 5**: Optionally run Prompt 6 to improve your next recording session.

Over time, you can specialize this workflow for specific recurring topics (e.g., LLM workflows, system design, content economics) by pre-filling parts of the prompts or saving topic-specific variants alongside this file.


