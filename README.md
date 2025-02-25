# Solana Cursor AI rules

## [Coding Guidelines `anchor.mdc` file is here](./.cursor/rules/anchor.mdc)

Here's a Cursor AI prompt I'm using for my Anchor projects.

## Creating new Projects in Composer

Here's the process I'm currently using to get results I'm happy with.

Make a `cursor/rules` folder in the root of your project. Add the `anchor.mdc` file there.

Then:

- Make a proof of concept in Excel. Yes Excel. That allows you to think about the economics of your project before you implement them.

- Install Rust Analyzer, use 'Cursor: New Composer' and the Anchor 'multiple' template so it's not a single giant lib.rs.

- Write this prompt:
  > I want you to create a (whatever) using Anchor and Rust. But first I want you to to tell me what you think about this design.
  >
  > (Your design - 'Each foo contains many bars, when the doThing instruction handler is run, tokens will be moved from....' etc.)
  >
  > Add the testing scenario you made in Excel to help it understand what you want to build.

Then:

- Let it criticize the design. Upgrade the prompt and restart from Step 1.
- When it doesn't have much feedback left, let it code.
- Cursor will start coding incrementally and then talk to you. Fix all the Rust Analyzer errors immediately and commit to git before continuing.
- Iterate through the generation process until you're done.
