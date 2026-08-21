# In-Class Activity: Creating and Resolving Merge Conflicts

## Overview

In this activity, you will work in a group to intentionally create Git merge conflicts and then resolve them correctly.

Merge conflicts are a normal part of collaborative development. They happen when Git cannot automatically determine how to combine changes made to the same part of a file.

By the end of this activity, you should be able to:

- Create and work on Git branches
- Push branches to GitHub
- Merge branches into `main`
- Recognize a merge conflict
- Read Git conflict markers
- Resolve conflicts correctly
- Complete and commit a merge
- Abort a merge safely when needed
- Use Git commands to investigate repository state

---

## 1. Get Into Groups

Form groups of **4 students**.

Assign each person one of these roles:

- **Student 1:** HTML Heading Change A
- **Student 2:** HTML Heading Change B
- **Student 3:** CSS Header Design A
- **Student 4:** CSS Header Design B

You will all work in the same GitHub repository.

---

## 2. Create a Shared Repository

One group member should:

1. Create a new **public GitHub repository**.
2. Invite the other three group members as **collaborators**.
3. Create the following files:

```text
index.html
style.css
```

Use this starter `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Git Collaboration Lab</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <header>
        <h1>Our Team Website</h1>
        <p>Welcome to our collaborative project.</p>
    </header>

    <main>
        <h2>About Our Team</h2>
        <p>We are learning how to collaborate with Git and GitHub.</p>
    </main>
</body>
</html>
```

Use this starter `style.css`:

```css
body {
    font-family: Arial, sans-serif;
    background-color: white;
    color: black;
}

header {
    background-color: lightgray;
    padding: 20px;
}
```

Commit and push these starter files to `main`.

---

## 3. Everyone Clones the Repository

Each group member should clone the shared repository:

```bash
git clone <repository-link>
cd <repo-folder>
```

Then check the repository status:

```bash
git status
```

Everyone should be on `main` with a clean working directory before continuing.

---

# Part 1: Create an HTML Merge Conflict

## 4. Student 1 Creates a Branch

Student 1 creates a branch using their first name:

```bash
git checkout -b <your-name>
```

Example:

```bash
git checkout -b alex
```

Confirm the active branch:

```bash
git branch
```

---

## 5. Student 1 Changes the Heading

In `index.html`, Student 1 changes:

```html
<h1>Our Team Website</h1>
```

to:

```html
<h1>Welcome to Team Awesome</h1>
```

Then commit and push the change:

```bash
git add index.html
git commit -m "Update site heading"
git push -u origin <your-name>
```

---

## 6. Student 2 Creates a Different Branch

Student 2 should also create a branch from the original `main` branch.

```bash
git checkout -b <your-name>
```

Student 2 changes the **same original line**:

```html
<h1>Our Team Website</h1>
```

to something different:

```html
<h1>Welcome to the Git Masters</h1>
```

Then commit and push:

```bash
git add index.html
git commit -m "Change team heading"
git push -u origin <your-name>
```

> **Important:** Student 2 should not pull Student 1's work before creating this change. Both branches need to contain different edits to the same original line.

---

## 7. Merge Student 1's Branch Into `main`

Choose one group member to act as the **integrator** for this part of the activity.

Switch to `main`:

```bash
git checkout main
git pull
```

Merge Student 1's branch:

```bash
git merge <student-1-branch>
```

Then push:

```bash
git push origin main
```

This merge should complete normally.

---

## 8. Merge Student 2's Branch

Now try to merge Student 2's branch:

```bash
git merge <student-2-branch>
```

Git should stop and report a merge conflict similar to:

