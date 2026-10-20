![Public Sector Accelerators logo](/docs/Logo_GPSAccelerators_v01.png)

# Outcome Builder

Guided Screen Flows that help program teams set up Outcome Management: outcomes, outcome activities, impact strategies, indicators and their reporting periods. Instead of creating each record by hand on separate objects, users follow a step-by-step wizard that creates and links the records for them, checks for duplicates, and shows a summary of what was set up.

The accelerator is built from standard Flow elements only. There is no Apex, no Lightning Web Components and no custom objects or fields, so admins can open, change and maintain everything in Flow Builder.

Accelerator Listing: [insert url to the public listing on the Accelerator site](https://gpsaccelerators.developer.salesforce.com/) (tbd once published)


## Description

Outcome Management gives organisations a standard data model for measuring impact. Setting it up by hand means working across many related objects in the right order: Outcome, Outcome Activity, Impact Strategy, Impact Strategy Assignment, Indicator Definition, Unit of Measure, Indicator Assignment, Time Period and Indicator Performance Period. It is easy to miss a link or create a duplicate.

This accelerator provides two guided flows:

- **Outcome Strategy Builder** lets users:
  - define new outcomes or select existing ones
  - link the outcomes to Programs, Benefits or Goal Definitions as Outcome Activities
  - group them under a new or existing Impact Strategy

  A Review screen lets users edit what they created, change or remove strategy links, and confirm. A summary of the Strategy → Outcome → Activity hierarchy is shown at the end.
- **Indicator Setup** sets up how an outcome or program is measured:
  - creates an Indicator Definition and its Unit of Measure, or reuses existing ones
  - assigns the indicator to an Outcome or Program
  - creates one Indicator Performance Period for each reporting period (monthly, quarterly or annually), with optional baseline and target values

  Nothing is saved until the user confirms, and then the setup is saved all or nothing.

Both flows check for problems early, while the user can still fix them:

- **Indicator name already used:** reported straight after the Define step.
- **Indicator already assigned to the record:** the user can add new reporting periods to the existing assignment instead of creating a duplicate.
- **New periods overlap the existing ones:** blocked before anything is saved.
- **Outcome already linked to the strategy:** skipped, and the user is told why.


## Key Assets

This accelerator is an **unmanaged package (metadata in `/force-app/main/default/`)** containing:

| Asset type | Count | Assets |
|---|---|---|
| Screen Flow (template) | 2 | `Outcome_Strategy_Builder`, `Indicator_Setup` |
| Autolaunched Flow (subflow) | 6 | `Outcome_Strategy_Create_Strategy_Assignments`, `Outcome_Strategy_Clean_Up_Session`, `Outcome_Strategy_Build_Session_Summary`, `Indicator_Setup_Create_Indicator_Definition`, `Indicator_Setup_Create_Indicator_Assignment`, `Indicator_Setup_Create_Performance_Periods` |
| Report Type | 1 | `Outcome_Accelerator_Outcomes` |
| Report folder and Report | 1 | `Outcome_Accelerator/Outcomes_Without_Activities_Or_Strategy` |

Also in the repository, not part of the package:

- `/docs/Test-Cases.md`: test cases to run in your sandbox after you install, with the test data to create first.

**Documentation:**

- This README.
- `docs/Test-Cases.md`.
- The description on every flow and flow element, viewable in Flow Builder.

### Flow naming conventions

Every element, resource and screen component in the flows starts with a prefix that shows what it is. When you open a flow in Flow Builder, use this key to see what each name refers to.

| Prefix | What it is | Example |
|---|---|---|
| `scr` | Screen | `scrIndicatorDef` |
| `disp` | Display Text component on a screen | `dispReviewAddToExistingNote` |
| `inp` | Input component on a screen (text, picklist, radio buttons, long text) | `inpIndicatorName` |
| `dt` | Data Table component on a screen | `dtExistingAssignmentPeriods` |
| `repeater` | Repeater component on a screen | `repeaterActivities` |
| `scr…_Section1` | Section on a screen, named after its screen | `scrReview_Section1` |
| `scr…_Section1_Column1` | Column inside that section | `scrReview_Section1_Column1` |
| `dec` | Decision | `decIndicatorNameTaken` |
| `asg` | Assignment | `asgSetAddToExistingAssignment` |
| `loop` | Loop | `loopOverlapTimePeriods` |
| `flt` | Collection Filter | `fltMatchingTimePeriod` |
| `get` | Get Records | `getExistingAssignmentPeriods` |
| `rc` | Create Records | `rcCreateIndicatorDefinition` |
| `ru` | Update Records | `ruEditedActivities` |
| `rd` | Delete Records | `rdDeleteActivities` |
| `rbk` | Roll Back Records | `rbkRollBackBeforeDuplicate` |
| `sub` | Subflow | `subCreateIndicatorAssignment` |
| `var` | Variable holding a single value | `varAddingToExistingAssignment` |
| `rec` | Record variable holding a single record | `recExistingAssignment` |
| `col` | Collection variable holding a list of values or records | `colOverlapTimePeriodIds` |
| `in` | Input variable that a subflow receives from the flow that calls it | `inStagedPeriods` |
| `out` | Output variable that a subflow returns to the flow that calls it | `outDuplicateFound` |
| `frm` | Formula | `frmSuccessIntro` |
| `choice` | Choice, a fixed option on a radio button or picklist | `choiceAddToExistingAssignment` |
| `dcs` | Choice set filled from records (record or picklist choice set) | `dcsExistingIndicators` |

Collection Filter and Loop elements also create their own `currentItem_…` variables, named automatically by Flow Builder.

## Before You Install
- **Turn on Outcome Management and Program and Benefit Management** in Setup (**Outcome Management Settings** and **Program and Benefit Management Settings**). Until both are on, objects such as Outcome, Impact Strategy and Program don't exist, and the installation fails. Once turned on, these features **cannot be turned off**.
- **Assign the permission sets.** You, as the installing admin, and every user who runs the flows need the **Outcome Management** and **Program Management** permission set licences, with the **Outcome Management** and **Advanced Program Management** permission sets. Confirm the exact licences for your edition in [Outcome Management editions, security and permissions](https://help.salesforce.com/s/articleView?id=ind.outcome_management_editions_security_permissions.htm&type=5).


**License Requirements**
A Salesforce org with one of these licences, all of which include Outcome Management:

- Nonprofit Cloud
- Education Cloud
- Public Sector Solutions
- Net Zero Cloud - please confirm the edition you  have offers Outcome Management data model as trial
- Energy and Utilities Cloud

**Accelerator or Technology-Specific Assumptions** 
- **Programs must be Active for the Outcome Strategy Builder.**
  - When linking activities, the Builder only lists Programs with Status **Active**.
  - When linking an existing strategy, it only lists Active Impact Strategies.
  - Indicator Setup lists all Programs.
- **Units of Measure are private by default.** The organisation-wide default for Unit of Measure is Private, so users only see the units they own. To let users pick units created by others, set the default to Public Read Only or share the units.
- **One reporting cycle per indicator assignment.**
  - Indicator Setup doesn't let reporting periods on the same assignment overlap, even with a different frequency, so reports that total targets or results across periods don't count them twice.
  - Plan each indicator's frequency up front.
  - To see annual figures, group monthly or quarterly periods by year in your reports.
- **Time zones.** Time Periods are matched on their start and end dates. Time Periods created by users in a time zone ahead of UTC (for example Sydney) may not be matched, so a second Time Period with the same dates can be created.

**General Assumptions**
- Install the accelerator in a sandbox or test environment first and test it thoroughly before using it in production. 
- The accelerator is designed for **Lightning Experience**. It is not supported in Salesforce Classic.
- The flows are provided as a starting point. Review the screen text, choices and logic against your organisation's processes, and change them in Flow Builder as needed.


## Installation

1. Install the accelerator in one of two ways:
   - **Install the package (version 1.0).** Log in to your sandbox, then open the sandbox install link: [Install in a sandbox](https://test.salesforce.com/packaging/installPackage.apexp?p0=04tQE00000ig3bF). Choose **Install for Admins Only**, then click **Install**. When you're ready for production, use the same link with `login.salesforce.com` in place of `test.salesforce.com`.
   - **Or deploy the metadata.** Download or clone this repository and deploy the `force-app` folder to your org with your usual tools. This repository always has the latest version of the accelerator.

   > **If the install link doesn't work,** the package may no longer be available. Deploy the metadata from this repository instead. Nothing else changes: the components, setup steps and test cases are the same.
2. Activate the flows in Setup → **Flows**: activate the six subflows first, then `Outcome_Strategy_Builder` and `Indicator_Setup`. Neither screen flow can run until all of its subflows are active.
3. (Optional) Create your own copy from a template. Both screen flows are marked as **templates**. To make your own version without changing the originals, go to Setup → **Flows** → **New Flow**, choose **Start from a template**, and pick **Outcome Strategy Builder** or **Indicator Setup**. Your copies call the subflows by API name, so keep the subflows installed and active.


## Post-Install Setup & Configuration

1. **Give users access.** For each user who will run the flows:
   1. Assign the **Outcome Management** and **Advanced Program Management** permission sets, with their permission set licences.
      - These give the Manage Outcome Management and Manage Program Management user permissions.
      - Without them, the Outcome and Program picklists in the flows are empty, even when the user has object access.
      - Confirm the exact licences for your edition in [Outcome Management editions, security and permissions](https://help.salesforce.com/s/articleView?id=ind.outcome_management_editions_security_permissions.htm&type=5).
   2. Give the **Run Flows** user permission, or the **Flow User** checkbox on the user record.
2. **Add the flows where users work.** For example:
   - add them to a Lightning app or home page with the Flow component
   - add them to the utility bar
   - add them as quick actions
3. **(Optional) Flow access settings.** The flows do not use **Override default behavior and restrict access to enabled profiles or permission sets**. If you turn that setting on, give each user's profile or permission set access to both flows. Otherwise every user, System Administrators included, sees "Insufficient Privileges".
4. **Plan the clean-up of abandoned sessions.**
   - The Outcome Strategy Builder saves outcomes and activities as the user goes. Choosing **Cancel** on the **What would you like to do next?** screen removes them. Closing the window part-way through keeps them.
   - Decide who runs the **Outcomes Without Activities or Strategy** report (in the **Outcome Builder Accelerator** report folder), and how often. Review the results before deleting anything.
5. **Test as a real user.** Run each flow once as a user with only the permissions above, not as a System Administrator. You can use `docs/Test-Cases.md` in this GitHub repository to see the tests you can run.


## FAQs

***Q: Does the accelerator include any Apex, Lightning Web Components or custom objects?***

A: No. It uses standard Flow elements and standard Outcome Management and Program Management objects only, so you can change everything in Flow Builder.

***Q: The install link shows an error saying the package can't be found or isn't available. What should I do?***

A: The package version behind the link may no longer be available. Download or clone this repository and deploy the `force-app` folder to your org with your usual tools. It contains the same components as the package.

***Q: The installation or deploy fails with "no CustomObject named … found". What's wrong?***

A: Outcome Management and Program and Benefit Management are not both turned on, or the installing or deploying user does not have the Outcome Management and Program Management permission set licences and permission sets. Turn on both features and assign the licences, then try again.

***Q: The Outcome or Program picklists in the flow are empty. Why?***

A: The user is missing the **Outcome Management** or **Advanced Program Management** permission set, or its permission set licence. Object access alone is not enough. For the Outcome Strategy Builder, also check that the Programs have Status **Active**.

***Q: What happens if a user closes Indicator Setup part-way through?***

A: Nothing is saved. Indicator Setup only saves when the user chooses **Confirm — save setup** on the Review screen. If anything fails at that point, every record created in that step is rolled back.

***Q: What happens if a user closes the Outcome Strategy Builder part-way through?***

A: The outcomes and activities created so far are kept, because the Builder saves as the user goes. To find these records, use the **Outcomes Without Activities or Strategy** report. Choosing **Cancel** at the end of a session removes the records created in that session instead.

***Q: Can I add next year's reporting periods to an indicator that is already assigned?***

A: Yes. In Indicator Setup, choose the existing indicator and the same outcome or program. The flow shows the periods the assignment already has and offers to add new ones. Periods that overlap the existing ones are blocked.

***Q: Why can't I add monthly periods to an indicator that already has an annual period for the same year?***

A: Each indicator assignment uses one set of reporting periods that don't overlap, so targets and results aren't counted twice. Indicator Setup blocks any overlap, including one between different frequencies such as Annual 2031 and Monthly January 2031.

If an annual target and monthly periods overlap, any report that totals the periods counts them twice. For example, an annual target of 120 and monthly targets of 10 add up to 240. Results that fall in both periods are also counted twice, and users can't tell which period counts.

To see annual figures, use monthly or quarterly periods and group them by year in your reports. If your organisation's reporting needs overlapping periods, you can change this rule in your copy of the flow in Flow Builder.

***Q: Why do I see two Time Periods with the same dates?***

A: Time Periods created by users in a time zone ahead of UTC are stored with a different time, so Indicator Setup may not match them and creates a new one. Check for duplicates after the first few setups if you have users in those time zones.

***Q: Users can't see a Unit of Measure that a colleague created. Why?***

A: The Unit of Measure organisation-wide default is Private. Set it to Public Read Only, or share the units with the users who need them.

## Additional Resources

- [Outcome Management (Salesforce Help)](https://help.salesforce.com/s/articleView?id=ind.outcome_management.htm&type=5)
- [Outcome Management editions, security and permissions (Salesforce Help)](https://help.salesforce.com/s/articleView?id=ind.outcome_management_editions_security_permissions.htm&type=5)
- [Flow Builder (Salesforce Help)](https://help.salesforce.com/s/articleView?id=platform.flow.htm&type=5)
- [Nonprofit Cloud Developer Guide](https://developer.salesforce.com/docs/atlas.en-us.nonprofit_cloud.meta/nonprofit_cloud/npc_intro.htm)


## Revision History

<strong>1.0 Initial release (2026)</strong> - Outcome Strategy Builder and Indicator Setup screen flows (marked as flow templates), six supporting subflows, and the abandoned-session clean-up report.


## Acknowledgements

Special thanks to **Meg Gray**, Nonprofit Product Management Director at Salesforce, for her guidance while this accelerator was being designed. Her feedback on the idea and the overall flow made sure it fits the Outcome Management data model as intended.

Thank you also to everyone who helped test the accelerator:

- Meg Gray, Product Management Director, Salesforce
- Hayley Morris, Principal Technical Architect, Salesforce
- David Aitken, SSI (Settlement Services International)
- Stevie Hight, Victim Support: Manaaki Tāngata
- Daniel Howell, GravityLab


## Terms of Use

Thank you for using Global Public Sector (GPS) Accelerators.  Accelerators are provided by Salesforce.com, Inc., located at 1 Market Street, San Francisco, CA 94105, United States.

By using this site and these accelerators, you are agreeing to these terms. Please read them carefully.

Accelerators are not supported by Salesforce, they are supplied as-is, and are meant to be a starting point for your organization. Salesforce is not liable for the use of accelerators.

For more about the Accelerator program, visit: [https://gpsaccelerators.developer.salesforce.com/](https://gpsaccelerators.developer.salesforce.com/)
