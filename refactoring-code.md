---
title: "Refactoring Code with GitHub Copilot"
teaching: 10
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- What are the different ways to use Copilot to refactor code?
- What is the difference between inline suggestions, plan mode, and agent mode?
- Why is it important to review and critically evaluate AI-generated code suggestions?
- How can plan mode help structure a refactoring task before making any changes?
- What risks should you consider when delegating larger-scale code changes to an AI agent?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Describe the three Copilot refactoring modes (inline suggestions, agent mode, plan mode) and when each is appropriate
- Use inline suggestions and agent mode to make targeted, small-scale code changes
- Apply plan mode to generate and review a structured refactoring plan before implementing changes
- Evaluate AI-generated code suggestions to identify whether changes are correct and maintainable
- Appreciate the importance of increasing skepticism and review as the autonomy and scope of AI-assisted changes increases

::::::::::::::::::::::::::::::::::::::::::::::::

In the previous episode we looked at Copilot giving us guidance and advice to improve our code.
In this episode, we'll look at how Copilot can assist with code modifications more directly,
looking at the following features:

- `Inline suggestions` - where Copilot provides coding suggestions as you type
- `Plan mode` - where an built-in Copilot agent explores our codebase and generates a step-by-step plan for improvement
- `Agent mode` - where Copilot directly and autonomously undertakes either small or large scale changes on request, with a single option to approve the changes at the end of the process

These are in ascending order of autonomy, authority and scope that we delegate to Copilot to make changes.
As we'll see, as we delegate more authority and scope,
the more we should increase our skepticism and diligence in reviewing and understanding suggestions made by such tools.

## Inline Suggestions

Within VSCode, Copilot can provide "inline" suggestions as we type,
which go much further than typical IDE autocomplete suggestions.

If we select the Copilot icon again in the status bar,
we can see the current inline settings:

![](fig/copilot-inline-settings.png)

So here, we can see that inline settings will apply to all Python files.
These suggestions will appear as "ghosted text" suggestion which you can autocomplete with tab,
very similar to how they appear with standard VSCode autocomplete.
There are also `Next edit suggestions` which go beyond the immediate context to make suggestions in other places in your code.
These predict the location and the content of the next edit you'll want to make.

