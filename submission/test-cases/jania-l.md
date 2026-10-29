# Test Cases: Jania L

**Test ID:** TC-JL-01
**Written by:** Jania L
**Title:** A costume's vote count cannot go below 0
**Traces to:** REQ-[check] (voting rules / vote count)
**Test type:** Boundary Value
**Preconditions:** Logged in as ghost_gary (password boo12345). A costume with exactly 0 votes exists in the Gallery.
**Test data:** Costume: "Vampire Lord" (0 votes)

**Steps:**
1. Open the Gallery page
2. Find "Vampire Lord" and confirm it shows 0 votes
3. Click Vote, then click Vote again to remove the vote
4. Click remove/unvote once more
5. Open the Leaderboard page

**Expected result:** The vote count never drops below 0. After step 4 it still reads "0 votes" in both the Gallery and the Leaderboard. (Boundary checks: 1 → 0 allowed, 0 stays 0, -1 never appears.)
**Actual result:** The vote count dropped to -1.
**Status:** Fail
**Severity:** Major
**V or V:** Verification

---

**Test ID:** TC-JL-02
**Written by:** Jania L
**Title:** Leaderboard order matches actual vote counts
**Traces to:** REQ-[check] (leaderboard ranking), G2 (judge identifies leader quickly)
**Test type:** Integration
**Preconditions:** Logged in as ghost_gary. Three costumes exist in the same category with known vote counts.
**Test data:** "Vampire Lord" = 5 votes, "Pumpkin King" = 3 votes, "Witchy Wendy" = 1 vote

**Steps:**
1. Open the Gallery and confirm the three vote counts above
2. Open the Leaderboard page
3. Read the order of the three costumes from top to bottom

**Expected result:** The Leaderboard lists them in this order: Vampire Lord (5), Pumpkin King (3), Witchy Wendy (1). The counts match the Gallery.
**Actual result:** The Leaderboard order did not match the vote counts. A costume with fewer votes was ranked above one with more.
**Status:** Fail
**Severity:** Critical (the contest cannot be judged fairly)
**V or V:** Verification

---

**Test ID:** TC-JL-03
**Written by:** Jania L
**Title:** Ranking is sorted by votes, not by submission time
**Traces to:** REQ-[check] (leaderboard ranking)
**Test type:** Unit
**Preconditions:** The app's code is open in app/. The leaderboard sorting function is identified (e.g., in app/[check].js).
**Test data:** Costumes in submission order: A (submitted first, 4 votes), B (submitted second, 2 votes), C (submitted last, 0 votes)

**Steps:**
1. Call the leaderboard sort function with costumes A, B, C
2. Read the order of the returned list

**Expected result:** The returned order is A (4), B (2), C (0): highest votes first, no matter when each costume was submitted.
**Actual result:** The returned order was C, B, A. The function sorts by most recent entry instead of by votes.
**Status:** Fail
**Severity:** Major
**V or V:** Verification

---

**Test ID:** TC-JL-04
**Written by:** Jania L
**Title:** Voting in the Spooky category adds the vote to the selected costume
**Traces to:** REQ-[check] (casting a vote)
**Test type:** Integration
**Preconditions:** Logged in as ghost_gary. At least two Spooky costumes exist with known vote counts.
**Test data:** Spooky costumes: "Vampire Lord" = 2 votes, "Headless Horseman" = 2 votes (listed directly after Vampire Lord)

**Steps:**
1. Open the Gallery and filter to the Spooky category
2. Click Vote on "Vampire Lord"
3. Read the vote counts on both costumes
4. Repeat steps 1–3 in another category (e.g., Funny) to compare

**Expected result:** "Vampire Lord" goes to 3 votes and "Headless Horseman" stays at 2. Voting behaves the same in every category.
**Actual result:** In Spooky, the vote went to the costume listed directly after the selected one. Other categories counted the vote correctly.
**Status:** Fail
**Severity:** Critical (votes are credited to the wrong costume)
**V or V:** Verification

---

**Test ID:** TC-JL-05
**Written by:** Jania L
**Title:** A user cannot cast more than 3 votes
**Traces to:** REQ-[check] (each user gets a maximum of 3 votes)
**Test type:** Boundary Value
**Preconditions:** Logged in as ghost_gary with 0 votes used. At least 4 costumes by other users exist.
**Test data:** Costumes: "Vampire Lord", "Pumpkin King", "Witchy Wendy", "Mummy Max"

**Steps:**
1. Vote for "Vampire Lord" (vote 1)
2. Vote for "Pumpkin King" (vote 2)
3. Vote for "Witchy Wendy" (vote 3)
4. Try to vote for "Mummy Max" (vote 4)
5. Check Mummy Max's vote count

**Expected result:** Votes 2 and 3 are accepted. Vote 4 is blocked with a message like "You have used all 3 votes," and Mummy Max's count does not change.
**Actual result:** The 4th vote was accepted, and the user kept voting past 3.
**Status:** Fail
**Severity:** Major
**V or V:** Verification

---

**Test ID:** TC-JL-06
**Written by:** Jania L
**Title:** A user cannot vote for their own costume
**Traces to:** REQ-[check] (no self-voting), G1 (fair contest) [check]
**Test type:** Security
**Preconditions:** Logged in as ghost_gary. ghost_gary has submitted a costume.
**Test data:** ghost_gary's costume: "Ghostly Gary" (0 votes)

**Steps:**
1. Open the Gallery
2. Find "Ghostly Gary" (owned by ghost_gary)
3. Click Vote
4. Check the vote count

**Expected result:** The vote is blocked (the button is hidden or disabled, or an error says you can't vote for your own costume). The count stays at 0.
**Actual result:** The vote was accepted, and "Ghostly Gary" went to 1 vote.
**Status:** Fail
**Severity:** Critical (users can rig the contest)
**V or V:** Verification

---
