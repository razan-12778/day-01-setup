# 🎯 Goal

This first challenge will test if your setup is correct, and help you understand how the bootcamp works.

- We'll update the data-engineering-challenges with the latest changes
- We'll have a look at the data-engineering-challenges repository structure
- A cheatsheet that can help you throughout your bootcamp journey
- Understand how VSCode chooses the correct poetry virtual environment for each challenge
- We'll complete a dummy challenge to see how your repository integrates with Kitt!

# 0️⃣ Updating your challenge repo

Open VS Code and connect to the remote.

Open an integrated terminal (a terminal in VS Code that is).

Move to the challenges folder:

```bash
cd ~/code/<user.github_nickname>/data-engineering-challenges
```

Let's update the repo to match your specific version of the bootcamp:

```bash
git reset --hard upstream/main-sda
```

Then:

```bash
git push --force origin main
```

> :warning: Never execute these commands again after this point: it will delete all the changes you have made during the bootcamp!
> 
> Going forward, at the start of every challenge, you will run `git pull upstream main-sda -X ours`. This will update your branch with the latest changes we made to the challenges, without overwriting your own code. Occasionally you'll run into a merge conflict, but that will be good practice for you.

Next, we will execute the last step of the installation process.

From the `data-engineering-challenges` folder root **on your VM** (you should already be there), we'll run `make install`, which triggers 2 operations:

- `make allow-envrc`: allow `direnv` to execute inside each folder (otherwise you have to manually "`direnv allow`" it)
- `make own-repo`: allows your user to be the linux "owner" of all files in this challenge folder

Let's make!

```bash
make install
```

This might take a while. You can continue reading, and execute the commands in another terminal. Make sure to keep this one open.

⚠️ If at the very end of this process you get a few errors like: `direnv: error .envrc file not found` - that is normal and nothing to worry about 👌 If you're concerned by anything, feel free to open a ticket with a TA!


<br>

# 1️⃣ Understand your repository structure

❓ Take a look at your `data-engineering-challenges` structure with `tree -a -L 2` and take some time to understand it.

```bash
.
├── 01-Software-Engineering-Best-Practices # Module level
│   ├── 01-Setup                           # Unit level
│   │   ├── 00-Test-Setup                  # Challenge level
│   │   │   ├── app
│   │   │   │   ├── __init__.py
│   │   │   │   └── main.py                # YOUR CODE HERE
│   │   │   ├── tests
│   │   │   │   ├── __init__.py
│   │   │   │   └── test_your_function.py  # OUR TEST CODE HERE
│   │   │   ├── .venv                      # your virtual-env for this challenge, created automatically by poetry
│   │   │   ├── .envrc                     # direnv call to activate your poetry venv as soon as you cd into the foler
│   │   │   ├── makefile                   # Contains `make test` and `make install` commands for you
│   │   │   ├── poetry.lock                # Created by you when running `make install`
│   │   │   ├── pyproject.toml             # We already wrote this for you so that poetry install will create all you need
│   │   │   └── README.md                  # Kitt-displayed readme
...
...
...
├── .dockerignore
├── .gitignore          # globally ignore file pattern (.env, etc...)
├── CHEATSHEET.md       # Some tips for you
├── Makefile            # Gobal bootcamp commands (e.g. run all `make install` for each challenges, run all tests etc...)
├── make.inc            # This file is accessed by every challenges-level makefile (for refactoring purposes)
├── README.md
├── common              # Le Wagon shared logic between all challenges (used for test purposes)
├── direnvrc-template   # You can remove it once you've added it to your ~/.direnvrc
└── yapf                # Formatting rules for you to auto-format your code
```

<br>

# 2️⃣ CHEATSHEET.md

👉 Read the `CHEATSHEET.md`  we created for you to help you throughout the camp! At least, focus on section 1️⃣ to 3️⃣ now

> 💡 You can use VSCode to render HTML by clicking on the top-right icon, or "Command Palette (Cmd-Shift-P)" --> "Open Preview to the side".

Although you can view it in VSCode, we recommend looking at it on GitHub at [this link](https://github.com/lewagon/data-engineering-challenges/blob/main/CHEATSHEET.md) - bookmark it for easy access!

<br>

# 3️⃣ How does VSCode know what poetry virtual environment to use?

We are using poetry to manage the python virtual environments and package dependencies for each individual challenge in the bootcamp. If you want to have a look yourself, they are contained in the `.venv/` directory in the root of each challenge folder!

For VSCode to recognise the virtual environment for a challenge, you **must** open VSCode at the **root of a challenge directory**. For this **Test Setup** challenge it would be:

```bash
code ~/code/<user.github_nickname>/data-engineering-solutions/01-Software-Engineering-Best-Practices/01-Setup/00-Test-Setup
```

Because each challenge has a separate virtual environment we don't have to worry about managing python package dependencies so that they work with ALL challenges 😉

This **VSCode hook** was set during your VM setup by modifying the following line in your **VSCode: Remote Settings (JSON)** to the following:

```bash
"python.defaultInterpreterPath": ".venv/bin/python",
```

This setting means that VSCode will look for a python binary (and associated virtual environment) at the location of `.venv/bin/python` **from the location you opened VSCode**.

💡 If you run into any issues with `python interpreter not found` when working on python files, make sure VSCode has been opened to the challenge root.

<br>

# 4️⃣ Try to pass the tests of this dummy challenge

❓ Open VSCode at `00-Test-Setup` level, then try to fill your code in `app.main`

Some challenges will create the virtual environment and install packages when you `cd` into them (thanks to `layout poetry` in the `.envrc`). That is not always the case though. So whenever you get a `module not found` error, try to run `poetry install` to ensure all packages are installed.

For most challenges there will be an included `Makefile` that has an associated function to test your code: `make test`

Have a look at the `tests/` folder:
- There are some optional tests that will NOT be checked by running `make test` (which does a `pytest -m "not optional"` under the hood: have a look in `make.inc` to see how it's working!)
- Optional tests can still be tested by running pytest manually (e.g `pytest tests/test_our_function.py`)

To test your code run:
```bash
make test
```

It should PASS (hopefully 😅) and you should see a new `test_output.txt` file created. This `test_output.txt` file is used by Kitt to track your progress during the unit. But for that to happen, you need to push your code to github first!

💡 A reminder of some useful git functions:

```bash
# Add files or all the files in a directory to staging
git add <my_first_file.py> <my_second_file.txt>
git add <my_directory/>

# Commit the staged changes
git commit -m "my meaningful commit message"

# Push the changes to the remote repository - github in our case
git push origin main
```

Check your progress status on Kitt's challenge page at the top right corner! It should be green.

You're done with the challenge, time to clean up. Do this at the end of every challenge.

```bash
rm -rf .venv
```

This will delete the files of your virtual environment. You're done, so you don't need it anymore. You can recreate it with `poetry install` anyway. If you don't delete these at the end of very challenge, your drive will quickly fill up. So don't forget this!

Don't forget to follow the progress of your buddy of the day and help them out if needs be!

<br>
