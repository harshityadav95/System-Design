# Low Level Design: Payment Tracking App

This is the problem statement.

We are to design the low level architecture of a payment tracking app. Here are some of it's features:

1. Adding expenses
2. Editing expenses
3. Settling expenses
4. Adding group expenses. Settling them in a simplified way.
5. Adding comments to expenses
6. Track all state change activity using an activity log.

Out of these, we identify the first 4 as core to a payment tracking app. These features will be implemented in the remaining videos of the chapter.

We define the objects in our system with a 'State Based Approach'.

We then find a way to show user balances for a group: summing group expenses per user.

_When finding the overall balances in a group, why didn't we use a "group by" clause to sum all the user balances from the expense table?_

The number of users in a group is variable. We could try to denormalize the expenses table into two tables of balances and expense\_info:

**balances**:

| expense\_id | user\_id | balance | paid | owes |
| ----------- | -------- | ------- | ---- | ---- |
|             |          |         |      |      |

**expense\_info**:

| id | title | desc | imageUrl | group\_id |
| -- | ----- | ---- | -------- | --------- |
|    |       |      |          |           |

On a 'getGroupExpenses' request for group id=123, we fire the database query:

_'select user\_id, sum(balance) from balances where expense\_id in (select id from expense where group\_id = '123') group by user\_id'_

This requires a nested query, which could perform poorly.\
However, this style of querying will perform very well for finding an individual user's expenses.\
Depending on what you are trying to optimize, you may not may not denormalize the tables.

Subset Sum Problem: [https://en.wikipedia.org/wiki/Subset\_sum\_problem](https://en.wikipedia.org/wiki/Subset\_sum\_problem)

Blog on the algorithm: **** [https://medium.com/@mithunmk93/algorithm-behind-splitwises-debt-simplification-feature-8ac485e97688](https://medium.com/@mithunmk93/algorithm-behind-splitwises-debt-simplification-feature-8ac485e97688)

Stackoverflow Question: [https://stackoverflow.com/questions/877728/what-algorithm-to-use-to-determine-minimum-number-of-actions-required-to-get-the/](https://stackoverflow.com/questions/877728/what-algorithm-to-use-to-determine-minimum-number-of-actions-required-to-get-the/)



We set our coding requirements by looking at the following:

1. APIs
2. Caching
3. Concurrency
4. Testing



We draw the low level architecture diagram of the system. We then move forward with 3 steps in mind:

1. Object definitions (Naming, composition and interfaces)
2. Algorithm
3. Test cases

Point to note:

Using a BigDecimal instead of a double value would be preferred in a financial application. The double may have precision errors.



**Algorithm:**

1. User passes groupId in request for payment graph
2. Check if user belongs to group
3. Get all expenses belonging to the group from Expense Service
4. Sum all expenses to get a map of user balances
5. Separate positive and negative balances into two heaps
6. Poll the two heaps and store the difference
7. Repeat the above step till the heaps are empty
8. Refactor code where necessary
9. Write tests and refactor more if necessary

**Tips:**

1. Don't jump into coding before you have understood the problem well
2. Attack the main problem first
3. Get comfortable with your IDE, and practice writing code

![](<.gitbook/assets/image (16).png>)

![](<.gitbook/assets/image (17).png>)

![](<.gitbook/assets/image (18).png>)

![](<.gitbook/assets/image (19).png>)

![](<.gitbook/assets/image (20).png>)

![](<.gitbook/assets/image (21).png>)

![](<.gitbook/assets/image (22).png>)

![](<.gitbook/assets/image (23).png>)

![](<.gitbook/assets/image (24).png>)

Code  : [https://gitlab.com/harshityadav95/splitwise-code-sample](https://gitlab.com/harshityadav95/splitwise-code-sample)\


{% embed url="https://youtu.be/xxtKnynf9_I" %}





