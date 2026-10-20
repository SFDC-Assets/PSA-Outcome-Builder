# Test Cases: Outcome Strategy Builder and Indicator Setup

Use these test cases to check the accelerator after you install it, and again after you change your own copy of a flow. Run them in a **sandbox or test org**, never in production: they create and delete records.

Record your result in the **Result / Notes** column of each table.

## Before you start

| | |
|---|---|
| Flows | `Outcome_Strategy_Builder` and `Indicator_Setup`, with their 6 subflows, all active. If you made your own copy from a template, run your copy. |
| How to run | Setup → **Flows** → open the flow → **Run**, or go to `/flow/Outcome_Strategy_Builder` or `/flow/Indicator_Setup` in your org. |
| Naming rule | Every record you type a name for starts with `TEST-<case>`, for example `TEST-F1-01 Outcome`. Each case gives the names to type. This makes the records easy to find and delete afterwards. |
| Order | Run the cases in order. Some cases use records from earlier ones (for example, F2-09 needs F2-03). |
| Spelling | Standard object and field labels, and standard values such as "Program" as an activity type or strategy level, use US spelling. |

## Create the test data

The flows pick from existing records, so create these first, in this order. Fill in any other fields your org requires.

| # | Object | Name | Fields to set |
|---|---|---|---|
| 1 | Unit of Measure | `TEST-Seed UOM` | Code `TESTSEED`, Type **Custom** |
| 2 | Benefit Type | `TEST-Seed Benefit Type` | Process Type **Program Management**, Unit of Measure `TEST-Seed UOM` |
| 3 | Program | `TEST-Seed Program 1` and `TEST-Seed Program 2` | Status **Active** (the Outcome Strategy Builder only lists Active programs) |
| 4 | Benefit | `TEST-Seed Benefit` | Benefit Type `TEST-Seed Benefit Type`, Program `TEST-Seed Program 1` |
| 5 | Goal Definition | `TEST-Seed Goal` | |
| 6 | Impact Strategy | `TEST-Seed Strategy` | Status **Active** |
| 7 | Outcome | `TEST-Seed Outcome` | Status **Active** |
| 8 | Indicator Definition | `TEST-Seed Indicator` | Status **Active**, Unit of Measure `TEST-Seed UOM` |
| 9 | Impact Strategy Assignment | (auto-numbered) | Impact Strategy `TEST-Seed Strategy`, Outcome `TEST-Seed Outcome`. F1-15 and F1-17 check how the flow handles a link that already exists. |

To run the optional access test (A-01), you also need the `Outcome_Accelerator_User` permission set from the `test-support` folder in the GitHub repository. Deploy it to your sandbox with your usual tools.

---

## Flow 1: Outcome Strategy Builder

"Activity" means the **Activity Description** field, which becomes the Outcome Activity's name.

### Happy path

