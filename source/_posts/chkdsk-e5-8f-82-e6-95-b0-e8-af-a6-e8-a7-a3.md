---
title: chkdsk参数详解
id: 177
categories:
  - 其它
  - 技术专栏
date: 2011-08-22 23:14:07
tags:
---

<div id="blog_text">

chkdsk参数详解

基于所用的文件系统，创建和显示磁盘的状态报告。Chkdsk 还会列出并纠正磁盘上的错误。如果不带任何参数，chkdsk 将显示当前驱动器中的磁盘状态。

语法

chkdsk [volume:][[Path] FileName] [/f] [/v] [/r] [/x] [/i] [/c] [/l[:size]]

参数

volume:

指定驱动器号（冒号分隔）、装入点或卷名。

[Path} FileName]

指定需要 chkdsk 检查碎片整理的文件或文件集的位置和名称。使用通配符（* 和 ?）可以指定多个文件。

/f

修复磁盘上的错误。必须锁定磁盘。如果 chkdsk 无法锁定驱动器，则会显示一条消息，询问您是否希望在下次重新启动计算机时检查该驱动器。

/v

当检查磁盘时，显示所有目录中每个文件的名称。

/r

找到坏扇区并恢复可读取的信息。必须锁定磁盘。

/x

仅在 NTFS 上使用。如果必要，首先强制卸载卷。该驱动器的所有打开句柄都无效。/x 还包含了/f 的功能。

/i

仅随 NTFS 使用。对索引项执行充分检查，降低运行 chkdsk 的所用时间量。

/c

仅随 NTFS 使用。跳过文件夹结构中的周期检查，减少运行 chkdsk 所需的时间量。

/l[:size]

仅随 NTFS 使用。将日志文件的大小更改为由用户输入的大小。如果省略该参数，则 /l 会显示当前日志文件的大小。

/?

在命令提示符显示帮助。

注释

运行 chkdsk

要在固定磁盘上运行 chkdsk 命令，您必须是该 Administrators 组的成员。

重新启动时检查锁定的驱动器

如果希望 chkdsk 修复磁盘错误，则此前不能打开该驱动器上的文件。如果有文件打开，会显示下述错误消息：

Chkdsk cannot run because the volume is in use by another processWould you like to schedule this volume to be checked the next time the system restarts?(Y/N)

如果选择下次重新启动计算机时检查该驱动器，则重新启动计算机后 chkdsk 会自动检查该驱动器并修复错误。如果该驱动器分区为启动分区，则 chkdsk 在检查完该驱动器后会自动重新启动计算机。

报告磁盘错误

chkdsk 命令会检查磁盘空间和文件分配表 (FAT)以及 NTFS 文件系统的使用情况。Chkdsk 在状态报告中提供特定于每个文件系统的信息。状态报告显示文件系统中找到的错误。在活动分区上运行 chkdsk 时，如果未含 /f 命令行选项，则它可能会因为无法锁定该驱动器而报告虚假信息。应该不定期使用 chkdsk 检查每个磁盘上的错误。

修复磁盘错误

只有指定 /f 命令行选项，chkdsk 命令才修复磁盘错误。Chkdsk 必须可以锁定驱动器以纠正错误。由于修复通常会更改磁盘的文件分配表，有时还会丢失数据，所以 chkdsk 会首先发送如下所示的确认消息：

10 lost allocation units found in 3 chains.

Convert lost chains to files?

如果按 Y，Windows 会在根目录中将所有丢失链保存在一个名为 Filennnn.chk 的文件中。chkdsk 结束后，可以查看这些文件是否包含了所需的数据。如果按 N，Windows 会修复磁盘，但对于丢失的分配单元，它不保存其内容。

如果不使用 /f 命令行选项，则在有文件需要修复时，chkdsk 会发送消息，但它不修复任何错误。

如果在大磁盘（例如，70 GB）或有大量文件（数百万）的磁盘上使用 chkdsk /f，这可能要花很长时间（比如说，数天）才能完成。因为 chkdsk 直到工作完成它才会交出控制权，所以计算机在这段时间内将不可用。

检查 FAT 磁盘

Windows 以下列格式显示 FAT 磁盘的 chkdsk 状态报告：

Volume Serial Number is B1AF-AFBF

72214528 bytes total disk space

73728 bytes in 3 hidden files

30720 bytes in 12 directories

11493376 bytes in 386 user files

61440 bytes in bad sectors

60555264 bytes available on disk

2048 bytes in each allocation unit

35261 total allocation units on disk

29568 available allocation units on disk

检查 NTFS 磁盘

Windows 以下列格式显示 NTFS 磁盘的 chkdsk 状态报告：
The type of the file system is NTFS.
CHKDSK is verifying files...
File verification completed.
CHKDSK is verifying indexes...
Index verification completed.
CHKDSK is verifying security descriptors...
Security descriptor verification completed.
12372 kilobytes total disk space.
3 kilobytes in 1 user files.
2 kilobytes in 1 indexes.
4217 kilobytes in use by the system.
8150 kilobytes available on disk.
512 bytes in each allocation unit.
24745 total allocation units on disk.
16301 allocation units available on disk.
存在打开文件的情况下使用 chkdsk

如果该驱动器上有打开的文件，则指定 /f 命令行选项后，chkdsk 会发送错误消息。如果未指定 /f 命令行选项并且存在打开的文件，则 chkdsk 会报告磁盘上丢失的分配单元。如果打开的文件没有记录在文件分配表时，可能会发生这种情况。如果 chkdsk 报告大量分配单元丢失，可以考虑修复该磁盘。

查找物理磁盘错误

使用 /r 命令行选项可查找文件系统中的物理磁盘错误。

报告磁盘坏扇区

在磁盘第一次准备运行时，chkdsk 报告的坏扇区标记为损坏。它们不会造成危险。

了解退出码

下表列出了 chkdsk 完成任务后报告的退出码。

退出码 说明

0 没有发现错误。

1 错误已找到并修复。

2 已执行清理磁盘（例如碎片收集），或者因为没有指定 /f 而未执行清理磁盘。

3 由于未指定 /f 选项，无法检查磁盘，错误不能修复或错误未修复。

故障恢复控制台提供了带有不同参数的 chkdsk 命令。

范例

如果要检查驱动器 D 中的磁盘，并且希望 Windows 修复错误，请键入：

chkdsk d:/f

如果遇到错误，chkdsk 会暂停并显示消息。Chkdsk 完成任务时会显示列有磁盘状态的报告。除非 chkdsk 已完成任务，否则无法打开指定驱动器上的任何文件。

在 FAT 磁盘上，要检查当前目录中所有文件的不相邻块，请键入：

chkdsk *.*

Chkdsk 显示状态报告，然后列出符合具有不相邻块条件的文件。

</div>