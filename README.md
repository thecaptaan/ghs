# ghs

`ghs` is a small shell script for managing multiple Git and GitHub identities from the terminal. It stores named profiles with Git user info and a GitHub token, then switches your global Git config and authenticates the GitHub CLI (`gh`) with one command.

## Why use it

If you work with multiple GitHub accounts or use different names/email addresses for personal and work projects, `ghs` makes switching easy:

- change global Git identity
- login to `gh` with a saved token
- inspect current Git and `gh` state
- keep profiles in a simple local database

## Requirements

- Bash shell
- `git` installed
- GitHub CLI (`gh`) installed

## Installation

Copy the script to a directory on your `PATH` and make it executable.

```bash
mkdir -p "$HOME/.local/bin"
cp ghs "$HOME/.local/bin/ghs"
chmod +x "$HOME/.local/bin/ghs"
```

If your shell does not already include `~/.local/bin`, add it to your profile:

```bash
export PATH="$HOME/.local/bin:$PATH"
```

## Usage

```bash
ghs add <profile> <git_name> <git_email> <gh_username>
ghs switch <profile>
ghs current
ghs list
ghs remove <profile>
ghs logout
ghs help
```

### Examples

Add a profile:

```bash
ghs add personal "Dev Kumar" dev@example.com devkumar
```

Switch to a saved profile:

```bash
ghs switch personal
```

Show current config and logged-in GitHub user:

```bash
ghs current
```

List saved profiles:

```bash
ghs list
```

Remove a profile:

```bash
ghs remove personal
```

Logout of GitHub CLI:

```bash
ghs logout
```

## How it works

- profiles are stored in `~/.config/git-gh-switch/profiles.db`
- each profile line contains: `profile|git_name|git_email|gh_user|token`
- `ghs add` prompts for a GitHub token; you must create this token yourself in GitHub
- `ghs switch` updates your global Git username/email and uses `gh auth login --with-token`
- `ghs current` shows the current repo/global Git identity and whether `gh` is logged in

## Token note

This script does not generate a GitHub token for you. Create a personal access token in GitHub under:

- `Settings` > `Developer settings` > `Personal access tokens`

Then paste the token when `ghs add ...` asks for it.

## Security notes

- This script stores GitHub tokens in plain text inside `~/.config/git-gh-switch/profiles.db`.
- Protect the directory with correct permissions; the script already sets `700` for the folder and `600` for the file.
- If you need stronger security, consider using an encrypted vault instead of plain text storage.

## License

This project is released under the MIT License.

The MIT License is required for this repository and allows reuse with minimal restrictions.
