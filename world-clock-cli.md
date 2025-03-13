# Project Requirements Document

## Project Name: World Time CLI

### Overview
World Time CLI is a Windows command-line application that provides world time information. Users can install it via `winget` and interact with it using the `wt` command. The application supports adding and removing time zones, displaying offsets in a user-friendly manner, and setting language preferences.

### Features

#### 1. Installation
- The application should be installable using `winget`:
  ```sh
  winget install [Vendor].WorldTime
  ```
- The application should be hosted in the Windows Package Manager Community Repository.

#### 2. Command Structure
- `wt` (without arguments) should display:
  - Local time
  - UTC time
  - Additional time zones added by the user
- `wt add [timezone]`
  - Adds a new timezone to the world time list
  - Example:
    ```sh
    wt add Tashkent
    ```
- `wt remove [timezone]`
  - Removes a timezone from the world time list
  - Example:
    ```sh
    wt remove Tashkent
    ```
- `wt language [lang_code]`
  - Sets the display language (supported: `uz`, `en`, `ru`, `kr`)
  - Example:
    ```sh
    wt language uz
    ```

#### 3. Display Formatting
- The application should use Spectre.Console for improved CLI visualization.
- The output should be formatted in a user-friendly way, e.g.:
  ```sh
  ┌───────────┬───────────┬─────────────┐
  │ Timezone  │ LocalTime │ Offset      │
  ├───────────┼───────────┼─────────────┤
  │ Local     │ 10:00 AM  │ -           │
  │ UTC       │ 02:00 PM  │ 0h          │
  │ Tashkent  │ 07:00 PM  │ +5h (5 hours ahead) │
  └───────────┴───────────┴─────────────┘
  ```
- Offsets should be displayed both in numeric format (`+5h, -3h`) and in plain English (`5 hours ahead, 3 hours behind`).

#### 4. Persistence
- The application should store user settings (time zones, language preferences) so they persist across system reboots.
- Storage options:
  - JSON configuration file in the user profile directory.
  - Windows registry (optional but not preferred).

#### 5. Technical Stack
- **Language:** C#
- **Framework:** .NET 8
- **CLI UI Library:** Spectre.Console
- **Packaging:** Self-contained .NET binary (must run even if .NET is not installed on the machine)
- **Deployment:** Hosted in the Windows Package Manager Community Repository

#### 6. Bonus Features
- Multi-language support (English, Uzbek, Russian, Korean).
- Enhanced offset descriptions in plain English.

### Deliverables
1. **Executable Binary:**
   - Self-contained .NET 8 application.
   - Packaged and installable via `winget`.
2. **Source Code Repository:**
   - GitHub repository with public access.
   - MIT License or similar open-source license.
3. **Documentation:**
   - ReadMe file with installation and usage instructions.
   - Contribution guidelines.
4. **Packaging & Distribution:**
   - Submission to Windows Package Manager Community Repository.
   - Validation that it works across Windows systems.

### Success Criteria
- Successful installation via `winget`.
- Consistent user settings across reboots.
- Properly formatted time display with time zone offsets.
- Functionality tested on multiple Windows versions (Windows 10, 11).
- Multi-language support works correctly.

---

### Notes
- The application should handle invalid inputs gracefully.
- Error messages should be clear and localized.
- The application should be lightweight and optimized for fast execution.

