```ascii
+--------------------------------------------------------------+
|                  AZURE APP SERVICE FLAVORS                   |
+----------------------+------------------+---------------------+
|        WEB APP       |     API APP      |     WEB JOB        | 
+----------------------+------------------+---------------------+
| Runs full websites   | Runs backend     | Runs background    |
| (HTML/CSS/JS).       | APIs (REST).     | tasks/scripts.     |
| Always online.       | Stateless logic. | Can be scheduled   |
| Scales for users.    | Called by apps.  | or continuous.     |
+----------------------+------------------+---------------------+
|     Used For:        |     Used For:    |     Used For:       |
| - Company sites      | - Mobile app     | - Cleanup tasks     |
| - Dashboards         |   backend        | - Data processing   |
| - Any web UI         | - Microservices  | - Long-running ops  |
+----------------------+------------------+---------------------+

                     +----------------------+
                     |      MOBILE APP      |
                     +----------------------+
                     | Backend for native   |
                     | iOS/Android apps.    |
                     | Uses API App + Push  |
                     | Notifications + Auth |
                     +----------------------+
                     |     Used For:        |
                     | - Mobile clients     |
                     |   needing cloud data |
                     +----------------------+
```
