  #linux #terminus #commands 
---

## Basic Commands

### 1. **`ls`**
   - **Purpose**: Lists the contents of your current location.
   - **Usage**: Type `ls` to see what’s around you.
   - **Example**: 
     ```
     ls
     ```

### 2. **`cd LOCATION`**
   - **Purpose**: Moves you to a new location.
   - **Usage**: Replace `LOCATION` with the name of the place you want to go.
   - **Example**: 
     ```
     cd hallway
     ```

### 3. **`cd ..`**
   - **Purpose**: Moves you back to the previous location.
   - **Usage**: Type `cd ..` to backtrack.
   - **Example**: 
     ```
     cd ..
     ```

### 4. **`less ITEM`**
   - **Purpose**: Interacts with or inspects an item in your surroundings.
   - **Usage**: Replace `ITEM` with the name of the object you want to examine.
   - **Example**: 
     ```
     less book
     ```

### 5. **`pwd`**
   - **Purpose**: Shows your current location.
   - **Usage**: Type `pwd` if you forget where you are.
   - **Example**: 
     ```
     pwd
     ```


# Terminus Game Walkthrough Guide

## **Chapter 1: Starting Your Journey**

### **1. Home**
- **Initial Text**: You start in your home. Use the following commands to explore:
  - `ls`: Look around.
  - `cd [location]`: Move to a new location.
  - `cd ..`: Go back to the previous location.
  - `less [item]`: Interact with items or people.
- **Items**:
  - `Letter`: Use `less Letter` to read the game instructions again.

---

### **2. WesternForest**
- **Initial Text**: You enter a forest and find the **Spell Casting Academy**.
- **Items**:
  - `Sign`: Use `less Sign` to read about the academy.
- **Next Step**: Enter the **SpellCastingAcademy** by typing `cd SpellCastingAcademy`.

---

### **3. SpellCastingAcademy**
- **Initial Text**: The academy is bustling with students.
- **Items/People**:
  - `HurryingStudent`: Use `less HurryingStudent` to interact. The student suggests you visit the **Lessons** hall.
- **Next Step**: Go to the **Lessons** hall by typing `cd Lessons`.

---

### **4. Lessons**
- **Initial Text**: You join an introductory lesson on the `mv` (move) spell.
- **Items/People**:
  - `Professor`: Use `less Professor` to learn the `mv` spell.
    - You can move objects with `mv [object] [new location]`.
- **Next Step**: Practice your new spell in the **PracticeRoom** by typing `cd PracticeRoom`.
- ![[Pasted image 20250224173556.png]]
- 

---
### **5. PracticeRoom**
- **Initial Text**: The room is filled with practice dummies.
- **Items**:
  - `Instructions`: Use `less Instructions` to learn how to practice.
  - `PracticeDummy1` to `PracticeDummy5`: Use `mv PracticeDummy1 Box` to move dummies into the **Box**.
- **Next Step**: Experiment with moving dummies and explore the **Box** by typing `ls Box`.

---

### **6. NorthernMeadow**
- **Initial Text**: A pony prances around.
- **Items**:
  - `Pony`: Use `less Pony` to ride it. The pony suggests heading east.
- **Next Step**: Go to the **EasternMountains** by typing `cd EasternMountains`.

![[Pasted image 20250224173854.png]]

---

### **7. EasternMountains**
- **Initial Text**: You find a cave entrance with an old man outside.
- **Items/People**:
  - `OldMan`: Use `less OldMan` to learn about the cave and the portal inside.
  - `OldManuscripts`: Use `less OldManuscripts` to learn the `help` and `man` commands.
- **Next Step**: Enter the **Cave** by typing `cd Cave`.

---

### **8. Cave**
- **Initial Text**: The cave is dark and dank.
- **Locations**:
  - `Staircase`: Leads to a dead end.
  - `DarkCorridor`: Leads to the **DankRoom**.
- **Next Step**: Go to the **DarkCorridor** by typing `cd DarkCorridor`.

---

### **9. DankRoom**
- **Initial Text**: A boulder blocks your path.
- **Items**:
  - `Boulder`: Use `less Boulder` to feel a breeze behind it. Move it with `mv Boulder SmallHole`.
- **Next Step**: After moving the boulder, a **Tunnel** is revealed. Enter it by typing `cd Tunnel`.

---

### **10. Tunnel**
- **Initial Text**: You find a **StoneChamber** with a glowing portal.
- **Items**:
  - `Rat`: Use `less Rat` to interact (but be careful, it bites!).
