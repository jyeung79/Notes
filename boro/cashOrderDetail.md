# Develop UI for Cash Order Detail Screen

https://app.asana.com/0/1125608679435325/1199617282609788

1. Work on the cash order details screen (RepaymentScreen.js)
2. Change the card layout according to the designs. Do not modify or remove existing elements. If there are doubts do check with Raheel on it.
3. There are these below scenarios for cards rendering you will find in Repayment Screen. Same should be mentioned in design
   1. Paid off /Recharged detail screen (Closed)
   2. NoPastDueNoDueToday
   3. NoPastDueDueToday
   4. PastDueNoDueToday
   5. PastDueDueToday
4. For payment history check the Payment History screen and include the payment history component with a modification to the mutation to get payment history for that specific order only. There are minor UI modifications on making it a section list and handling of the status of the payments by showing diff icons. We do have most of these icons specified check if you need any extra ones. What do you mean change the mutation? Is that the mutation of the RepaymentScreen?

2 states : 
* [Past, setPast]
* [DueToday, setDueToday]