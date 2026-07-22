# Virtual Environment Setup Guide — QueryGenie Project

This guide explains what a virtual environment is, why we need it, and how to set it up on both Mac and Windows.

---

## What Is a Virtual Environment? (Explained in the Simplest Way)

Imagine your computer has only **one big kitchen**. Every project you ever build has to cook its food in that same kitchen, using the same pots, pans, and ingredients.

Now imagine:
- Project A needs sugar that is exactly 2 years old
- Project B needs sugar that is exactly 5 years old

If there is only one kitchen with only one jar of sugar, both projects cannot get what they need at the same time. One of them will break.

This is exactly what happens with Python projects. Every Python project needs certain **packages** (like `langchain`, `streamlit`, `openai`) and each package has a **version number**. One project might need `langchain==0.2.0`, and a completely different project on the same laptop might need `langchain==0.3.5`. If you install everything directly onto your computer (the "one big kitchen"), the second install will overwrite the first, and your first project will stop working.

A **virtual environment** solves this by giving **each project its own separate, private kitchen**. Nothing installed inside Project A's environment can affect Project B. They live in their own isolated bubble.

---

## Will the Project Work Without a Virtual Environment?

Short answer: **it might work today, but it is risky, and for this course it is compulsory.**

Here is why:

- Without an environment, all packages get installed directly onto your laptop's main Python. This is called the "global" or "system" Python.
- If you (or any other software on your laptop) install a different version of any package later, this project can suddenly stop working, and you will not know why.
- If two of your projects need different versions of the same package, you cannot have both working at the same time without a virtual environment.
- Every student's laptop already has different things installed on it. Without an environment, your teacher cannot guarantee that the same steps will produce the same result on every laptop in the batch.

So yes, technically Python can run without an environment. But for a real project, especially one with many specific package versions (like this one), skipping the environment is like skipping a seatbelt — it might be fine for a short drive, but it is not how it is done properly, and it is not how it is done in real companies.

---

## Do I Need to Activate the Environment Every Single Time?

**Yes.** Every time you close and reopen your terminal (Command Prompt, PowerShell, or Mac Terminal), the environment gets deactivated automatically. This is normal behavior, not a mistake.

So the rule is simple:

> Every time you open a new terminal window to work on this project, activate the environment first, before running any Python or pip command.

You do **not** need to create the environment again and again — you only create it once. But you **do** need to activate it every time you start a new terminal session.

How do you know it is active? Look at the start of your terminal line. If you see `(myenv)` written before your folder path, the environment is active. If you don't see it, it is not active, and you must activate it again before continuing.

---

# PART 1 — MAC SETUP

## Step 1: Open Terminal

Press `Command + Space`, type `Terminal`, and press Enter.

## Step 2: Go to Your Project Folder

```bash
cd ~/Desktop/QueryGenie
```

If your folder is somewhere else, replace the path above with the correct one.

## Step 3: Create the Virtual Environment (Only Once)

```bash
python3.10 -m venv myenv
```

This creates a new, empty, isolated environment named `myenv` inside your project folder. You only run this command one time for the whole project — not every day.

If `python3.10` is not recognized, check your Python 3.10 installation first using:

```bash
python3.10 --version
```

## Step 4: Activate the Environment (Every Time You Work on This Project)

```bash
source myenv/bin/activate
```

After running this, you should see `(myenv)` appear at the beginning of your terminal line. This confirms the environment is active and ready.

## Step 5: Upgrade pip (Recommended, Only Needed Occasionally)

```bash
pip install --upgrade pip
```

## Step 6: Install All Project Packages (Only Once, Unless requirements.txt Changes)

```bash
pip install -r requirements.txt
```

## Step 7: When You Are Done Working, Deactivate (Optional but Good Practice)

```bash
deactivate
```

This exits the environment and returns you to your normal system terminal. You do not have to do this — closing the terminal window also deactivates it automatically. But if you want to keep working in the same terminal window on something else, run this command first.

---

# PART 2 — WINDOWS SETUP

## Step 1: Open Command Prompt or PowerShell

Search for `cmd` or `PowerShell` in the Start Menu and open it.

## Step 2: Go to Your Project Folder

```powershell
cd C:\Users\YourName\Desktop\QueryGenie
```

Replace `YourName` and the path with your actual folder location.

## Step 3: Create the Virtual Environment (Only Once)

```powershell
py -3.10 -m venv myenv
```

This creates the isolated environment named `myenv` inside your project folder. You only need to run this command one single time for the whole project.

## Step 4: Activate the Environment (Every Time You Work on This Project)

```powershell
myenv\Scripts\activate
```

After running this, you should see `(myenv)` appear at the beginning of your terminal line. This confirms it is active.

If you get a security error saying "running scripts is disabled on this system," run this command once, then try activating again:

```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

## Step 5: Upgrade pip (Recommended, Only Needed Occasionally)

```powershell
python -m pip install --upgrade pip
```

## Step 6: Install All Project Packages (Only Once, Unless requirements.txt Changes)

```powershell
pip install -r requirements.txt
```

## Step 7: When You Are Done Working, Deactivate (Optional but Good Practice)

```powershell
deactivate
```

This exits the environment. Closing the terminal window also does this automatically.

---

## Quick Summary Table

| Action | How Often |
|---|---|
| Create the environment (`venv` command) | Only once, ever, per project |
| Activate the environment | Every single time you open a new terminal to work on this project |
| Install packages (`pip install -r requirements.txt`) | Only once, unless the package list changes |
| Deactivate | Optional, only if you want to leave the environment without closing the terminal |

---

## One-Line Rule to Remember

**Before touching any Python file, running any script, or installing any package for this project — always check that `(myenv)` is showing in your terminal first.**

If it is not showing, activate it before doing anything else.