After **Review Your Outcomes**, click **Next** with no rows ticked to open **What would you like to do next?**. Choose the action there.

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| F1-01 | Welcome → **Let's Begin!** → **Create a new outcome** (Active, Increase, Short-Term) → activities **Yes** → 1 row, Type **Program**, TEST-Seed Program 1 → **No, I'm done adding outcomes** → Review: **Next** → **Confirm — but no Strategy linking needed** | Outcome `TEST-F1-01 Outcome`; Activity `TEST-F1-01 Activity` | Success lists the outcome as not linked, with "→ Program: TEST-Seed Program 1". The activity has Type Program and Program set. The Review screen shows the Outcome Activities table and no action choice. | |
| F1-02 | New outcome → 3 rows: Program (Program 1), Benefit (TEST-Seed Benefit), Goal Definition (TEST-Seed Goal) → done → no strategy | Outcome `TEST-F1-02 Outcome`; Activities `TEST-F1-02 Program`, `TEST-F1-02 Benefit`, `TEST-F1-02 Goal` | Each activity has only its own lookup set. | |
| F1-03 | New outcome → activities **No** → done → Review: **Next** → **Confirm — proceed to strategy linking** → **Create a new strategy** (Level Organisational, Type Theory of Change) → tick the outcome → **No, I'm done** | Outcome `TEST-F1-03 Outcome`; Strategy `TEST-F1-03 Strategy` | The Review screen shows no Outcome Activities table. The strategy has Status Planned and Level Organizational. 1 assignment. Success shows strategy → outcome. | |
| F1-04 | New outcome → linking → **Link to an existing active strategy** → TEST-Seed Strategy | Outcome `TEST-F1-04 Outcome` | 1 assignment on TEST-Seed Strategy. The strategy is unchanged. Only Active strategies are listed. | |
| F1-05 | 2 new outcomes (**Yes, add another outcome**) → new strategy, tick only A | Outcomes `TEST-F1-05 Outcome A`, `TEST-F1-05 Outcome B`; Strategy `TEST-F1-05 Strategy` | 1 assignment (A). B shows as not linked. | |
| F1-06 | Outcome A with 1 Program activity, and outcome B → A to a new strategy → **Yes, link another strategy** → B to TEST-Seed Strategy | Outcomes `TEST-F1-06 Outcome A`, `TEST-F1-06 Outcome B`; Activity `TEST-F1-06 Activity`; Strategy `TEST-F1-06 Strategy` | 2 assignments. Success shows each outcome under its own strategy, with the activity target. | |
| F1-07 | **Use an existing outcome** → TEST-Seed Outcome → 1 Program activity | Activity `TEST-F1-07 Activity` (on TEST-Seed Outcome) | No new outcome. The activity is on TEST-Seed Outcome. | |

### Review and edit

