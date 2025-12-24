# BalatroAudioHotSwitch

Seamlessly switch audio devices while playing Balatro. Change your Windows audio output (speakers to headphones, etc.) without restarting the game.

## Features

- **Instant Audio Device Switching**: When you change your Windows default audio device, the game audio automatically switches when you alt-tab back
- **No Audio Interruption**: Music continues playing on the new device
- **Zero Configuration**: Just install and play

## Requirements

- [Lovely Injector](https://github.com/ethangreen-dev/lovely-injector) (mod loader for Balatro)
- Windows (uses OpenAL32.dll)

## Installation

### Step 1: Install Lovely Injector
If you haven't already, install [Lovely](https://github.com/ethangreen-dev/lovely-injector/releases):
1. Download `lovely-x86_64-pc-windows-msvc.zip` from releases
2. Extract `version.dll` to your Balatro game folder (same folder as `Balatro.exe`)

### Step 2: Install This Mod
1. Navigate to `%AppData%/Balatro/Mods/` (create `Mods` folder if it doesn't exist)
2. Copy the `BalatroAudioHotSwitch` folder there

### Step 3: Install Updated OpenAL Soft (Required)
The mod requires **OpenAL Soft 1.22.0+** (Balatro ships with 1.19.1 which lacks the required extension).

#### Windows
1. Copy the included `OpenAL32.dll` to your Balatro game folder
2. Replace the existing `OpenAL32.dll` (backup original if desired)

**Alternatively**, download from [OpenAL Soft Releases](https://github.com/kcat/openal-soft/releases) (use `bin/Win64/soft_oal.dll`, rename to `OpenAL32.dll`)

#### Linux
Install OpenAL Soft 1.22.0+ from your package manager:
```bash
# Debian/Ubuntu
sudo apt install libopenal1

# Arch
sudo pacman -S openal

# Fedora
sudo dnf install openal-soft
```
Check version with: `pacmd list-modules | grep openal` or verify the extension is available.

#### macOS
Install via Homebrew:
```bash
brew install openal-soft
```
You may need to symlink or set `DYLD_LIBRARY_PATH` to use the Homebrew version instead of the system OpenAL.

### Final Structure
```
Balatro/                          (Steam game folder)
├── Balatro.exe
├── version.dll                   (Lovely injector)
├── OpenAL32.dll                  (Updated 1.22.0+)
└── ...

%AppData%/Balatro/Mods/
└── BalatroAudioHotSwitch/
    └── lovely.toml
```

## Usage

1. Start Balatro normally
2. Change your Windows audio output device (Settings > Sound > Output)
3. Alt-tab back to the game
4. Audio instantly plays through the new device

## How It Works

The mod adds:
1. A `love.focus()` handler that triggers when you alt-tab back to the game
2. FFI code that calls OpenAL Soft's `alcReopenDeviceSOFT` function to seamlessly switch audio output

This uses the `ALC_SOFT_reopen_device` extension available in OpenAL Soft 1.22.0+, which can move audio output to a different device while preserving all audio state.

## Disabling the Debug Console

Lovely Injector opens a debug console by default. To disable it:

1. Right-click **Balatro** in Steam → **Properties**
2. Add to **Launch Options**:
   ```
   --disable-console
   ```

## Troubleshooting

### Audio doesn't switch
- Ensure you replaced `OpenAL32.dll` in the game folder with version 1.22.0+
- Verify the mod is installed in `%AppData%/Balatro/Mods/BalatroAudioHotSwitch/`
- Check that Lovely's `version.dll` is in the game folder

### Game crashes on startup
- Verify `OpenAL32.dll` is the 64-bit version
- Try restoring the original DLL to confirm it's a DLL issue

### No audio at all
- Restore the original `OpenAL32.dll`
- Check Windows audio settings

## Compatibility

- **Game Version**: Tested with Balatro 1.0.1+
- **Platform**: Windows, Linux, macOS (requires OpenAL Soft 1.22.0+) - *tested on Windows only*
- **Other Mods**: Compatible with Steamodded and most Lovely-based mods

## Credits

- [OpenAL Soft](https://github.com/kcat/openal-soft) for the `ALC_SOFT_reopen_device` extension
- [Lovely Injector](https://github.com/ethangreen-dev/lovely-injector) for the mod loader

## License

MIT License. Balatro is property of LocalThunk.
