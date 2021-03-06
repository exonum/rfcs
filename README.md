# Exonum RFCs
[Exonum RFCs]: #exonum-rfcs

Exonum RFC process is partially derived from [the Rust RFC process](https://github.com/rust-lang/rfcs/blob/master/README.md)

Many changes, including bug fixes and documentation improvements can be
implemented and reviewed via the normal GitHub pull request workflow.

Some changes though are "substantial", and we ask that these be put through a
bit of a design process and produce a consensus among the Exonum community.

The "RFC" (Request for Comments) process is intended to provide a consistent
and controlled path for new features to enter Exonum, so that all
stakeholders can be confident about the direction the Exonum project is evolving
in.

## Table of Contents
[Table of Contents]: #table-of-contents

  - [Opening](#exonum-rfcs)
  - [Table of Contents]
  - [When you need to follow this process]
  - [Before creating an RFC]
  - [What the process is]
  - [The RFC life-cycle]
  - [Reviewing RFCs]
  - [Implementing an RFC]
  - [RFC Postponement]
  - [Help! This is all too informal!]
  - [License]

## When you need to follow this process
[When you need to follow this process]: #when-you-need-to-follow-this-process

You need to follow this process if you intend to make "substantial" changes to
Exonum core or the RFC process itself. What constitutes a
"substantial" change is evolving based on community norms and varies depending
on what part of the ecosystem you are proposing to change, but may include the
following.

  - Changes in the public interface of the `exonum` crate.
  - Architecture changes.
  - Adding new features.
  - Removing existing features.
  - Changing features behavior.

Some changes do not require an RFC:

  - Rephrasing, reorganizing, refactoring, or in other words "changing shape does
    not change meaning".
  - Additions that strictly improve objective, numerical quality criteria
    (warning removal, speedup, enhanced platform coverage, improved parallelism
    and errors trapping, etc.)
  - Additions that are only likely to be _noticed by_ other developers-of-exonum,
    invisible to users-of-exonum.

If you submit a pull request to implement a new feature without going through
the RFC process, it may be closed with a polite request to submit an RFC first.

## Before creating an RFC
[Before creating an RFC]: #before-creating-an-rfc

A hastily-proposed RFC can significantly reduce chances of its acceptance. Low
quality proposals, proposals for previously-rejected features, or those that
don't fit into the near-term roadmap, may be quickly rejected, which can be
demotivating for the unprepared contributor. Laying some groundwork ahead of the
RFC can make the process smoother.

Although there is no single way to prepare for submitting an RFC, it is
generally a good idea to pursue feedback from other project developers
beforehand to ascertain that the RFC may be desirable; having a consistent
impact on the project requires concerted effort toward consensus-building.

The most common preparations for writing and submitting an RFC include talking
the idea over on [Gitter], filing and discussing ideas on the
[RFC issue tracker].

As a rule of thumb, receiving encouraging feedback from long-standing project
developers is a good indication that the RFC is worth pursuing.

## What the process is
[What the process is]: #what-the-process-is

In short, to get a major feature added to Exonum, one must first get the RFC
merged into the RFC repository as a markdown file. At that point the RFC is
"active" and may be implemented to be included into Exonum.

  - Fork the RFC repo [RFC repository]
  - Copy `0000-template.md` to `text/0000-my-feature.md` (where "my-feature" is
    descriptive. Don't assign RFC number just yet).
  - Fill in the RFC. Put care into the details: RFCs that do not present
    convincing motivation, demonstrate understanding of the impact of the
    design, or are disingenuous about the drawbacks or alternatives tend to be
    poorly-received.
  - Submit a pull request. As a pull request the RFC will receive design
    feedback from a larger community, and the author should be prepared to
    revise it in response.
  - Build consensus and integrate feedback. RFCs that have a broad support are
    much more likely to make progress than those that don't receive any
    comments. Feel free to reach out to the RFC assignee in particular to get
    help identifying stakeholders and obstacles.
  - RFCs rarely go through this process unchanged, especially as alternatives
    and drawbacks are shown. You can make edits, big and small, to the RFC to
    clarify or change the design, but make changes as new commits to the pull
    request, and leave a comment on the pull request explaining your changes.
    Specifically, do not squash or rebase commits after they are visible on the
    pull request.
    - At some point, maintainer of the RFC repository will propose a "motion for
      final comment period" (FCP), along with a *disposition* for the RFC (merge,
      close, or postpone).
      - This step is taken when enough tradeoffs have been discussed and
        the maintainers of the related repositories are in position to make the
        decision. That does not require consensus amongst all participants in the
        RFC thread (which is usually impossible). However, the argument supporting
        the disposition on the RFC needs to have already been clearly articulated,
        and there should not be a strong consensus *against* that position.
        The maintainers of the related repositories use their best judgment in
        taking this step, and the FCP itself ensures there is ample time and
        notification for stakeholders to push back if it is made prematurely.
      - For RFCs with lengthy discussion, the motion to FCP is usually preceded by
        a *summary comment* trying to lay out the current state of the discussion
        and major tradeoffs/points of disagreement.
      - Before actually entering FCP, *all* the maintainers of the related
        repositories must sign off.
    - The FCP lasts ten calendar days, so that it is open for at least 5 business
      days. This way all
      stakeholders have a chance to lodge any final objections before the decision
      is reached.
    - In most cases, the FCP period is quiet, and the RFC is either merged or
      closed. However, sometimes substantially new arguments or ideas are raised,
      the FCP is canceled, and the RFC goes back into development mode.
  - Whoever merges the RFC should do the following:
    - Assign an id, using the PR number of the RFC pull request. (If the RFC
      has multiple pull requests associated with it, choose one PR number,
      preferably the minimal one.)
    - Add the file into the `text/` directory.
    - Create a corresponding issue in the Exonum repository most affected by the
      RFC.
    - Fill in the remaining metadata in the RFC header, including links to
      the original pull request(s) and the newly created issue.
    - Merge everything to the RFC repository.

## The RFC life-cycle
[The RFC life-cycle]: #the-rfc-life-cycle

Once the RFC becomes "active" the author may implement it and submit the
feature as a pull request to the Exonum repo. Being "active" is not a rubber
stamp, and in particular still does not mean the feature will ultimately be
merged; it does mean that in principle all the major stakeholders have agreed
to the feature and are amenable to merging it.

Furthermore, the fact that a given RFC has been accepted and is "active"
implies nothing about what priority is assigned to its implementation, nor does
it imply anything about whether an Exonum developer has been assigned the task of
implementing the feature. While it is not *necessary* that the author of the
RFC also writes the implementation, it is by far the most effective way to see
the RFC through to completion: authors should not expect that other project
developers will take on responsibility for implementing their accepted feature.

Modifications to "active" RFCs can be done in follow-up pull requests. We
strive to write each RFC in a manner that it will reflect the final design of
the feature; but the nature of the process means that we cannot expect every
merged RFC to actually reflect what the end result will be at the time of the
next major release.

In general, once accepted, RFCs should not be substantially changed. Only very
minor changes should be submitted as amendments. More substantial changes
should be presented in the form of new RFCs, with a note added to the original
RFC.

## Reviewing RFCs
[Reviewing RFCs]: #reviewing-rfcs

While the RFC pull request is up, the shepherd may schedule meetings with the
author and/or relevant stakeholders to discuss the issues in greater detail. In
either case a summary from the meeting will be posted back to the RFC pull
request.

The maintainers of related repositories make final decisions about RFCs after the
benefits and drawbacks are well understood. These decisions can be made at any
time, but the maintainers will regularly issue decisions. When the decision is
made, the RFC pull request will either be merged or closed. In either case, if
the reasoning is not clear from the discussion in the thread, the maintainer of
the RFC repository will add a comment describing the rationale for the decision.

## Implementing an RFC
[Implementing an RFC]: #implementing-an-rfc

Some accepted RFCs represent vital features that need to be implemented right
away. Other accepted RFCs can represent features that can wait until some
arbitrary developer feels like doing the work. Every accepted RFC has an
associated issue tracking its implementation in the Exonum repository; the
associated issue can be assigned a priority via the triage process that the
team uses for all issues in the Exonum repository.

The author of the RFC is not obligated to implement it. Of course, the RFC
author (like any other developer) is welcome to post an implementation for
review after the RFC has been accepted.

If you are interested in working on the implementation for an "active" RFC, but
cannot determine if someone else is already working on it, feel free to ask
(e.g. by leaving a comment on the associated issue).

## RFC Postponement
[RFC Postponement]: #rfc-postponement

Some RFC pull requests are tagged with the "postponed" label when they are
closed (as part of the rejection process). An RFC closed with "postponed" is
marked as such because we want neither to think about evaluating the proposal
nor about implementing the described feature until some time in the future, and
we believe that we can afford to wait until then to do so. Historically,
"postponed" was used to postpone features until after 1.0. Postponed pull
requests may be re-opened when the time is right.

Usually an RFC pull request marked as "postponed" has already passed an
informal first round of evaluation, namely the round of "do we think we would
ever possibly consider making this change, as outlined in the RFC pull request,
or some semi-obvious variation of it." (When the answer to the latter question
is "no", then the appropriate response is to close the RFC, not postpone it.)

## Help! This is all too informal!
[Help! This is all too informal!]: #help-this-is-all-too-informal

The process is intended to be as lightweight as reasonable for the present
circumstances. As usual, we are trying to let the process be driven by
consensus and community norms, not impose more structure than necessary.

## License
[License]: #license

Licensed under either of

* Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
* MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

### Contributions

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.

[RFC issue tracker]: https://github.com/exonum/rfcs/issues
[RFC repository]: https://github.com/exonum/rfcs
[Gitter]: https://gitter.im/exonum
