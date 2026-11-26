# Game-Memory-Manipulation

In this project I used Cheat Engine to find and change values stored in AssaultCube's memory. I was able to change my health, ammo, and throwables, change a bot's health, make my weapon do more damage, and lock values so the game can't change them.

## Scope
Everything was done offline, in single player against bots, on my own machine. No online or multiplayer games were touched.

## Tools
- Cheat Engine
- AssaultCube (offline, bots only)

## How It Works

### Finding a value when it's shown on screen (ex. health)
1. Set the value type to **4 Bytes** and the scan type to **Exact Value**.
2. Enter your current health and click **First Scan** (don't press Enter).
3. Take damage, enter the new health value, and click **Next Scan** (not New Scan).
4. Repeat until only a few addresses are left. When an address turns from white to red, its value just changed, which means it's likely the one you're looking for. If several change, keep scanning until it's narrowed down to one.
5. To ensure you don't die, after taking damage and doing next scan, do a couple unchanged value scans to narrow down. 
6. Double click the address to add it to the list at the bottom, then edit the value.

<img width="1591" height="765" alt="Screenshot (23)" src="https://github.com/user-attachments/assets/dfd8c0cf-4f45-4550-8cc6-fac69a1dfb22" />
<img width="1583" height="769" alt="Screenshot (24)" src="https://github.com/user-attachments/assets/e4c412d4-b4b6-4697-9861-dc94951dae61" />


### Finding a value when it's NOT shown on screen
1. Keep the value type on **4 Bytes**, but change the scan type to **Unknown Initial Value**.
2. Click **First Scan**.
3. Then damage the bot and change the scan type to **Decreased Value** 
4. Click **Next Scan** and repeat after every hit until you've found the address, then change the value.
5. To ensure the bot doesnt die, after each decreased value scan, do a couple unchanged value scans.
6. This same concept can be used for any unknown value. P.S. you can also assume what the first value. For example, I assumed the bots initial value was 100 and so when I was searching by scanning, I was also looking for a first value of 100. 

<img width="1744" height="761" alt="Screenshot (20)" src="https://github.com/user-attachments/assets/9d182aa9-d8a7-4059-845a-81fc8bf1feeb" />

### Locking a value so it can't change
1. Find the address using the steps above (for me it was a 4 Byte value).
2. Right click the address and choose **Find out what writes to this address**.
3. Go back into the game and make the value change, for example by taking damage or firing your weapon. Cheat Engine shows the instruction that changed the value.
4. Click **Replace**. This swaps the instruction out for one that does nothing (a NOP), so the value stays where it is no matter what happens in game.

### EXAMPLE OF PREVIOUS - Changing a bot's health and weapon damage
Using **Find out what accesses this address** on my weapon and shooting a bot, I found the instruction that handles damage. From there I could change how much damage my weapon does, or replace the instruction so the bot no longer takes damage.

<img width="1650" height="773" alt="Screenshot (26)" src="https://github.com/user-attachments/assets/36e8ec4c-690a-4f39-90e8-f5796b8a1d58" />
<img width="1645" height="776" alt="Screenshot (27)" src="https://github.com/user-attachments/assets/446e5ae5-5abf-480a-b4ef-29f6e2536bdf" />

## What I Learned
- How to search a program's memory and narrow down results to find specific values like health, ammo, and throwables
- The difference between data types (4 Byte integers vs floats/doubles) and why picking the right one matters
- How to find the instructions that read and write to an address, and how replacing them changes how the game behaves
- How to apply the same steps to bots, not just my own character

## Why This Matters for Security
The biggest takeaway from this project is that anything running on the user's machine can be changed. I was able to edit my health, ammo, and damage while the game was running, which shows why a program can't fully trust data on the client side. That's why online games check important values on the server and use anti-cheat to detect memory tampering.

This goes beyond games. Attackers use the same techniques to patch software, like bypassing license checks or changing how a program behaves. On the defensive side, malware analysts use the same skills I practiced here (scanning memory, finding what code reads and writes to a value, and patching instructions) to understand what malicious programs are doing. Security tools like EDR also watch for this kind of memory tampering.

## Next Steps
- Find pointers and static addresses so values can be found again after restarting the game
- Write a simple external trainer in Python or C++
