# Project Requirements Document

## Project Name: World Time CLI

### Overview

World Time CLI is a Windows command-line application that provides world time information. Users can install it via `winget` and interact with it using the `wt` command. The application supports adding and removing time zones, displaying offsets in a user-friendly manner, and setting language preferences.

### Features

#### 1. Installation

- The application should be installable using Windows Package Manager (`winget`):
  ```sh
  winget install [Vendor].WorldTime
  ```
- This means that the application must be published to the Windows Package Manager Community Repository, ensuring it is publicly available and easily installable by any Windows user.
- Installation should not require additional dependencies; the app must be self-contained.

##### Importance of Vendor in Winget

- The **Vendor** represents the organization or individual responsible for publishing the application.
- Ensures **uniqueness** to avoid conflicts with similarly named applications.
- Provides **trust and verification**, as Microsoft requires identity verification.
- Helps manage **updates**, ensuring the correct version is distributed.

#### 2. Command Structure

##### a) Display Local and World Time

- Running `wt` without arguments should display:
  - Local time of the host machine.
  - UTC (Coordinated Universal Time).
  - Additional time zones added by the user.
- Example output:
  ```sh
  ------------------------------------------------------
  | Location       | Local Time         | Offset       |
  ------------------------------------------------------
  | Local         | 12:30 PM           | -           |
  | UTC          | 4:30 PM             | -           |
  | Tashkent     | 7:30 PM             | +5h         |
  ------------------------------------------------------
  ```
- The **Offset** column should display differences from local time in hours (e.g., `+5h`, `-3h`).
- Bonus: Display offset in **plain English** (e.g., "5 hours ahead").

##### b) Adding and Removing Time Zones

- Users can add a time zone using:
  ```sh
  wt add Tashkent
  ```
  - This saves "Tashkent" to the list of tracked time zones.
- Users can remove a time zone using:
  ```sh
  wt remove Tashkent
  ```
  - This deletes "Tashkent" from the list of tracked time zones.

##### c) Language Selection

- Users can set the language using:
  ```sh
  wt language en  # English
  wt language uz  # Uzbek
  wt language ru  # Russian
  wt language kr  # Korean
  ```
- The application should display offsets and other messages in the selected language.

#### 3. Application Requirements

##### a) Development Stack

- **Platform:** Windows CLI
- **Framework:** .NET 8
- **UI Library:** Spectre.Console (for rich CLI output)
- **Data Storage:** Local settings file (ensuring settings persist across OS restarts)

##### b) Standalone Executable

- The application **must be distributed as a standalone executable**, meaning:
  - Users should be able to run it without installing .NET runtime.
  - The build process should use **self-contained deployment**.
  - The final binary should be **packaged and optimized** for performance.

###### Why a Standalone Executable?

- **Portability:** Users can run the app on any Windows machine without installing dependencies.
- **Performance:** Eliminates the need for JIT compilation at runtime.
- **Ease of Deployment:** Simplifies installation and avoids version mismatches.
- **Winget Compatibility:** Ensures smooth installation and execution via `winget install`.

#### 4. Persistence

- User preferences (e.g., added time zones, language settings) should **persist** between sessions.
- Data should be stored in a **configuration file** (e.g., JSON or SQLite).

### Summary

- **Installation via `winget`** to ensure easy deployment.
- **Time zone management** (add/remove time zones).
- **Formatted output** with Spectre.Console for a better CLI experience.
- **Offset display** in both numerical and plain English format.
- **Multi-language support** (English, Uzbek, Russian, Korean).
- **Standalone deployment** to avoid dependency issues.
- **Persistent settings** across sessions for user convenience.

