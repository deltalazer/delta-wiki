# delta!lazer documentation

Learn how to use section gimmicks, hitobject controls, and all the features that make delta unique.

## Getting started

### What is delta?

delta is a community-driven fork of osu!lazer that introduces **section gimmicks and hitobject gimmicks**, mapper-defined rules that can change gameplay behavior throughout a beatmap. With delta, mappers can create maps where HP mechanics, difficulty settings, and even mods change dynamically from section to section.

This opens up entirely new possibilities for creative mapping, including:

- Challenge sections with custom HP drain/recovery
- Forced mod sections (HD/FL/HR for specific parts)
- Per-section difficulty changes (AR/OD/CS)
- Judgment limits (max 100s, no misses, etc.)

### Installation

Download the installer from the [download page](https://delta.mikuuu.xyz/home/download). delta!lazer connects to its own server, so your osu! account is unaffected.

```
# Windows
Run deltalazer-win-Setup.exe

# Linux
chmod +x deltalazer-linux-x64.AppImage
./deltalazer-linux-x64.AppImage
```

### First steps

After launching delta, you can play existing maps normally or create new maps with section gimmicks. To add gimmicks to a map:

1. Open the beatmap editor
2. Open the Section Gimmicks toolbox
3. Click "Add Section" to create a new gimmick section
4. Configure the section's start/end time and enable desired gimmicks

## Section gimmicks

### Overview

Section gimmicks are per-section mapper-defined rules. A beatmap can have multiple sections, each with its own set of active gimmicks. Sections are defined by their start and end times (in milliseconds), and they cannot overlap.

::: alert-note
**Note:** Use `EndTime = -1` to indicate "until the end of the map."
:::

### Creating sections

In the editor, use the Section Gimmicks panel to manage sections:

- **Add Section:** Creates a new section in current time
- **Use Current Time:** Sets the start or end time to current playback position
- **Copy/Paste Settings:** Copy gimmick config between sections (times are not copied)

### Section settings

Each section has toggles for different gimmick groups. Enable only the groups you need:

| Gimmick group | Description |
| :-- | :-- |
| HP Gimmick | Custom HP drain/gain values |
| No Miss | Instant fail on any miss |
| Count Limits | Max 300s/100s/50s allowed |
| No Missed Slider End | Fail on dropped slider tails |
| Great Offset Penalty | HP penalty for late/early 300s |

## HP gimmicks

### Custom HP values

When HP Gimmick is enabled (Reverse HP disabled), you can define custom HP changes for each judgment result:

| Field | Description | Example |
| :-- | :-- | :-- |
| `HP300` | HP change on 300 (Great) | `-0.02` (gain 2%) |
| `HP100` | HP change on 100 (Ok) | `0.05` (lose 5%) |
| `HP50` | HP change on 50 (Meh) | `0.10` (lose 10%) |
| `HPMiss` | HP change on Miss | `0.20` (lose 20%) |

### No Drain mode

When `NoDrain=true`, continuous HP drain is disabled for the section. Only your custom HP values apply. This is **required** when enabling HP Gimmick.

::: alert-warning
**Note:** NoDrain **needs to be enabled** when HP Gimmick is active. You cannot have passive HP drain alongside custom HP values in the same section.
:::

### ReverseHP

ReverseHP inverts the standard HP behavior:

- **300 (Great):** No HP change (HP300 is ignored)
- **100/50/Miss:** HP is *recovered* instead of lost

::: alert-note
When ReverseHP is enabled, `HP100`, `HP50`, or `HPMiss` must be **negative values** to heal the player.
:::

## Count limits

### Max 300s/100s/50s

With Count Limits enabled, you can set maximum allowed counts for each judgment type within the section. Exceeding any limit causes immediate failure.

```
EnableCountLimits=True
Max300s=-1    # Unlimited (or set a number)
Max100s=3     # Fail after 3 100s
Max50s=0      # No 50s allowed
```

Use `-1` for unlimited. This is useful for creating "accuracy challenge" sections.

### No Miss mode

When `EnableNoMiss=true`, any miss (including sliderbreaks) causes immediate failure. This is separate from Count Limits and can be combined with other gimmicks.

### No Missed Slider End

When `EnableNoMissedSliderEnd=true`, failing to complete a slider tail (dropping the slider before the end) causes immediate failure. Useful for slider-focused challenge sections.

## Difficulty overrides

### AR/OD/CS overrides

Override the map's difficulty settings on a per-section basis. You can set:

- **AR (Approach Rate):** How fast circles approach
- **OD (Overall Difficulty):** Timing window strictness
- **CS (Circle Size):** Hit circle radius

::: alert-warning
**Unsafe Mode:** By default, values are clamped to safe ranges. Enable "allow values past limits (unsafe)" to use extreme values, but this may cause crashes or visual glitches!
:::

### Stack leniency override

Control how aggressively stacked objects are offset. Lower values = tighter stacks.

### Tick rate override

Override the slider tick rate for the section. Higher values = more slider ticks.

### Gradual changes

Difficulty overrides support gradual transitions. Instead of instantly changing AR from 8 to 10, you can specify a start point and end point for the change to happen smoothly.

Each difficulty parameter (AR, OD, CS) can have independent gradual start/end times within the same section.

## Forced mods

### Hidden/Flashlight

Force Hidden (HD) or Flashlight (FL) for specific sections. The mod activates automatically when entering the section and deactivates when leaving.

**Flashlight options:**

- Custom FL radius
- Gradual fade-in
- Gradual shrink to target radius

### Hard Rock

Force Hard Rock (HR) for sections. This flips the playfield vertically and increases difficulty settings.

### Fun mods

Force "fun mods" like Barrel Roll, Traceable, and other experimental modifiers. Added in v2.2.1!

## Great Offset Penalty

### How it works

Even when you hit a 300 (Great), if your timing is off by more than the threshold, you take an HP penalty. This encourages precise timing even within the "Great" window.

```
EnableGreatOffsetPenalty=True
GreatOffsetThresholdMs=18     # Tolerance in ms
GreatOffsetPenaltyHP=-0.03    # HP loss when exceeded
```

### Configuration

- **GreatOffsetThresholdMs:** How many ms off-center is tolerated
- **GreatOffsetPenaltyHP:** HP penalty when exceeded (must be ≤ 0)

::: alert-note
This penalty is applied *in addition to* any HP Gimmick values. NoDrain **must be** enabled when Great Offset Penalty is active.
:::

## Per-hitobject gimmicks

### Object-level control

Beyond section-level gimmicks, delta supports **per-hitobject gimmicks**. This allows you to apply special rules to individual circles, sliders, or spinners.

Use cases include:

- Specific circles with different AR/CS
- Individual objects with custom HP values
- Stack leniency overrides per object

### Linking to objects

Hitobject gimmicks are stored linked to the object itself, not the timestamp. This means moving a gimmicked object will keep its gimmick settings intact (fixed in v2.2).

## Uncapped SV

### What changed

delta includes improvements to slider velocity handling in the editor, including a removal of the legacy 10x effective cap for SV scaling. This makes high-SV gimmick mapping more consistent and less constrained.

::: alert-note
Based on commit `a08f791`: *"stabilise shift-drag SV scaling and remove 10x legacy cap"*.
:::

### Mapping notes

- Shift-drag SV edits are more stable during timeline manipulation.
- Very high SV values are no longer artificially blocked by the old 10x limit.

## File format reference

### Serialization

Section gimmicks are stored in the `.osu` file under the `[BeatmapSectionGimmicks]` header. Each line defines one section:

```
[BeatmapSectionGimmicks]
0,0,15000,EnableHPGimmick=True|NoDrain=True|HP300=0|HP100=-0.05|HP50=-0.10|HPMiss=-0.20
1,15000,30000,EnableCountLimits=True|Max100s=3|Max50s=0
2,30000,-1,EnableNoMiss=True
```

Format: `Id,StartTime,EndTime,Key=Value|Key2=Value|...`

---

Need help? Join the [Discord community](https://discord.gg/dfPwhRtGVZ).