After you save an edit or relink, the flow returns to the same review screen, so you can correct it again. To move on, leave all rows unticked and click **Next**. On Review Your Outcomes, this opens the next-step screen.

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| F1-08 | New outcome + 1 activity → on Review, edit the outcome: rename it to `TEST-F1-08 Outcome Edited`, Term Long-Term | Outcome `TEST-F1-08 Outcome` (renamed); Activity `TEST-F1-08 Activity` | The flow returns to Review, showing the new name and term. Untick everything, click Next, then choose **Confirm — but no Strategy linking needed**. The saved record and Success show the new name and term. | |
| F1-09 | New outcome + 1 activity on Program 1 → on Review, edit the activity and change it to TEST-Seed Program 2 | Outcome `TEST-F1-09 Outcome`; Activity `TEST-F1-09 Activity` | The flow returns to Review, showing Program 2. Untick everything, click Next, then choose an action. The activity points to Program 2. | |
| F1-10 | New outcome → new strategy → on Review Your Strategy Records, edit the strategy: rename it to `TEST-F1-10 Strategy Edited`, Level Department | Outcome `TEST-F1-10 Outcome`; Strategy `TEST-F1-10 Strategy` (renamed) | The flow returns to Review Your Strategy Records, showing the new name. Both are saved. There is no Status field on the edit screen, and Status stays Planned. | |
| F1-11 | 3 new outcomes (A, B, C) → new strategy, tick A and B → on Review Your Strategy Records, tick A's assignment → relink it to C | Outcomes `TEST-F1-11 Outcome A`, `TEST-F1-11 Outcome B`, `TEST-F1-11 Outcome C`; Strategy `TEST-F1-11 Strategy` | The relink screen opens with **Link it to a different outcome** already selected. The flow returns to Review Your Strategy Records. The 2 assignments are now on B and C. No duplicate. | |
| F1-12 | Same session: tick the assignment on C → choose C again | none | "Select a different replacement Outcome…". **Previous** returns to the relink screen. | |
| F1-13 | Same session, on the relink screen: choose B (already linked to this strategy) → **Previous** → choose A → untick everything → Next | none | B gives "That Outcome is already linked to this Impact Strategy." and no duplicate. A is saved, and the flow returns to strategy review. Success shows the strategy linked to A and B. | |
| F1-14 | New outcome → TEST-Seed Strategy → **Yes, link another strategy** → TEST-Seed Strategy again | Outcome `TEST-F1-14 Outcome` | Still only 1 assignment. Success lists the outcome once under TEST-Seed Strategy and shows the "Some links already existed" note. | |
| F1-15 | **Use an existing outcome** → TEST-Seed Outcome → activities **No** → **No, I'm done adding outcomes** → TEST-Seed Strategy | none. TEST-Seed Outcome is already linked to TEST-Seed Strategy (test data row 9), so no assignment is created. | The Review screen is skipped, because nothing new was created. The flow goes straight to Link Outcomes to a Strategy. After linking, the strategy review screen is also skipped, because no assignment was created. Success lists TEST-Seed Outcome under TEST-Seed Strategy, with no "Outcomes not linked to a Strategy" section, and shows the "Some links already existed" note. | |
| F1-16 | New outcome + 1 activity → on Review, edit the outcome (rename it to `TEST-F1-16 Outcome Edit 1`) → back on Review, edit it again (`TEST-F1-16 Outcome Edit 2`) → untick → **Next** → **Confirm — but no Strategy linking needed** | Outcome `TEST-F1-16 Outcome`; Activity `TEST-F1-16 Activity` | Each return to Review shows the latest name, with no rows ticked. The second edit screen shows Edit 1, not your original typing. The saved name is Edit 2. You choose the action only once, after the last edit. | |
| F1-17 | A new outcome, and the existing TEST-Seed Outcome, both with activities **No** → Review: **Next** → **Confirm — proceed to strategy linking** → tick both outcomes → TEST-Seed Strategy | Outcome `TEST-F1-17 Outcome` (1 new assignment) | The strategy review table lists only the TEST-F1-17 Outcome assignment, not the TEST-Seed one that already existed. Success lists both outcomes under TEST-Seed Strategy and shows the "Some links already existed" note. Only one new Impact Strategy Assignment record is created. | |
| F1-18 | 2 new outcomes (A, B) → new strategy, tick A and B → on Review Your Strategy Records, tick A's assignment → **Remove this link** → **Save** | Outcomes `TEST-F1-18 Outcome A`, `TEST-F1-18 Outcome B`; Strategy `TEST-F1-18 Strategy` | Choosing **Remove this link** hides the outcome picker and shows a red warning. The flow returns to Review Your Strategy Records with only B's assignment. Untick → **Next**: Success lists B under TEST-F1-18 Strategy and A under "Outcomes not linked to a Strategy". 1 assignment remains; both outcomes and the strategy are kept. | |
| F1-19 | New outcome → TEST-Seed Strategy → on Review Your Strategy Records, tick the assignment → **Remove this link** → **Save** | Outcome `TEST-F1-19 Outcome` | Nothing is left to review, so the flow goes straight to Success, which lists the outcome under "Outcomes not linked to a Strategy". No assignment remains for TEST-F1-19 Outcome; TEST-Seed Strategy is unchanged. If you remove the only link to a strategy you created in this session, the strategy is kept but Success does not list it. | |

### Cancel

| # | Steps | Records you create (the flow removes them) | Expected | Result / Notes |
|---|---|---|---|---|
| F1-20 | 2 new outcomes with 1 activity each → Review: **Next** → **Cancel — delete all records from this session** | Outcomes `TEST-F1-20 Outcome A`/`B`; Activities `TEST-F1-20 Activity A`/`B` | The warning appears when Cancel is selected. "Session Cancelled" says 2 new outcomes were removed. All 4 records are gone. | |
| F1-21 | Existing TEST-Seed Outcome + 1 activity, and a new outcome + 1 activity → Cancel | Outcome `TEST-F1-21 Outcome`; Activities `TEST-F1-21 Existing Activity`, `TEST-F1-21 New Activity` | Only the new outcome is deleted. TEST-Seed Outcome and its F1-07 activity stay. Both F1-21 activities are deleted. | |
| F1-22 | New outcome → Review: **Next** → select **Cancel — delete all records from this session** → **Previous** → Review: **Next** → **Confirm — but no Strategy linking needed** | Outcome `TEST-F1-22 Outcome` | **Previous** returns to Review with no rows ticked. The outcome is kept, and Success lists it as not linked. | |
| F1-23 | Open the flow, then close the tab on Welcome | none | Nothing is created. Welcome has only **Let's Begin!**. | |

