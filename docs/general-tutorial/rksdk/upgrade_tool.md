---
sidebar_position: 3
---

# upgrade_tool

upgrade_tool 是 Rockchip 为 Linux 平台下进行 USB 烧录所开发的软件。

---

## 安装 upgrade_tool

请下载并解压以下文件以安装 upgrade_tool：

- [upgrade_tool v2.1](https://dl.radxa.com/tools/linux/Linux_Upgrade_Tool_V2.1.zip)

## 使用 upgrade_tool

<Tabs queryString="mode">
<TabItem value="CLI" label="命令行模式">

upgrade_tool 在命令行模式下的使用类似于 [rkdeveloptool](rkdeveloptool#使用-rkdeveloptool)。

:::caution
命令行模式下，upgrade_tool 不支持选择 Maskrom 设备，也无法选择待写入的存储介质。

如果需要从多个设备中选择特定的一个，请使用[交互模式](upgrade_tool?mode=Interactive)。
:::

### 查看已连接的 Maskrom 设备

```bash
./upgrade_tool ld
```

### 写入文件

:::caution
写入文件时，upgrade_tool 不会自动对压缩文件进行解压缩。

请首先将使用到的文件进行解压缩，并在 upgrade_tool 中指定解压缩后的文件。
:::

```bash
sudo ./upgrade_tool db <loader>
sudo ./upgrade_tool wl 0 <image>
```

可以在 [Loader](Loader) 页面找到部分产品所使用的 Loader 文件下载链接。

### 重启设备

```bash
sudo ./upgrade_tool rd
```

</TabItem>
<TabItem value="Interactive" label="交互模式">

如果在执行 upgrade_tool 的时候不带任何参数，则会自动进入交互模式。

此模式下会首先要求选择待写入的设备：

```bash
$ sudo ./upgrade_tool
Using /home/rock/Linux_Upgrade_Tool/config.ini
Program Log will save in the /root/upgrade_tool/log/
List of rockusb connected
DevNo=1 Vid=0x2207,Pid=0x350b,LocationID=21     Mode=Maskrom
DevNo=2 Vid=0x2207,Pid=0x350b,LocationID=22     Mode=Maskrom
DevNo=3 Vid=0x2207,Pid=0x350b,LocationID=23     Mode=Maskrom
Found 3 rockusb,Select input DevNo,Rescan press <R>,Quit press <Q>:
```

选择好设备后，upgrade_tool 会显示当前模式下可用的所有命令。此后的操作类似[命令行模式](upgrade_tool?mode=CLI)。

</TabItem>
</Tabs>

---

## 并行写入

由于 upgrade_tool 在进行设备写入时会阻塞当前终端，所以如果需要同时对多个设备写入时，需要多次执行 upgrade_tool 来创建多个交互模式会话。
