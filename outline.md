---
title: Contributing to R
author: Gabriel Becker and Martin Maechler
output: 
  beamer_presentation:
    latex_engine: xelatex
    slide_level: 3
---
# Introduction
## Who We Are

### Martin Maechler - R-core (and R-Foundation)
- Creator and maintainer of Matrix 
- ETH Zurich
- ESS Project Lead

### Gabriel Becker - Frequent Collaborator with R-core
- Arguably most feature additions to R from external collaborator "in recent times"
  - Proposed ALTREP, collaborated with Luke Tierney on design and implementation
  - `debug(., signature = )` and `debugcall(print(.))` with Michael Lawrence
  - `head(mymatrix, c(5, 5))` with Martin Maechler
  - `update_PACKAGES()` with Michael Lawrence
  - `sessionInfo()` returns RNG information when not the default, with Martin Maechler
  - `which()`  long vector and ALTREP support, with Martin Maechler
  - `ifelse()` refactor for significant speedup with Kurt Hornik
  - `duplicated()`/`unique()`/`anyDuplicated()` sortedness-aware ALTREP fastpass (r-devel only) with Michael Lawrence

## Learning How to Contribute to R
### The Goal
- **Is** 
  - Helping R
  - By helping R-core maintain it
  - Thus benefiting ourselves and the larger R community
- **Is Not**
  - Professional advancement
  - Personal recognition/fame (though acknowledgement will occur)

### We will focus on
- What kinds of efforts help
  - and how to perform them
- What kinds of well-meaning efforts do not help
  - And how to avoid or improve them
  

# Gabe As A Case Study - Illustrative Examples From a Patchy Past
## Circa 2013 - The CV Padder (AKA Graduate Student)
### PR#15253 - `identify()` Hanging
- Simon Anders posted a reproducible example
- I diagnosed the problem:
  - Hang was in `locator` C function not identify logic per se. 
- I wrote and submitted a proposed patch
  - No reponse on bugzilla other than bug status changed to closed-fixed
  - Could see changes in trunk; patch accepted

### Bitwise Operations For Raw Vectors
- Reported by Mark Bravington
- I proposed a patch modifying the C code underlying `bitw*()` R functions
- Bitwise logical operators implemented in raw methods to standard logical operators
- Patch not accepted
- Bug closed as fixed after documentation change by R-core member

### Take Away Points
- Working patches will not always be accepted, even if nothing is wrong
  - These take the same amount of work from the submitter
- Sometimes a patch changing behavior is not the correct fix
  - Even to problems that are real

## Circa 2015 - Icaris Flies With Such Very Sturdy Wings
### PR#16385 - All logical operaters (silently) behave as `!` when called with 1 argument
- Reported By Barry Rowlingson
-
```
`&`(TRUE)
FALSE
```
- Huh, that's weird...

### A Patch - My Benevolent Gift to R-core and R
...

### And Then
**Gabe**
Created attachment 1828 [details]
Fix patch

I called checkArity with the signature of Rf_checkArity (including the call) in the previous patch, which was incorrect. This patch is tested and works.

Arity check needs to happen before the special casing for unary ops (assumed to be `!` ) on simple logical scalars.

### 😬😬😬 (3 grimace face emojis)
**Martin**
<snip>
There's more:  Gabriel said "tested"... well, not really: You did not run R's own tests successfully:  These are listed as S3 generic primitives and so
 &.foo <- function(x, ...) { ........ }
has worked and this is detected by our checks.
Consequently, the checkArity() test should *not* happen before dispatch...

I'm currently testing things

### So ... Yeah. Not Great.
Take away points:
- Test your patches
- Test your patches
- Test your patches

## Circa 2017 - Feature Engineering Over and Over

### My First Added Feature (That ~0 People Know About Or Use)
- `debug(., signature = )` and `eval(debugcall(print(myclassedobj)))`
- S3 and S4 dispatch aware debugging.
  - Masks the weird .local thing function shenanigans you got in S4 method debugging at the time.

### Some things to keep in mind about this success story
- I worked (as my job) with Michael Lawrence at Genentech during this time
  - He had just been elected to R-core
- 20+ comment conversation on bugzilla about it
  - 4 distinct versions of the patch