### Validation and errors

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| F1-24 | Click Next with a required pick left blank: existing outcome, Benefit on a Benefit row, existing strategy | none | "Please select a choice." | |
| F1-25 | Paste the 256-character string below as an outcome name, an activity description and a new strategy name | none (blocked) | "…255 characters or fewer." The help text says "Maximum 255 characters." | |
| F1-26 | On Link Outcomes to a Strategy, tick no outcome and click Next | Outcome `TEST-F1-26 Outcome` | "Select 1 or more rows." No strategy is created. | |
| F1-27 | **Previous** from the strategy screen | (same session as F1-26) | Back to **What would you like to do next?**, with the choice kept. **Previous** again returns to Review with no rows ticked. | |

256-character string for F1-25 (copy it all, on one line):

```
TEST-F1-25 kemubcrdlsbqgbcnnchcrnbsdhuusbssmbhbrejnerdsjrvfdssugldrwcsbtgpvrnykosoljhzfwyhcsjqpkxojtcdqnfykepnbvcyrszkkwltpszoccipwvcbxwjusvojwmvlaolftdpbgyjexhmmpcfomrienriwnlvmhecfehvhapsfijaenrltskewqtuvxboyvzrmmmmdpumbgcgofdktbdaserdltacgtmeuiltlpddpop
```

### Report

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| F1-28 | New outcome, activities **No** → close the tab on Review → Reports → Outcome Accelerator → **Outcomes Without Activities or Strategy** | Outcome `TEST-F1-28 Outcome` | TEST-F1-28 Outcome is listed. TEST-F1-01 Outcome is not listed, because it has an activity. | |

---

## Flow 2: Indicator Setup

The flow names some records for you:

- The Indicator Assignment is named `<Indicator name> Assignment`.
- Each Indicator Performance Period is named `<Indicator name> - <Frequency> - <Period>`.

So they also start with `TEST-`.

### Happy path

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| F2-01 | **Outcome Change** → TEST-Seed Outcome → **Create a New Indicator** `TEST-F2-01 Indicator` (Active, description "Survey <2030>") + **Create a New Unit of Measure** `TEST-F2-01 UOM` (code TESTF201) → **Annually**, 1 Jan 2031 – 31 Dec 2033 → periods: baseline 10 / target 20, then blank, then target −5 → **Confirm — save setup** | Indicator `TEST-F2-01 Indicator`; UOM `TEST-F2-01 UOM`; the assignment and 3 periods (auto-named) | Success lists everything, with `<2030>` shown as text. The UOM has Type Custom. The assignment has the Outcome only. The 3 periods are stored as entered. | |
| F2-02 | **Program Performance** → TEST-Seed Program 1 → new indicator `TEST-F2-02 Indicator` (Planned) + existing TEST-Seed UOM → **Quarterly** 2031 | Indicator `TEST-F2-02 Indicator`; the assignment and 4 periods | The assignment has the Program only. The summary says "Program". | |
| F2-03 | Outcome → TEST-Seed Outcome → **Use an Existing Indicator** → TEST-Seed Indicator → **Monthly**, 1 Jan – 31 Mar 2031 | `TEST-Seed Indicator Assignment` and 3 periods | No new indicator. 3 monthly periods. | |
| F2-04 | Program 2 → existing TEST-Seed Indicator → Monthly, 1 Jan – 15 Mar 2031 | `TEST-Seed Indicator Assignment` (on Program 2) and 3 periods | The March period ends on 15 Mar. The Jan and Feb Time Periods from F2-03 are re-used, not duplicated. | |

