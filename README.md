# Stationeers-IC10-Automation
Some automation projects for Stationeers, coded using IC10.

# Printers automation with automated production batch
In this automation, one Vending machine is checking the quantity of each item and requesting production when the quantity is below the minimum quantity.
All the machines read the requested hash, and the machine that can produce it start to request ingots to the second Vending machine.
When all the ingots are available, the printer start a batch of 10 products (the quantity can be adjusted in the program).
After producing, the stacker will export the produced items and then, the ingots will return to the Vending Machine.
The products are sorted to go to the first Vending machine.
