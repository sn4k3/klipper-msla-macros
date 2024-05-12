# Klipper macros for mSLA

This is a collection of macros for the Klipper 3D printer firmware for mSLA printers.  
Based on [jschuh / klipper-macros](https://github.com/jschuh/klipper-macros) work.

✅ Use only for mSLA and DLP.  
❌ Do not use with FDM! For FDM use [jschuh / klipper-macros](https://github.com/jschuh/klipper-macros) instead.

# Installation

To install the macros, first clone this repository inside of your printer_data/config directory with the following command.

```bash
cd ~/printer_data/config
git clone https://github.com/sn4k3/klipper-msla-macros.git
```

## Moonraker Configuration

You can have Moonraker to keep macros up to date by adding the following into your **moonraker.conf**.

```ini
[update_manager klipper-msla-macros]
type: git_repo
origin: https://github.com/sn4k3/klipper-msla-macros.git
path: ~/printer_data/config/klipper-msla-macros # UPDATE THIS FOR YOUR PATH!!!
primary_branch: main
is_system_service: False
managed_services: klipper
```

## Klipper Setup

Then paste the below sections into your printer.cfg to get started. Or even better, paste all of it into a seperate file in the same path as your config, and include that file. That will make it easier if you want to remove these macros in the future.

You may need to customize some settings for your own config. All configurable settings are in **globals.cfg**, and can be overridden by creating a corresponding variable with a new value in your **[gcode_macro _km_options]** section. Do not directly modify the variable declarations in globals.cfg. The macro initialization assumes certain default values, and direct modifications are likely to break things in very unexpected ways.

```ini
[virtual_sdcard]
path: ~/printer_data/gcodes
on_error_gcode: CANCEL_PRINT

[pause_resume]

[display_status]

[respond]

# The sections below here are required for the macros to work. If your config
# already has some of these sections you should merge the duplicates into one
# (or if they are identical just remove one of them).
[idle_timeout]
gcode:
  _KM_IDLE_TIMEOUT # This line must be in your idle_timeout section.

[save_variables]
filename: ~/printer_data/variables.cfg # UPDATE THIS FOR YOUR PATH!!!

# All customizations are documented in globals.cfg. Just copy a variable from
# there into the section below, and change the value to meet your needs.

[gcode_macro _km_options]
# Z position to park toolhead on M600 (set "max" to infer from stepper config).
variable_m600_park_z: "max"
# Z position to park toolhead on print end (set "max" to infer from stepper config).
variable_print_end_park_z: "max"
# Delay in milliseconds after resuming a paused print for plate to stabilize.
variable_print_resume_delay: 4000
gcode: # This line is required by Klipper.
# Any code you put here will run at klipper startup, after the initialization for these macros. 
```

# Troubleshooting

- Double check that you followed the installation instructions and are not seeing any console or log errors.
- Ensure that you're running the most current version of stock Klipper, and not a fork or otherwise altered or outdated copy.
- Ensure you're using the most current version of these macros and haven't made changes to any files in the klipper-macros directory.
- Ensure that you've restarted Klipper after any updates or config changes.
- Run CHECK_KM_CONFIG in the Klipper console and fix any errors it reports to the console and/or logs (it won't output anything if no config errors were detected).
- Verify your slicer settings and review that the gcode output is correct. Pay particular attention the initialization portions of the gcode and the parameters passed to PRINT_START.
- Look for similar issues and post troubleshooting questions in the Github Q&A Discussion.

# Contributing

I'm happy to accept bugfix PRs. I'm also potentially open to accepting new features or additions. However, I may decline the PR if it's something I'm not interested in or just looks like it would be a hassle for me to maintain.

## Formatting
There's no standard style for Klipper macros, so please just try to follow the style in the files. That stated, here are a few rules to remember:

- Wrap at 80 characters if at all possible
- Indent 2 spaces, and in line with the logical block when wrapping (no tabs)
- Prefix internal macros with _ or _km_
- Prefix any sort of global state with _KM_ (e.g. _KM_SAVE_GCODE_STATE)

## Commit Messages
These are the rules for commit messages, but you can also just look at the commit log and follow the observed pattern:

- Use the 50/72 rule for commit messages: No more than 50 characters in the title and break lines in the description at 72 characters.
- Begin the title with the module name (usually the main file being modified, minus any extension) followed by a colon.
- Title-only commit messages are fine for simple commits, but be sure to include a blank line after the title.
- Squash multiple commits if what you're working on makes more sense as a single logical commit. This might require you to do a force push on an open PR.