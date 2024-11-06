# GoboNG - Spicy GoboLinux tools

![screenshot-2024-10-30-19-15-13](https://github.com/user-attachments/assets/ff70e548-7a34-4061-b15d-6a3d85bd8c14)

This repository holds GoboLinux system tools that may prove useful in GoboLinux development.

They are not perfect in any way and subject to change. Once these tools mature enough (and prove their usefullness) they can be considered for an inclusion to official GoboLinux `Scripts` or as separate entities.

## IsSymlinked

`IsSymlinked` checks whether the program files of a Program are symlinked against `/System/Index/**`.

It lists potential conflicts as well.

*Example usage:*
```bash
$ IsSymlinked Zsh [<version_number>]
```

*Sample output:*
```
IsSymlinked: ✗‼ ZSH/5.8/bin/zsh.old  (** Internally broken symlink! **)
IsSymlinked: ✔  ZSH/5.8/bin/zsh
IsSymlinked: ✔  ZSH/5.8/bin/zsh-5.8
IsSymlinked: ✔↑ ZSH/5.8/share/zsh/5.8/scripts/newuser  -> via Parent: 5.8@
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshbuiltins.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshmisc.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshall.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshroadmap.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshexpn.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshoptions.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshzle.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshtcpsys.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshparam.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshcompctl.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zshmodules.1
IsSymlinked: ✔  ZSH/5.8/share/man/man1/zsh.1
IsSymlinked: ✔↑ ZSH/5.8/lib/zsh/5.8/zsh/nearcolor.so  -> via Parent: zsh@
```
### In order to scan only a single program file:
> [!NOTE]  
> Interface not yet implemented

*Example usage:*
```bash
$ IsSymlinked -f /Programs/ZSH/5.8/bin/zsh.old
```
*Sample output:*
```bash
IsSymlinked: ✗‼ ZSH/5.8/bin/zsh.old  (** Internally broken symlink! **)
```

## GetConflicts

`GetConflicts` iterates over all programs (`/Programs/*/Current`) and shows potential conflicts.

Conflicts are saved locally to `conflicts.txt`.

*Example usage:*
```bash
$ GetConflicts
```

*Sample `conflicts.txt`:*

```
ALSA-Firmware 1.2.4 [Linux-Firmware 20241002-GIT]
ATK 2.38.0 [At-Spi2-Core 2.54.0]
At-Spi2-ATK 2.26.2 [At-Spi2-Core 2.54.0]
BuildLiveCD 017 [SYSLINUX 6.02]
CoreUtils 9.5 [Shadow 4.2.1]
Cpio 2.13 [Tar 1.35]
Glslang 11.8.0 [GLSLang 15.0.0]
GRUB-EFI 2.04 [GRUB 2.04]
HiColor-Icons 0.15 [Hicolor-Icon-Theme 0.18]
Leafpad 0.8.18.1 [GParted 1.6.0]
LibEvent 2.1.12 [LibEV 4.33]
LibRSVG 2.46.4 [GDK-Pixbuf 2.42.12]
LibXcvt 0.1.2 [Xorg-Server 1.20.7]
LibXRandR 1.5.2 [XRandR 1.5.1]
MkFontDir 1.0.7 [MkFontScale 1.2.1]
Net-Tools 1.60 [InetUtils 2.5]
```

For example, according to this sample `Net-Tools 1.60` is not symlinked (well, at least one of its program files) due to conflicting program files with `[InetUtils 2.5]`.

## FindRuntimeDeps

`FindRuntimeDeps PKG_NAME [PKG_VERSION]` iterates over all executable files for a package and shows all libraries linked against them (basic runtime dependencies).

Dependencies are logged to `stdout`.

*Example usage:*
```bash
$ FindRuntimeDeps ATK 2.38.0
$ FindRuntimeDeps ATK
```

if no `PKG_VERSION` is provided `Current` is used.
