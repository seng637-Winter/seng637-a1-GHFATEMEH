>   **SENG 637 - Dependability and Reliability of Software Systems**

**Lab. Report \#1 – Introduction to Testing and Defect Tracking**

| Group: 15     |
|-----------------|
| Mehrnaz             |   
| Fatemeh           |   
| Alireza            |   
| Sahar              |   
| Sina          |   
| Zahra



**Table of Contents**

[1 Introduction	1](#_Toc439194677)

[2 High-level description of the exploratory testing plan	1](#_Toc439194678)
  
[3 Comparison of exploratory and manual functional testing	1](#_Toc439194679)

[4 Notes and discussion of the peer reviews of defect reports	1](#_Toc439194680)

[5 How the pair testing was managed and team work/effort was
divided	1](#_Toc439194681)

[6 Difficulties encountered, challenges overcome, and lessons
learned	1](#_Toc439194682)

[7 Insights from Bug Report Analytics 1](#_Toc439194683)

[8 Comments/Feedback on the lab and lab document 1](#_Toc439194684)

# Introduction

The system under test for this lab is an ATM simulation system with two different versions, 1.0 and 1.1. In this lab, we performed different types of testing to identify bugs in the system.
Initially, we worked with the system and executed various transactions to become familiar with its functionality. Next, we developed a test plan to determine the features to test, features and modules that cannot be tested, different test types, and the roles of bug reporters and reviewers.
After selecting the features for testing, we began testing the system using exploratory testing. For each bug identified in the ATM system, we created an issue with the bug type on Jira. We reported the bug following a specific format, including a summary of the issue, the function being tested, the initial state of the system, steps to reproduce the bug, expected result, actual result, priority, affected version, etc.
Subsequently, we conducted Manual Scripted Testing. During this phase, we executed each test suite and reported bugs if they weren't identified in the exploratory testing phase.
In the end, we tested the ATM version 1.1, checking if the bugs reported in version 1.0 were fixed or not. Based on the testing, we changed the status to Done or IN-PROGRESS and added comments for bugs that still remained in version 1.1. We also conducted manual scripted testing once again on version 1.1 to identify any new bugs.

Before this lab, we learned about exploratory and manual functional testing in the course. From the lecture, we understood that exploratory (Manual Non-scripted) testing involves designing and executing tests simultaneously. The term 'Unscripted' doesn't imply conducting the test without preparation and a plan; however, understanding the system is important. We also learned that Manual functional (Scripted) testing is pre-designed and recorded as a script. These scripts can then be executed at a later time, either by the same tester or a different one. The idea is to follow a predetermined set of steps and conditions to ensure consistency and repeatability in the testing process, but there is a probability that something may be missed each time. After completing this assignment, we have gained a deeper understanding of these types of testing, how they operate, and how they help to find bugs. In addition, we discovered how exploratory testing helps us in scripted testing.

# High-level description of the exploratory testing plan

## 1. Scope of Testing 

### 1.1 Feature to be tested 

| Module Name | Applicable Rules  | Description |
| --- | --- | --- |
| System Startup | Operator | The system is started up when the operator runs the operator switch to the “On” position. |
| System Shutdown | Operator | The system shuts down when the operator makes sure that no Customer is using the machine, and then turns the operator switch off. |
| Session | Customer | A session begins wjen the customer inserts his/her card into the card reader slot and ends when the card is ejected. |
| Authentication (PIN + card Number) | Customer, Bank | Customer Inserts his Personal Identification Number (PIN) and the Card number and PIN will be sent to the bank for validation. |
| Re-enter PIN | Bank | When a transaction is disapproved due to an invalid PIN, the customer re-enters the PIN and the request is sent to the bank. If the bank approves or disapproves for a different reason, the process continues; if not, the PIN re-entry repeats. |
| Retain Card | Customer, Bank | After three incorrect PIN entries by Customer, the card is retained, a screen informs the customer to contact the bank, and the session ends. |
| Transfer (Transaction) | Customer, Bank | In a transfer transaction, the customer selects an account to transfer from and another to transfer to from available options, then enters an amount on the keyboard. Approval must be obtained from the bank before Transfer between accounts.|
| Deposit (Transaction) | Customer, Operator, Bank | In a deposit transaction, Customer Initiates the deposit at an ATM, inputs the amount, and inserts the cash or checks in an envelope. The Operator Checks the envelope's contents, and verifies against the claimed amount. Approval must be obtained from the bank before deposit.|
| Withdraw (Transaction) | Customer, Bank | During a withdrawal, the customer selects an account type and amount. The system checks if it can fulfill the request before sending it to the bank. If there's insufficient money, the Customer is prompted to choose a different amount. Upon bank approval, cash is dispensed and recorded in the ATM's log, followed by a receipt issuance. |
| Balance Inquiry | Customer, Bank | An inquiry transaction asks the customer to choose a type of account to inquire about from a menu of possible accounts. Approval must be obtained from the bank before giving the information.|
| Another Transaction Massage | Customer, Bank | If a transaction fails or ends, the ATM prompts the customer to choose whether to perform another transaction. |
| Cancel Key | Customer | Customers can abort an ongoing transaction by pressing the cancel key. |
| Show log (clear log + hide log) | Customer | The ATM system keeps a log of transactions to help resolve issues caused by hardware failures during transactions. |
| Print Receipt (date+ time + location+ type of transaction+ accounts+ amount+ ending+ available balance + “to” account for transfer) | Customer | The ATM System provides the customer with a receipt for each successful transaction, showing the date, time, machine location, type of transaction, account(s), amount, and ending and available balance(s) of the affected account ("to" account for transfers). |
| User Interfaces (UI) | Customer | User Interface Design focuses on what Customer might need to do and ensures that the interface has elements that are easy to access (e.g., enter key, cancel key and clear key).
| User experience (UX) | Customer | It is important to pay attention to the correctness and clarity of all text elements within an application, including labels, instructions and error messages to show to the Customer.
### 1.2 Feature not to be tested

**Functionality:**
- Approval must be obtained from the bank before cash is dispensed.
- Approval must be obtained from the bank before physically accepting the envelope.
- Sending a message to the back indicating that the customer has deposited the envelope.
- Removing envelopes after shutdown.
  
**User Interfaces (UI):**
- The user-friendly, visually appealing, usability and performance for the user interface.

**User experience (UX):**
- The interaction and experience users have with the ATM system.

**Database logical:**
- The structured storage of crucial information such as account details, transactions, and logs.
  
**Security:**
- The way to implement robust measures to store data securely, safeguarding it against potential attacks and ensuring that sensitive information is stored in a protected environment.

## 2. Test Type 
In the project of the ATM system, we will focus on functional testing. Functional testing verifies that the software is working as it should and is also free of bugs. To confirm that, the tester simulates an end-user real-life scenario and compares the test results with the anticipated results from the requirements document. We will do the following functional testing for this project: 

- Unit Testing will be used to test individual functions or components of the ATM 	software, such as cash withdrawal or balance inquiry, in isolation. 

- Integration Testing will be used to test individual software modules combined and tested as a group. The focus is on identifying issues in the interaction between integrated units. For example, testing how the ATM system communicates with the bank's database to verify a user's PIN and check account balance. This would include ensuring that the ATM correctly sends a request to the bank's server, receives the account information, and displays it to the user 

- System testing will be used to test the complete and integrated software system to ensure it meets specified requirements. The ATM project it includes Conducting tests on the entire ATM system to verify it performs all intended functions correctly, such as dispensing cash, accepting deposits, displaying account balances, and printing transaction receipts, under various scenarios and conditions. 

- Regression Testing will ensure that the bugs are fixed, and new code changes do not adversely affect existing software functionalities.

## 3. Test Logistic 

### 3.1 Who will test? 
The exploratory testing will be conducted by two teams: Team 1 (Mehrnaz, Sahar, Sina) and Team 2 (Fatemeh, Alireza, Zahra). In each team, two individuals will conduct the testing, while the third person will document the results. After completing their respective tasks, both teams will share their findings and conduct a collective analysis. If a bug is discovered by just one team, all members from both teams will collectively review and test it before reporting it in Jira.

### 3.2 When will the test occur?
After becoming familiar with the ATM system and writing the test plan, dividing the people in the group into two individual groups, each feature outlined in the high-level requirements is tested.

# Comparison of exploratory and manual functional testing


## Comparison

| Aspect                | Exploratory Testing                                                                                           | Manual Functional Testing                                                                                          |
|-----------------------|---------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| **Advantages**          | - Adapts based on real-time findings<br>- Efficiently uncovers complex, usability, and edge case issues        | - Ensures systematic verification of functionalities<br>- Ideal for regression testing with repeatable test cases  |
| **Disadvantages**         | - May result in less detailed documentation<br>- Success heavily relies on tester's expertise      | - Time-consuming and resource-intensive<br>- Requires updates to adapt to application changes |
| **Effectiveness**     | - Highly effective in early stages and for uncovering unexpected issues        | - Provides guaranteed coverage of defined scenarios<br>- Ensures functionalities remain unaffected by new changes        |
| **Efficiency**        | - Offers rapid feedback and allows focusing on critical areas        | - Requires significant effort but allows for predictable execution times            |

For comparing exploratory testing and manual testing in our ATM project, we had a set of 40 tests that helped us deeply understand the system's issues. These tests were great for checking both old and new versions of our ATM, helping us see which bugs got fixed. Unlike exploratory testing, which missed some system parts, manual testing was way more organized and useful. With manual testing, we had clear steps and knew exactly what to test, making it easier to spot and note down bugs properly. This method kept us organized, especially when testing in a specific order, which helped us avoid wrongly calling something a bug. The lack of details in exploratory testing makes it harder to track and report bugs accurately. Plus, manual testing prevented us from repeating work or missing bugs, a common issue with exploratory testing where we often had to redo tests to confirm bugs. Furthermore, manual testing proved to be more reliable and efficient, giving us a clear roadmap for testing the ATM system. Though exploratory testing was less systematic, it did help us get a good feel for the system's operation and user interface. It also helped us to find bugs in real-time when using the system.

# Notes and discussion of the peer reviews of defect reports

## Enhanced Defect Reporting through Specific Scenario Logging

### Feedback:
Initially, our defect logging process grouped multiple scenarios under a single bug report. For example, a single report might cover incorrect withdrawal amounts across different account types and amounts. This approach, while streamlined, was found to obscure the specific details necessary for accurately tracking and resolving each unique scenario.

### Actionable Item:
In response to this feedback, we decided to refine our approach to logging defects. We agreed to create separate bug reports for each account type and withdrawal amount. This change aimed to isolate and address the specifics of each scenario, capturing detailed expected and actual results.

### Impact:
This revised approach significantly enhanced the clarity and utility of our defect reports. By providing a distinct report for each scenario, we improved the specificity of our tracking and resolution processes. This not only facilitated more effective communication within the team but also allowed for more accurate prioritization and resolution of defects. The adoption of a more granular and scenario-specific reporting method marked a positive step towards more efficient and effective defect management.

## Clarity on Bug ID Naming Convention

### Feedback:
Initial bug IDs used the prefix "SENGG15" (e.g., SENGG15-1, SENGG15-2), which was found to be overly complex and cumbersome for team communication. Feedback from team members suggested the need for a more streamlined and intuitive system.

### Actionable Item:
We simplified the ID prefix to directly reflect the project name, changing it to "ATM" (e.g., ATM-1, ATM-2). This adjustment made the bug IDs clearer and more concise, facilitating easier reference and discussion.

### Impact:
The new naming convention significantly improved communication efficiency within the team, demonstrating the importance of adaptability and the positive impact of addressing feedback in our workflows.

## Categorizing the Bugs

### Feedback:
Initially, bugs were categorized by incorporating the function name into the bug title, such as "MFT - Withdraw: $60 withdrawal from checking account shows incorrect data in the receipt." Here, "MFT" denotes manual function testing, and "Withdraw" specifies the ATM project function. Feedback suggested that this method, while informative, could be streamlined for better management and visibility.

### Actionable Item:
Based on the feedback, we transitioned to using Jira's Epic feature to categorize bugs. This approach allows for a more organized and visually distinct classification through the use of different colors for each Epic, enhancing the ability to filter and manage issues within Jira effectively.

### Impact:
Adopting Epics for bug categorization has significantly improved our project management workflow, offering a clearer, more efficient way to track and address bugs. This method has enhanced team productivity and streamlined issue resolution processes.

![Jira Board](https://github.com/seng637-Winter/seng637-a1-GHFATEMEH/blob/main/Assignment%201-Answers/resources/jira-board.png)


# How the pair testing was managed and team work/effort was divided 

In this project, we divided into three smaller teams:  
Team 1: Mehrnaz, Sahar, Sina   
Team 2: Fatemeh, Alireza, Zahra    

During the exploratory testing phase, Teams 1 and 2 operated independently, employing a pair testing strategy where two team members executed tests while the third documented the outcomes. Upon completion, both teams shared and collectively analyzed their findings. A majority of the bugs were identified by both teams' reports. When a bug was found by only one team, all members from both teams reviewed and tested it together before adding it to Jira.  
For the manual scripted testing phase, Both teams worked together again. Team 1 worked on the first 20 Manual Functional Tests (MFTs), while the second team worked on the remaining 20. This method mirrored our previous strategy, with two members of team testing and another documenting the findings. We made a point not to report defects that were already discovered during the exploratory testing phase.  
Following the update to version 1.1 of the ATM system, we conducted regression testing to reassess the status of bugs reported earlier. In this part, we again worked in two groups and reported The defect statuses in the new version. Bugs that were corrected in version 1.1 were labeled as Done, while those still present were tagged as IN-PROGRESS, accompanied by a note indicating their persistence in version 1.1. Additionally, we actively looked for and documented any new issues that had not been detected in previous assessments.
After completing these tasks, we collectively reviewed all issues to ensure their correctness. We then divided the report into different sections for individual work, with each part being reviewed by another team member to guarantee the accuracy and comprehensiveness of our final documentation.

# Difficulties encountered, challenges overcome, and lessons learned

## Difficulties Encountered and Challenges Overcome:
-  Working together as a team had its ups and downs. We sometimes disagreed on how to do things or did not communicate well, which slowed us down. Learning to talk openly and make decisions together was key to moving forward and getting our work done.
- Selecting an appropriate issue tracking system also posed a challenge. We had to evaluate the features of platforms like Jira and Azure DevOps to determine which best suited our workflow. Choosing between Jira and Azure DevOps, we compared their features in-depth, considering our project needs and team preferences, and finally selecting the Jira that aligned best with our workflow.
- Learning How to Use Jira was challenging At first. using Jira with all its features seemed a bit complex for our team. But as we spent more time with it, figuring out how to manage tasks and track our project got easier. We learned how to set up our work and keep everything organized. 
- Although we had established a defect report format, consistently Sticking to it proved difficult. The challenge was not in creating a standard format but in ensuring everyone followed it precisely.
- Prioritizing bug fixes involved an evaluation of their impact, which was managed by assessing their potential impact on users and the system, fixing critical ones first.
- While conducting exploratory testing on the system, we did not have all of the detailed requirements of the system and  we encountered cases where we were unsure whether certain issues were bugs or not. For instance, Card 1 has Checking and Savings accounts, but the system also presents a Money Market Note as an option.



## Lessons Learned:
- The importance of thoroughly understanding the instructions and requirements before beginning testing, can significantly save time and write effective bug reports within a chosen reporting system.
  
- In exploratory testing, we cannot find all the bugs in the system because tests are designed and executed at the same time. However, in scripted testing, as we have a scenario and test case, we can check all the scenarios.

- Additionally, understanding the importance of teamwork, working collaboratively, using diverse perspectives, and sharing knowledge was crucial in overcoming the challenges we faced.
  
# Insights from Bug Report Analytics

The pie chart titled "Bugs Reported by Status" provides a visual breakdown of the current state of bug reports. It shows that the majority (59%) of the bugs have been resolved, indicating effective issue resolution processes within the team. However, there remains a significant portion (28%) of newly reported bugs, suggesting ongoing testing and identification of issues. The smallest segment (13%) represents bugs that have not yet been fixed, which could either be due to lower priority, more complex issues, or those requiring additional information. This distribution offers a snapshot of the project's health in terms of software quality assurance and highlights areas that may need further attention or resources.

![Jira Board](https://github.com/seng637-Winter/seng637-a1-GHFATEMEH/blob/main/Assignment%201-Answers/resources/Bugs%20reported%20by%20status.png)

The chart "Bugs Reported by Priority" displays the urgency levels of logged bugs. Most bugs are marked with medium priority, indicating a steady workflow for the team. High and highest priority bugs are fewer, suggesting good system stability or effective issue prevention. Low priority items are minimal, and likely not affect the system's overall performance significantly.

![Jira Board](https://github.com/seng637-Winter/seng637-a1-GHFATEMEH/blob/main/Assignment%201-Answers/resources/Picture2.png)

# Comments/Feedback on the lab and lab document

The lab's documentation could improve in coherence and enhancements in guidance making it more clear and beneficial for students. For example, the table of contents does not align perfectly with the structure of the report sections. Sections like "Comments and Feedback" are present in the report but are not listed in the table of contents, leading to some confusion. Another example, the first sentence in step 5 of Regression Testing is unclear.