```text
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

**Do not panic and do not delete random code.**

Git is asking you to decide what the final file should contain.

---

## 9. Inspect the Conflict

Run:

```bash
git status
```

Then open `index.html`.

You should see conflict markers similar to:

```html
<<<<<<< HEAD
<h1>Welcome to Team Awesome</h1>
=======
<h1>Welcome to the Git Masters</h1>
>>>>>>> student2
```

### What the markers mean

```text
<<<<<<< HEAD
```

Everything below this line, until `=======`, is the version currently on the branch you are on.

```text
=======
```

This separates the two competing versions.

```text
>>>>>>> student2
```

Everything above this line, back to `=======`, came from the branch being merged.

---

## 10. Resolve the Conflict

Your group must decide what the final heading should be.

You may keep Student 1's version:

```html
<h1>Welcome to Team Awesome</h1>
```

You may keep Student 2's version:

```html
<h1>Welcome to the Git Masters</h1>
```

Or you may combine the ideas into something new:

```html
<h1>Welcome to the Awesome Git Masters</h1>
```

When finished, delete all conflict marker lines.

The file must contain **none** of these:

```text
<<<<<<<
=======
>>>>>>>
```

Save the file.

---

## 11. Finish the Merge

Stage the resolved file:

```bash
git add index.html
```

Check your status:

```bash
git status
```

Commit the resolution:

```bash
git commit -m "Resolve heading merge conflict"
```

Push the final result:

```bash
git push origin main
```

Then inspect the project history:

```bash
git log --oneline --graph --all
```

---

# Part 2: Create a CSS Merge Conflict

## 12. Students 3 and 4 Update `main`

Before creating new branches, Students 3 and 4 should make sure they have the latest version of `main`:

```bash
git checkout main
git pull origin main
```

---

## 13. Student 3 Creates a CSS Branch

Create a new branch:

```bash
git checkout -b <student-3-name>-colors
```

Change this block in `style.css`:

```css
header {
    background-color: lightgray;
    padding: 20px;
}
```

to:

```css
header {
    background-color: navy;
    padding: 20px;
    color: white;
}
```

Commit and push:

```bash
git add style.css
git commit -m "Add navy header design"
git push -u origin <student-3-name>-colors
```

---

## 14. Student 4 Creates a Different CSS Branch

Student 4 should also begin from the same version of `main`.

Create a branch:

```bash
git checkout -b <student-4-name>-colors
```

Change the same CSS block to:

```css
header {
    background-color: darkgreen;
    padding: 40px;
    color: yellow;
}
```

Commit and push:

```bash
git add style.css
git commit -m "Add green header design"
git push -u origin <student-4-name>-colors
```

---

## 15. Merge Student 3's CSS Branch

On the integrator's computer:

```bash
git checkout main
git pull
git merge <student-3-name>-colors
git push origin main
```

This merge should succeed.

---

## 16. Merge Student 4's CSS Branch

Now run:

```bash
git merge <student-4-name>-colors
```

You should receive another merge conflict.

The conflict may look similar to:

```css
<<<<<<< HEAD
header {
    background-color: navy;
    padding: 20px;
    color: white;
}
=======
header {
    background-color: darkgreen;
    padding: 40px;
    color: yellow;
}
>>>>>>> student4-colors
```

As a group, decide what the final design should be.

For example, you might create a third version:

```css
header {
    background-color: navy;
    padding: 40px;
    color: white;
}
```

Remember: resolving a conflict does **not** always mean choosing one side. You may combine both changes or write something new.

Then complete the merge:

```bash
git add style.css
git commit -m "Resolve CSS merge conflict"
git push origin main
```

---

# Part 3: Create and Resolve a Conflict Without Step-by-Step Help

## 17. Group Challenge

Your group must now intentionally create another merge conflict without following exact code-change instructions.

### Requirements

Two students must:

1. Start from the same current version of `main`.
2. Create separate branches.
3. Modify the **same `<p>` element** in `index.html` differently.
4. Commit their changes.
5. Push their branches.
6. Merge the first branch into `main`.
7. Attempt to merge the second branch.
8. Identify the conflict.
9. Resolve the conflict.
10. Commit the resolution.
11. Push the final version to GitHub.

Use these commands when you need to investigate what Git is doing:

```bash
git status
git branch
git branch -r
git log --oneline --graph --all
git diff
```

---

# Part 4: Practice Aborting a Merge

## 18. Create Another Conflict

Create another intentional conflict using two branches.

When Git reports the conflict, **do not resolve it yet**.

First run:

```bash
git status
```

Then cancel the merge:

```bash
git merge --abort
```

Run:

```bash
git status
```

Your repository should return to the state it was in before you attempted the merge.

This command is useful when you begin a merge and decide that you are not ready to resolve it yet.

---

# Part 5: Final Team Website

## 19. Finish the Site

Your final `main` branch must contain contributions from all four group members.

The finished site must include:

- A customized `<h1>`
- A customized introductory paragraph
- A customized header design
- At least one additional HTML section
- Valid HTML
- Valid CSS
- Contributions from all four students
- No unresolved merge conflicts

Before submitting, make sure none of these appear anywhere in your files:

```text
<<<<<<<
=======
>>>>>>>
```

Test the website in a browser before continuing.

---

## 20. Host the Site on GitHub Pages

1. Go to your repository on GitHub.
2. Open **Settings**.
3. Open **Pages**.
4. Configure GitHub Pages to publish from the `main` branch.
5. Wait for GitHub to publish the site.
6. Open the published URL and confirm that the site works.

---

# Submission

## 21. Submit the Following

Each group should submit:

- A link to the GitHub repository
- A link to the GitHub Pages website
- A screenshot of at least one merge conflict
- A screenshot showing the same file after the conflict was resolved
- The output of:

```bash
git log --oneline --graph --all
```

Each student should also answer the following questions in their own words:

1. Why did Git create the merge conflict?
2. What does `HEAD` represent inside a conflict?
3. How did your group decide which code to keep?
4. Could Git have safely decided which version was correct automatically? Why or why not?
5. What does `git merge --abort` do?
6. What command helped you most when determining what was happening in the repository?

---

# Git Merge Conflict Quick Reference

## Check what is happening

```bash
git status
```

## View branches

```bash
git branch
```

## View remote branches

```bash
git branch -r
```

## View commit history

```bash
git log --oneline --graph --all
```

## Merge another branch into your current branch

```bash
git merge <branch-name>
```

## After resolving a conflict

```bash
git add <file-name>
git commit -m "Resolve merge conflict"
```

## Cancel a merge that is currently in conflict

```bash
git merge --abort
```

---

## Remember

A merge conflict does **not** mean you broke Git.

It means Git found two different changes and does not know which final result you want.

Your job is to:

1. Read both versions.
2. Decide what the final code should be.
3. Remove the conflict markers.
4. Test the code.
5. Stage the resolved file.
6. Commit the resolution.

**Do not guess. Read what Git is telling you.**