### Review, edit, cancel

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| F2-05 | TEST-Seed Outcome, new indicator `TEST-F2-05 Indicator` + new UOM `TEST-F2-05 UOM` (code TESTF205), Annually 2034 → on Review, rename them to `TEST-F2-05 Indicator Edited` / `TEST-F2-05 UOM Edited` → **Confirm — save setup** | Indicator `TEST-F2-05 Indicator Edited`; UOM `TEST-F2-05 UOM Edited`; the assignment and 1 period | The saved names match the Review screen. | |
| F2-06 | Program 1, new indicator `TEST-F2-06 Indicator` (existing UOM), Quarterly 2035 → on Review, rename it to `TEST-F2-06 Indicator Edited` → **Edit reporting periods** → change to Annually 2035 → **Confirm — save setup** | Indicator `TEST-F2-06 Indicator Edited`; the assignment and 1 period | The frequency and dates are pre-filled. The Review edit is kept. 1 annual period. | |
| F2-07 | Any setup (`TEST-F2-07 Indicator`) → **Cancel — nothing has been saved** | none | "Setup has been cancelled. Nothing was saved." | |
| F2-08 | Fresh run → Periods screen | none | No frequency is selected. The dates are 1 Jan – 31 Dec of the current year. | |

### Duplicates

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| F2-09 | Existing TEST-Seed Indicator on TEST-Seed Outcome again (already assigned in F2-03) → Define: **Next**. Select **Go back and choose a different indicator** → **Next**. On Define, **Next** again → leave **Add new reporting periods to the existing assignment** → **Next** → Annually 2036 → **Confirm — save setup** | Period `TEST-Seed Indicator - Annually - 2036` on the existing assignment; Time Period 2036 | The **Indicator Already Assigned** screen shows straight after Define, before Periods, and lists the 3 Jan–Mar 2031 periods. **Go back** returns to Define. Review shows "Adding reporting periods to the existing assignment". Success says the periods were added to the existing assignment and lists only the 2036 period. TEST-Seed Outcome still has only one TEST-Seed Indicator Assignment. | |
| F2-10 | After F2-09: existing TEST-Seed Indicator on TEST-Seed Outcome → **Add new reporting periods…** → Monthly 1 Jan – 28 Feb 2031 → **Confirm — save setup**. Then **Previous** → **Edit reporting periods** → Annually 2031 → **Confirm — save setup** | none | **Reporting Periods Overlap** lists the Jan and Feb 2031 periods and explains that periods on one assignment can't overlap. **Previous** returns to Review. With Annually 2031 it lists all 3 Jan–Mar periods: an overlap is blocked whatever the frequency. Nothing is saved. | |
| F2-11 | TEST-Seed Outcome → new indicator `TEST-F2-11 Indicator` + existing UOM → Annually 2037. On Review, switch to **Use an Existing Indicator** → TEST-Seed Indicator → **Confirm — save setup** | none | The save-time duplicate assignment screen shows (a backstop for changes made on Review). **Previous** returns to Review. No `TEST-F2-11 Indicator` and no 2037 Time Period. | |
| F2-12 | As F2-09 on TEST-Seed Program 2 (assigned in F2-04), with Quarterly 1 Jan – 31 Dec 2036 | 4 periods `TEST-Seed Indicator - Quarterly - …2036` on the existing assignment; Time Periods Q1–Q4 2036 | As F2-09. The already-assigned screen lists the 3 F2-04 periods (March ends on 15 Mar), and Success lists only the 4 quarters. | |
| F2-13 | New indicator named `TEST-F2-01 Indicator` (already exists) + new UOM `TEST-F2-13 UOM` → Define: **Next** | none | **Indicator Name Already Used** shows straight after Define, before Periods. **Previous** returns to Define. `TEST-F2-13 UOM` is **not** created. | |
| F2-14 | New indicator `TEST-F2-14 Indicator` + new UOM `TEST-F2-14 UOM` → Annually 2038. On Review, rename the indicator to `TEST-F2-01 Indicator` → **Confirm — save setup** | none | The save-time duplicate indicator screen shows (a backstop for changes made on Review). **Previous** returns to Review. No `TEST-F2-14 UOM` and no 2038 Time Period. | |

