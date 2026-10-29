Test ID: TC-00 Written by: Instructor Title: Costume name at the maximum length is accepted Traces to: REQ-04 (Costume names must be between 1 and 50 characters) Test type: Boundary Value Preconditions: Logged in as ghost_gary (password boo12345). On the Submit Costume page. Test data:

Name: TheHeadlessHorsemanOfSleepyHollowRidesAgainTonight (exactly 50 characters)
Description: Rides a pumpkin-powered scooter.
Category: Spooky
Steps:

Type the test data name into the Costume name field
Type the test data description into the Description field
Select Spooky from the Category dropdown
Click Submit Costume
Open the Gallery page
Expected result: A success message appears. The costume appears in the Gallery with the full 50-character name, not truncated. Actual result: Success message appeared. The costume appears in the Gallery with the full name. Status: Pass Severity: N/A V or V: N/A