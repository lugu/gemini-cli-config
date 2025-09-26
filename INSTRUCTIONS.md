# Meta instructions for problem resolution

This is the guidelines for the AI assisted process of resolving problems. To
resolve the problem, follow the workflow defined below. Use those guidelines to
generate prompts for Gemini. Those prompt instructions must be direct and
precise. They must use verbs like "must" and "shall". Details like the file
names and the line must be present.

The resolution of the problem is always done in 3 steps:

1.  Discovery / analysis
2.  Planning
3.  Testing and implementation

Between each step, the Gemini generates a markdown file explaining its
findings. This file serves as input to the next phase.

## Overview of the process

### Step 0: Initialization

User run the following commands to create a work directory and copy these
instructions. The temporary directory is referred to `gemini-workdir` in this
file.

```shell
WORKDIR=`mktemp -d -p . gemini.XXXX`
rsync -ai /google/src/head/depot/google3/experimental/users/ludovicguegan/prompts/meta/INSTRUCTIONS.md $WORKDIR
sed -i "s#gemini-workdir#$WORKDIR#g" $WORKDIR/INSTRUCTIONS.md
touch $WORKDIR/PROBLEM.md
```

User writes a description of the problem to solve in `PROBLEM.md` and commit
the change in git.

After initialization, the work directory contains:

-   `INSTRUCTIONS.md` -- this file with the instructions to drive the resolution
    of the problem
-   `PROBLEM.md` -- a problem statement from the user

### Step 1: Discovery & Analysis

The user will prompt Gemini with:

```txt
/clear
Step 1: Discovery. Given the problem statement at @gemini-workdir/PROBLEM.md and the process at @gemini-workdir/INSTRUCTIONS.md, perform the Discovery & Analysis phase and generate the results in @gemini-workdir/DISCOVERY.md.
```

Gemini will:

1.  Analyze the problem statement.
2.  Follow the "Discovery phase" guidelines below.
3.  Generate `gemini-workdir/DISCOVERY.md` containing a summary of the analysis,
    hypotheses, investigations, and a preferred solution.

### Step 2: Planning

The user will prompt Gemini with:

```txt
/clear
Step 2: Planning. Given the problem statement at @gemini-workdir/PROBLEM.md, the process at @gemini-workdir/INSTRUCTIONS.md, and the analysis in @gemini-workdir/DISCOVERY.md, perform the planning phase and generate the results in @gemini-workdir/PLAN.md.
```

Gemini will:

1.  Use the insights from `DISCOVERY.md`.
2.  Follow the "Planning phase" guidelines below.
3.  Generate `gemini-workdir/PLAN.md` containing a detailed, step-by-step
    implementation plan.

### Step 3: Implementation

The user will prompt Gemini with:

```txt
/clear
Step 3: Implementation. Given the problem statement at @gemini-workdir/PROBLEM.md, the guidelines at @gemini-workdir/INSTRUCTIONS.md, the analysis in @gemini-workdir/DISCOVERY.md and the plan in @gemini-workdir/PLAN.MD, execute the plan and summarize the changes in @gemini-workdir/CHANGE.md.
```

Gemini will:

1.  Follow the steps outlined in `PLAN.MD`.
2.  Perform the necessary code changes or actions.
3.  Generate `gemini-workdir/CHANGE.md` summarizing the problem, solution, and
    implementation.

## Discovery phase Guidelines

Your job is to analyze the task and the system. Postulate hypotheses and
investigate potential solutions.

**Key Activities:**

-   **Formulate the problem:** Rephrase the problem in specific, actionable
    terms.
-   **Decompose:** If complex, break down into smaller, sequential sub-problems.
-   **Investigate each sub-problem:**
    -   **Reproduce:** Determine how to reproduce the issue (if applicable).
    -   **Confirm:** Run tests to confirm the problem, requesting more
        information if needed.
    -   **Gather Evidence:** Collect logs, error messages, and other data.
    -   **System Info:** Identify relevant code paths, boq/pod nodes,
        dashboards, and documentation.
    -   **Describe:** Briefly describe the system or component being addressed.
    -   **Hypothesize:** Formulate several potential causes or solutions.
    -   **Critique:** Evaluate the strengths and weaknesses of each hypothesis.
    -   **Prioritize:** Order hypotheses from most to least likely.
    -   **Investigate:** Verify hypotheses using code, documentation, and tests.
        Be cautious of outdated documentation.
    -   **Summarize:** Document findings for each investigation.
    -   **Iterate:** If unsolved, identify missing information and refine
        hypotheses.
-   **Propose Solution:** Once a preferred solution is clear, describe it in
    detail, including affected files and line numbers. Use strong, directive
    language (e.g., "The function must be changed to...").

**Constraints:**

-   **Read-Only:** Do NOT change code during this phase unless strictly
    necessary to test a hypothesis. Any temporary changes must be reverted.

**Output:**

-   A markdown file (`DISCOVERY.MD`) summarizing the analysis, findings, and the
    proposed solution.

## Planning phase Guidelines

Based on the output of the discovery phase (`DISCOVERY.MD`), create a concrete
plan of action.

**Key Activities:**

-   **Task Breakdown:** Define a set of minimal, isolated tasks.
-   **Verifiability:** Ensure each task has a clear way to verify its
    completion.
-   **Testing:** Include a test case for each task to demonstrate the expected
    behavior.
-   **Sequencing:** Order the tasks logically.
-   **Final Steps:** The plan must conclude with:
    -   A task to summarize the overall changes made.
    -   A task to create `gemini-workdir/CHANGE.md`, suitable for a commit
        message, explaining the problem and the implemented solution.

**Output:**

-   A markdown file (`PLAN.MD`) detailing the step-by-step plan.

## Implementation phase Guidelines

Execute the plan outlined in `gemini-workdir/PLAN.MD`.

**Key Activities:**

-   Follow each step in `PLAN.MD` sequentially.
-   Act autonomously, iterating on steps as needed until complete.
-   If a task cannot be completed, document the rationale in
    `gemini-workdir/FAILURE.MD`, explaining the blockers.

**Output:**

-   The implemented solution.
-   A markdown file (`gemini-workdir/CHANGE.MD`) summarizing the changes. This
    file is written in a way that it can be used to submit the change. It
    explains the problem and described how the change solve the problem. The
    first line of the file shall be a very concise description of the change
    (less than 120 characters) followed by an empty line.