- I disagreed with Michael and Martin about design/API for `debugcall`
  - My patches after the first iteration implemented their preferred API
- It was accepted, but not until after feature freeze for the upcoming version of R
  - This meant it remained exclusive to R devel for ~a year until the next primary release
- Patch was still further refactored by Michael in the process of commiting it into the R sources

## Circa 2016 - 2019  - The ALTREP Years
### The Proposal
- Proposed ALTREP at DSC 2016 (research conference/R-core meeting)
  - Was in contact with Luke before meeting, had informal interest
  - Accepted as a way forward following meeting
  - Was proposing enormous internal change - "Backwards Compatible" were first two words in talk title.
  
### Implementation 2017-2018

- My proof of concept was a type of vector that was a "window" to another vector's data
  - "Worked" but didn't respect R's copy on write semantics
- Luke implemented the ALTREP framework
  - I contributed code, soem is still in, some was buggy so not accepted/taken back out.
- Luke asked that Michael review my code internally (we were still both at Genentech) before submitting
  - This was hard to hear but is understandable.

### Post Inclusion 2019
- I've submitted several accepted patches which implement or further ALTREP support in the R internals
  - Some directly to Luke, most via Bugzilla

### Take Away

- Submitting code to R-core members should be very mature
  - This is difficult, comes with pratice somewhat
	- Code review may be painful but is a great tool to get better
  - R-core collectively have little time spread across
	- their own work
	- Teaching or day-job duties
	- all collaborations/patches they are currently involved with
  
## Take Away Points
- Members of R-core are open to significant collaboration, but
  - Their time is scarce and extremely precious
  - Significant chunks of code take serious work from them to vet

## Circa 2020 - Heady Days
### Making `head()`s or `tail()`s of Large Rectangular Things
- Requested as change by Michael Chirico on R-devel
- Backwards Compatible form proposed by me on R-devel

### The Patch
- I propose patch
  - Passed all R's tests (I had learned)
  - "quite a bit of breakage on CRAN" - Martin Maechler

### The Patch Cont
- Martin requested I not submit more code, as he was iterating on a local copy to track down and fix CRAN breakages

### Suharto Anggono 
- Suggested numerous changes to the patch
  - This was frustrating, but
  - He correctly identified multiple flaws in initial versions of the patch
  - Martin agreed with him on some of the coding style/implementation changes, not on others

### Take Away Points
- Patches are not your baby
  - Critiques can feel like attacks but better patches make R better.
  - Learn from this type of feedback
- It is often easier for R-core members to make changes themselves than accept/iterate on patches


## Circa 2020-2021 - Duplicated Efforts
### (Significant) speedup to duplicated/unique/etc for sorted ALTREPs
- Large chunk of code
- I Partially-fully implemented it multiple times
  - Bug fixes
  - Multiple (refactors)/code cleanup passes
  - Extremely exhaustive testing script
- All *before* even initial submission on bugzilla

### Luke was focused on the Native Pipe
- He didn't have time to closely vet hundreds of lines of unsolicited C code
- Patch sat in bugzilla and I waited
  - Not personal, Luke just didn't have bandwidth

### Enter Michael Lawrence
- Contacted me and collaborated on getting the patch in
  - Asked me to formalize my test script into unit tests (oops, duh!)
  - Got it in, we hoped in time for release, but it turned out not
	- Will be in next release assuming no problems identified

### Take Away Points
- Sometimes a suggestion/patch not being taken up has nothing to do with the suggestion
  - Especially when it was unsolicited
- Never submit a significant code change without regression tests
- Be patient - Occasional reminders are ok, Nagging (at the very best) wont help



## Overall Take Aways
### No One is Perfect ... **But**
- As external collaborators with R-core members
  - When making work for them its our job to make as little as possible
  - Be respectful, not demanding. 
  - Sometimes the answer is no, even if you still think the idea is good
	- Even if the idea **is** good (this is different than the point above, and its important you understand why)
	- If you cant accept that this isn't the right game for you to play
  - Never "shoot from the hip", each change is a change 

