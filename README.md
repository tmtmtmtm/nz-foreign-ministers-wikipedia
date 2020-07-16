Note: This repo is largely a snapshop record of bring Wikidata
information in line with Wikipedia, rather than code specifically
deisgned to be reused.

The code and queries etc here are unlikely to be updated as my process
evolves. Later repos will likely have progressively different approaches
and more elaborate tooling, as my habit is to try to improve at least
one part of the process each time around.

---------

Step 1: Tracking page
=====================

Starting point: https://www.wikidata.org/w/index.php?title=Talk:Q6866148&oldid=1232376878

(Wikidata only knows of 5 officeholders to start with)

Step 2: Scraper
===============

Comparison/source = https://en.wikipedia.org/wiki/Minister_of_Foreign_Affairs_(New_Zealand)

a couple of minor changes required to the standard scraper, mainly
because it uses 'Term of office' rather than 'Term of Office'. This has
varied between the two from time to time, so I've switched to making
this a case-insensitive check.

Run as:

    bundle exec ruby scraper.rb| tee wikipedia.csv

Step 3: Get local copy of Wikidata information
==============================================

    wd sparql office-holders.js Q6866148 | tee wikidata.json

Step 4: Create missing P39s
===========================

    bundle exec ruby new-P39s.rb wikipedia.csv wikidata.json |
      wd ee --batch --summary "Add missing P39s from https://en.wikipedia.org/wiki/Minister_of_Foreign_Affairs_(New_Zealand)"

These were created in edit group: https://tools.wmflabs.org/editgroups/b/wikibase-cli/5dca68650bbb/

Step 5: Add missing qualifiers
==============================

    bundle exec ruby new-qualifiers.rb wikipedia.csv wikidata.json

This gave four changes that could be applied, all of which looked OK,
so I selected those and then ran:

    pbpaste | wd aq --batch --summary "Add missing qualifiers, from https://en.wikipedia.org/wiki/Minister_of_Foreign_Affairs_(New_Zealand)"

(With a larger set, and the need to pick and choose, I'd send to file
and edit)

Those were added as https://tools.wmflabs.org/editgroups/b/wikibase-cli/c0e4f03f1ea6f

Step 6: Refresh the Tracking Page
=================================

We finish with: https://www.wikidata.org/w/index.php?title=Talk:Q6866148&oldid=1232404831

