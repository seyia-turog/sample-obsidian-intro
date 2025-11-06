# 🧭 Overview

Here’s what we’ll do:

1. Clone & set up the repo locally  
2. Check existing branches (to find the latest code from the frontend dev)  
3. Create a safety backup of `{{target_branch}}`  
4. Merge the latest frontend branch into `{{target_branch}}`  
5. Resolve any conflicts (if they appear)  
6. Push & verify  

> 🛠️ **Requirements:**  
> - Git installed  
> - Access to push & create branches  
> - Confirm your access level (CTO or admin can elevate if needed)

---

## 🧱 Step 1. Clone the repo

If you haven’t already, run:

```bash
git clone {{repo_url}}
cd {{repo_folder_name}}
🧱 Step 2. Check the branches
You want to see what branches exist — this helps you identify where the frontend devs have been pushing.

Run:

bash
Copy code
git fetch --all
git branch -a
You’ll see something like:

bash
Copy code
* main
  remotes/origin/dev
  remotes/origin/frontend-update
  remotes/origin/ui-fixes
  remotes/origin/feature/login-page
👉 Ask the frontend dev which branch has the latest code.
Let’s say it’s {{frontend_branch}}.

🧱 Step 3. Create a backup of {{target_branch}}
This is critical — so you can roll back if something breaks.

Run:

bash
Copy code
git checkout {{target_branch}}
git pull origin {{target_branch}}
git checkout -b {{backup_branch_name}}
git push origin {{backup_branch_name}}
✅ This creates a safety branch {{backup_branch_name}} on GitHub.

🧱 Step 4. Merge the latest frontend branch into {{target_branch}}
Switch back to {{target_branch}} and merge the latest frontend updates:

bash
Copy code
git checkout {{target_branch}}
git pull origin {{target_branch}}
git merge origin/{{frontend_branch}}
If you get:

vbnet
Copy code
Already up to date
Then your branch already has everything.
If not, Git will begin merging.

🧱 Step 5. Handle merge conflicts (if any)
If Git shows messages like this:

pgsql
Copy code
Auto-merging src/App.js
CONFLICT (content): Merge conflict in src/App.js
Automatic merge failed; fix conflicts and then commit the result.
Do this:

Open each file mentioned in the message.

You’ll see conflict markers like:

text
Copy code
<<<<<<< HEAD
// code from {{target_branch}}
=======
// code from {{frontend_branch}}
>>>>>>> {{frontend_branch}}
Keep or combine the correct parts (ask the dev if unsure).

Save the files.

Then run:

bash
Copy code
git add .
git commit -m "Resolved merge conflicts from {{frontend_branch}}"
🧱 Step 6. Push updated {{target_branch}}
Once the merge and any conflict resolution are complete, push the updated branch:

bash
Copy code
git push origin {{target_branch}}
✅ That’s it — {{target_branch}} on GitHub now includes the latest code from {{frontend_branch}}.

🧩 Step 7. Verify
If you have an environment to test, run:

bash
Copy code
npm install
npm run build
Or run locally to ensure everything works visually.

🧾 Optional: Communicate Cleanly
Once done, post a message to your team (e.g., on Slack, Teams, or Email):

✅ {{project_name}} Repo Update:

Merged latest {{frontend_branch}} branch into {{target_branch}}

Created safety backup {{backup_branch_name}}

No merge conflicts encountered (or list resolved files)

Ready for deployment testing