### Test, test, test
- Always test your patches via the full battery of R's `make check-devel` *before submission*
- Submit some form of testing/confirmation
  - Test script that R-core can consider and add as formal tests
	- Note if you don't have this, you're not ready to submit
  - Formal tests, sometimes, but these take a lot of care in designing


# Contributing to R
## No patches yet
- Discuss Tomas and Luke's blogposts

### Confirming bugs

### Generating reproducible examples which **only** use base packages

### Careful bug analysis

**Code Analysis Practicum 1?**

## Submitting Bugs
### Confirm it's present in up-to-date R-devel

### Confirm it's a bug in R itself
Run it
- in plain R (no RStudio or other IDE)
- with no non-base/non-recommended packages loaded

### Within R isolate it as much as you can
- To an R function which hits C code, or to the actual offending pure-R function
- Bonus points if you can (correctly) narrow it down further to which C funciton and why its choking

### Search bugzilla for if it has already been reported

### (obtain bugzilla account and) Submit bug

# Possibly Patches (but Still Probably Not)

## R-core's Engineering Philosophy (as my interactions with them have lead me to understand it)
- Backwards compatability is **very** important and thus given very high weight
- Anything that can be implemented in an addon package generally should be
  - At **least** as a testing/maturation stage
  - in many cases, indefinitely/"permanently"
  - "lots of people use/want/would want this" is in general not a counterargument to this
- There probably won't ever be any new Recommended packages which ship with R
  - packages being extremely widely used and people really liking them do not change this
- Helping them squash bugs saves them work, proposing new features creates work for them
  - Even if you submit a patch
	- Even if the patch you submit is perfect
- Most things within/by R-core happen on an "Individual Iniative + Lack of Opposition" model
  - Convincing/working with the one relevant R-core member tends to be enough (for most things)

## So (for your own sake and R-core's) Do Not
- Start with feature additions
- Submit patches which change existing (non-bug) behavior without hearing from R-core they are interested
  - Not agreeing with the documented existing behavior does not make something a bug
- Expect quick turn around on wishlist items you file without a proposed patch
- Try to get another R-core member to overrule one whose decision you don't like
  - I don't know of any times where this happened. Lets keep it that way
	- I seriously doubt they would be amused (or that it would work anyway)
  - This is different than engaging in ongoing discussions on, e.g., r-devel

# Documentation Patches
## Typo reporting/fixes always welcome
- Usually a patch not required, just pointing it out on the r-devel mailing list
  - generally receives prompt and appreciative response

## Larger changes to documentation
- Only when its really necessary or R-core has solicited/stated interest in a patch
- **Must be at least as technically correct as the old documentation**
  - Do not trade clarity/approachability for correctness
  - This means you must deeply, fully understand what the relevant function/system is doing

# Code Patches
## Do Not Submit
- patches which contains a purely-whitespace change
  - **check this** before you submit
- patches whose diff file you have not actually looked at
- patches that do not update documentation - as necessary - to reflect your changes
- patches which do not include unit/regression tests as appropriate
- **patches you have not personally tested the exact diff file you are submitting against R-devel/trunk**
  - make check-devel in the R build directory
- **patches which bundle enhancements (even related ones) with bugfixes**
- **patches which bundle multiple bugfixes which are not inextricably linked**

## Do

# Feature Additions
## Filing Wishlist items in bugzilla
- Low cost
- Appreciated
- Absolutely no guarantee of it ever being done

## Unsolicited feature addition patches
- Generally ... don't
- Bring up on R-devel mailinglist or bugzilla wishlist item first

## Solicited/interest confirmed behavior additions
- Good to collaborate, voice your opinion
  - R-core member has final say on all matters
	- this is because they own the code, and any problems with it, once they commit it
- Be prepared to refactor code before submitting it to your collaborator
  - This is generally a good sign, your first pass is rarely your best
- Test it tow ithin an inch of its life, then keep going
  - Think of any possible corner case you can, and test every single one of them
- If at all possible, have someone else technically skilled in R review it

# Add part about why not speedups




# Finding your way around a checkout of the R sources
## Finding core C-code

## Finding tests

## Finding R code/documentation/etc of base packages

**R checkout and Build Practicum? Is that even in scope?**
 Uwe Ligges article about the tarball. R journal
