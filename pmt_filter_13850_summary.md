# PMT Filter 13850 — Work Summary & Learnings

_Source: Jira filter [13850](https://skeps.atlassian.net/issues?filter=13850) · 200 tickets · Products: AF (Auto Finance / Partner Direct / Lyon), UCL, PF · Compiled 2026-07-23_

Theme-organized summary built from each ticket's summary + description, grouped by functional area, ordered oldest → newest within each area to show how the logic evolved over time.

**Contents**

- [Underwriting / Credit Policy](#underwriting--credit-policy) (20)
- [Pricing / Offer Configuration](#pricing--offer-configuration) (20)
- [Funding / Payments / Change Orders](#funding--payments--change-orders) (27)
- [DocuSign / Loan Agreement](#docusign--loan-agreement) (6)
- [Merchant / Partner Portal](#merchant--partner-portal) (29)
- [Customer Journey / Borrower UX](#customer-journey--borrower-ux) (22)
- [Verbiage / UI Copy](#verbiage--ui-copy) (19)
- [Notifications (Email/SMS)](#notifications-emailsms) (10)
- [API / Integrations](#api--integrations) (16)
- [Data / DWH](#data--dwh) (6)
- [UAT Feedback / Fixes](#uat-feedback--fixes) (7)
- [Other](#other) (18)

## Underwriting / Credit Policy (20)

- **[PMT-2996](https://skeps.atlassian.net/browse/PMT-2996)** — AF: Late Fees | SD-1668 | SD-1670. Note: “Late fee” and “SAC to Non-SAC” will be based on evaluation
- **[PMT-3162](https://skeps.atlassian.net/browse/PMT-3162)** — AF: Late Fees - Existing Application Migration. After the Late Fee release, this deployment will take care of existing applications.
- **[PMT-3489](https://skeps.atlassian.net/browse/PMT-3489)** — AF: Update Decline Reason and Decline Code for Credit history check | SD-1906. Credit history check and Credit history check - Co-borrower have:
- **[PMT-3648](https://skeps.atlassian.net/browse/PMT-3648)** — AF - AAN update | SD-1889. The new information in adverse action notice.
- **[PMT-4041](https://skeps.atlassian.net/browse/PMT-4041)** — AF - Update of Disposable Income Check Criteria | SD-1975. To alleviate compliance concerns and increase approval rates in low-risk segments, the disposable income decline
- **[PMT-4378](https://skeps.atlassian.net/browse/PMT-4378)** — UCL - Alter decline screen to say email in 1-3 days instead of letter mailed.. Change the verbiage of the decline screen (mobile and web) and mention 1-3 business days instead of 7-10 business days.
- **[PMT-4380](https://skeps.atlassian.net/browse/PMT-4380)** — AF: Update 40-I and 30-A logic. We have to use `(rh_prin - rh_misc)` instead of `(rh_total_pay - rh_misc)` in rtxn_prin_amt_n parameter for 40-I. For corresponding 30-A, we have to use this updated logic `(rh_prin - rh_misc).`
- **[PMT-4411](https://skeps.atlassian.net/browse/PMT-4411)** — AF: Create new Decline Code for Custom Decline and Amsive | SD-2021 part1. AF team wants to decline any pending application using a particular decline code and reason. It will not be part of the Decline Waterfall but only required for manual decline.
- **[PMT-4575](https://skeps.atlassian.net/browse/PMT-4575)** — AF - Revising HIL Partner Direct Max Loan Amount and FICO Bands | SD-2049. Updating Pricing Config for PD.
- **[PMT-5177](https://skeps.atlassian.net/browse/PMT-5177)** — AF: Update 40-C, 40-R and 30-A to pass additional field | SD-2071. We have to pass field rtxn_addl_desc with value SKEPS_DRAW in all our 40-C, 40-R, and 30-A calls except during 30-A calls during Refunds.
- **[PMT-5314](https://skeps.atlassian.net/browse/PMT-5314)** — AF: Late Fee Policy Update. Slice-HIL is coming up with new late fee policy where they are removing the $30.00 cap on the late fee. The policy will be applied to any new underwriting (evaluation date = release date) after the policy goes live on...
- **[PMT-5350](https://skeps.atlassian.net/browse/PMT-5350)** — AF: Decline Waterfall Updates | SD-2091. Updated Length of Mortgage Experience to include HI20S
- **[PMT-5384](https://skeps.atlassian.net/browse/PMT-5384)** — UCL - Income calculator/Manual review/ Decline. Income calculator tab - Give Lender the capability to Verify stated income of the customer.
- **[PMT-5597](https://skeps.atlassian.net/browse/PMT-5597)** — AF: Add address check logic in Home Ownership Check. We found one case where we will have to update the logic for home ownership check. Here is the sequence of events:
- **[PMT-5765](https://skeps.atlassian.net/browse/PMT-5765)** — AF: Update Decline Waterfall | SD-2153. Underwriting Impact:
- **[PMT-6291](https://skeps.atlassian.net/browse/PMT-6291)** — UCL - logic change for the report date shown in the decline mails. context from Manish~~  - Logic was updated by Prakhar
- **[PMT-6506](https://skeps.atlassian.net/browse/PMT-6506)** — AF: Update Decline Waterfall for 360 Finance usecase (API Merchants) | SD-2283. Updated Decline Waterfall with API Merchants logic:
- **[PMT-6783](https://skeps.atlassian.net/browse/PMT-6783)** — AF: Created underwriting check: FICO < 560 | SD-2363. Created check “FICO < 560”
- **[PMT-6820](https://skeps.atlassian.net/browse/PMT-6820)** — AF: Update three underwriting checks | SD-2378. User Story 1:
- **[PMT-7666](https://skeps.atlassian.net/browse/PMT-7666)** — UCL - consume new fields from credit check API and send this again in credit check api | SD-2401. At present, when a customer isn’t pre-approved, the risk tiers are created using BAU (risktiergroup). UCL team is working with Provenir to perform an A/B testing where we also start getting risk tiers based on v-score...

## Pricing / Offer Configuration (20)

- **[PMT-2817](https://skeps.atlassian.net/browse/PMT-2817)** — AF: Update Custom Config 5. All MDR’s in Custom Pricing Policy 5 will be reduced by 50 bps.
- **[PMT-2892](https://skeps.atlassian.net/browse/PMT-2892)** — AF: Fix "Fixed Rate APRs" and "Show Offers and Fees Table" for Custom Config | SD-1589. Issue1:
- **[PMT-3066](https://skeps.atlassian.net/browse/PMT-3066)** — AF: Fix how we create Usury offers. As a sample, we create two offers like this for a 640 - 659 fico band:
- **[PMT-3209](https://skeps.atlassian.net/browse/PMT-3209)** — AF: Offer Screen SAC UI Update | SD-1724.
- **[PMT-3248](https://skeps.atlassian.net/browse/PMT-3248)** — AF: Update Custom Config 3 and 4 | SD-1859.
- **[PMT-3259](https://skeps.atlassian.net/browse/PMT-3259)** — AF: CR: Create Custom Config 7 for Skeps Merchant. Req1: Create the Config
- **[PMT-3750](https://skeps.atlassian.net/browse/PMT-3750)** — CR: AF: Update the background color of offer tiles. Update the background color of offer tiles on offer screen.
- **[PMT-3830](https://skeps.atlassian.net/browse/PMT-3830)** — AF - selected offer config and pricing policy effective date in a new file to FNBO-HIL. Consume the new fields and send them to EXL in the daily files.
- **[PMT-3866](https://skeps.atlassian.net/browse/PMT-3866)** — AF: Add 12.99% Fixed Offer to Custom Pricing 4 | SD-1928. Existing Fixed Rates for Custom Config 4 are: 8.99, 9.99, 10.99, 11.99
- **[PMT-4319](https://skeps.atlassian.net/browse/PMT-4319)** — AF - add the UW metrics files in the config so that they can be sent to FNBO on a daily basis. Files: \['hil_application_assignment.txt','hil_application_status.txt','hil_document_lineage.txt','hil_application_assignment.abc','hil_application_status.abc','hil_document_lineage.abc'\]
- **[PMT-4537](https://skeps.atlassian.net/browse/PMT-4537)** — AF - change request: add another status to allow offer term change in the lender portal. one of the status to update the offer terms from the lender portal were missed. it needs to be added.
- **[PMT-5392](https://skeps.atlassian.net/browse/PMT-5392)** — UCL - Offers tab in Application Details Page. Summary
- **[PMT-5655](https://skeps.atlassian.net/browse/PMT-5655)** — UCL - Changes in the offer screen. Customers get a letter / email that they can get approved for $XX for APR as low as say 9.99%. When they see the first offer, which is of longest term, they find APR of 15.99%. A lot of customers may get turn-off at t...
- **[PMT-5684](https://skeps.atlassian.net/browse/PMT-5684)** — AF: Lyon - Reduce APR by 0.50%. Testing Scope:
- **[PMT-6095](https://skeps.atlassian.net/browse/PMT-6095)** — AF: Custom Offer Configuration 8. Requirement 1:
- **[PMT-6492](https://skeps.atlassian.net/browse/PMT-6492)** — AF: Create Custom Config 9.
- **[PMT-6507](https://skeps.atlassian.net/browse/PMT-6507)** — AF: Create new pricing for 360 Finance. We have to set it up for the 360 Fin API merchant
- **[PMT-6619](https://skeps.atlassian.net/browse/PMT-6619)** — AF: Fix the pricing issue for usury which is creating double offers. Updated usury min terms in following pricing:
- **[PMT-6721](https://skeps.atlassian.net/browse/PMT-6721)** — UCL - UI changes in the offer screens. Objective: UCL team wants to highlight the offers with lowest apr and lowest payment amount tags. these tags were already present on the new screen created during the A/B testing. we need to add them to the original s...
- **[PMT-6937](https://skeps.atlassian.net/browse/PMT-6937)** — AF: Move pricing to DB.

## Funding / Payments / Change Orders (27)

- **[PMT-2852](https://skeps.atlassian.net/browse/PMT-2852)** — CR: AF: Dynamically change DMF when Amount Financed is changed. Existing feature:
- **[PMT-2889](https://skeps.atlassian.net/browse/PMT-2889)** — AF: Move minimum monthly payment criteria to merchant level | SD-1568. Minimum monthly payment is set as $99 for complete AF Universe
- **[PMT-3096](https://skeps.atlassian.net/browse/PMT-3096)** — AF: Update SAC MDRs for PD Configs | SD-1742.
- **[PMT-3208](https://skeps.atlassian.net/browse/PMT-3208)** — AF: Close Funding UI Update | SD-1794.
- **[PMT-3798](https://skeps.atlassian.net/browse/PMT-3798)** — CR: AF: Dynamically change DMF when Amount Financed is changed. When Amount Financed is changed, we change DMF as per below logic.
- **[PMT-3929](https://skeps.atlassian.net/browse/PMT-3929)** — AF: Platform level API update for 1andFund. The APIs supported on v2 are published here:
- **[PMT-4246](https://skeps.atlassian.net/browse/PMT-4246)** — AF - revert the changes made for NACHA file delay deployed in the April release. There were changes made for the NACHA file delay for the Amerifirst (PD and Lyon). There was an impact on the Lyon side and now, AF wants to revert the changes. Please revert the changes
- **[PMT-4295](https://skeps.atlassian.net/browse/PMT-4295)** — AF - Show Funding Request Canceled when Change order is called after initiate funding | SD-2056. 1. Add new status “Funding Request Canceled“
- **[PMT-4621](https://skeps.atlassian.net/browse/PMT-4621)** — AF - show the change order activity statuses in the portals | SD-2059.
- **[PMT-4659](https://skeps.atlassian.net/browse/PMT-4659)** — UCL - UCL Debt Consolidation Payment Overage Limits | SD-2057. In cases of consolidaited loan, earlier, system didn’t have a cap on the loan amount requested against an individual balance. this allowed the customers to game the system by requesting any amount and then overpay the...
- **[PMT-4660](https://skeps.atlassian.net/browse/PMT-4660)** — UCL - capture the account type (savings/checking) and send this information in the NACHA file | SD-2073. This is the logic to be used for Entry Detail Record (Type 6) in Nacha File
- **[PMT-4705](https://skeps.atlassian.net/browse/PMT-4705)** — AF - merchant should be able to intiate funding and change order on Funding request cancled. In case of funding request canceled status, the merchant should be able to initiate funding request and change order request for the portal. the behavior should be same as Funding request expired and Funding request r...
- **[PMT-4857](https://skeps.atlassian.net/browse/PMT-4857)** — AF - change DMF to Lyon Project Fee in the customer journey | SD-2066. Change the “Disbursement Management Fee” to “Lyon Project Fee” in the customer journey. This needs to be replaced in all of the screens and Emails.
- **[PMT-4909](https://skeps.atlassian.net/browse/PMT-4909)** — AF - funding report - column name change - show store name instead of merchant.
- **[PMT-5033](https://skeps.atlassian.net/browse/PMT-5033)** — AF - for L3 merchant users which don't have the refund permission, give them the permission. Some of the merchant L3 users don’t have the refund permissions. We need to
- **[PMT-5353](https://skeps.atlassian.net/browse/PMT-5353)** — AF: Support Funding Request SMS | SD-2123. Requirement:
- **[PMT-5378](https://skeps.atlassian.net/browse/PMT-5378)** — UCL - Disbursement tab in FNBO UCL Lender portal. Problem Summary
- **[PMT-5766](https://skeps.atlassian.net/browse/PMT-5766)** — AF - funding journey updates - part 2 | SD-2168. Emails
- **[PMT-5861](https://skeps.atlassian.net/browse/PMT-5861)** — UCL - Edit Disbursement details functionality. This is an extension of
- **[PMT-5937](https://skeps.atlassian.net/browse/PMT-5937)** — UCL - Pending approval/Funding approved in Applications listing. Please find screen designed below.
- **[PMT-6277](https://skeps.atlassian.net/browse/PMT-6277)** — AF: CR - Pass Loan Agremeent Amount in Single Funding Request Email. Trigger: When PD merchant requests funding and customer receives the email
- **[PMT-6587](https://skeps.atlassian.net/browse/PMT-6587)** — AF: CR: Update the formulae for the max_payment field in Pagaya API. Existing Formulae:
- **[PMT-6727](https://skeps.atlassian.net/browse/PMT-6727)** — AF: Support Flexible Funding Configuration (MANUAL). User Story 1:
- **[PMT-6792](https://skeps.atlassian.net/browse/PMT-6792)** — UCL - allow PO box in the street address field in the journey and disbursement tab. In our platform, there is only one address field definition and validation as documented in:
- **[PMT-6867](https://skeps.atlassian.net/browse/PMT-6867)** — AF: Update Change Order TILA sentence | SD-2394. Pain Point 1:
- **[PMT-6955](https://skeps.atlassian.net/browse/PMT-6955)** — UCL - allow monthly housing payment edit from the lender portal | SD-2332. UCL team wants to be able to edit the housing payment value in the lender portal.
- **[PMT-7604](https://skeps.atlassian.net/browse/PMT-7604)** — AF: Create a separate NACHA file for Pagaya. Scope: Both Funding NACHA File and Refund NACHA File

## DocuSign / Loan Agreement (6)

- **[PMT-3265](https://skeps.atlassian.net/browse/PMT-3265)** — AF: FNBO's DocuSign Urgent Compliance Requirement | SD-1850 | SD-1851. We have to ensure that following sections start from new pages:
- **[PMT-4859](https://skeps.atlassian.net/browse/PMT-4859)** — AF - make changes to the signed loan agreement email | SD-2105. Make verbiage changes to signed loan agreement email of PD. We will need extra parameter to change verbiage based on single funding vs multi-funding. I have used multifunding (values: 0 or 1) for the logic. We can upd...
- **[PMT-5119](https://skeps.atlassian.net/browse/PMT-5119)** — AF - docusign iframe enhancements. Improve the docusign experience for the customer
- **[PMT-5470](https://skeps.atlassian.net/browse/PMT-5470)** — UCL - UI changes in the customer journey + loan agreement | SD-2142. there are certain UI + verbiage changes in the customer journey that UCL team wants to make. attached is the latest ppt for the same.
- **[PMT-6784](https://skeps.atlassian.net/browse/PMT-6784)** — AF: Update the value shown on Loan Agreement Amount | SD-2370. There are 3 screens in the customer journey where we are showing the field name as Loan Agreement Amount, but we were not always showing Loan Agreement Amount. In some cases (Lyon and Single Funding PD), we are showin...
- **[PMT-7820](https://skeps.atlassian.net/browse/PMT-7820)** — UCL - updates to the loan agreement for contract amounts less than $10k | SD-2420. UCL team wants to make updates to their agreement less than $10k. Since we create the agreement we will need to make changes in the doc file that’s used to generate the loan agreement.

## Merchant / Partner Portal (29)

- **[PMT-2956](https://skeps.atlassian.net/browse/PMT-2956)** — AF: Removing Docs from PD Merchant Resource tab. We have to remove the following from the resource tab:
- **[PMT-3021](https://skeps.atlassian.net/browse/PMT-3021)** — AF: CR: Put Compliance Operating Instructions back in PD Resources section.
- **[PMT-3241](https://skeps.atlassian.net/browse/PMT-3241)** — AF: Merchant Termination / Suspension | SD-1586.
- **[PMT-3514](https://skeps.atlassian.net/browse/PMT-3514)** — AF: Update Merchant Primary Category List - Add Home Security category | SD-1925. PRD: [
- **[PMT-3601](https://skeps.atlassian.net/browse/PMT-3601)** — AF: Annual Compliance Training - Partner Portal (ticket 2) | SD-1908. Previous Requirement:
- **[PMT-3773](https://skeps.atlassian.net/browse/PMT-3773)** — CLONE - AF: Lyon & PD AutoPay Updates | SD-1723. Make AutoPay consent checkbox mandatory for Lyon applications.
- **[PMT-3926](https://skeps.atlassian.net/browse/PMT-3926)** — AF - fix the validation issues seen in the frontend when the user tries to edit the merchant details. refer  more details. it contains
- **[PMT-3967](https://skeps.atlassian.net/browse/PMT-3967)** — AF - update the sample merchant agreement - part 2. FNBO legal team wants to add a clause in the merchant agreement.
- **[PMT-4111](https://skeps.atlassian.net/browse/PMT-4111)** — AF: Updates to Lyon Credit Policy. 1. Increased FICO cut-off from 640 to 660
- **[PMT-4387](https://skeps.atlassian.net/browse/PMT-4387)** — UCL - Txn and Application File query update | Add store ID and merchant name to daily files. The tickets are linked in this PMT ticket.
- **[PMT-4438](https://skeps.atlassian.net/browse/PMT-4438)** — AF - merchant risk framework - migration script for AF PD merchants to be default Tier 2. AF wants the PD merchants to have merchant tier as tier 2 as a default.
- **[PMT-5065](https://skeps.atlassian.net/browse/PMT-5065)** — AF - add merchant ref id to data files. Add merchant_reference_id to following HIL files:
- **[PMT-5415](https://skeps.atlassian.net/browse/PMT-5415)** — AF: Update the sample merchant agreement | SD-2137. During merchant signup, there is a checkbox with hyperlink:
- **[PMT-5513](https://skeps.atlassian.net/browse/PMT-5513)** — AF - UI changes in the lender portal and merchant portal because of TIC. Make the UI changes in the HIL lender portal and merchant portal to incorporate the TIC changes.
- **[PMT-5858](https://skeps.atlassian.net/browse/PMT-5858)** — UCL - Capture liabilities updated from lender portal in DWH. Add the additional liabilities and their balance added on the lender dashboard in data warehouse application_approval_to_funding table.
- **[PMT-5982](https://skeps.atlassian.net/browse/PMT-5982)** — AF: Update Merchant user permissions. New permission in the release: can_complete_work_and_initiate_refund
- **[PMT-6042](https://skeps.atlassian.net/browse/PMT-6042)** — AF: Merchant Application and Borrower Application Updates | SD-2155. Requirement 1: Merchant Application - Primary Category
- **[PMT-6172](https://skeps.atlassian.net/browse/PMT-6172)** — PF / AF: Portal Enhancements | SD-2263. Requirement 1 (PF):
- **[PMT-6656](https://skeps.atlassian.net/browse/PMT-6656)** — AF: Adverse Action letter on Lyon applications with frozen files | SD-2338. User Story 1:
- **[PMT-6659](https://skeps.atlassian.net/browse/PMT-6659)** — AF: Update Lyon Merchant ID as a bulk update | SD-2291. Script 1 - 1st Run
- **[PMT-6681](https://skeps.atlassian.net/browse/PMT-6681)** — AF: Update fields on PD Merchant Portal | SD-2292. Requirement:
- **[PMT-6719](https://skeps.atlassian.net/browse/PMT-6719)** — UCL - changes to the merchant logos in the customer journey. Objective: We need to update the logos in the customer journey for the 3 merchants of UCL: NEA, SEIU and Union Plus.
- **[PMT-6870](https://skeps.atlassian.net/browse/PMT-6870)** — AF: Update Pending Expiration Days for PD Merchants | SD-2380. Existing Value: 60 Days
- **[PMT-6932](https://skeps.atlassian.net/browse/PMT-6932)** — AF: Update validation for Lyon Merchant ID field - Frontend only | SD-2388. On frontend, we have to make “Lyon Merchant ID” a required field. Please ensure it is done at all the three places:
- **[PMT-6939](https://skeps.atlassian.net/browse/PMT-6939)** — UCL - show the TTY response in the report section in the lender portal | SD-2295. We receive a TTY response in bureau and check offers Provenir api. this is a teletype format that banking system has been using since 1970s and UCL team is comfortable reading this file. at present this is present ins...
- **[PMT-6942](https://skeps.atlassian.net/browse/PMT-6942)** — UCL - allow edit of mother's maiden name and middle name in lender portal | SD-2332. We want to allow editing of mother’s maiden name and middle name in the lender portal. Currently, borrower details edit doesn’t include the mother’s maiden name and middle name.
- **[PMT-7017](https://skeps.atlassian.net/browse/PMT-7017)** — AF - add another required document for merchant applicants to upload during the onboarding | SD-2393. We need to add one more document in the merchant onboarding. This document will be required in the journey.
- **[PMT-7827](https://skeps.atlassian.net/browse/PMT-7827)** — PF/AF - add the following functionalities in the merchant portal - Part 2. We need to add the following functionalities in the AF merchant portal (PD and Lyon)
- **[PMT-7916](https://skeps.atlassian.net/browse/PMT-7916)** — UCL - hybrid journey - show a field in the lender portal for applications part of test population. For the hybrid journey A/B test, UCL team wants an indicator in lender portal so that they know a customer is part of the test population or not. At present there isn’t any indicator.

## Customer Journey / Borrower UX (22)

- **[PMT-4214](https://skeps.atlassian.net/browse/PMT-4214)** — UCL - Add "to" in the phone entry screen.. Objective: Add “to” to all the flows in mobile screen for UCL.
- **[PMT-4227](https://skeps.atlassian.net/browse/PMT-4227)** — UCL - update the channel id for UCL. Objective: update the channel ids in the below api.
- **[PMT-4251](https://skeps.atlassian.net/browse/PMT-4251)** — UCL - Handle the special characters when generating the consolidation file. This ticket was cloned from a bug.
- **[PMT-4357](https://skeps.atlassian.net/browse/PMT-4357)** — UCL - make changes to the union plus logo size. Request from the UAT is to make this logo similar in size to what we have for NEA.
- **[PMT-4377](https://skeps.atlassian.net/browse/PMT-4377)** — UCL - add promo code in a location on the phone screen for UCL checking that promo code matches links.. The UCL team wants to verify within the journey if the promo code matches the links. For this we need to add the promo code on the phone number screen as per the desgin.
- **[PMT-4500](https://skeps.atlassian.net/browse/PMT-4500)** — UCL - handling of hyphen, apostrophe and dot in the last name (Experian and Provenir). handle the mentioned special characters so that Experian and Provenir doesn’t give an error.
- **[PMT-4742](https://skeps.atlassian.net/browse/PMT-4742)** — UCL - validation for the city field in the customer journey. To add the validation on the city field. according to the spotlight these are characters are allowed. this is in the consoldation file city field.
- **[PMT-4772](https://skeps.atlassian.net/browse/PMT-4772)** — UCL - update the folder path for the consolidation file. update the folder path name for the UCL consolidation file.
- **[PMT-4822](https://skeps.atlassian.net/browse/PMT-4822)** — UCL - change the "Are you a US citizen" to "Are you a US citizen/Permanent Resident" in the customer journey. change the verbiage of "Are you a US citizen?" to "Are you a US citizen/Permanent Resident?".
- **[PMT-4926](https://skeps.atlassian.net/browse/PMT-4926)** — UCL - ability to change between loan type (personal and consolidation) post approval. We want to give customers the option to switch loan type from Personal to Business and vice versa. Customer can change loan type during their journey as mentioned in the figma attatched below.
- **[PMT-5234](https://skeps.atlassian.net/browse/PMT-5234)** — UCL - increase the session time in the customer journey. Problem statement:
- **[PMT-5397](https://skeps.atlassian.net/browse/PMT-5397)** — UCL - Cancel Application feature. Lender user with permission can cancel the application. There will not be any cancellation after ~~ready to fund~~ contract is signed since we have auto approval and the app moves directly to funding request approved
- **[PMT-5552](https://skeps.atlassian.net/browse/PMT-5552)** — PF V2 - Fraud management (UCL). PRD link:
- **[PMT-5802](https://skeps.atlassian.net/browse/PMT-5802)** — UCL - Cancel Contract functionality. Summary
- **[PMT-5866](https://skeps.atlassian.net/browse/PMT-5866)** — UCL - Updates in Personal and Consolidation Screen. Kindly follow the below Figma link for updates in the PL and CL and CL w/o liability screen.
- **[PMT-5992](https://skeps.atlassian.net/browse/PMT-5992)** — UCL - Add Promo Code, Citizenship status, and Residence Status. Find the design file below
- **[PMT-6000](https://skeps.atlassian.net/browse/PMT-6000)** — UCL - When the Fraud check is updated user is directed to the all customers screen. This is in relation to
- **[PMT-6046](https://skeps.atlassian.net/browse/PMT-6046)** — UCL - manitain different terminal screens in the journey based on the loan type. this ticket was used to make changes to the terminal screen
- **[PMT-6130](https://skeps.atlassian.net/browse/PMT-6130)** — UCL - Show Mothers's maiden name and Borrower's middle name  in Borrower Information | SD-2277. Mother’s Maiden Name will not be editable.
- **[PMT-6173](https://skeps.atlassian.net/browse/PMT-6173)** — UCL - Show Risk attributes | SD-2276. Risk attributes to show
- **[PMT-6940](https://skeps.atlassian.net/browse/PMT-6940)** — UCL - provide the action to remove the liabilities in the liabilities tab | SD-2304. Currently we only have the ability to add and edit the liabilities from the lender portal. Lender wants to introduce a new action to remove/delete the liabilities from the portal. Once deleted, it should reflect back ...
- **[PMT-7024](https://skeps.atlassian.net/browse/PMT-7024)** — UCL - A/B testing - call assisted flow for customer journey | SD-2385. Figma

## Verbiage / UI Copy (19)

- **[PMT-3598](https://skeps.atlassian.net/browse/PMT-3598)** — AF: Updating verbiage on "Welcome to the Partner Portal" email | SD-1910. Requirement:
- **[PMT-4060](https://skeps.atlassian.net/browse/PMT-4060)** — PF - make verbiage changes for AF and TGUC in the upload doc journey's more info popup. Update the verbiage in the more info modal for AF and TGUC. The verbiage is mentioned in the design file for this ticket.
- **[PMT-4286](https://skeps.atlassian.net/browse/PMT-4286)** — UCL - changes to the e-consent verbiage shown to the customer. Need to make change to the verbiage of the e-consent in UCL.
- **[PMT-4322](https://skeps.atlassian.net/browse/PMT-4322)** — PF - make verbiage changes for AF and TGUC in the upload doc journey's more info popup - part 2. NOTE - Based on the requirement from FNBO, there was a line missed to be removed from the design file. Need to make the change for it.
- **[PMT-4896](https://skeps.atlassian.net/browse/PMT-4896)** — UCL - change the "Are you a US citizen" to "Are you a US citizen/Permanent Resident" in the customer journey - verbiage change from UCL team. Old implementation - change the verbiage of "Are you a US citizen?" to "Are you a US citizen/Permanent Resident?".
- **[PMT-4966](https://skeps.atlassian.net/browse/PMT-4966)** — UCL - verbiage change in the error shown in the consolidation screen. Verbiage of the error message needs to be changed. Feedback from UAT:
- **[PMT-5016](https://skeps.atlassian.net/browse/PMT-5016)** — UCL - verbiage change in the consolidation screen in the UCL journey. there was a change done with  . we need to revert the verbiage changed with it.
- **[PMT-5290](https://skeps.atlassian.net/browse/PMT-5290)** — UCL - verbiage changes in the footer of customer journey | SD-2151. All URL Footers -
- **[PMT-5853](https://skeps.atlassian.net/browse/PMT-5853)** — UCL - Update text on Switching between Personal to consolidation. 1. If customer has selected Personal the text should be - “If you don't see the credit options you were looking for you may benefit from switching to a consolidation loan”
- **[PMT-5980](https://skeps.atlassian.net/browse/PMT-5980)** — AF: Fix the Status and CTA. @Saket Vatsa For the following backend status:
- **[PMT-6064](https://skeps.atlassian.net/browse/PMT-6064)** — UCL - Change Header in Disbursement table. @Saket Vatsa Please change account description in consolidation table to “Creditor Name”
- **[PMT-6697](https://skeps.atlassian.net/browse/PMT-6697)** — AF | CR - changes in the verbiage in the terms and conditions screen in the Lyon portal | SD-2170. Objecive:
- **[PMT-6936](https://skeps.atlassian.net/browse/PMT-6936)** — UCL - verbiage changes in the terminal screen for personal and consolidation journeys. We are going to update the verbiage shown in the screen for personal and consolidation journeys for all 4 merchants.
- **[PMT-7776](https://skeps.atlassian.net/browse/PMT-7776)** — UCL - hybrid journey - update the verbiages in the customer journey as per the design. Need to update the verbiages in the customer journey screen in the hybrid journey. UCL had requested different text in the screens if the variant is test for customer.
- **[PMT-7808](https://skeps.atlassian.net/browse/PMT-7808)** — AF | Merchant portal revamp - move the request increase limit CTA from funding tab to offers tab.
- **[PMT-7855](https://skeps.atlassian.net/browse/PMT-7855)** — AF - updated the verbiage for Business Registration document in merchant onboarding flow. Production verbiage of the Business Registration document includes: “for each state you operate in“. But in the latest code on preprod, the statement is missing. We need to add this statement back for the Business Reg...
- **[PMT-7915](https://skeps.atlassian.net/browse/PMT-7915)** — UCL - verbiage changes part 1. UCL team is asking for changes in the customer journey as these were already aligned with them as part of the scope of hybrid journey. Note - these changes are only for the test population customer journey after lende...
- **[PMT-7917](https://skeps.atlassian.net/browse/PMT-7917)** — UCL - verbiage changes - Part 2. UCL team has requested for changes in the phone and SSN screen verbiage for all resumptive flows
- **[PMT-7954](https://skeps.atlassian.net/browse/PMT-7954)** — AF - merchant onboarding flow - update the verbiage of the documents to be uploaded. FNBO wants to update the verbiage of the documents uploaded in the merchant onboarding flow. Documents to be updated:

## Notifications (Email/SMS) (10)

- **[PMT-3497](https://skeps.atlassian.net/browse/PMT-3497)** — AF: Add email triggers for borrower application | SD-1620 | SD-1907. Requirement 1:
- **[PMT-3861](https://skeps.atlassian.net/browse/PMT-3861)** — PF - add the purpose link to the emails for TGUC, AF. To add the purpose link in the loan agreement signed confirmation email. Purpose link will be used by the customer to download the loan documents. purpose link ticket
- **[PMT-4553](https://skeps.atlassian.net/browse/PMT-4553)** — AF: Pass Application ID in the AA Emails. Requirement:
- **[PMT-4566](https://skeps.atlassian.net/browse/PMT-4566)** — AF - migrate the email templates from the SendGrid main account to a dedicated AF account.
- **[PMT-5394](https://skeps.atlassian.net/browse/PMT-5394)** — PF v2 - Communications Tab in Application details page (UCL). Problem Statement
- **[PMT-5867](https://skeps.atlassian.net/browse/PMT-5867)** — UCL - Communications tab Edit email. This is an extension of
- **[PMT-5902](https://skeps.atlassian.net/browse/PMT-5902)** — UCL - Request more information email Template. Please find the attached template ID and Version ID for Request More information.
- **[PMT-5931](https://skeps.atlassian.net/browse/PMT-5931)** — UCL - Update email on Approved. Please find the template ID and Version ID of Email that will go when customer is approved. This will go when customer is approved automatically and when customer gets approved after manual review.
- **[PMT-6440](https://skeps.atlassian.net/browse/PMT-6440)** — UCL - Stop reminder Emails for Fraud review applications. Applicants which are getting paused due to fraud review, should not get reminder email to complete their application.
- **[PMT-6890](https://skeps.atlassian.net/browse/PMT-6890)** — AF: Update cc email address for non prod. There are several merhant emails where we cc the emails to Brian Anderson like the below one:

## API / Integrations (16)

- **[PMT-3239](https://skeps.atlassian.net/browse/PMT-3239)** — AF: Core Logic API integration | SD-1491. /token - To generate the token
- **[PMT-3465](https://skeps.atlassian.net/browse/PMT-3465)** — AF: Handle fileInquiriesImpactedScore in NOAA, Amsive, and Bureau File | SD-1889. Use Case:
- **[PMT-3772](https://skeps.atlassian.net/browse/PMT-3772)** — AF: Null handling of Core Logic APIs. If any of the following APIs fail (non 200 response):
- **[PMT-3847](https://skeps.atlassian.net/browse/PMT-3847)** — AF: Fix Conduent related issues. Req1: Integrate Retail Monetary History API to fetch date and amount for 30C and 30I transactions
- **[PMT-3986](https://skeps.atlassian.net/browse/PMT-3986)** — AF: Send Agreement to Axway. AF team needs to ingest the Loan Agreement and Change Order in Axway.
- **[PMT-3994](https://skeps.atlassian.net/browse/PMT-3994)** — AF: Update Amsive Template and Logic for Doc Requested List. Req1: Added Document Requested handling section
- **[PMT-4208](https://skeps.atlassian.net/browse/PMT-4208)** — AF: Updates to Conduent APIs | SD-1988 | SD-2017. Requirement 1: Pass Co-Borrower DOB in a new Conduent update API (SD-2017)
- **[PMT-4416](https://skeps.atlassian.net/browse/PMT-4416)** — AF - Pagaya | 2nd look lender API integration with HIL | SD-2058. Underwriting Updates:
- **[PMT-4554](https://skeps.atlassian.net/browse/PMT-4554)** — AF: Updates to Conduent APIs | SD-2036. Requirement: Fix deferment-related issues in Conduent
- **[PMT-6039](https://skeps.atlassian.net/browse/PMT-6039)** — AF: Integrate Work Number for Income Verification | SD-2197. Vendor API PRD:
- **[PMT-6436](https://skeps.atlassian.net/browse/PMT-6436)** — AF - Pagaya | 2nd look lender API integration with HIL | SD-2058 | Part 2. The following were done in Part 1:
- **[PMT-6551](https://skeps.atlassian.net/browse/PMT-6551)** — AF/PF: Webhook Enhancements | SD-2315. PF: Document Requested Webhook
- **[PMT-6683](https://skeps.atlassian.net/browse/PMT-6683)** — AF: Create API Merchant | SD-2284. Create API Merchant based on this configuration:
- **[PMT-6800](https://skeps.atlassian.net/browse/PMT-6800)** — AF: Update scheduler logic for Digital Verification and Conduent | SD-2370. Monthly payment shown on the digital verification screen is using the funding amount in case of single funding PD and Lyon
- **[PMT-6869](https://skeps.atlassian.net/browse/PMT-6869)** — AF: Soft Handle Pagaya Create Token API. Only Pagaya PAA API is soft handled.
- **[PMT-7525](https://skeps.atlassian.net/browse/PMT-7525)** — AF - template change in amsive for the letter sent to coborrower in case of frozen bureau on borrower. FNBO realised they can’t use the template 1 in cases where borrower has a frozen bureau and coborrower report is pulled. they suggested creating a new template: 2b and that needs to be sent in such cases.

## Data / DWH (6)

- **[PMT-3645](https://skeps.atlassian.net/browse/PMT-3645)** — AF: New application data fields in data file feed from EXL to FNBO | SD-1917. What we need at borrower level:
- **[PMT-5107](https://skeps.atlassian.net/browse/PMT-5107)** — UCL - user creds update to access DWH in production. DWH connection creds to be updated. Earlier were being picked from “`DB_MYSQL_HOST`" . Now needs to picked from “`DWH_MYSQL_HOST`". Currently, this is hardcoded but we need to push it.
- **[PMT-5943](https://skeps.atlassian.net/browse/PMT-5943)** — AF: Create migration script for grandfathering of PMT-4396. We need to ensure that:
- **[PMT-5970](https://skeps.atlassian.net/browse/PMT-5970)** — UCL DWH - Update mapping to accommodate fraud rules. If any manual review data is not getting captured in UCL DB based on current mappings, then they will need to be investigated and updated.
- **[PMT-6871](https://skeps.atlassian.net/browse/PMT-6871)** — AF: Data File Updates | SD-2316. 1. Integrate 360 Finance API flow in hil_transaction file
- **[PMT-6876](https://skeps.atlassian.net/browse/PMT-6876)** — UCL - make changes for the ODS file. @Anahita Chaudhary @Nishant Bansal  please verify the changes needed in this ticket.

## UAT Feedback / Fixes (7)

- **[PMT-2743](https://skeps.atlassian.net/browse/PMT-2743)** — AF - Bug in Send application link screen footer text. Replace [noreply@bnpl.fnbo.com](mailto:noreply@bnpl.fnbo.com) to [donotreply@bnpl.fnbo.com](mailto:donotreply@bnpl.fnbo.com)
- **[PMT-3946](https://skeps.atlassian.net/browse/PMT-3946)** — AF - ADA testing for HIL - June. Objective: we need to have the HIL journeys - HIL customer, and HIL merchant - tested to ensure that screen readers will work for people with sign impairment.
- **[PMT-4702](https://skeps.atlassian.net/browse/PMT-4702)** — AF and FNBO - fix the ADA testing issues. 1. Fix the loan agreement issues in the docusign iframe.
- **[PMT-4886](https://skeps.atlassian.net/browse/PMT-4886)** — UCL - ADA critical issues fix. | portal/journey | customer |
- **[PMT-5055](https://skeps.atlassian.net/browse/PMT-5055)** — UCL - ADA fixes. 1. Revert back to the nested checkbox issue since that’s a serious issue and FNBO compliance team are okay to have it in the report.
- **[PMT-5778](https://skeps.atlassian.net/browse/PMT-5778)** — AF - fix the serious issues found in the ADA report | SD-2170. Here’s the breakup of the issues found in the latest report.
- **[PMT-8000](https://skeps.atlassian.net/browse/PMT-8000)** — AF - iphone - placeholder ticket for fixing the resolution issue on iphones. @Saket Vatsa requesting you to fill in the necessary details in this

## Other (18)

- **[PMT-2732](https://skeps.atlassian.net/browse/PMT-2732)** — AF - Textual changes in 3 screens | SD-1603. Screen1: Enter Expected Financing Amount Screen
- **[PMT-2845](https://skeps.atlassian.net/browse/PMT-2845)** — CR: AF: Remove the error check of $50,000 on Work Order UI. This new error check of $50,000 has to be removed and UAT offered again.
- **[PMT-3078](https://skeps.atlassian.net/browse/PMT-3078)** — AF: CR: Change MT25S to MT20S | SD-1738. We have to change mt25s to mt20s in the mortgage check.
- **[PMT-3474](https://skeps.atlassian.net/browse/PMT-3474)** — AF: Customer Level Limit Update | SD-1890. PRD: [
- **[PMT-3599](https://skeps.atlassian.net/browse/PMT-3599)** — AF: Update the docs in the Resource Tab  | SD-1911. Requirement:
- **[PMT-3742](https://skeps.atlassian.net/browse/PMT-3742)** — AF - remove the "General Contracting" from the primary category | SD-1927. Remove the “General Contracting” option shown in the primary category shown while merchant onboarding. This a request from Michael as it’s not useful when tracking performance by merchant category.
- **[PMT-4039](https://skeps.atlassian.net/browse/PMT-4039)** — AF - AF056 Check Name Update | SD-1973. Requirement1:
- **[PMT-4040](https://skeps.atlassian.net/browse/PMT-4040)** — AF - Revising HIL Partner Direct Max Loan Amount | SD-1974. Updated Max Loan Amount for FICO 600-719 ranges
- **[PMT-4285](https://skeps.atlassian.net/browse/PMT-4285)** — Platform v2: Requested document and document section changes | SD-1919 - AF.
- **[PMT-4773](https://skeps.atlassian.net/browse/PMT-4773)** — AF: Remove 30-I related check for reversing the transaction before draws.
- **[PMT-5611](https://skeps.atlassian.net/browse/PMT-5611)** — UCL - UI changes in Application details page. Summary
- **[PMT-5995](https://skeps.atlassian.net/browse/PMT-5995)** — AF/UCL - Changes in the screen requested by FNBO to incorporate the documents links | SD-2170. Objecive:
- **[PMT-6100](https://skeps.atlassian.net/browse/PMT-6100)** — UCL - Update phone number on credit freeze screen. Please update the mobile number in Web and Mobile flow on the credit freeze screen.
- **[PMT-6138](https://skeps.atlassian.net/browse/PMT-6138)** — UCL - Edit taxable indicator of Borrower/Co-borrower from Income Calculator tab | SD-2275. In the Verified income section → Show and Give the option to edit Borrower and Co-Borrower taxable indicator
- **[PMT-6221](https://skeps.atlassian.net/browse/PMT-6221)** — UCL - Max 9 limit on add liability. Hi @Sheelpriy Gautam
- **[PMT-6301](https://skeps.atlassian.net/browse/PMT-6301)** — UCL - Allowed characters in Bank/Creditor Name. @Saket Vatsa @Nishant Bansal @Sheelpriy Gautam
- **[PMT-6478](https://skeps.atlassian.net/browse/PMT-6478)** — AF: Create Decision Engine with Modernize (360 Finance) | SD-2317. Slice-HIL is integrating with Modernize, and they would like to set up a black box at the Modernize server so that one level of qualification happens within the Modernize server, and only Apps that are highly likely t...
- **[PMT-6728](https://skeps.atlassian.net/browse/PMT-6728)** — AF: Support other_monthly_income in income calculation. Existing Logic: