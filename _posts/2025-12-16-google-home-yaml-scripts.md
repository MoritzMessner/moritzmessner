---
layout: default
title: "Automating Your Smart Home with Google Home YAML Scripts"
date: 2025-12-16
description: "Learn how to create powerful smart home automations using Google Home's YAML script editor with practical examples for cinema mode and night light scenes."
categories: [smart-home, automation, yaml]
---

# Automating Your Smart Home with Google Home YAML Scripts

Google Home's script editor offers a powerful way to create custom automations for your smart home using YAML. While the web interface provides a visual editor, understanding the YAML structure gives you more control and makes it easier to version control your automations.

## Getting Started

To create Google Home scripts:

1. Go to [home.google.com](https://home.google.com)
2. Navigate to the **Automations** tab
3. Press **+ Add new** to create a new script

The script editor uses YAML, which gives you full control over your automations. While it might look intimidating at first, the structured format is actually quite intuitive once you understand the basics.

## Understanding the YAML Structure

Every Google Home automation script follows this basic structure:

```yaml
metadata:
  name: Script Name
  description: What this script does.

automations:
  - starters:
      - type: starter.type.here
    
    condition:  # optional
      type: condition.type.here
    
    actions:
      - type: action.type.here
```

**Key points to remember:**

- `automations:` is a list (each automation starts with `-`)
- `starters:` is a list (each starter starts with `-`)
- `condition:` is **NOT** a list (no `-` before it)
- `actions:` is a list (each action starts with `-`)

## Starters: Triggering Your Automations

### Voice Commands

The most common trigger is a voice command:

```yaml
starters:
  - type: assistant.event.OkGoogle
    eventData: query
    is: "activate cinema mode"
```

This allows you to say "Hey Google, activate cinema mode" to trigger the automation. You can also run it manually from the Google Home app.

### Time-Based Triggers

Schedule automations to run at specific times:

```yaml
starters:
  - type: time.schedule
    at: 07:30
    weekdays:
      - MONDAY
      - TUESDAY
      - WEDNESDAY
      - THURSDAY
      - FRIDAY
```

### Device State Triggers

React when a device changes state:

```yaml
starters:
  - type: device.state.OnOff
    device: Samsung The Frame 50 - Living Room
    state: on
    is: true
```

This triggers when the TV turns on, perfect for creating automatic ambiance adjustments.

## Conditions: Adding Intelligence

Conditions make your automations context-aware. Only after sunset should your cinema mode activate automatically:

```yaml
condition:
  type: time.between
  after: sunset
  before: sunrise
```

You can also check device states:

```yaml
condition:
  type: device.state.OnOff
  device: Samsung TV - Living Room
  state: on
  is: false
```

### Complex Conditions

Combine multiple conditions with logical operators:

```yaml
condition:
  type: and
  conditions:
    - type: device.state.OnOff
      device: Samsung TV - Living Room
      state: on
      is: true
    - type: or
      conditions:
        - type: time.between
          after: 20:00
        - type: time.between
          weekdays:
            - SATURDAY
            - SUNDAY
```

## Actions: Making Things Happen

### Controlling Lights

Turn lights on or off:

```yaml
- type: device.command.OnOff
  devices:
    - Living Room Lights - Living Room
  on: true
```

Set brightness (0-100):

```yaml
- type: device.command.BrightnessAbsolute
  devices:
    - Living Room Light - Living Room
  brightness: 25
```

Set color temperature for the perfect ambiance:

```yaml
- type: device.command.ColorAbsolute
  devices:
    - Living Room Light - Living Room
  color:
    temperature: 2202K
```

### Adding Delays

Create sequences with pauses between actions:

```yaml
- type: time.delay
  for: 5min
```

Formats supported: `30sec`, `5min`, `2hours`

### Voice Announcements

Broadcast messages to your speakers:

```yaml
- type: assistant.command.Broadcast
  devices:
    - Kitchen Speaker - Kitchen
  message: "Dinner is ready!"
```

### Media and Volume Control

```yaml
- type: device.command.SetVolume
  devices:
    - Living Room Speaker
  volumeLevel: 30

- type: device.command.MediaPause
  devices:
    - Living Room Speaker
```

## Real-World Example: Cinema Mode

Let's look at a practical automation that creates the perfect movie-watching atmosphere. This script automatically dims your lights when you turn on the TV after sunset:

```yaml
metadata:
  name: TV Cinema Auto
  description: Automatically dim lights to cinema mode when TV turns on after sunset.

automations:
  - starters:
      - type: device.state.OnOff
        device: Samsung The Frame 50 - Living Room
        state: on
        is: true

    condition:
      type: time.between
      after: sunset
      before: sunrise

    actions:
      # Turn off main lights to reduce glare
      - type: device.command.OnOff
        devices:
          - Ceiling Light - Living Room
          - Desk Light - Living Room
        on: false

      # Set ambient lights to very low warm glow
      - type: device.command.BrightnessAbsolute
        devices:
          - Sofa Light - Living Room
          - Window Light - Living Room
        brightness: 10

      - type: device.command.ColorAbsolute
        devices:
          - Sofa Light - Living Room
          - Window Light - Living Room
        color:
          name: "warm white"

      # Turn on lava lamp for ambient effect
      - type: device.command.OnOff
        devices:
          - Lava Lamp - Living Room
        on: true
```

## Real-World Example: Night Light

Safe navigation at night without blinding yourself:

```yaml
metadata:
  name: Night Light Mode
  description: Very dim hallway light for nighttime navigation without blinding.

automations:
  - starters:
      - type: assistant.event.OkGoogle
        eventData: query
        is: "night light"

    condition:
      type: time.between
      after: 22:00
      before: 06:00

    actions:
      # Hallway lights at minimal brightness
      - type: device.command.BrightnessAbsolute
        devices:
          - Hallway Light 1 - Hallway
          - Hallway Light 2 - Hallway
        brightness: 5

      - type: device.command.ColorAbsolute
        devices:
          - Hallway Light 1 - Hallway
          - Hallway Light 2 - Hallway
        color:
          name: "warm white"
```

## Important Notes on Device Naming

Device names in Google Home scripts must **exactly match** how they appear in your Google Home app. The format is always:

```
Device Name - Room Name
```

For example:
- `Ceiling Light - Living Room`
- `Kitchen Speaker - Kitchen`
- `Front Door Lock - Front Porch`

## Common Pitfalls and Fixes

| Error | Cause | Fix |
|-------|-------|-----|
| `Expected subfields: [is]` | OkGoogle starter missing required field | Add both `eventData: query` and `is: "phrase"` |
| `Expected subfields: [devices]` | Used `device:` (singular) | Use `devices:` (plural) as a list |
| `Expected subfields: [on]` | Wrong OnOff syntax | Use `on: true` or `on: false` (boolean) |
| `value should be [FieldPath] but is [Map]` | Nested object where simple value expected | Use `eventData: query` not `eventData: {query: ...}` |

## Advanced Features

### Light Effects

Create gradual transitions with sleep and wake effects:

```yaml
# Gradual dim over 30 minutes
- type: device.command.LightEffectSleep
  devices:
    - Bedroom Lights
  duration: 30min
  sleepBrightness: 0

# Gradual brighten over 15 minutes
- type: device.command.LightEffectWake
  devices:
    - Bedroom Lights
  duration: 15min
  wakeBrightness: 100
```

### Robot Vacuum Control

Automate your robot vacuum:

```yaml
- type: device.command.StartStop
  devices:
    - Robot Vacuum - Kitchen
  start: true

# Or send it back to dock
- type: device.command.Dock
  devices:
    - Robot Vacuum - Kitchen
```

## Tips for Success

1. **Start simple**: Begin with a basic voice-triggered automation before adding conditions and complex sequences
2. **Test incrementally**: Add one action at a time and test to ensure each works correctly
3. **Use descriptive names**: Your future self will thank you for clear metadata names and descriptions
4. **Version control**: Keep your YAML files in a Git repository to track changes
5. **Check device names**: The most common error is mismatched device names - always verify in the Google Home app

## Conclusion

Google Home's YAML script editor unlocks powerful automation capabilities for your smart home. By understanding the structure and available commands, you can create sophisticated routines that make your home truly intelligent. Start with simple automations and gradually build more complex scenarios as you become comfortable with the syntax.

Whether you're creating the perfect movie-watching ambiance, automating your morning routine, or simply making your home more convenient, YAML scripts give you the flexibility to design exactly what you need.

Happy automating! 🏠✨
