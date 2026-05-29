==============================================================================
 MODEL COMPARISON  -  model-comparison.html
==============================================================================

WHAT THIS IS
------------
A single, self-contained web page for comparing large-language-model pricing
and specs across many providers (OpenAI, Anthropic, Google, Mistral, xAI,
DeepSeek, Moonshot, Meta, and more). Everything it needs - the page, styling,
code, and the full model dataset - is packed into this one HTML file. There is
nothing to install and no internet connection required.


HOW TO OPEN IT
--------------
Just double-click "model-comparison.html". It opens in your default web
browser (Chrome, Safari, Edge, Firefox - any modern browser works).

It also works on a phone or tablet: email or AirDrop the file to the device
and open it. No app, no setup.

Because everything is built in, it runs completely offline. Nothing you type
or import is sent anywhere - it all stays in your browser.


WHAT YOU CAN DO
---------------
1. BROWSE / SEARCH MODELS
   - Use the search box to find a model by name, e.g. "gpt-5", "sonnet",
     "kimi", "gemini". You can type multiple words ("kimi 2.5").
   - Filter to a single provider with the "All providers" dropdown.
   - Sort the cards by provider, input price, output price, or context window.
   - "Chat models only" is on by default (hides embedding/rerank models).

2. COMPARE SIDE-BY-SIDE (PINNING)
   - Click "Pin" on any model card to keep it on screen.
   - Pinned models stay visible no matter how you search or filter, so you can
     line several up next to each other.
   - Your pinned models are remembered the next time you open the file (saved
     in your browser).

3. REAL-RUN COST CALCULATOR
   - The table near the top prices a real usage run across your pinned models.
   - Enter the token counts for each step of a run:
       * Uncached input  - fresh input tokens
       * Cached input    - tokens served from the prompt cache (cheaper)
       * Output          - tokens the model generated
   - Each row also has a name and a short description so you can document what
     that step did.
   - The cost table below updates instantly and can be sorted by any column
     (Input $, Cached $, Output $, or Total $) - click a column heading.
   - With nothing pinned, a default set of flagship models is priced so the
     table is never empty.
   - "Download sample CSV" gives you a template with example data.
   - "Import CSV" loads a run from your own CSV file (see format below).

4. LIGHT / DARK MODE
   - The button in the top-right corner toggles light and dark themes. Your
     choice is remembered.


CSV FORMAT (FOR IMPORT)
-----------------------
The importer is flexible about column names. A simple file looks like this:

   step,description,uncached_input,cached_input,output
   1. Plan,Read the code and outline the work,670726,750720,19845
   2. Implement,Write the change,130441,163072,1627

Accepted column names (case / spaces / underscores don't matter):
   - name        : step, label, turn, name
   - description : description, note, desc, details
   - uncached    : uncached_input, uncached, input, input_tokens
   - cached      : cached_input, cached, cache
   - output      : output, output_tokens, completion

Numbers can include commas (e.g. 670,726). At least one token column is
required. The easiest way to start is to click "Download sample CSV", edit it,
then "Import CSV".


ABOUT THE DATA
--------------
Pricing and context-window data is compiled from the LiteLLM model database
plus a pricing snapshot, with a few manual corrections. Prices are shown per
1 million tokens. Models hosted by multiple clouds (e.g. the same model on
AWS, Azure, and the native provider) are collapsed to one entry.

This file is a SNAPSHOT. Prices change - always confirm against the provider's
official pricing page before relying on a number for billing decisions.

A small star (the symbol after a provider name on some cards) marks entries
whose pricing came from the newer pricing snapshot.


==============================================================================
 FOR MAINTAINERS (how this file is produced)
==============================================================================

This single file is generated from a small multi-file project:

   index.html      - page structure
   styles.css      - styling (light + dark themes)
   app.js          - all interactivity (search, pinning, calculator, CSV, theme)
   models.js       - the model dataset (auto-generated)
   doozer-logo.png - the logo
   sample-run.csv  - the downloadable CSV template
   readme.txt      - this file (copied into dist/ on each build)

   generate_data.py - rebuilds models.js from the LiteLLM database +
                       aipricing.json snapshot + overlay.json (manual fixes)
   bundle.py        - inlines all of the above into this one HTML file

To refresh the data and rebuild this file:

   python3 generate_data.py    # rebuild models.js
   python3 bundle.py           # rebuild dist/model-comparison.html

bundle.py inlines the CSS and JavaScript, embeds the logo and sample CSV as
data URIs, and writes the result to dist/model-comparison.html.
==============================================================================
