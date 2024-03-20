# Roadmap Link:

### https://miro.com/app/board/uXjVPT4cevQ=/

# Bug Report Demo:

- `Bug Title:` This is the title of bug help to identify the bug in one-liner description.

- `Description:` This should cover bug description along with the exact Steps To Reproduce, Expected Result, Actual Result and example.

- `Status:` This field indicates the actual status of the bug in the Bug life cycle.

#### Here is the list of Bug Statuses in standard Bug life cycle:

- **New, Assigned, Resolved, Reopened, Verified (Vary based on Bug Tracking Tool)**

- `Bug Assignee:` This is the name of the developer who is responsible to resolve the bug.

- `Bug Cc:` Add the manager and lead email address in CC list.

In the Bug Tracking Tool, this field is auto-populated based on configuration.

- `Reported On:` The date on which the bug is occurred & reported the bug.

- `Browser:` This field indicates on which browser & version this issue occurs.

- `Bug Type:` The bug is categories into a different category like Functional, Navigational, GUI etc.

- `Environment:` On which OS, platform this bug occurs.

- `Component:` This field indicates the sub-modules of the product.

- `Priority:` Urgency to fix the bug?

#### Priority can be set as P1 to P5.

- The P1 means “first fix this bug i.e. priority is highest” and P5 means “No urgent, when get time then fix it”.

- `Blocker:` Unless and until this fix no further testing can be done

- `Critical:` Application is crashing or Losing the data.

- `Major:` Major function under test is not working.

- `Minor:` Minor function under test is not working.

- `Trivial:` UI issues

- `Enhancement:` Asking for new changes as an enhancement.Severity: This tells you about the impact of the bug.

#### Types of Severity:

- `Product`

- `Reporter:` Your name.

In the Bug Tracking Tool, this field is auto-populated.

`Reproduces:` In this section, you can have options like Always and Sometimes etc.


`URL:` The page URL on which bug occurred.

`Build number:` The Build number field describes the number of Builds on which the bug is found in.


#  Test Plans report Demo:

Test Plan: Login Functionality

`1. Introduction:`
- The purpose of this test plan is to verify the functionality and reliability of the login feature of the XYZ web application. This document outlines the test objectives, scope, approach, and resources required for testing.

`2. Objectives:`

- To ensure that users can log in successfully to the XYZ web application using valid credentials.
- To verify that appropriate error messages are displayed for invalid login attempts.
- To validate the security and integrity of the login process.
  
`3. Scope:`

- The login functionality will be tested across supported web browsers (Chrome, Firefox, Safari).
- Testing will cover both positive and negative test scenarios, including valid and invalid login credentials.
- The test environment will include the development and staging environments of the XYZ web application.

`4. Test Approach:`

- Manual testing will be performed by QA testers to verify the login functionality.
- Test cases will be designed to cover various login scenarios, including valid login, invalid username, invalid password, and account lockout.
- Automated regression tests may be developed using `Selenium WebDriver` for future regression testing.
  
`5. Test Cases:`

- Test Case 1: Verify successful login with valid credentials.
- Test Case 2: Verify error message for invalid username.
- Test Case 3: Verify error message for invalid password.
- Test Case 4: Verify account lockout functionality after multiple failed login attempts.

`6. Test Environment:`

- Web Application: XYZ (version X.X)
- Operating Systems: Windows, macOS, Linux
- Web Browsers: Chrome, Firefox, Safari
- Test Data: Valid and invalid login credentials
  
`7. Test Schedule:`

- Testing will commence on [start date] and is expected to be completed by [end date].
- Test execution will be conducted in multiple iterations, with regular status updates provided to stakeholders.

`8. Risks and Assumptions:`
  
**Risk:** Network or server issues may impact test execution.
**Assumption:** The test environment will be stable and accessible throughout the testing period.

`9. Deliverables:`

- Test Plan Document
- Test Cases Document
- Test Execution Reports

10. Approvals:

