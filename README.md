# ArchGuard RFCs - [Active RFC List](https://github.com/archguard/rfcs/pulls)

Many changes, including bug fixes and documentation improvements can be implemented and reviewed via the normal GitHub pull request workflow.

Some changes though are "substantial", and we ask that these be put through a bit of a design process and produce a consensus among the ArchGuard core team.

The "RFC" (request for comments) process is intended to provide a consistent and controlled path for new features to enter the project.


## When you need to follow this process

You should consider using this process if you intend to make "substantial"
changes to ArchGuard or its documentation. Some examples that would benefit
from an RFC are:

  - A new feature that creates new API surface area, and would
     require a feature flag if introduced.
  - The removal of features that already shipped as part of the release
     channel.
  - The introduction of new idiomatic usage or conventions, even if they
     do not include code changes to ArchGuard itself.

Some changes do not require an RFC:

  - Rephrasing, reorganizing or refactoring
  - Addition or removal of warnings
  - Additions that strictly improve objective, numerical quality
  criteria (speedup, better browser support)
  - Additions only likely to be _noticed by_ other implementors-of-ArchGuard,
  invisible to users-of-ArchGuard.

## Before creating an RFC

A hastily-proposed RFC can hurt its chances of acceptance. Low quality proposals, proposals for previously-rejected features, or those that don't fit into the near-term roadmap, may be quickly rejected, which can be demotivating for the unprepared contributor. Laying some groundwork ahead of the RFC can make the process smoother.

Although there is no single way to prepare for submitting an RFC, it is generally a good idea to pursue feedback from other project developers beforehand, to ascertain that the RFC may be desirable; having a consistent impact on the project requires concerted effort toward consensus-building.

## What to expect



## What the process is

In short, to get a major feature added to ArchGuard, one usually first gets
the RFC merged into the RFC repo as a markdown file. At that point the RFC
is 'active' and may be implemented with the goal of eventual inclusion
into ArchGuard.

* Fork the RFC repo http://github.com/archguard/rfcs
* Copy `0000-template.md` to `text/0000-my-feature.md` (where
'my-feature' is descriptive. Don't assign an RFC number yet).
* Fill in the RFC. Put care into the details: **RFCs that do not
present convincing motivation, demonstrate understanding of the
impact of the design, or are disingenuous about the drawbacks or
alternatives tend to be poorly-received**.
* Submit a pull request. As a pull request the RFC will receive design
feedback from the larger community, and the author should be prepared
to revise it in response.
* Build consensus and integrate feedback. RFCs that have broad support
are much more likely to make progress than those that don't receive any
comments.
* Eventually, the team will decide whether the RFC is a candidate
for inclusion in ArchGuard. Note that a team review may take a long time,
and we suggest that you ask members of the community to review it first.
* RFCs that are candidates for inclusion in ArchGuard will enter a "final comment
period" lasting 3 calendar days. The beginning of this period will be signaled with a
comment and tag on the RFCs pull request.
* An RFC can be modified based upon feedback from the team and community.
Significant modifications may trigger a new final comment period.
* An RFC may be rejected by the team after public discussion has settled
and comments have been made summarizing the rationale for rejection. A member of
the team should then close the RFCs associated pull request.
* An RFC may be accepted at the close of its final comment period. A team
member will merge the RFCs associated pull request, at which point the RFC will
become 'active'.


## The RFC lifecycle

Once an RFC becomes active, then authors may implement it and submit the
feature as a pull request to the ArchGuard repo. Becoming 'active' is not a rubber
stamp, and in particular still does not mean the feature will ultimately
be merged; it does mean that the core team has agreed to it in principle
and are amenable to merging it.

Furthermore, the fact that a given RFC has been accepted and is
'active' implies nothing about what priority is assigned to its
implementation, nor whether anybody is currently working on it.

Modifications to active RFCs can be done in followup PRs. We strive
to write each RFC in a manner that it will reflect the final design of
the feature; but the nature of the process means that we cannot expect
every merged RFC to actually reflect what the end result will be at
the time of the next major release; therefore we try to keep each RFC
document somewhat in sync with the language feature as planned,
tracking such changes via followup pull requests to the document.

## Implementing an RFC

The author of an RFC is not obligated to implement it. Of course, the
RFC author (like any other developer) is welcome to post an
implementation for review after the RFC has been accepted.

If you are interested in working on the implementation for an 'active'
RFC, but cannot determine if someone else is already working on it,
feel free to ask (e.g. by leaving a comment on the associated issue).

## RFC Postponement

Some RFC pull requests are tagged with the "postponed" label when they are closed (as part of the rejection process). An RFC closed with "postponed" is marked as such because we want neither to think about evaluating the proposal nor about implementing the described feature until some time in the future, and we believe that we can afford to wait until then to do so. Historically, "postponed" was used to postpone features until after 1.0. Postponed pull requests may be re-opened when the time is right. We don't have any formal process for that, you should ask members of the relevant sub-team.

Usually an RFC pull request marked as "postponed" has already passed an informal first round of evaluation, namely the round of "do we think we would ever possibly consider making this change, as outlined in the RFC pull request, or some semi-obvious variation of it." (When the answer to the latter question is "no", then the appropriate response is to close the RFC, not postpone it.)

### Help this is all too informal! 

The process is intended to be as lightweight as reasonable for the present circumstances. As usual, we are trying to let the process be driven by consensus and community norms, not impose more structure than necessary.

## Inspiration

ArchGuard's RFC process owes its inspiration to the [Rust RFC process], and [React RFC process].

[React RFC process]: https://github.com/reactjs/rfcs
[Rust RFC process]: https://github.com/rust-lang/rfcs

We've changed it in the past in response to feedback, and we're open to changing it again if needed.
