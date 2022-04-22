# How to modify the proposed assignments

The edge browser is a tool for visualizing edges, or assignments, created by OpenReview’s automatic paper matching algorithm. You can use it to browse, sort, search, and create proposed assignments between reviewers and papers before deploying them.&#x20;

When you first open the edge browser, all papers will appear in a column on the left. You can click on a certain paper to see a second column of reviewers pop up to the right. Similarly, if you click on a reviewer, all of their assigned papers will pop up in another column to the right, and so on.

![](<../../../.gitbook/assets/image (8) (1).png>)

The color of each item represents the relationship between that item and the one selected at left:

1. <mark style="background-color:green;">Light green</mark> means that the item is assigned to the item selected at left.&#x20;
2. <mark style="background-color:red;">Light red</mark> means that the item has conflict with the item selected at left.&#x20;
3. <mark style="background-color:orange;">Light orange</mark> means that the item both has conflict and is assigned to the item selected at left.&#x20;
4. White means that the item is not assigned to and has no conflict with the item selected at left.&#x20;

Each item will display various edges calculated by the matcher and used to make assignments, such as the Bid, Affinity, Aggregate scores, and Conflicts. The trashcan button can be used to remove an edge. You can create new assignments using the ‘Assign’ button.

'Assignments' tells you how many papers are assigned to a given reviewer. You may also see 'Custom Max Papers' here if certain reviewers requested a specific max number of papers. You can filter out reviewers who have met their quota with the checkbox 'Only show reviewers with fewer than max assigned papers.' Once a reviewer has hit their quota, the 'Assign' button will be disabled and you will only be able to assign them additional papers using the 'Invite Assignments' button after deployment.

![](<../../../.gitbook/assets/image (6).png>)

You can search for specific papers by paper title or number at the top of the first column. At the top of the subsequent columns you can also search for specific reviewers by profileID, name, or email. You can sort subsequent columns on the right by whatever edges are displayed, such as Assignment, Aggregate Score, Bid, Affinity Score, and/or Conflict, using the 'Order By' dropdown.

You can copy, edit, and create matching configurations as many times as you want until deployment. You can also use the ‘View Statistics’ button on the assignment page to view a breakdown of paper assignments. When you are happy with your assignments, [you can deploy them.](how-to-deploy-the-proposed-assignments.md)
