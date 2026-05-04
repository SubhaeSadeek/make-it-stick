# Pop!_OS 24.04 Ultimate Bangla Unicode Keyboard Setup (OpenBangla / Avro Style)

A complete persistent setup guide for installing and configuring **Avro-style Bangla Unicode typing** on **Pop!_OS 24.04**, with autostart, proper IBus integration, font support, and common app compatibility fixes.

---

## Why This Guide Exists

On Pop!_OS 24 (especially newer COSMIC-based sessions), third-party input methods like Bangla IMEs can behave inconsistently:

* installed but not typing
* shortcut key not switching
* candidate suggestion popup missing
* works in browser but not VS Code
* disappears after reboot

This guide provides a **battle-tested stable configuration** so Bangla typing works like **Windows Avro Keyboard**.

---

# Phase 1 — Install OpenBangla Keyboard (Official Latest Method)

Open terminal and run:

```bash
bash -c "$(wget -q https://raw.githubusercontent.com/OpenBangla/OpenBangla-Keyboard/master/tools/install.sh -O -)"
```

### Why use this instead of `apt install`?

Because the official installer usually provides:

* latest build
* updated compatibility fixes
* proper dependencies

---

# Phase 2 — Install Full IBus Stack and Helpers

```bash
sudo apt update
sudo apt install ibus ibus-gtk ibus-gtk3 ibus-gtk4 ibus-clutter ibus-m17n im-config dconf-cli -y
```

This installs the Linux input method framework required for Bangla IME communication.

---

# Phase 3 — Force the System to Use IBus

Run:

```bash
im-config -n ibus
```

Then verify:

```bash
im-config
```

Expected output should mention:

```text
Current configuration: ibus
```

This ensures Pop!_OS does not try to bypass the input framework.

---

# Phase 4 — Add Required Environment Variables (Critical)

Open your profile file:

```bash
nano ~/.profile
```

Add the following lines at the bottom:

```bash
export GTK_IM_MODULE=ibus
export QT_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export CLUTTER_IM_MODULE=ibus
```

Save with:

```text
CTRL+O → ENTER → CTRL+X
```

### Why this matters

These variables force:

* GTK apps
* QT apps
* Chromium apps
* desktop shell apps

to properly recognize the IBus Bangla input method.

Without this, Bangla often installs but behaves half-dead.

---

# Phase 5 — Create Automatic IBus Startup After Login

Create autostart directory and config:

```bash
mkdir -p ~/.config/autostart
nano ~/.config/autostart/ibus.desktop
```

Paste:

```ini
[Desktop Entry]
Type=Application
Exec=ibus-daemon -drx
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name=IBus Daemon
Comment=Start IBus input method daemon
```

Save and exit.

### What this does

Every time you log in, Pop!_OS automatically launches:

```bash
ibus-daemon -drx
```

This prevents:

* IME disappearing after reboot
* source switching failing
* Bangla input silently dying

---

# Phase 6 — Reboot the System Fully

Do a complete reboot.

Not just logout.

A full reboot is needed so:

* environment variables load
* IBus daemon initializes cleanly
* keyboard framework resets correctly

---

# Phase 7 — Add OpenBangla as an Input Source

After reboot:

Go to:

```text
Settings → Keyboard → Input Sources → Add
```

Search for:

```text
Bangla
```

Add:

```text
Bangla (OpenBangla Keyboard)
```

If not visible, also check under **Other**.

---

# Phase 8 — Set a Manual Keyboard Switch Shortcut

Go to:

```text
Settings → Keyboard Shortcuts
```

Set input source switching to one of these:

```text
Super + Space
```

or

```text
Alt + Shift
```

### Important

Disable any conflicting default source switch shortcut.

Otherwise the system may intercept the key before IBus does.

---

# Phase 9 — Configure OpenBangla Preferences

Search app menu for:

```text
OpenBangla Keyboard Preferences
```

Inside preferences choose:

## Input Method:

```text
Avro Phonetic
```

Enable these options:

* candidate window
* dictionary suggestion
* smart backspace
* auto correct
* inline preedit

This gives you the near-Windows-Avro typing experience.

---

# Phase 10 — Install Good Bangla Unicode Fonts

```bash
sudo apt install fonts-noto-core fonts-lohit-beng-bengali fonts-sil-charis -y
```

These fonts ensure:

* proper Bangla glyph rendering
* no broken square boxes
* nicer browser/editor display

---

# Phase 11 — Fix Bangla Input in VS Code / Chrome / Electron Apps

Some Electron/Chromium apps ignore IME under modern sessions.

Use these launch commands:

## VS Code

```bash
code --enable-features=UseOzonePlatform --ozone-platform=x11
```

## Google Chrome

```bash
google-chrome --ozone-platform=x11
```

This resolves many:

* candidate popup issues
* composition problems
* missing Bangla suggestion windows

---

# Phase 12 — Test Bangla Unicode Typing

Open any text editor and type:

```text
ami banglay unicode likhte pari
```

Expected Bangla output:

```text
আমি বাংলায় ইউনিকোড লিখতে পারি
```

If this works, the setup is successful.

---

# Optional: Manual Emergency Restart Command

If Bangla ever stops responding during a session, run:

```bash
ibus-daemon -drx
```

This instantly revives the input daemon.

---

# Optional Bonus Features Available Later

This guide can be extended with:

* top bar EN | অ tray indicator
* permanent VS Code launcher fix
* terminal Bangla input support
* browser-wide candidate popup tuning
* Telegram/Discord/LibreOffice compatibility tweaks

---

# Recommended Final Stack

The most stable Bangla Unicode typing stack on Pop!_OS 24 is:

```text
IBus + OpenBangla Keyboard + Avro Phonetic Layout + Unicode Fonts + Autostart IBus
```

---

# End Result

You get:

* Avro style phonetic Bangla typing
* full Unicode output
* persistent after reboot
* suggestion popup
* stable systemwide Bangla input

Essentially: **Windows Avro experience, but on Linux.**

---

If any phase fails during installation, run the commands step by step and inspect terminal output before moving to the next phase.

