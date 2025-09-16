# HubSpot Theme Fetcher

This script provides a workaround for the HubSpot CLI bug where `hs fetch <theme> ./<destination>` fails due to the "." character in the destination path.

## Problem

The HubSpot CLI has a bug that prevents fetching entire themes when the destination path contains a "." character:

```bash
# This fails:
hs fetch Omega-2.0 ./Omega-2.0

# But these work:
hs fetch Omega-2.0/modules ./Omega-2.0/modules -o
hs fetch Omega-2.0/custom_work ./Omega-2.0/custom_work -o
```

## Solution

This script dynamically uses `hs ls <theme>` to get all subfolders and files, then fetches each one individually.

## Usage

### Basic Syntax
```bash
fetcher <src> <dest> [options]
```

### Examples

```bash
# Fetch Omega-2.0 to Omega-2.0 with overwrite
fetcher Omega-2.0 Omega-2.0 -overwrite

# Fetch themeversion1.1 to newfolder with overwrite
fetcher themeversion1.1 newfolder -o

# Fetch foldercity88.7 to foldercity (no overwrite)
fetcher foldercity88.7 foldercity

# Fetch MyTheme to ./my-local-theme with overwrite
fetcher MyTheme ./my-local-theme -overwrite
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

## Example Output

```
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
