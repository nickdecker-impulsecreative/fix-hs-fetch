# HubSpot Fetcher Fixer

## AI SLOP

This is ai slop. I've reveiwed and tested this myself. I'd advise testing this into a destination folder that is not your current desitnation folder to ensure you are doing this correct.  You will need to give the file permissions to execut, then you will be able to run teh script. Check the quickstart below, then after that we have the problems on why you need to use this.

This script provides a workaround for the HubSpot CLI bug where `hs fetch <source> <destination>` fails due to the "." character in the destination path.

## Quick Start

1. **Download the `fetcher` file** from this repo (or clone the entire repo)
2. **Open your terminal** and navigate to where you downloaded it
3. **Make the script executable:**
   ```bash
   chmod +x fetcher
   ```
4. **Run the script:**
   ```bash
   ./fetcher <source folder with . in it> <destination folder> 
   ```

That's it! The script will fetch all files from your HubSpot source folder.

### Optional: Add to PATH (so you can run `fetcher` instead of `./fetcher`)

If you want to use `fetcher` from anywhere without the `./`, you can add the current directory to your PATH:

```bash
# Add current directory to PATH (temporary for current session)
export PATH=$PATH:$(pwd)

# Or add permanently to your shell profile
# For zsh (default on macOS):
echo 'export PATH=$PATH:/path/to/your/fetcher/directory' >> ~/.zshrc
source ~/.zshrc

# For bash:
echo 'export PATH=$PATH:/path/to/your/fetcher/directory' >> ~/.bashrc
source ~/.bashrc

# Your terminal may use a different shell - check with: echo $SHELL

# Now you can run it from anywhere:
fetcher <source folder with . in it> <destination folder>
```
```

## Problem

The HubSpot CLI has a bug that prevents fetching entire themes when the destination path contains a "." character:

```bash
# This fails:
hs fetch Omega-2.0 Omega-2.0

# But these work:
hs fetch Omega-2.0/modules Omega-2.0/modules -o
hs fetch Omega-2.0/custom_work ./Omega-2.0/custom_work -o
```

## Solution

This script dynamically uses `hs ls <source>` to get all subfolders and files, then fetches each one individually.

## Usage

```bash
./fetcher <src> <dest> [options]
```

### Examples

```bash
# Fetch Omega-2.0 to Omega-2.0 with overwrite
./fetcher Omega-2.0 Omega-2.0 -overwrite

# Fetch themeversion1.1 to newfolder with overwrite
./fetcher themeversion1.1 newfolder -o

# Fetch foldercity88.7 to foldercity (no overwrite)
./fetcher foldercity88.7 foldercity
```

### Arguments

- `src`: Source theme name in HubSpot Design Manager
- `dest`: Local destination directory path

### Options

- `-o` or `-overwrite`: Overwrite existing files (default: skip existing files)
- `-h` or `--help`: Show help message

## Features

- **Dynamic**: Automatically discovers all subfolders and files using `hs ls`
- **Safe**: Asks for confirmation before proceeding
- **Informative**: Shows colored progress output and summary
- **Flexible**: Works with any theme name and destination
- **Error Handling**: Continues on individual failures and reports summary

## Requirements

- HubSpot CLI (`hs`) must be installed and in PATH
- Must have access to the specified theme in HubSpot Design Manager

## Troubleshooting

### "command not found: fetcher"
You need to use `./fetcher` not just `fetcher`. The `./` tells your terminal to run the file in the current directory.

### "hs: command not found"
Make sure the HubSpot CLI is installed:
```bash
npm install -g @hubspot/cli
```

## Example Output

```
> ./fetcher Omega-2.0 Omega-2.0 -o

==========================================
HubSpot Theme Fetcher
==========================================

[INFO] Getting list of items from Omega-2.0...
[INFO] Found 12 items to fetch:
  - css
  - custom_work
  - expansion_packs
  - images
  - js
  - modules
  - sections
  - templates
  - vendors
  - fields.json
  - license.txt
  - theme.json

This script will fetch all items from 'Omega-2.0' to 'Omega-2.0'
Overwrite mode is enabled (existing files will be replaced)

Do you want to continue? (y/N): y

[INFO] Fetching: Omega-2.0/css -> Omega-2.0/css
[SUCCESS] Successfully fetched: css

[INFO] Fetching: Omega-2.0/custom_work -> Omega-2.0/custom_work
[SUCCESS] Successfully fetched: custom_work

...

==========================================
[SUCCESS] Fetch completed!
[INFO] Successfully fetched: 12 items
==========================================
```
