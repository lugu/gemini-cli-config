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
instructions. The temporary directory is referred to `gemini.XXXX` in this
file.

```shell
WORKDIR=`mktemp -d -p . gemini.XXXX`
touch $WORKDIR/PROBLEM.md
```

Note: If there are several directories matching this pattern, use the more
recent one.

User writes a description of the problem to solve in `PROBLEM.md` and commit
the change in git.

### Step 1: Discovery & Analysis

The user will prompt Gemini with:

```txt
!tmux new-session -d -s discovery 'gemini -c "/discovery"'
```

Gemini will:

1.  Analyze the problem statement.
2.  Follow the "Discovery phase" guidelines below.
3.  Generate `gemini.XXXX/DISCOVERY.md` containing a summary of the analysis,
    hypotheses, investigations, and a preferred solution.

### Step 2: Planning

The user will prompt Gemini with:

```txt
!tmux new-session -d -s planning 'gemini -c "/planning"'
```

Gemini will:

1.  Use the insights from `DISCOVERY.md`.
2.  Follow the "Planning phase" guidelines below.
3.  Generate `gemini.XXXX/PLAN.md` containing a detailed, step-by-step
    implementation plan.

### Step 3: Implementation

The user will prompt Gemini with:

```txt
!tmux new-session -d -s implementation 'gemini -c "/implementation"'
```

Gemini will:

1.  Follow the steps outlined in `PLAN.MD`.
2.  Perform the necessary code changes or actions.
3.  Generate `gemini.XXXX/CHANGE.md` summarizing the problem, solution, and
    implementation.
