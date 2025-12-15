## Mini lab: Estimating workload costs by using Pricing calculator

1. Went to `https://azure.microsoft.com/en-us/pricing/calculator/`
2. Clicked tabs `Products`, `Example scenarios`, `Saved estimates` and `FAQ`

Learned that:

* Produts is where you chooses Azure services that you want to include in the estimate.
* Example scenarios is where to find reference architectures or common cloud based solutions to use as a starting point.
* Saved estimates is where i can find my previously saved estimates, incase i need to fall back.
* FAQ is where to discover info about Pricing calc and how to navigate it or find commonly asked questions and answers to it.

## Adding services to estimate

1. Went back to `Products` tab to select services for following categories:
<img width="880" height="213" alt="image" src="https://github.com/user-attachments/assets/e05ede44-365e-4cd6-93c8-ea08c4517c83" />

2. Scrolled to the bottom to see services listed with default configs
<img width="912" height="364" alt="image" src="https://github.com/user-attachments/assets/1dcddea8-5b06-40d4-b4f6-7357a7135827" />

## Configuring services to match my requirements

1. Under `Virtual machines` set the following values with changes to my specifics

What was asked:
<img width="876" height="356" alt="image" src="https://github.com/user-attachments/assets/0dab7922-e8b8-41d1-b878-e0e609edb457" />

What i changed to:
<img width="902" height="263" alt="image" src="https://github.com/user-attachments/assets/158ef8c7-4ded-4011-9b01-328e8b5add61" />

Left the rest default.

2. Under Azure SQL database set the following values:

<img width="884" height="449" alt="image" src="https://github.com/user-attachments/assets/5107004e-cc2b-4dc3-8def-d88915fcad8f" />

With changes to region by switching it to `NorwayEast` since its the closest region to me. And left the rest default.

3. Application Gateway values

<img width="885" height="356" alt="image" src="https://github.com/user-attachments/assets/a327e7e8-fa83-40e5-9c8c-d3cc73c05b7a" />

Changed the region to `NorwayEast` and added 2x 730 gateway hours to match Virtual machines. Left the rest default.

Leaving the total cost of setup to:
<img width="900" height="112" alt="image" src="https://github.com/user-attachments/assets/32f4e720-74c7-49fb-9aca-edfc398bc788" />


### My thoughts and summary

Pricing calculator is actually pretty great to pick and choose services you would want to use before actually deploying or building. Since I dont work for a company its even more so perfect for me, i can budget better without the overthinking of how much this solution is going to cost me. Definetly going to use it for my own projects as well as for labs in the future. Great for picking instances, looking over specifics, how much is VM series X,Y and Z going to hurt my wallet, what storage options they have, how much will that add to the cost, which zones to pick etc etc.

All in all? Pretty great free tool!
