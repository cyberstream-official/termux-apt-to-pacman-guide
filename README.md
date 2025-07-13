# **Switching Termux Package Manager: From APT to Pacman**

This guide provides step-by-step instructions for replacing the default APT package manager in Termux with Pacman.

---

## **Instructions:**

The following steps will guide you through the complete process of setting up a new environment, installing Pacman, and switching over from the default package manager.

### **1. Creating a New Environment**

This initial step creates a separate directory for the new Pacman environment, ensuring that the existing APT-managed setup is not affected during the installation process.

```zsh
mkdir "$PREFIX/../usr-n" && cd "$_"
```

### **2. Pacman Bootstrap Setup**

This command downloads and extracts the core files required to get Pacman running on your device's specific architecture.

```zsh
curl -s "https://api.github.com/repos/termux-pacman/termux-packages/releases/latest" | grep -o "https://github.com/termux-pacman/termux-packages/releases/download/[^\"]*bootstrap-$(uname -m).zip" | head -1 | xargs -I {} sh -c 'cd "$PREFIX/../usr-n" && curl -L -o bootstrap.zip {} && unzip bootstrap.zip && rm bootstrap.zip'
```

> [!NOTE]
> A stable internet connection is required for this step to complete successfully.

### **3. Setting Up Symbolic Links**

This command creates essential symbolic links within the new environment, allowing Pacman and its packages to function correctly.

```zsh
awk -F "←" '{system("ln -s ""$1"" ""$2""")}' SYMLINKS.txt
```

> [!NOTE]
> The `SYMLINKS.txt` file is included in the bootstrap files downloaded in the previous step.

### **4. Activating the Pacman Environment**

This is the most critical phase where you will switch from the old APT environment to the new Pacman environment.

1.  #### **Launching a Failsafe Session:**

    Starting a failsafe session is crucial as it prevents Termux from closing unexpectedly while you are modifying its core directories.

    1.  **Method 1: From within the Termux Application**
        1.  Open the Termux application.
        2.  Swipe from the left edge of the screen to the right to open the navigation drawer.
        3.  Long-press the "**NEW SESSION**" button.
        4.  A menu will appear. Tap on "**FAILSAFE**" to start the failsafe session.
    2.  **Method 2: From Your Device's Home Screen**
        1.  Navigate to the Termux application icon on your device's launcher or home screen.
        2.  Long-press the Termux icon until a context menu appears.
        3.  Select "**Failsafe**" from the menu options.

2.  #### **Replacing the Old Environment:**

    This command finalizes the switch by replacing the original `usr` directory with the new Pacman environment you have just set up.

> [!warning]
> This is a destructive and irreversible action. It will permanently delete your previous APT environment and all packages installed with it. Ensure you have backed up any critical data before proceeding.

    ```zsh
    rm -rf "$PREFIX/../usr" && mv "$PREFIX/../usr-n" "$PREFIX/../usr"
    ```

### **5. Initializing Pacman**

These final commands initialize Pacman's security keys, which are necessary to verify the authenticity of packages before installation.

```zsh
pacman-key --init
pacman-key --populate
```

---

## **QuickStart Guide:**

Here are the basic commands to get you started with managing packages using Pacman.

- ##### **Updating and Upgrading All Packages:**
  This command synchronizes the package database and upgrades all installed packages to their latest versions.
  ```zsh
  pacman -Syu
  ```
- ##### **Installing Packages:**
  Use this command to install a new package.
  ```zsh
  pacman -S <package_name>
  ```
- ##### **Uninstalling Packages:**
  Use this command to remove an installed package.
  ```zsh
  pacman -R <package_name>
  ```
  [Click here for more pacman commands.](https://wiki.archlinux.org/title/Pacman)

### **Reference:**

- [Termux Wiki - Switching Package Manager](https://wiki.termux.com/wiki/Switching_package_manager)
- [ArchWiki - Pacman](https://wiki.archlinux.org/title/Pacman)
