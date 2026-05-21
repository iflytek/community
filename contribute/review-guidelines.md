# Review Guidelines

<!-- toc -->
- [Tips for Code Reviewers](#tips-for-code-reviewers)
  - [Managing Time](#managing-time)
    - [Block Time Off to Review](#block-time-off-to-review)
    - [Taking a Break or Stepping Away](#taking-a-break-or-stepping-away)
  - [Loop in Others When Domain-Specific Knowledge Is Needed](#loop-in-others-when-domain-specific-knowledge-is-needed)
  - [Asking Questions](#asking-questions)
  - [Asking for Changes](#asking-for-changes)
    - [Commit Hygiene](#commit-hygiene)
  - [Be Clear on Progress and Time](#be-clear-on-progress-and-time)
  - [Checking Out a Pull Request](#checking-out-a-pull-request)
<!-- /toc -->

## Tips for Code Reviewers

If you're looking for tips on preparing your pull request for review, check out the [Pull Request Guidelines](./pull-request-guidelines.md). This page is for reviewers.

### Managing Time

#### Block Time Off to Review

Often it can be hard to find dedicated, uninterrupted time to review pull requests. If you can, allocate some time and block it off on your calendar to help stay on top of the incoming queue.

#### Taking a Break or Stepping Away

If you are taking a break, going on vacation, or stepping away for a bit, set your GitHub status to busy. This signals to others that you are not currently available for reviews.

If you need to step away for an extended period, or if you need to focus on other areas, consider marking yourself as `[emeritus_approver]` in some of the areas where you are a reviewer or approver.

### Loop in Others When Domain-Specific Knowledge Is Needed

The project has a large and complex code base with sections that may require domain-specific knowledge. If you are unsure or uncomfortable reviewing a portion of a pull request, it is better to decline a review and reassign to an owner or contributor with more expertise in that area.

If you are brought in for your knowledge in a specific area, try and provide meaningful comments to serve as breadcrumbs in the future to help others gain a better understanding of that area.

### Asking Questions

You are encouraged to ask questions and seek an understanding of what the pull request is doing; however, your question might be answered further into the review. You can stage your questions, and before you submit your review, revisit your own comments to see if they're still relevant or update them after gaining further context.

Often a question may turn into a request for further comments or changes to explain what is happening at that specific point.

In your questions, try and be empathetic when phrasing. Instead of:

*"Why did you do this?"*

try

*"Am I understanding this correctly? Can you explain why...?"*

Remember a review is a discussion, often with multiple parties — be reasonable. Try to focus and summarize in ways which constructively move the conversation forward instead of retreading ground.

### Asking for Changes

It's okay to ask for changes to be made on a pull request. In your comments, you should be clear on what is a 'nit' or small thing to be improved and what is a **required** change needed to accept the pull request.

Be clear and state upfront architectural or larger changes. These should be resolved first before addressing any further nits.

It's also okay to say *"No"* to a pull request. As a community, we want to respect people's time and efforts, but sometimes things just don't make sense to accept. As reviewers, you are the stewards of the codebase and sometimes that means pushing back on potential changes.

#### Commit Hygiene

It can be seen as trivial, but you can ask the author to break apart their pull request into smaller chunks, or change a commit message to be more informative. They are the *"permanent record"* of the change and should accurately describe both what and why it is being done.

### Be Clear on Progress and Time

Be upfront with the author about where the state of their pull request is and what needs to be completed for it to be accepted.

No one likes it if their pull request misses a release, but it is a fact of life. Try and be upfront about it. Don't push a pull request through out of guilt or deadlines. Remember, you are a steward of the codebase.

### Checking Out a Pull Request

If a pull request is too complex to review through the GitHub UI, you can pull it down locally to evaluate. You can do so using the following command:

```
git fetch origin pull/<PR ID>/head:<BRANCHNAME>
git checkout <BRANCHNAME>
```

**Example:**

```
git fetch upstream pull/1245/head:foo
git checkout foo
```
