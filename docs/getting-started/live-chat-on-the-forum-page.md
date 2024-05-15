# Live Chat on the Forum Page

Live chat is a new OpenReview feature available to all venues to support synchronous dialog among reviewers and ACs. Similar to a Slack channel or chat room, messages will be visible to all participants and appear in real time as they are posted. In addition, there is an option to enable browser notifications so participants can be alerted when new messages are posted.

This feature was developed with encouragement and funding from the Alfred P. Sloan Foundation. For more information, see the Motivations section below.

#### Enabling Live Chat

Program Chairs can enable this feature from the venue management forum using a [Comment Stage](../reference/stages/comment-stage.md). First, click the Comment Stage button, then select the option "Enable Chat Between Committee Members". Members of the committee must be selected as participants.&#x20;

Similarly, live chat can be disabled for all forum pages by clicking Comment Stage and selecting the disable chat option.

Once enabled, the live chat interface will be visible on all forum pages under the “Committee Members Chat” tab. Chat messages are hidden in the “Discussion” tab to prevent confusion and keep the discussions with paper authors and other users separate.

#### Chat Features

* Real-time: Type a simple message, and it appears immediately to other reviewers who have the same OpenReview forum page open in their browser, enabling interactive discussion.
* Private: By default, messages are visible all program chairs, area chairs, and reviewers of a paper. The messages will never be visible to the authors or the public. As an extra reminder, the chat participants are below the text input box. The ability to send private messages to a subset of the chat participants is planned for a future release.
* Rich text: Chat messages can use Markdown for formatting and LaTeX for math notation. When composing a message you can preview how it will look by clicking the Preview button. To copy the unformatted text of a posted chat message, hover over the message and click the View Raw button.
* Replies: Users can reply to specific message from earlier in the chat by hovering over the message and clicking the Reply button.
* Permalinks: Users can copy a URL pointing to a specific chat message by hovering over the message and clicking the Copy Link button.
* Notifications: Browser and email notifications are supported. User should enable browser notifications from their browser. Email notifications will be sent in bulk either every 5 messages or every 4 hours.&#x20;
* Reactions: Users can add emoji "reactions" to chat messages, in the same way they can react to messages on apps like Slack and GitHub. To add a reaction, hover over the message and click the "Add Reaction" button, then select the emoji you want to use. All the reactions a message has gotten will be shown below the message content as separate buttons. Clicking one of these buttons will either add a reaction of that type, or remove your reaction if you have already added one.

#### Motivation

In the past, many peer-reviewed yearly conferences in computer science (such as NeurIPS and CVPR) held an in-person meeting among the 20-30 “area chairs” of the conference to discuss and debate each paper.  These meetings enabled extensive synchronous communication and serendipitous discussion during the final decision-making phase of the peer review process.&#x20;

Unfortunately, these practices of in-person area-chair meetings are increasingly rare due to growing conference size, travel expenses, and (more recently) risks from the pandemic. Given the current universal use of web tools for reviewing, one would hope that the scientific communities would take advantage of internet technology to greatly increase interactivity during the peer review process, but unfortunately, reviewing workflows remain largely unchanged. &#x20;

One exception is the introduction of “rebuttal statements” (an opportunity for authors to write a response to first drafts of the reviews), and asynchronous messages among the reviewing team. However, due to the lack of synchronous communication, discussion of these rebuttals (and scientific debate among the decision-making team) is hampered – often lackluster or incomplete. &#x20;

Synchronous communication creates beneficial social pressure for engagement and the opportunity for interactive clarification without the extensive “context switching” inherent in asynchronously juggling many tasks. One highly experienced computer science reviewer and OpenReview user told us, “The burden of context switching after 24 hours away from a conversation can sometimes be the bottleneck in writing a response; re-remembering the details of a paper often takes even more time than writing a reply to a co-reviewer. Synchronous communication could dramatically reduce the burden and increase quality of reviewing by removing this context-switching bottleneck.”

While reviewers may naturally find each other online at the same time, and benefit from the chat feature, scheduled chat discussion sessions may be even more helpful. Scheduling synchronous sessions will be one key aspect of recapturing the benefits of in-person synchronous discussion.  The details and cultural considerations of this scheduling will be driven by the program chairs. The in-person meetings were a burden, but also provided a scientific/social occasion that was enjoyed and cherished; we anticipate that with some sensitive design, program chairs can provide a similar occasion. The burden can be reduced by culling from the process submissions that are clear rejections.&#x20;

Why text messaging rather than audio/video? Although speaking is easy, typing has multiple advantages: typed messages can be more easily archived, read later, skimmed quickly, analyzed, and studied. Many people are used to interactive synchronous text chat tools, such as Slack, and find them very productive. Furthermore, text has high accessibility, and is recommended by the Web Content Accessibility Guidelines (WCAG 2.0). With text entry, users can naturally make some statements publicly and mark others as private; scientists are already used to this ability in typed chat systems such as Slack or Zoom. Typed chat messages support not only fully-synchronous interaction, but also interactive semi-synchronous interaction (e.g. with two minute interruption or delay), which fits modern work styles. If some users are slow typists, they can use voice recognition on their devices to type messages at speaking speed.\
\
