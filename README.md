# hello-devops

Go to https://github.com
 and log in to your account.

Click the “+” icon in the top right corner and select “New repository”.

Fill out the repository details:

Repository name: hello-devops

Description: (optional) e.g. "A starter project for DevOps practice."

Public or Private: choose as per your preference.

✅ Check the box “Initialize this repository with a README”

Click Create repository.

You now have a repository named hello-devops with a README.md file inside.

✅ Option 2: Using Command Line (Git CLI)
Prerequisites:

Git must be installed (git --version)

You must be authenticated to GitHub via SSH or a personal access token

🔧 Steps:

Create a local folder:

mkdir hello-devops
cd hello-devops


Initialize Git and add a README.md:

git init
echo "# hello-devops" > README.md
git add README.md
git commit -m "Initial commit with README"


Create a new GitHub repo from the command line:

If you're using the GitHub CLI (gh), run:

gh repo create hello-devops --public --source=. --remote=origin --push


This will create the GitHub repository and push your local code (including the README.md) to GitHub.


Your repository hello-devops will now be live at:

https://github.com/mohamadfarook/hello-devops


It will contain:

hello-devops/
└── README.md


