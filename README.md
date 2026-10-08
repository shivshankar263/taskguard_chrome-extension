# 🛡️ Task Guard

**Task Guard** is a Manifest V3 Chrome extension designed to enforce focus and prevent accidental tab closures. It ensures you cannot close your browser until you explicitly mark your designated primary task as complete. 

If you accidentally force-quit Chrome, Task Guard will automatically restore your primary task tab upon restart and issue a desktop notification.

---

## ✨ Features
*   **Enforced Completion:** Injects a `beforeunload` listener into your chosen tab, pausing any browser closure attempts until the task is marked completed in the extension popup.
*   **Visual Banner:** Displays a prominent, click-through red banner at the top of your active task tab so you never lose track of it.
*   **Dynamic Status Icons:** The extension toolbar icon automatically turns **Red** when a task is active and **Green** when it is completed.
*   **Tab Selector:** Easily choose any currently open tab as your primary task through a clean UI wizard.
*   **Task Schedules:** 
    *   *Daily Tasks:* Automatically reset at midnight.
    *   *Permanent Tasks:* Persist until explicitly completed.
*   **Crash Recovery:** If Chrome is force-closed while a task is active, the tab is instantly re-opened on the next launch with a native desktop reminder notification.

---

## 🛠️ Installation (Local / Unpacked)

Since this extension is not yet on the Chrome Web Store, you can run it locally in Developer Mode.

1. Download the `.zip` file from this repository and extract it to a folder.
2. Open Google Chrome and navigate to `chrome://extensions/`.
3. Enable **Developer mode** using the toggle switch in the top right corner.
4. Click the **Load unpacked** button.
5. Select the folder containing the extracted extension files.
6. Click the **Puzzle piece** icon in your Chrome toolbar and **Pin** Task Guard for easy access.

---

## 💻 How it Works (Technical Overview)

Task Guard relies on a combination of Chrome APIs to ensure strict task management:
*   **`chrome.scripting`**: Dynamically injects `content.js` into the user's selected tab to apply the visual banner and the closure protection.
*   **`chrome.storage.local`**: Persists the active task's Tab ID, URL, and completion status so it survives browser restarts.
*   **`window.beforeunload`**: Intercepts the browser's shutdown event. Modern browsers restrict custom dialog text here to prevent spam, so it uses the native browser warning. Note: *This requires the user to have interacted with the page (clicked anywhere) at least once for the browser to allow the warning.*
*   **`chrome.runtime.onStartup`**: Checks local storage for incomplete tasks immediately when Chrome launches and triggers recovery protocols if needed.
*   **`chrome.notifications`**: Triggers a system-level alert if Chrome restarts with a pending task.

---

## 📂 Project Structure

```text
├── manifest.json        # Extension configuration & permissions (Manifest V3)
├── background.js        # Service worker for cross-tab logic & startup recovery
├── popup.html           # The UI wizard for selecting and completing tasks
├── popup.js             # Logic for the popup UI and communicating with background.js
├── content.js           # The injected script handling the banner and beforeunload
├── icon-active.png      # Toolbar icon (Red state)
├── icon-completed.png   # Toolbar icon (Green state)
└── README.md
