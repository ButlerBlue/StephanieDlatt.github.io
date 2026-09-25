# Portfolio candidates

Sanitized highlights swept from weekly digests. Staging only, nothing here is published until promoted into index.html by hand. Raw source lives in the private accomplishments repo.

## Writing: A permissions error is not always a permissions problem

**Suggested section:** Writing

Swept from a vendor troubleshooting call on 2026-08-26, not a weekly digest. Shorter LinkedIn version staged in the private repo at `linkedin-calendar/candidates/check-the-version-first.md`.

A vendor's integration could authenticate to Salesforce but could not run a single query. Account not supported, contact not supported, organization not supported. By the time I joined the call, the thread had already gone several rounds, and the working theory was that the integration user simply needed more access. One option on the table was to skip the restricted user entirely and buy the vendor a full license.

I asked two questions instead: can you share your screen, and which API version are you calling? The answer was 52. The user they had been given was a restricted integration license, a license type newer than that version of the API. Every request was being evaluated against rules written before that kind of user existed, so the errors were describing a permissions problem that was not really there yet.

Once the version moved forward, the errors changed, and that was the useful part. Each one now named a real, specific gap, and we closed them one object at a time, deliberately, until every query ran. One error we did not fix by granting access. It asked for a setup-level permission that a production integration user does not get, and the vendor's queries never needed it. Once the real gaps were closed, it stopped appearing.

A full license would probably have worked on the first try. It would also have hidden a configuration gap that every future integrator on the same restricted license would walk straight into. Fixing the narrow path once meant it holds for all of them. When the error says access, check what is judging the request before you start handing access out.

## Week ending 2026-09-25

**Suggested section:** Case Study

An organization asked me to review a set of chapter accounts it suspected were duplicated, and the fix on the table was to clear an ID field on the extras. I went through all 32 flagged accounts, confirmed 20 genuine duplicate pairs, separated out 2 tangled special cases, and turned up 15 more unrelated duplicates nobody had flagged. The more useful part was explaining why the proposed fix would not have worked: the sync between the two platforms matches records on both an external ID and email, so clearing one key leaves the other still linking the records. We agreed to hold off on changes until the goal of the cleanup is settled, because the right fix depends on what you are actually trying to solve.

## Deep archive sweep, 2026-09-25

Not a weekly digest — pulled from a full quarterly career archive (2021-2026) built the same day, after a GitHub Projects board search turned up a previously undocumented pre-ticketing-system execution log. Full detail, real names, and ticket numbers live in `research/2026-09-25-quarterly-roi-upskilling-archive.md` in the private repo.

**Promoted to index.html:** Leadership, "Defining the categories before there was a system to track any of it" — the origin of a still-used internal taxonomy, roughly twenty pre-2024 vendor integrations that had no searchable record until today, and a trainer/trainee contrast from the same era.

**Held back, not used today:**
- The raw volume figure (roughly twenty integrations across two vendor platforms) could support its own Case Study with more room to breathe, if a future pass wants to build out situation/approach/outcome beats rather than a single Leadership paragraph.
- Everything about the current comp/title conversation and the stalled internal upgrade programme is explicitly excluded — that's live, unresolved, and internal by nature, not portfolio material regardless of how it's worded.

**Suggested section:** Case Study

A member sync duplicate-detection bug was quietly rejecting valid relationship records for a specific membership type, one built around a lifetime status rather than a renewing one. My first hypothesis was that the sync used an insert instead of an upsert, but building out real test data disproved that within an hour, which redirected the investigation before it burned days chasing the wrong fix. The actual cause turned out to be in how the platform's deduplication key gets constructed for that membership type, a defect that had only surfaced for one organization so far but was structurally certain to hit every other organization using that same membership type as the platform scaled. I built the diagnostic queries, documented the mechanism, and staffed the fix to engineering instead of patching around it organization by organization.

**Suggested section:** Leadership

An individual's access issue had been bouncing between people for weeks, with repeated manual account fixes that kept failing to stick. The real cause was that the person had multiple unlinked login accounts in the underlying billing platform, so every fix landed on an account they were not actually using to log in. Once I understood why the patch did not hold, I stopped recommending it, routed the actual fix to the team that owns account linking, and flagged that anyone else in that organization with more than one chapter affiliation was probably hitting the identical problem. A fix that has to be reapplied is not a fix. Catching that early meant the effort went toward the systemic cause instead of patching the same account a third time next month.

## Week ending 2026-09-11

**Suggested section:** Case Study

A sync had been quietly skipping about fifty member records, and it turned out not to be a configuration problem at all. Those records qualified for sync through a background recalculation, and the platform does not stamp that kind of change as a real edit, so an incremental sync keyed on modification time never saw them. I forced the timestamp to bring the backlog through, then wrote up the pattern so the same symptom could be recognized elsewhere instead of re-diagnosed one group at a time. The fix took an afternoon. Naming the class of bug was the part worth keeping.

**Suggested section:** Case Study

Members of one organization were getting locked out of a third party platform, and the vendor's matching logic was the reason: it identified people by email address rather than by the permanent member ID. That holds up until someone has more than one login or changes their email, which in a membership organization is not an edge case. I wrote the explanation in language the client-facing team could send along without translating it first, and split the next steps explicitly between us, the client, and the vendor so nothing stalled in the gap between three parties. The open question I am still holding the vendor to: whether their fix actually switches to the permanent ID, or just refreshes the stored emails and recreates the same problem a year from now.

**Suggested section:** Leadership

A reporting gap kept resurfacing because everyone assumed it was a defect waiting on a fix. It was not. The qualification field the sync depends on is packaged, and it structurally cannot return the records in question. I said so plainly instead of leaving room for a fix that was never coming, and got agreement that the manual process is the long term answer for that organization rather than a stopgap. Calling a permanent limitation permanent is less satisfying than promising a fix, and a great deal more useful to the people planning around it.

**Suggested section:** Leadership

Two access requests landed the same week and both got a no. A vendor asked for admin level access to a production org; they are getting a dedicated named login scoped to what they actually need instead. Separately, an organization wanted member statements routed to a shared officer inbox, which would have quietly broken the link between a person's login and their own record. Neither no was the popular answer in the moment. Both were cheaper than the incident that follows the yes.
