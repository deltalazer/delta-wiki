# delta!lazer FAQ

Find answers to common questions about delta and section gimmicks.

## General

### What is delta?

delta is a community-driven fork of osu!lazer that adds section gimmicks and per-hitobject control. It allows mappers to create maps with custom gameplay rules that change throughout the map.

### Is this official?

No, delta is an unofficial community project. It uses osu!lazer as a base but adds experimental features not present in the official client.

### Can I use this for ranked play?

Not on osu!'s servers. delta!lazer connects only to its own server, [delta.mikuuu.xyz](https://delta.mikuuu.xyz), which has its own accounts, leaderboards and pp. Your osu! account is never used.

### Where can I get help?

Join our [Discord server](https://discord.gg/dfPwhRtGVZ)! The community is active and happy to help with any questions about using section gimmicks or building maps.

### Can I play normal osu! maps with delta?

Yes! delta is fully compatible with standard osu! beatmaps. Maps without section gimmicks will play exactly like they do in osu!lazer.

## Section gimmicks

### What are section gimmicks?

Section gimmicks are per-section mapper-defined rules that control gameplay behavior. Each section of your map can have different HP values, judgment limits, forced mods, and difficulty settings.

### How do I add section gimmicks to my map?

In the editor, use the Section Gimmicks toolbox to add sections. Each section has a start and end time, and you can configure various gimmick groups for each section.

### Can I have multiple gimmicks active at once?

Yes! You can enable multiple gimmick groups (HP Gimmick, No Miss, Count Limits, etc.) on the same section. They all apply simultaneously.

### Do gimmicks affect score submission?

Scores are submitted to delta!lazer's own server, never to official osu! leaderboards.

### Can sections overlap?

No, sections cannot overlap. This is enforced by validation when saving. Each time point in your map can only belong to one section.

### How do I copy gimmick settings between sections?

Select a section, use "Copy Gimmick Settings", then select target sections and use "Paste Gimmick Settings". Only gimmick config is copied - the time range stays the same.

## Editor workflow

### How do I create a new section?

Click "Add Section" in the Section Gimmicks toolbox. A new section appears with placeholder times. Select it and use "Set Here" buttons to set start/end times from current playback position.

### How do I set section timing quickly?

Play through your map and press "Use Current Time" when you reach the desired start or end point. You can also type times manually in milliseconds.

### Can I apply section gimmicks to all difficulties at once?

Yes! When saving, you can choose "This difficulty" or "Whole mapset". Choosing "Whole mapset" copies section gimmicks to all difficulties in your beatmap set.

### What happens if I edit a section's timing?

Changes are saved when you commit the edit (press Enter or click away). The editor validates that sections don't overlap before allowing you to save the beatmap.

### Can I delete a section?

Yes, select the section and click the remove button. This removes all gimmick settings for that time range.

### How do I know which section is active during playback?

The active section is highlighted in the Section Gimmicks toolbox during playback. You can also see section boundaries in the timeline.

## HP gimmicks

### How does ReverseHP work?

With ReverseHP enabled, the HP behavior is inverted, so negative values reduce hp and positive values heal.

### Can I disable HP drain entirely?

Yes, No Drain is a requirement for HP Gimmicks.

### What values should I use for HP gimmicks?

It depends on your desired difficulty. A typical setup might be HP300=0, HP100=0.05, HP50=0.10, HPMiss=0.20. Negative values drain HP, positive values heal.

### Why is NoDrain required for HP Gimmicks?

HP Gimmicks require NoDrain=true because continuous drain would interfere with your custom HP values. This ensures consistent, predictable HP behavior based only on hit judgments.

### Why do positive values reduce HP instead of healing?

I keep forgetting to add the negative sign when configuring HP values, so I made it so that positive values reduce HP and negative values heal.

### Do slider ticks and slider ends count for HP gimmicks?

Yes! You can route slider judgments through the HP gimmick system to control exactly how much HP sliders give or drain.

## Difficulty overrides

### What difficulty values can I override per-section?

You can override AR (Approach Rate), OD (Overall Difficulty), CS (Circle Size), Stack Leniency, and Slider Tick Rate on a per-section basis.

### Can I use negative AR?

Yes! delta supports negative AR values, which can create unique reading challenges by hiding approach circles.

### How does gradual difficulty override work?

When you set gradual timing, difficulty values smoothly interpolate from the baseline to your target value over the specified duration. AR/CS/OD values are quantized to one decimal place for smooth transitions.

### What is the "unsafe" difficulty override option?

Unsafe overrides allow extreme values outside normal ranges. Use with caution - extreme values can cause unexpected behavior or crashes.

### Can I override stack leniency?

Yes, Stack Leniency can be overridden per-section. This affects how stacked notes are displayed and positioned.

## Forced mods

### What mods can I force per-section?

You can force Hidden (HD), HardRock (HR), Flashlight (FL), and Fun Mods like Traceable, Barrel Roll, and others on a per-section basis.

### How does per-section Flashlight work?

You can customize FL radius, fade distance, and shrink behavior per-section. Gradual FL transitions follow the section's finish timing for smooth fades.

### Can I force multiple mods on the same section?

Yes! You can force any combination of mods simultaneously. Each mod can be toggled independently.

### What are Fun Mods?

Fun Mods are experimental mods like Traceable (shows cursor trail), Barrel Roll (rotates playfield), and others. In v2.2.1, you can adjust their intensity values per-section.

### Do forced mods affect scoring?

Forced mods apply their gameplay effects but don't affect score multipliers in the traditional sense.

## Advanced features

### What is Uncapped SV?

Uncapped SV removes the legacy 10x slider velocity cap from osu!stable. You can now use extreme SV values for creative mapping. Shift-drag SV scaling has been stabilized for precise control.

### What is Great Offset Penalty?

Great Offset Penalty applies additional HP drain when a 300 hit is outside a specific timing window (e.g., >18ms early/late). It rewards precise timing without changing the judgment itself.

### Can I limit specific judgment counts?

Yes! Count Limits let you set Max300s, Max100s, and Max50s per section. Exceeding any enabled limit causes immediate failure. Use -1 for unlimited.

### What is No Missed Slider End?

This gimmick causes immediate failure if the player doesn't complete a slider end/tail, like no miss but specifically for sliders.

### Can I create per-hitobject gimmicks?

Yes! delta supports per-hitobject gimmicks in addition to section-wide gimmicks. Check the [documentation](/wiki/Delta/Documentation#per-hitobject-gimmicks) for details.

## Compatibility

### Can I open delta maps in regular osu!lazer?

Yes! The `[BeatmapSectionGimmicks]` section is ignored by vanilla clients. The map will play normally without gimmick features.

### Can I export to osu!stable format?

Yes, but gimmicks will be stripped. osu!stable doesn't support section gimmicks, so maps export as standard beatmaps.

### Do section gimmicks work in multiplayer?

Section gimmicks work in local multiplayer sessions. Keep in mind that delta!lazer connects to its own server, not the official osu! servers.

### Will delta break my existing maps?

No! delta is fully backward compatible. All your existing maps will work exactly as they do in osu!lazer.

## Troubleshooting

### The editor crashes when I enable Fun Mods. What's wrong?

This was a known issue in earlier versions (fixed in v2.2.1+). Make sure you're using the latest version. Fun mods now safely handle missing dependencies in the editor.

### My gimmick settings aren't saving. Help!

Check for validation errors: sections can't overlap, NoDrain must be true for HP gimmicks, and ReverseHP requires positive healing values. The editor will show errors before allowing save.

### Why does the game fail immediately when I start playing?

Check if you have No Miss enabled on your first section - any miss will cause instant failure. Also verify Count Limits aren't set too low.

### Gradual difficulty changes look choppy. How do I smooth them?

As of v2.2.0+, gradual transitions are automatically smoothed and quantized. Make sure your gradual finish timing is set appropriately (longer duration = smoother transition).

### My hitobject textbox edits aren't persisting. What do I do?

This was fixed in recent versions. Textbox edits now persist on deselect. If you're experiencing this, update to the latest version.

### How do I report bugs or request features?

Join our [Discord server](https://discord.gg/dfPwhRtGVZ) or open a pull request on the [GitHub repository](https://github.com/deltalazer/delta).

## Technical

### What .NET version is required?

delta requires .NET 8 Runtime. If you're using the self-contained builds, it's included. Otherwise, download it from Microsoft.

### How do I build from source?

Clone the [repository](https://github.com/deltalazer/delta), then run `dotnet build osu.Desktop/osu.Desktop.csproj -c Debug`. See the README on GitHub for full instructions.

### Where are gimmicks stored in the .osu file?

Section gimmicks are stored in a `[BeatmapSectionGimmicks]` section in the .osu file. The format is: `Id,StartTime,EndTime,Key=Value|Key2=Value|...`

### Can I convert maps with gimmicks back to vanilla format?

Yes, the gimmicks section is ignored by the vanilla osu! client. Maps will play normally without the gimmick features.

### How are sections identified in the file format?

Each section has an Id (starting from 0), StartTime in milliseconds, EndTime in milliseconds (or -1 for map end), and pipe-separated key-value pairs for gimmick settings.
