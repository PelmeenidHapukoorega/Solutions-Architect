# Self service password reset

1. Needs to have P1 or P2 license of Entra ID to be enabled
2. Microsoft Entra account with authentication policy administrator role > to be used to set up SSPR
3. Non administrative account > To test SSPR. Why non administrative? Because microsoft entra imposes extra requirements on admin accounts for SSPR. This user and all user accounts must have valid license to use SSPR
4. Non admin account must be member of Security group with test configuration. This group would be used to limit who to roll SSPR out to.

## Scope of SSR rollout

3 settings for SSPR enabled property:

* **None:** No users in the Microsoft Entra organization can use SSPR. This value is the default.
* **Selected:** Only the members of the specified security group can use SSPR. You can use this option to enable SSPR for a targeted group of users who can test it and verify that it works as expected. When you're ready to roll it out broadly, set the property to Enabled so that all users have access to SSPR.
* **All:** All users in the Microsoft Entra organization can use SSPR.