### Validation

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| F2-15 | Indicator name of 256 characters; description of 1001 characters; UOM name, code or description of 256 characters (on Define and on Review) | none | Inline length messages. The help text states the limits. | |
| F2-16 | End date before the start date | none | "Enter an end date on or after the start date that creates no more than 50 reporting periods." | |
| F2-17 | New indicator `TEST-F2-17 Indicator` (existing UOM), TEST-Seed Outcome, Monthly Jan 2031 – Mar 2035 → blocked. Change the end date to 28 Feb 2035 → click Next through the 50 period screens → **Confirm — save setup** | Indicator `TEST-F2-17 Indicator`; the assignment and 50 periods | 51 periods are blocked, and 50 are saved. | |
| F2-18 | Leave required fields blank: type, parent, indicator choice, new-indicator fields, frequency | none | The standard required-field message. | |
| F2-19 | Baseline description of 1001 characters on a period screen | none | "…1000 characters or fewer." | |

### Error and rollback

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| F2-20 | Run the Outcome Strategy Builder to create outcome `TEST-F2-20 Outcome` (activities No, no strategy). In Indicator Setup, choose that outcome, new indicator `TEST-F2-20 Indicator` + new UOM `TEST-F2-20 UOM`, Annually 2041. On Review, delete TEST-F2-20 Outcome in another tab, then **Confirm — save setup** | Outcome `TEST-F2-20 Outcome` (you delete it) | An error (ENTITY_IS_DELETED) is shown. No TEST-F2-20 indicator or UOM, and no 2041 Time Period. **Previous** returns to Review. | |

---

## Access (optional, as a non-admin user)

| # | Steps | Records you create | Expected | Result / Notes |
|---|---|---|---|---|
| A-01 | Log in as a Standard User with only these: the Outcome Management and Program Management permission set licences, the **Outcome Management** and **Advanced Program Management** permission sets, the `Outcome_Accelerator_User` permission set from `test-support/`, and Run Flows. Repeat F1-01 and F2-01 with the prefix `TEST-NA` | Same as F1-01 and F2-01, named `TEST-NA…` | No "Insufficient Privileges". The pick lists are filled. Unit of Measure sharing is Private by default, so "existing UOM" shows only the user's own units. | |

---

## Records to delete after testing

In each object's list view, filter on **Name starts with** `TEST-` and delete. Delete from the top down, children first:

| Object | Names |
|---|---|
| Indicator Performance Period | All starting `TEST-` (auto-named from the indicator) |
| Indicator Assignment | All starting `TEST-` |
| Indicator Definition | All starting `TEST-` |
| Impact Strategy Assignment | Auto-numbered (ISA-…). Delete the ones created on your test dates. |
| Outcome Activity | All starting `TEST-` |
| Outcome | All starting `TEST-` |
| Impact Strategy | All starting `TEST-` |
| Benefit | `TEST-Seed Benefit` |
| Benefit Type | `TEST-Seed Benefit Type` (after the Benefit, before the Unit of Measure) |
| Unit of Measure | All starting `TEST-` |
| Program | `TEST-Seed Program 1`, `TEST-Seed Program 2` (and any Program Recommendation Rule the org creates) |
| Goal Definition | `TEST-Seed Goal` |
| **Time Period** | **Not** `TEST-` **named.** The flow names them by date ("2031", "Q1 2031", "January 2031" …). Delete only the Time Periods created on your test dates, and check that no real Indicator Performance Periods use them first. |
