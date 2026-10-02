# Improper Coupon Validation - Business Logic

## Overview

During testing of an e-commerce platform, I found a business logic vulnerability in the promotional coupon validation system. This flaw allows users to bypass the intended pricing restrictions for subscription plans. A coupon intended for an entry-level annual plan could be applied to a higher-tier multi-year subscription. The backend accepted the discount without verifying that the coupon was valid for the selected plan.

## Technical Details

**Vulnerability Type:** Improper Coupon Validation (Business Logic Flaw)

**Target Surface:** Android Mobile App and Web Application

The issue was caused by the backend not enforcing the coupon's plan-specific restrictions. The backend checked that the coupon existed and was still valid, but did not verify that it belonged to the selected SKU or subscription tier. Consequently, a promotional code strictly scoped for an entry-level plan could be applied to higher-tier, multi-year subscriptions.

## Steps to Reproduce

To verify the vulnerability, I performed the following steps:

1. Opened the target e-commerce platform's mobile application.

2. Navigated to the Premium Membership section and added a top-tier, multi-year plan to the checkout cart.

3. Applied a third-party rewards coupon obtained through a partner rewards program, explicitly intended for an annual entry-level plan.

4. Observed that the discount system accepted the coupon, applying a full discount across the higher-tier plan and dropping the total payable amount to only the standard convenience fee.

5. Completed the payment. The system successfully provisioned full multi-year premium access instead of the intended basic access.

> **Note:** This behavior was verified on both mobile and web surfaces. To maintain target anonymity and respect responsible disclosure guidelines, all visual proof-of-concept evidence has been intentionally withheld.

## Business Impact

- **Direct Financial Loss:** The flaw allows a user to obtain a higher-value subscription while paying significantly less than the intended price.

- **Abuse Potential:** Because coupon eligibility was not enforced server-side, the same validation weakness could potentially be reused with other coupons or subscription tiers.

- **Subscription Integrity:** The application provisions a subscription that does not match the restrictions of the coupon used during checkout.

## Remediation Strategy

To eliminate this vulnerability, the platform must implement the following controls within the promotional pipeline:

- **Strict SKU/Tier Validation:** Enforce server-side checks to verify that the coupon is valid for the selected `product_id` and `plan_id`.

- **Discount Recalculation:** Calculate the applicable discount server-side and enforce maximum discount limits instead of allowing targeted vouchers to override the final payable amount.

- **Final State Verification:** Re-verify the selected plan and applied coupon before provisioning the subscription.
