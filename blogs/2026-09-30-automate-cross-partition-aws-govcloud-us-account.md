---
title: "Automate cross-partition AWS GovCloud (US) account bootstrapping"
url: "https://aws.amazon.com/blogs/publicsector/automate-cross-partition-aws-govcloud-us-account-bootstrapping/"
date: "2026-09-30"
author: "Mitch Nolan"
feed_url: "https://aws.amazon.com/blogs/publicsector/feed/"
---
In the post Automate AWS GovCloud (US) account creation using AWS Organizations APIs, we showed how to programmatically create GovCloud (US) accounts using AWS Organizations APIs. That post ends at account creation—the new accounts exist but the standard account remains in the organization root while the GovCloud (US) account remains a standalone account. You must manually complete the steps to join a GovCloud (US) organization and move accounts to the correct organizational units (OUs).
