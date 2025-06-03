
## Optimize Win10 VM – Stop Background RAM & CPU Hogs

---

### 1. **Disable Startup Programs**

1. Press `Ctrl + Shift + Esc` to open **Task Manager**
    
2. Go to **Startup** tab
    
3. Disable everything you don't need (e.g., OneDrive, Skype, Cortana, etc.)
    

---

###  2. **Turn Off Background Apps**

1. Press `Win + I` → Settings
    
2. Go to: `Privacy` → **Background apps**
    
3. Turn off `Let apps run in the background`
    

---

### 3. **Set Windows Performance to 'Best Performance'**

1. `Win + S` → Search: `View advanced system settings`
    
2. Under **Performance** → click **Settings**
    
3. Choose **Adjust for best performance**
    

---

###  4. **Disable Unnecessary Services**

1. Press `Win + R` → type `services.msc`
    
2. Disable or set to **Manual**:
    

|Service Name|Set to|
|---|---|
|Windows Update|Manual|
|Superfetch / SysMain|Disabled|
|Windows Search|Disabled|
|Print Spooler (if not needed)|Disabled|
|Remote Registry|Disabled|
|Xbox Services|Disabled|

⚠️ Only disable what you’re sure about.

---

### 5. **Remove Bloatware**

Run this PowerShell as Admin to remove bundled junk:



`Get-AppxPackage | Remove-AppxPackage`

(Or selectively uninstall apps like Xbox, Mail, News, etc.)

-puru