- **Next Step**: Enter the **Portal** by typing `cd Portal` to reach **Terminus TownSquare**.
- ![[Pasted image 20250224174048.png]]
![[Pasted image 20250224174303.png]]


---

## **Chapter 2: Terminus TownSquare**
![[Pasted image 20250224174544.png]]


### **1. TownSquare**
- **Initial Text**: The town square is sunny but tense.
- **Items/People**:
  - `RandomCitizen1` and `RandomCitizen2`: Use `less` to talk to them and learn about the Dark Wizard.
  - `DistraughtLady`: Use `less DistraughtLady` to hear about her kidnapped baby.
- **Next Step**: Explore the **Library** by typing `cd Library`.

---

### **2. Library**
- **Initial Text**: The library is cozy but smells of mildew.
- **Items**:
  - `totallyRadSpellbook`: Use `less totallyRadSpellbook` to learn about the `sudo` spell.
  - `inconspicuousLever`: Use `less inconspicuousLever` to reveal a **BackRoom**.
- **Next Step**: Enter the **BackRoom** by typing `cd BackRoom`.

---

### **3. BackRoom**
- **Initial Text**: You meet **Grep** and the **Librarian**.
- **Items/People**:
  - `Grep`: Use `less Grep` to interact.
  - `Librarian`: Use `less Librarian` to get a quest to search for "dark wizard" in the `historyOfTerminus` book.
    - Use `grep "dark wizard" historyOfTerminus` to complete the quest.




![[Pasted image 20250224175019.png]]





---

### **4. Marketplace**
- **Initial Text**: The marketplace has a sleazy vendor.
- **Items**:
  - `rmSpell`:  learn the `rm` spell.
  - `mkdirSpell`:  learn the `mkdir` spell.
- **Next Step**: Visit the **ArtisansShop** by typing `cd ArtisansShop`.

---

### **5. ArtisansShop**
- **Initial Text**: The shop is filled with clocks and a working artisan.
- **Items/People**:
  - `Artisan`: Use `less Artisan` to get a quest to create gears.
    - Use touch Gear` to create a gear.



---

### **6. Farm**
- **Initial Text**: The farm is ruined, and the farmer is distressed.
- **Items/People**:
  - `Farmer`: Use `less Farmer` to learn about his ruined crops.
    - Use `cp EarOfCorn AnotherEarOfCorn` to create more corn until the farmer is satisfied.
![[Pasted image 20250224175211.png]]

---

### **7. BrokenBridge**
- **Initial Text**: The bridge is missing a plank.
- **Items**:
  - `Plank`: Use `touch Plank` to create a new plank and fix the bridge.
- **Next Step**: Cross the bridge to reach the **Clearing**.

---

### **8. Clearing**
- **Initial Text**: A crying man sits near rubble.
- **Items/People**:
  - `CryingMan`: Use `less CryingMan` to learn about his destroyed house.
    - Use `mkdir House` to create a new house for him.
![[Pasted image 20250224181854.png]]


---

### **9. OminousLookingPath**
- **Initial Text**: The path leads to a cave blocked by brambles.
- **Items**:
  - `ThornyBrambles`: Use `rm ThornyBrambles`  to remove them.
- **Next Step**: Enter the **CaveOfDisgruntledTrolls**.

---

### **10. CaveOfDisgruntledTrolls**
- **Initial Text**: The cave smells terrible, and a child is trapped in a cage.
- **Items/People**:
  - `UglyTroll`, `UglierTroll`, : Use `rm` to remove the trolls.
  ![[Pasted image 20250224182137.png]]

![[Pasted image 20250224183202.png]]

![[Pasted image 20250224184154.png]]
---





def count_vowels(s):
    vowels = "aeiouAEIOU"
    count = 0
    for char in s:
        if char in vowels:
            count += 1
    return count

# Example usage
string = "Hello, world!"
print(count_vowels(string))  # Output will be 3





```
# Ask for the filename from the user
filename = input("Enter the filename: ")

# Initialize count and sum
count = 0
total_sum = 0

try:
    # Open and read the file
    with open(filename, 'r') as file:
        for line in file:
            # Split the line into words
            words = line.split()
            for word in words:
                # Check if the word is a number
                if word.isdigit():
                    count += 1
                    total_sum += int(word)

    print(f"Count of numbers: {count}")
    print(f"Sum of all numbers: {total_sum}")

except FileNotFoundError:
    print("File not found. Please make sure the filename is correct.")

except Exception as e:
    print(f"An error occurred: {e}")

```













IHTFP

CaveOfDisgruntledTrolls