Let's say we want to add a new section describing our coding style.
Add the following at a suitable point in the file (you won't need to add the header if it already exists):

```markdown
## Python Coding Style
- 
```

You should see a suggestion appear direcly after your cursor,
something like `Use type hints for function parameters and return types`, `Use 'snake_case' for variable and function names` or similar.
You can accept this suggestion by pressing `Tab`.
If you continue to add new lines after this, you may find it continues to suggest other things to include,
so we end up with, for example:

```markdown
### Coding Style
- Use type hints for function parameters and return types
- Use `snake_case` for variable and function names
- Use `CamelCase` for class names
```

It does this by rapidly incorporating contextual information from a number of sources to infer a suggestion,
including:

- The code file you are editing
- Any code you have currently selected
- Frameworks, languages and dependencies
- Any instructions files

Copilot suggestions are a starting point, but we should alwyays review, understand, and amend as necessary,
as opposed to blindly accepting suggestions.

:::::::::::::::::::::::::::::::::::::: challenge

## Amend the Suggestions

3 mins.

Review the `copilot-instructions.md` file, and add/amend to fit your coding style.

:::::::::::::::::::::::::: solution

For example:

```markdown
### Python Coding Style
- Comment code sections for clarity, especially to explain purpose and logic, and to describe data processing steps
```

As we'll see shortly, these coding style guidelines will inform future refactoring of our code.

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

Of course, we can also do this for code.
In `inflammation-plot.py`, begin adding an additional parameter to the functions calls in the code,
by placing an additional `, ` at the end of the given list of parameters, and see which inline suggestions are being made, e.g.:

```python
    axes1.plot(data.mean(axis=0), ...)
```

You might see a suggestion for a `label` and/or `color` parameter.
For example, add in one for `color`:

```python
    axes1.plot(data.mean(axis=0), color='blue')
```

If we approve this change and then do the same for the other `axes2` and `axes3` variables,
it suggests variants of that for the other calls to `plot`,
and you can approve these by selecting `Tab`, e.g.

```python
    axes2.plot(data.mean(axis=0), color='red')
    axes3.plot(data.mean(axis=0), color='green')
```

So the suggestions made are based on the context of the code you are editing:
it's generating the most likely suggestions based on what we've entered before.
We've added a `color` parameter to a previous `.plot` call,
so doing something similar for other `.plot` calls is likely.

## Agent Mode: What about Small Changes?

Agent mode differs from inline suggestions by offering the ability to enact changes step-by-step.
Unlike inline suggestions which appear as you type, this mode allows you to request broader changes across multiple lines or functions,
so it can be used for small or large scale changes.

To use Agent mode to make a small edit, you highlight the code you want to modify before requesting the change you want.

For example:

1. Select `+` to create a new chat conversation.
1. Select `inflammation-plot.py` in the chat context.
1. Select `Agent` as the Copilot mode in the chat window.
1. Select a model of your choice.
1. Select the entire for loop in `inflammation-plot.py`; you'll notice that the chat context now includes this file with the selected line numbers.
1. Enter `Add a comment about this code above this loop`.
1. Press `Enter`.

You'll now see a comment added above the loop highlighted in green, e.g.

```python
# Process each inflammation data file and generate a 3-panel visualization
# showing the mean, maximum, and minimum inflammation values across patients
```

You'll also see a `Keep` or `Undo` pop-up displayed at the bottom.
Read through the comment, and if you agree that the comment summarises the code sufficiently, select `Keep`.

::::::::::::::::::::::::::::::::: callout

## The Temptation to Blindly Accept!

So note that here, we properly scrutinise the suggestion as opposed to accepting it blindly!
it would be all too easy to just assume it's correct and just accept it,
but it's helpful to remember that tools like Copilot are like a more helpful autocomplete,
not a thinking teammate.
As such, skepticism and review must become a key practice when using such tools.

:::::::::::::::::::::::::::::::::::::::::

### Plan Mode: Creating a Plan for Refactoring

We could now use agent mode to do larger-scale changes,
such as refactoring our entire codebase.
However. instead of "one-shotting" the development of code using generative AI, 
a far better approach is to plan and implement our code in a step-wise, 
incremental fashion.
This way, we can understand and review changes before they are implemented.
So how should we go about this?

Let's use the built-in VSCode plan agent that helps developers break down tasks into clear and actionable steps before writing code.
Instead of jumping straight into implementation, it researches the requested task, analyses any existing project code in read-only mode, and generates structured step-based plans for features or for other code modification activities.
In short, it aims to guide users through a thoughtful planning phase prior to coding to reduce errors and encourage better design and implementation decisions.

Let's try it out now.

1. Select `+` to create a new chat conversation.
1. Select `Plan` from the Copilot mode dropdown in the chat panel.
1. Select `inflammation-plot.py` in the chat context.
1. Select `Claude Haiku 4.5` selected in the model dropdown.
1. Enter the following into the chat:

   `How should I refactor this code to make it more modular, readable, and documented?`

1. Press `Enter`.
1. Answer any clarifying questions from the plan agent.
1. Observe the step-by-step thinking and actions undertaken by the agent.
1. When the planning agent concludes, you'll have a number of options to proceed. Select the option to `Open in Editor`, and `Keep`.
1. Save the file that appears by selecting `Save as prompt file` which should appear in the bottom right of the editing window.

You should find you end up with something similar to this, saved as a prompt file (either in the repository root or in the `.github/prompts` directory),
although the content will likely differ:

```markdown
# Plan: Refactor inflammation-plot.py for modularity and readability

## TL;DR
Extract the visualization logic into dedicated functions with clear responsibilities, add comprehensive docstrings at module and function levels, use descriptive variable names, and apply the main-guard pattern. This will make the code more reusable, testable, and easier to maintain.

## Steps

1. **Add module-level docstring** — Describe what the script does, its input (data files), and output (PNG plots)

2. **Extract plot creation into `create_inflammation_plot()` function** — Move the subplot creation and data plotting logic (lines 14-27) into a separate function that takes a data array and returns a matplotlib figure. This function handles:
   - Creating the figure with specified dimensions
   - Setting up three subplots
   - Plotting mean, max, and min statistics
   - Returning the figure object

3. **Extract statistics calculation and plotting into helper function (optional)** — Create a small helper like `plot_statistic()` to reduce repetition when adding the three plots (mean, max, min). Or keep it simple and just document the pattern inline.

4. **Create main processing function** — Extract the file globbing, sorting, and iteration loop into a `main()` or `process_files()` function that orchestrates the workflow

5. **Add function docstrings** — Each function needs:
   - One-line summary
   - Parameters (what they are, expected types/format)
   - Returns (what is returned)
   - Brief description of behavior

6. **Improve variable names** — Replace `axes1`, `axes2`, `axes3` with `axes_avg`, `axes_max`, `axes_min` for clarity

7. **Extract configuration constants** — Move hardcoded values to the top:
   - Figure size: `FIGURE_SIZE = (10.0, 3.0)`
   - Colors: `COLOR_MEAN = 'blue'`, `COLOR_MAX = 'red'`, `COLOR_MIN = 'green'`
   - Output extension: `OUTPUT_EXT = '.png'`

8. **Add main guard** — Wrap the entry point with `if __name__ == '__main__':` to allow importing as a module

## Relevant files
- inflammation-plot.py — Refactor entire file with functions, docstrings, and constants

## Verification
1. Script runs without errors: `python inflammation-plot.py`
2. All 12 PNG files are generated in the workspace root
3. Each function has a complete docstring (module, function-level)
4. Variable names clearly indicate purpose (no generic `axes1`, `axes2`, `axes3`)
5. All configuration values are defined as constants at module top
6. Code is syntactically valid (no PEP8 linting errors per `.github/copilot-instructions.md`)

## Decisions
- Keep the core logic simple — don't over-engineer with factories or complex patterns
- Extract constants only for values used more than once or that represent meaningful configuration
- Document the statistics meaning (average across patients, max/min across time points) in function docstrings

## Further Considerations
1. **Testing strategy** — Would you like the refactored code to be more testable (e.g., separating I/O from logic so functions can be unit tested)?
   - Recommended: Separate file I/O and visualization logic, making the core plot function testable with mock numpy arrays

2. **Output directory** — Currently saves PNG files in the workspace root. Should outputs go to a dedicated `output/` directory instead?
   - Recommended: Keep in root for now (inline with current behavior), but mention this as future improvement

3. **Error handling** — Should the code validate that data files exist and handle malformed CSV gracefully?
   - Recommended: Add basic try-except around file loading with helpful error messages
```

Just by itself, this feature is incredibly useful.
We should of course review the plan and ensure it's suggestions are suitable and in line with what we want from this codebase and amend it appropriately,
and once ready, we could then manually follow this plan and implement the changes.
That way, we maintain full control of those changes.

The review and refine component is particularly important.
We should ask questions such as:

- Are there any questions posed by the plan agent that we need to answer?
- Do the suggested changes solve the actual purpose of the request, or only a plausible-looking version of it?
- Has the AI changed anything outside the intended scope?
- Is the proposed design consistent with the existing architecture and coding conventions?
- Importantly, is the code simple enough to understand, maintain, and review?
- Are there meaningful new or updated tests that prove the change works?
- Could the change introduce security, privacy, licensing, or performance risks?
- Are any dependencies, generated code, or copied patterns acceptable for the project?
- Can a human developer explain and take responsibility for every part of the change?

Essentially, amend the plan until it is within the scope of what is required,
using techniques, design choices, and technologies that we understand,
for which we are capable of taking responsibility to adapt and maintain in the future.
If we can't do these things, we need to refine the plan.

:::::::::::::::::::::::::::::::::::::: challenge

## Refine the Plan

5 mins.

Review the generated plan, answering any questions and refining it to your satisfaction,
then save the file.

:::::::::::::::::::::::::: solution

For example, a generated plan asked questions about the following points:

```markdown
## Further Considerations
1. **Testing strategy** — Would you like the refactored code to be more testable (e.g., separating I/O from logic so functions can be unit tested)?
   - Recommended: Separate file I/O and visualization logic, making the core plot function testable with mock numpy arrays

2. **Output directory** — Currently saves PNG files in the workspace root. Should outputs go to a dedicated `output/` directory instead?
   - Recommended: Keep in root for now (inline with current behavior), but mention this as future improvement

3. **Error handling** — Should the code validate that data files exist and handle malformed CSV gracefully?
   - Recommended: Add basic try-except around file loading with helpful error messages
```

Which were added to a `Decisions` section in the plan:

```markdown
- Separate file I/O and visualization logic, making the core plot function testable with mock numpy arrays
- Place output files within a dedicated `output/` directory
- Add basic try-except around file loading with helpful error messages
```

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

So why not just use the agent mode to develop the code without developing a plan first?
Whilst this would be faster, there are some key benefits to a planning step:

- Importantly, we are now moving from **ad-hoc** development to **intentional** development,
which externalises key decisions, and forces us to consider ways forward and make such decisions openly,
and capture these within a defined plan that we validate and refine before moving to implementation.
- We may extend this plan with other sections and further detail as needed.
- It has created a concrete document we can discuss and refine with colleagues before we proceed.
- Importantly, it also provides a "checkpoint": if the implementation is unsatisfactory we can remove the implementation, amend the plan, and ask Copilot to create the implementation again.


## Agent Mode: Larger-scale Changes

We can also ask Copilot in agent mode to make much larger, potentially multi-file changes across our codebase.
So instead of asking Copilot to change a specific piece of code, you give it a goal, such as adding a feature, or refactoring a module.
Copilot then plans how to achieve that goal and works across the repository to do so.
It may read and modify multiple files, add or update tests, adjust configuration, and iterate over several steps before presenting the result.
In this way, agent mode behaves more like a junior developer taking on a task, rather than a pair programmer responding line-by-line.

Note the increase in authority to modify code, which represents a much greater risk:
the impact of changes is greater and requires more careful (and potentially more involved) review.

Since we've already come up with a plan for refactoring our code,
let's now use agent mode to follow this plan and implement the changes.

1. Select `+` to create a new chat conversation
1. Select `inflammation-plot.py` in the chat context, then select `+` and the plan file to ensure it's also included.
1. Set Copilot's mode to `Agent`
1. Select `Claude Haiku 4.5` from the models dropdown
1. Enter the following chat prompt: `Refactor the code following the plan`
1. Once suggested improvements appear, review the changes and either:
   - If you agree with them and think they are an improvement on the original code, `Keep` the suggestions
   - If you don't agree with them, cancel the suggestions and select the circular `Retry` icon at the bottom left of the chat response until you get something more acceptable

An example of output:

```python
"""
Inflammation data visualization script.

This module processes inflammation data from CSV files and generates
visualization plots showing average, maximum, and minimum inflammation
values across patients over time.

Input: CSV files in the data/ directory (inflammation-*.csv)
Output: PNG plots saved to the output/ directory for each data file
"""

import glob
import numpy as np
from matplotlib import pyplot as plt

# Configuration constants
FIGURE_SIZE = (10.0, 3.0)
COLOR_MEAN = 'blue'
COLOR_MAX = 'red'
COLOR_MIN = 'green'
OUTPUT_EXT = '.png'
DATA_PATTERN = 'data/inflammation-*.csv'
OUTPUT_DIR = 'output/'


def create_inflammation_plot(data):
    """
    Create a visualization plot for inflammation data.
    
    Parameters
    ----------
    data : numpy.ndarray
        A 2D array where rows represent patients and columns represent
        time points. Each cell contains an inflammation value.
    
    Returns
    -------
    matplotlib.figure.Figure
        A figure object containing three subplots showing the mean,
        maximum, and minimum inflammation values across patients.
    """
    fig = plt.figure(figsize=FIGURE_SIZE)
    
    axes_avg = fig.add_subplot(1, 3, 1)
    axes_max = fig.add_subplot(1, 3, 2)
    axes_min = fig.add_subplot(1, 3, 3)
    
    axes_avg.set_ylabel('average')
    axes_avg.plot(data.mean(axis=0), color=COLOR_MEAN)
    
    axes_max.set_ylabel('max')
    axes_max.plot(data.max(axis=0), color=COLOR_MAX)
    
    axes_min.set_ylabel('min')
    axes_min.plot(data.min(axis=0), color=COLOR_MIN)
    
    fig.tight_layout()
    
    return fig


def process_files():
    """
    Process all inflammation data files and generate visualization plots.
    
    This function:
    1. Finds all inflammation CSV files in the data directory
    2. Loads each file as a numpy array
    3. Creates a visualization plot for the data
    4. Saves the plot as a PNG file in the output directory
    
    Returns
    -------
    None
    """
    filenames = glob.glob(DATA_PATTERN)
    filenames.sort()
    
    for filename in filenames:
        print(f"Processing {filename}")
        
        try:
            data = np.loadtxt(fname=filename, delimiter=',')
        except FileNotFoundError:
            print(f"Error: File {filename} not found")
            continue
        except ValueError as e:
            print(f"Error: Could not parse {filename}: {e}")
            continue
        
        fig = create_inflammation_plot(data)
        
        base_filename = filename.split('/')[-1].replace('.csv', '')
        output_filename = f"{OUTPUT_DIR}{base_filename}{OUTPUT_EXT}"
        fig.savefig(output_filename)
        print(f"Saved plot to {output_filename}")


if __name__ == '__main__':
    process_files()
```

It will also provide a list of changes made.
As per the plan above:

- Constants have been defined at the top, removing hardcoded elements
- Modularised the codebase by refactoring into two separate functions
- Docstrings have been added for each of the functions and the module
- The subplots are generated within a loop using `zip()` to provide corresponding pairs of array elements into the loop.
If we didn't like this particular style, we might use Edit mode on this segment to simplify it

Note that it differs substantially from the version shown from a similar question made in Ask mode earlier,
since LLMs are probablistic,
but also more importantly because it was implemented based on a plan we refined.

However - whilst it is more modular,
it's now a lot larger whereas before it was 32 lines - 
we might consider this to be quite an over-engineered overkill!

::::::::::::::::::::::::::::::::: callout

## What if I don't get a Good Response?

If you aren't getting a good response despite retrying several times,
it suggests that the prompt is either not reflective of what you're really after or isn't specific enough,
so try amending the request.

:::::::::::::::::::::::::::::::::::::::::

Now once the suggestions have been integrated,
we're still able to undo these changes, e.g. either by selecting `Edit` and `Undo` from the VSCode menu,
or pressing `Ctrl + Z` (or `Cmd/Windows Key + Z`).
We're also able to edit a previous prompt in the chat window,
perhaps adding more specifics for what we want.

## Summary

In this episode we explored three ways GitHub Copilot can assist with refactoring code,
each offering a different level of autonomy:

- **Inline suggestions**, which provide lightweight, context-aware completions as you type — useful for small, targeted edits with immediate feedback.
- **Agent mode**, which can apply changes across multiple lines or functions based on a single instruction — powerful for targeted refactoring, but requires careful review of the result.
- **Plan mode**, that analyses your codebase and generates a structured, step-by-step refactoring plan *before* any changes are made — shifting development from ad-hoc to intentional, and giving you a reviewable document to refine and discuss before proceeding.

A key theme throughout is the importance of **critical review**:
as the autonomy and scope of AI assistance increases, so too must your scrutiny.
AI-generated suggestions are a starting point, not a finished product,
and whilst they increase productivity,
you remain responsible for understanding and maintaining every change that goes into your codebase.

::::::::::::::::::::::::::::::::::::: keypoints 

- Copilot offers three levels of refactoring support: inline suggestions, agent mode, and plan mode, in ascending order of autonomy and scope.
- Inline suggestions appear as ghosted text while typing and are driven by immediate code context.
- Agent mode can make targeted edits across multiple lines or functions when given a specific instruction.
- Plan mode analyses the codebase and produces a step-by-step refactoring plan without making any changes, enabling review before implementation.
- AI-generated suggestions should always be reviewed and understood before accepting; increasing autonomy requires increasing scrutiny.

::::::::::::::::::::::::::::::::::::::::::::::::
