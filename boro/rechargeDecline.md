## rechargeDeclineScreen

### fn: checkIfNotificationEnabled

expoToken: `null || string`
existingStatus: `'granted' || 'denied' ||'undetermined'`

These two variables shouldn't be `undefined` because `existingStatus` is the return of async `Notifications.getPermissionsAsync()` call and expoToken is passed into props from the screen

1. local notification permissions
2. database expo token 
Wait what about the condition that you set it once in the app
So since the user in logined then the expoToken in the database doesn't change. Only when they sign out
So if they sign out then erase the expoToken, otherwise no point

Now the issue is that `expoToken` that is being passed in from `cashLimitWidget` is still `null` even after being updated in the database.

### Encountered issues

ankita.testdecline2@sjsu.edu
Testing@123

|Issue |Expected clicking on `Notify me!` on creditDecline persists |
|-|-|
|Action| Click on `Notify me!` on `creditDecline` screen|
|Expected Behavior| Notifications should already be enabled in `rechargeDeclineScreen`|
|Actual Behavior| Doesn't pass in the `expoToken` prop into `rechargeDeclineScreen`.|
|Cause| `expoToken` being passed into `rechargeDeclineScreen` is still `null`. `cashLimitWidget` didn't get fetch the new values.|

|Issue|Expected clicking on `Notify me!` on rechargeDeclineScreen persists |
|-|-|
|Action| Click on `Notify me!` on `rechargeDeclineScreen` screen|
|Expected Behavior| Notifications should already be enabled in `creditDecline`|
|Actual Behavior| Doesn't pass in the new `notificationEnabled` prop into `creditDecline` screen from `cashOrder` screen.|
|Cause| `notificationEnabled` being passed into `creditDecline` is still `disabled`. `creditDecline` didn't fetch the new values.|

#### Todo - cashLimitWidget should refresh or refetch mutation when popping back from `rechargeDeclineScreen`
#### Todo - creditDecline has to check if expoToken is enabled