## Azure Automation Account

>Think of any general purpose Runbook Automation tool like `ResOne Automation`. You can create and schedule jobs centrally

`Create Azure Run As account` option in creation page = create ad service principal and assigning it contributor role for all automation tasks.

>Azure Automation account is linked with a `tenant id` hence can be used across regions in multiple subscriptions associted with the tenant


![image](https://user-images.githubusercontent.com/13016162/71761024-f6247c80-2eeb-11ea-964e-1d312c9d0a39.png)

>Azure automation can be used to automate on-premise resources using Hybrid Runbook Worker

![image](https://user-images.githubusercontent.com/13016162/71761281-1dc91400-2eef-11ea-9911-a8a523a00769.png)


#### A very basic automation runbook could be start-stop VM on a schedule time


