## BoroCash Screens

1. `BoroCashNavigation` - Detemines which nav stack or screen to go first
   1. `waitlistScreen`
   2. `applicationStack`
      1. All the landing questions flow
   3. `cashStack`

### Recharge Status

That's what Lakshay was talking about. `recharge` status for a `cash_order` means that there was a recharge amount. So I'm assumming that `disbursing` state is for any new cash orders. The problem is that `recharge` itself is given a new cash order.


#### Clarify on the conditions for CashLimitWidget

![](images/2021-04-30-09-38-03.png)

4 States (5?)

1. BoroCash_Approved
2. BoroCash_Declined
3. Recharge_Available
4. Recharge_Unavailable

Not really an issue. More related to issue with BoroCash Error message.

### Borocash Orders

Q: Now that we are rolling out this $25 cash disbursement.
A: No this shouldn't affect anything for cashLimitWidget since you the logic should still be the same.

Q: Which QA server do I use? Will QA4 be used only by Siyun and Isak now?

### Recharge Accounts

What is their state?

Okay I think this needs a `BE fix`. Basically if a `cash_order` is in `recharge` status, the `recharge_amount` should be added into `cash_used`.

File: `/workflow/cash_limit.rb`

Because currently for users who `recharged` and is not in `disbursing` state, they should be in `recharge_unavailable` state and their `cash_used` should reflect their `recharge_amount`.

> Actually the accounts where `isRechargeEnabled === 0` have a combination of `repay and paidoff` status

Q: So what if a user is in `repay` and decides to take a `recharge`? What is the status of their `cash_order`?
A: It'll still be in `repay` status.

**It's all good. I checked and it's only one specific user**

### Another Question on Amount and Such

```
raluchukwu.onyiuke.10@my.csun.edu
12345678
```

Q: What is the `amount`? Why the `cash_order` showing `amount=962.11` but the `cash_used=857`?