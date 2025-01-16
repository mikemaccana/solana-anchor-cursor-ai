# solana-anchor-ai-prompt

After a few days of messing around, here's the process I'm currently using to get results I'm happy with:

- Make a proof of concepts in Excel. Yes Excel. That allows you to think about the economics of your project before you implement them.

- Install Rust Analyzer, use 'Cursor: New Composer' and the Anchor 'multiple' template so it's not a single giant lib .rs.

- Write this prompt:
> I want you to create a (whatever) using Anchor and Rust. But first I want you to to tell me what you think about this design. 
>
> (Your design - 'Each foo contains many bars, when the doThing instruction handler is run, tokens will be moved from....' etc.)
> 
> Add the testing scenario you made in Excel to help it understand what you want to build.

Then some coding guidelines for how you want your Anchor projects to look. This will be longer than you think. Mine are below.

- Let it criticize the design. Upgrade the prompt and restart from Step 1.
- When it doesn't have much feedback left, let it code.
- Cursor will start coding incrementally and then talk to you. Fix all the Rust Analyzer errors immediately and commit to git before continuing.
- Iterate through the generation process until you're done.

# Coding Guidelines to add in prompt

## Coding guidelines

There is an Anchor project in the 'programs/MY PROGRAM' directory and tests in the 'tests' directory. You may only modify files in these folders.

Use a newline after each key in the account constraints, so the macro and the matching key/value have some space from other macros and their matching key/value.

Create files inside the 'state' folder for whatever state is needed.

Use 'context' rather than 'ctx'. In general, avoid abbreviations, and use full words.

Delete unused constants, unused instruction handlers and comments that no longer apply.

Use 'context.bumps.foo' not 'context.bumps.get("foo").unwrap()' - the latter is outdated. 

When making structs ensure strings and Vectors have a max_len attribute. Vectors have two numbers, the first is the max length of the vector, the second is the max length of the items in the vector.

Use the INIT_SPACE macro for all structs stored onchain. Make a constant for ANCHOR_DISCRIMINATOR that is a usize of 8. Do not use magic numbers. I don't want to see '8 + 32' or whatever. Do NOT use magic numbers. Do not make constants for the sizes of various data structures, you can use INIT_SPACE for this. 

Create files inside the 'handlers' folder for whatever instruction handlers are needed. Put Account Constraints here, but ensure the names end with 'AccountConstraints' rather than just naming them the same thing as the function. Handlers that are only for the admin should be in a new folder called 'admin' inside the 'handlers' folder. 

Create unit tests in TS in the tests directory. Use 'test' rather than 'it'. In the 'before' section in the tests, create a token called 'USDC_LOCALNET' to represent USDC on localnet for running these tests. 

Don't do things like write '...implement the thing...' or '...test code for creating an event...'. Instead, make the actual code. 

Use anchor_lang::solana_program::clock to get the current time where needed.

Return useful error messages. Write code to handle common errors like insufficient funds, bad values for parameters, and other obvious situations.

Remember this is Solana not Ethereum. Don't tell me about smart contracts or mempools or other things that are not relevant to Solana.

Add "pub bump: u8" to every struct stored in PDA. Save the bumps inside each when the struct inside the PDA is created.

Write all code like Anchor 0.30.1. Do not use unnecessary macros that are not needed in Anchor 0.30.1.
