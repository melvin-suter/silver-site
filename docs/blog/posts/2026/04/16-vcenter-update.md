---
title: "vCenter Update doesn't work anymore"
date: 2026-04-16
draft: false
tags:
- vmware
---

You are trying to update your vCenter, but suddenly you get a network error? Same.... And spoiler: it was not the network.

Basically Broadcom wen’t “fuck everyone” and started to move everything over to their side. Even the vCenter repositories. Even better, they added a download token too...

But no, they don’t change the vCenter to refelect those changes, I mean, that would be too obvious.

What now?

## The Fix

Get your download token: https://knowledge.broadcom.com/external/article/390121?

I won’t summarize here, because they like to change everything about twice a day.

Now change the URL in your update settings in vami to this:

```
https://dl.broadcom.com/<download-token>/PROD/COMP/VCENTER/vmw/8d167796-34d5-4899-be0a-6daade4005a3/<target-version>
```

Yes, including the very specific verion. Why have an auto check, if you have to set the version manually anyway. :)
So for 8.0.3 its like this:

```
https://dl.broadcom.com/<download-token>/PROD/COMP/VCENTER/vmw/8d167796-34d5-4899-be0a-6daade4005a3/8.0.3.00800
```

And also change the ESXi URLs if needed:

```
https://dl.broadcom.com/<Download Token>/PROD/COMP/ESX_HOST/main/vmw-depot-index.xml
https://dl.broadcom.com/<Download Token>/PROD/COMP/ESX_HOST/addon-main/vmw-depot-index.xml
https://dl.broadcom.com/<Download Token>/PROD/COMP/ESX_HOST/iovp-main/vmw-depot-index.xml
https://dl.broadcom.com/<Download Token>/PROD/COMP/ESX_HOST/vmtools-main/vmw-depot-index.xml
```