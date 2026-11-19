# StarResonanceTool
WIP data parser for the game `Blue Protocol: Star Resonance`

This tool extracts and processes game assets from PKG files, including Lua scripts, protocol buffers, and asset bundles.

## Building

- Compile via Visual Studio 2022 or the command `dotnet build`

## Usage

```
StarResonanceTool.exe [options]
```

### Command Line Options

| Option | Description |
|--------|-------------|
| `-h, --help` | Show help message and exit |
| `-p, --pkg <path>` | Path to the meta.pkg file |
| `-o, --output <path>` | Output directory path |
| `-d, --dll <path>` | Path to DummyDll directory |
| `-a, --assetbundles` | Extract asset bundles (default: skip) |
| `--all` | Process all entries (processes everything in the PKG file) |
| `-l, --lang <langs>` | Comma-separated languages for the tables: `english`, `chinese`, `japanese`, `korean` or `all` (default: `english`) |

### Examples

**Basic usage:**
```bash
StarResonanceTool.exe --pkg "C:\game\meta.pkg" --dll "C:\Il2CppDumper\DummyDll" --output "C:\extracted"
```

**Extract everything including asset bundles:**
```bash
StarResonanceTool.exe --all --assetbundles --dll "C:\Il2CppDumper\DummyDll" --output "C:\full_extraction"
```

**Extract everything with the tables in all four languages:**
```bash
StarResonanceTool.exe --pkg "C:\game\meta.pkg" --all --assetbundles --lang all --dll "C:\Il2CppDumper\DummyDll" --output "C:\full_extraction"
```

### Output Structure

The tool creates the following directory structure in the output folder:

```
output/
├── Bundles/     # Asset bundles (.ab files) - only if --all and --assetbundles are used
├── Lua/         # Lua scripts (.luac files) - only if --all is used
├── Proto/       # Protocol buffer schemas - only if --all is used
├── Unk/         # Other data files (.bin files) - only if --all is used
└── Ztable/      # Parsed basic game data (.json files), one folder per language given to --lang
    ├── en/ZTable/
    ├── cn/ZTable/
    ├── jp/ZTable/
    └── kr/ZTable/
```

Files that already exist in `Bundles/` and `Unk/` are skipped, not overwritten. When extracting a newer game version into the same output folder, delete `Bundles/` and `Unk/` first. `Ztable/`, `Lua/` and `Proto/` are overwritten.

### File Types Processed

- **Asset Bundles:** Unity asset bundles (starts with "UnityFS")
- **Lua Scripts:** Compiled Lua bytecode (starts with 0x1B4C7561)
- **Protocol Buffers:** Proto files containing "proto2" or "proto3"
- **ZTables:** Json files containing the parsed ztables data
- **Other Data:** Various game data files

### Requirements

- .NET 8.0 or later
- Il2CppDumper DummyDll files for proper table parsing
- Access to the game's PKG files

### Disclaimer
Bokura owns the original assets to the game, all credits go to its rightful owner. I am not liable for any damages caused if you get banned from using a mod created by this tool, or its derivatives. I DO NOT CLAIM ANY RESPONSIBILITY FOR ANY USAGE OF THIS SOFTWARE, THE SOFTWARE IS MADE 100% FOR EDUCATIONAL PURPOSES ONLY