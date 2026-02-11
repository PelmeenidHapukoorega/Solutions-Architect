# Notes about determining NSG effective rules

Things to consider when creating effective rules:

1. Processing order

Traffic is evaluated in a specific sequence depending on the direction. If you have NSGs at both the Subnet and the NIC level, it works like this:


