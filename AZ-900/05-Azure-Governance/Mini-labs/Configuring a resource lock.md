<img width="568" height="898" alt="image" src="https://github.com/user-attachments/assets/7b8f902c-3b69-4e0b-94b2-d465c1c83f86" />## Mini lab: Configuring a resource lock

1. Went to portal `https://portal.azure.com/#home`
2. Created new resource group titled `Treasure_chest`
3. Create new storage account  named `jewelsandgold`
4. Set the region for `NorwayEast` since its closest to me
5. Chose `LRS` for redundacy because its the cheapest option and im doing this for testing reasons
6. Reviewed the following settings:

<img width="811" height="575" alt="image" src="https://github.com/user-attachments/assets/47d2611b-193a-4dbb-9287-860f2a43f037" />

7. Selected `Review and create` and waited for deployment.
8. Deployment took around 15 seconds, clicked `Go to resource`
9. Opened up `Settings` from the left panel and chose `Locks`
10. Clicked `Add`
11. Named it `GoldKey` added a note `To see treasure` and chose the lock type for `Read-only` since im greedy and dont want anyone to touch my treasure:

<img width="355" height="352" alt="image" src="https://github.com/user-attachments/assets/fe5ca78b-1d43-49c6-9d0d-e68ff2d3667c" />

12. Clicked `Ok`
13. Needed to create a container for the traeasure, so from left pane clicked `Containers` then `add new` and named it to `vibraniumchest`
14. Got the following error:
<img width="337" height="195" alt="image" src="https://github.com/user-attachments/assets/35b9bad4-a916-4a3c-be2a-343a1fb02053" />

Why? Because i created a Read only lock which prevents me from doing anything other than to view it.

15. Went back to the lock itself and changed the type to `Delete`
16. Went back to `Containers` and create `vibraniumchest` again:
<img width="713" height="308" alt="image" src="https://github.com/user-attachments/assets/d2c30ecd-e90c-4d11-9542-022d8bcd1eb2" />

17. Went back to `Locks` and clicked `Delete` because I cant delete storage before since lock prevents you do to so. In other words, to delete any containers or resources that have locks, you need to remove the lock prior.
18. After deleting the lock, i went back to `Storage accounts` and clicked on `jewelsandgold` and then `Delete`
<img width="568" height="898" alt="image" src="https://github.com/user-attachments/assets/8b51e7ab-62a3-48c3-9afc-7080e680434c" />

19. Clicked `Delete` 
<img width="343" height="61" alt="image" src="https://github.com/user-attachments/assets/04edd492-e782-41d8-be9b-a490e5aac761" />

And with that the lab was done.

### Summary

Locks are pretty nice in terms of controlling who can access your valuables and who cant. The process of adding locks is quick, simple and quite easy to manage. Nothing complicated. With this lab i learned how to add locks, what both 2 types of locks do, what needs to be done in order to change resources, what prevents the deletion of resources and the logic that you need to remove locks prior to changing anything and that includes deletion.
