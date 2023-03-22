# OpenReview ECCV 2020 Summary Report

**OpenReview ECCV 2020 Summary Report**

[Andrew McCallum](https://people.cs.umass.edu/\~mccallum/) (Professor, UMass Amherst; Director OpenReview project)

[Melisa Bok](https://www.cics.umass.edu/people/bok-melisa) (Lead Developer, OpenReview project)

[Thomas Brox](https://lmb.informatik.uni-freiburg.de/people/brox/) (Professor, U. Freiburg; ECCV 2020 Program Co-chair)

[Rene Vidal](http://cis.jhu.edu/\~rvidal/) (Professor, JHU; Computer Vision Foundation Board member)

In 2020 the organizers of [ECCV](https://eccv2020.eu) (one of the flagship conferences in computer vision) decided to move from CMT to OpenReview. This report provides a summary of the ECCV 2020 workflow, the OpenReview services provided, the system performance, and enhancements planned for the next ECCV.

(The Computer Vision Foundation, CVF, has the long-term goal of unifying the CVPR conference workflow tools under one integrated infrastructure. Seeing the success of OpenReview for ICLR over the past seven years, CVF has been providing the OpenReview Foundation with a multi-year financial gift towards this new software development. CVPR is hoping to move to OpenReview in the future.)

ECCV 2020 workflow was not fundamentally different from its previous years: double blind, closed reviewing, with area chairs, closed reviewer discussion, author reponses, and meta-reviews.

Workflow details and timing were planned extensively with shared Google Docs, and three video conference meetings with the OpenReview team. Through the submission and reviewing process OpenReview technical staff provided 24/7 support to the ECCV program chairs, including rapid responses and custom work over weekends and evenings.

Below is a summary of key workflow steps and services. (Detailed workflow is described [here](https://docs.google.com/document/d/1J\_QyRNQpJHLNkethTYtGXLhHgf3ZtTGenOuUQYEdSpQ/edit#heading=h.czbul1eao42z).)

* **Reviewer recruiting**. Based on a list provided by ECCV PCs, OpenReview invited over 5k reviewers, and automatically gathered their responses. We also coordinated with ICLR to invite additional reviewers from ICLR’s 2020 reviewer pool.
* **Reviewer & author registration**. OpenReview already had profiles for approximately 100k researchers. For ECCV we added an additional \~3k reviewer profiles, and incorporated their papers from DBLP, running our own version of author coreference, augmented by verification performed by OpenReview staff. ECCV required all authors to register with OpenReview (mostly for the purposes of conflict-of-interest resolution, and gathering multiple email addresses per person); this resulted in \~12k additional profiles being created.
* **Conflicts-of-interest gathering**. Author and reviewer profiles include not only current institution domain names, but DBLP url, Google Scholar url, past advisors, and other non-institutional conflicts. OpenReview could in the future also create conflicts based on paper co-authorship within the last _N_ years; in future ECCV may use this feature also.
* **Reviewer expertise modeling**. Expertise models were built for all reviewers, using modern deep learning embedding methods run on titles and abstracts of reviewers’ papers. In future, OpenReview expertise modeling will also use paper full-text and citation graphs. ECCV 2020 decided to use a combination of both OpenReview reviewer-paper affinities as well as those from TPMS.
* **Paper submissions**. As requested by ECCV 2020 PCs, draft paper titles and abstracts were submitted one week before the full-paper deadline. OpenReview received 7646 paper submissions. In the 24 hours before the final deadline, OpenReview received over 24k submission updates, and had over 19k active users (over 3.7k active simultaneous users during the last hour of submissions). The OpenReview multi-server system never surpassed 50% CPU usage, and maintained smooth operation with rapid system response throughout. (In contrast, the ECCV static web server simply providing submission deadline information became unresponsive.) In addition, during the submission period over 55k email messages were sent to authors (sent to each author for each update).
* **Paper double-submission check**. ECCV used the OpenReview service that checks for double submissions against ICML 2020 and NeurIPS 2020.
* **Bidding**. Both ACs and reviewers bid on papers, assigned as a “task” that was not complete until a given number of bids had been entered. During reviewer bidding, ACs and reviewers were able to search submissions by keyword.
* **Paper-reviewer assignment**. Paper-reviewer affinities included: the OpenReview reviewer expertise model, TMPS affinity scores, area chair reviewer suggestions, reviewer bids, conflicts of interest. During area chair reviewer suggestions, candidate reviewers could be shown ordered by various criteria, including OpenReview affinity, TPMS affinity, and reviewer bids, (and custom reviewer loads). Optimization of paper-reviewer matching was performed by both [Min-Cost-Flow](https://developers.google.com/optimization/flow/mincostflow) and [FairFlow](https://arxiv.org/abs/1905.11924v1) \[Kobren, et al, 2019]. The optimizer’s meta-parameters can be easily tuned, and the ECCV 2020 program chairs ran the optimizer many times (with \~30 minute turn-around time). Each resulting match comes with various requested summary statistics about the quality of the match. The results of a paper-reviewer match could be browsed by PCs and ACs using OpenReview’s “Edge Browser,” which provides a MacOS-Finder-“column-view”-like nested browsing, as well as extensive searching, and the ability to make suggested edits to the assignment, while seeing reviewer loads, and meta-data for reviewers, including their institution, job title, and link to profile. (The same paper matching system was used to do secondary area chair assignment, and emergency reviewer assignment during the reviewing stage.)
* **Specialized consoles**: OpenReview provided specialized consoles for reviewers, area chairs, and program chairs, including functionality such as task lists, reviewing status, search, reviewer re-assignment, aggregate statistics, status of bids for each revidewer, status of review completion, sending email to remind reviewers, the ability to dump data as downloadable CSV files.
* **Reviewing and discussion**. Reviews were entered directly into the OpenReview system, visible immediately to the ACs, then visible to authors and reviewers of the same paper after the reviewing deadline. As a specially-ECCV-requested enhancement, OpenReview implemented [MarkDown](https://www.markdownguide.org) in time for the entry of author responses. (LaTeX formula rendering has already been available since Spring 2019.) OpenReview processed 15,152 reviews, 4,117 meta reviews and 2,752 secondary meta reviews. In addition, 9,506 confidential comments and 10,874 rebuttal comments were entered.
* **Review rating**. ECCV PCs requested that area chairs be able to rate the quality of each review on the scale -1, 0, 1, 2. From this reviewers were assigned an aggregate rating, what also included information about their tardiness. These aggregated reviewer ratings are stored (privately) in OpenReview, so that they will be easily (and programmatically) available to future ECCV program chairs. (We are also hoping to encourage private sharing of these ratings across conferences.)
* **Paper ranking**. ECCV program chairs requested that ACs be able to enter a ranking of their assigned papers.
* **Decisions**. PCs downloaded various CSV files into Google Sheets, including AC decisions. Some decisions were modified by the PCs. Then OpenReview emailed and posted the decision based on this Google Sheet. (In future, OpenReview may provide browsing, sorting, and editing directly through its UI; avoiding the need for Google Sheets. Alternatively, we may more closely embrace Google Sheets––leveraging its features––with live bi-directional data updates between OpenReview and the Google Sheet.)
* **Camera-ready revisions**. OpenReview created additional upload invitations and tasks for accepted paper authors, including copyright form, supplementary materials (including videos), camera-ready LaTeX zip file.
* **Conference track formation**. OpenReview also provided affinity scores between accepted papers, as input to paper clustering, for conference track assignments.

**Feedback from Thomas Brox, ECCV 2020 Program Co-chair**: Very happy with how OpenReview worked, and would recommend it to future program chairs. Particularly liked: (a) very stable and reliable system, (b) great response time and availability of the team, (c) excellent custom service (even implementing custom features we needed), (d) expressive conflict management (this was a primary impetus for moving to OpenReview, (e) reviewer assignment tools. Improvements that would be helpful for next year: feature allowing program chairs to impersonate another user (as CMT allows); additional reviewer-assignment constraints limiting the number of papers from the same institution on one paper, and multiple of the new features listed below.

**OpenReview team’s plans for improvement**, including

new system features:

* Allow program chairs to impersonate another user (as CMT allows), for purposes of understanding reviewer and area chair questions.
* Additional reviewer-assignment constraints limiting the number of papers from the same institution on one paper.

new UI features:

* Reviewer re-assignment directly from the convenient OpenReview “Edge Browser” interface (without the need to visit the PC console).
* Faster load times of the PC console when there are >5k submitted papers.
* Improved UI and organization of the “forum” page containing per-paper reviews and discussion: Easier way to read one-to-one discussion and distinguish between different types of replies: reviews, comments, rebuttals. More self-documenting “idiot-proof” UI widget for discussion participants to select the readers of the comments they enter.
* Allow ACs to download all their assigned paper files in a zip file.
* Add the ability for ACs and reviewers to bid “in blocks,” for example, bidding (positively or negatively) on all submissions containing a keyword, or in an area.’
* Add additional UI options for filtering papers, area chairs, or reviewers by various criteria, and then taking actions (such as sending email) on those objects satisfying the criteria.

new data gathering features:

* Improved expertise data, by automatically gathering the most recent computer vision conference publications that are not yet in DBLP. Improved expertise model based on the full-text and citations of each reviewers’ papers.
* Provide summary statistics of the number of past computer vision publications authored by each reviewer.

simple, alternative configuration for the next ECCV (no new system features needed):

* Restrict the list of papers shown to reviewers during the bidding stage: only the top _N_ relevant submissions to each reviewer (rather than allowing reviewers to see all submissions).
* Allow the reviewer to edit the review after the rebuttal stage without showing the change to the authors until final decisions are released.
