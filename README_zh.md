## 支持的文件系统列表

### 历史
`xfstests` 原名来源于其最初为测试 SGI Irix 操作系统上的 XFS 文件系统而开发。当 XFS 被移植到 Linux 时，`xfstests` 也随之迁移，现在仅支持 Linux 平台。

`xfstests` 包含许多可用于其他文件系统的测试用例，这些被称为“通用”（generic）测试用例，位于 `tests/generic/` 目录下。随着越来越多的文件系统开始使用 `xfstests` 并贡献补丁，它现已成为 Linux 主要文件系统的回归测试套件。因此，它不再仅限于 XFS 测试，而是更广泛地称为“fstests”。

### 支持的文件系统
`xfstests` 对文件系统没有严格限制，任何文件系统都可以尝试使用 `xfstests` 进行测试。

不过，不同文件系统在 `xfstests` 中的支持程度不同，分为以下四个级别：

- **L1**：`xfstests` 基本上可以在指定文件系统上运行。
- **L2**：指定文件系统对通用测试失败的修复支持较少。
- **L3**：指定文件系统有一定的支持，包含一些专属测试用例。
- **L4**：指定文件系统积极支持，包含大量专属测试用例。

（“+”表示略高于当前级别，但未达到下一级别；“-”表示略低于当前级别。）

| 文件系统   | 支持级别 | 备注                                                                 |
|------------|----------|----------------------------------------------------------------------|
| XFS        | L4+      | 无                                                                   |
| Btrfs      | L4       | [Btrfs 开发说明](https://btrfs.readthedocs.io/en/latest/dev/Development-notes.html#fstests-setup) |
| Ext4       | L4       | 无                                                                   |
| Ext2       | L3       | 无                                                                   |
| Ext3       | L3       | 无                                                                   |
| overlay    | L3       | 无                                                                   |
| f2fs       | L3-      | 无                                                                   |
| tmpfs      | L3-      | 无                                                                   |
| NFS        | L2+      | [NFS 测试说明](https://linux-nfs.org/wiki/index.php/Xfstests)         |
| Ceph       | L2       | 无                                                                   |
| CIFS       | L2-      | [CIFS 测试说明](https://wiki.samba.org/index.php/Xfstesting-cifs)     |
| ocfs2      | L2-      | 无                                                                   |
| Bcachefs   | L2       | 无                                                                   |
| Exfat      | L1+      | 无                                                                   |
| AFS        | L1       | 无                                                                   |
| FUSE       | L1       | 无                                                                   |
| GFS2       | L1       | 无                                                                   |
| Glusterfs  | L1       | 无                                                                   |
| JFS        | L1       | 无                                                                   |
| pvfs2      | L1       | 无                                                                   |
| Reiser4    | L1       | Reiserfs 已被移除，仅剩 Reiser4                                      |
| ubifs      | L1       | 无                                                                   |
| udf        | L1       | 无                                                                   |
| Virtiofs   | L1       | 无                                                                   |
| 9p         | L1       | 无                                                                   |

---

## 构建 FSQA 套件

### Ubuntu 或 Debian
1. 确保软件包列表是最新的，并安装所有必要的软件包：
   ```bash
   sudo apt-get update
   sudo apt-get install acl attr automake bc dbench dump e2fsprogs fio gawk \
       gcc git indent libacl1-dev libaio-dev libcap-dev libgdbm-dev libtool \
       libtool-bin liburing-dev libuuid1 lvm2 make psmisc python3 quota sed \
       uuid-dev uuid-runtime xfsprogs linux-headers-$(uname -r) sqlite3 \
       libgdbm-compat-dev
   ```

2. 安装用于测试的文件系统相关软件包：
   ```bash
   sudo apt-get install exfatprogs f2fs-tools ocfs2-tools udftools xfsdump \
       xfslibs-dev
   ```

### Fedora
1. 从标准仓库安装所有必要的软件包：
   ```bash
   sudo yum install acl attr automake bc dbench dump e2fsprogs fio gawk gcc \
       gdbm-devel git indent kernel-devel libacl-devel libaio-devel \
       libcap-devel libtool liburing-devel libuuid-devel lvm2 make psmisc \
       python3 quota sed sqlite udftools xfsprogs
   ```

2. 安装用于测试的文件系统相关软件包：
   ```bash
   sudo yum install btrfs-progs exfatprogs f2fs-tools ocfs2-tools xfsdump \
       xfsprogs-devel
   ```

### RHEL 或 CentOS
1. 启用 EPEL 仓库：
   - 参考 [EPEL 文档](https://docs.fedoraproject.org/en-US/epel/#How_can_I_use_these_extra_packages.3F)

2. 从标准仓库和 EPEL 安装所有必要的软件包：
   ```bash
   sudo yum install acl attr automake bc dbench dump e2fsprogs fio gawk gcc \
       gdbm-devel git indent kernel-devel libacl-devel libaio-devel \
       libcap-devel libtool libuuid-devel lvm2 make psmisc python3 quota sed \
       sqlite udftools xfsprogs
   ```
   或者，从源代码编译 EPEL 软件包：
   - [dbench 下载](https://dbench.samba.org/web/download.html)
   - [indent 下载](https://www.gnu.org/software/indent/)

3. 构建并安装 `liburing`：
   - 参考 [liburing GitHub](https://github.com/axboe/liburing)

4. 安装用于测试的文件系统相关软件包：
   - XFS：
     ```bash
     sudo yum install xfsdump xfsprogs-devel
     ```
   - exfat：
     ```bash
     sudo yum install exfatprogs
     ```
   - f2fs：参考 [f2fs-tools](https://git.kernel.org/pub/scm/linux/kernel/git/jaegeuk/f2fs-tools.git/about/)
   - ocfs2：参考 [ocfs2-tools](https://github.com/markfasheh/ocfs2-tools)

### SUSE Linux Enterprise 或 openSUSE
1. 从标准仓库安装所有必要的软件包：
   ```bash
   sudo zypper install acct automake bc dbench duperemove dump fio gcc git \
       indent libacl-devel libaio-devel libattr-devel libcap libcap-devel \
       libtool liburing-devel libuuid-devel lvm2 make quota sqlite3 xfsprogs
   ```

2. 安装用于测试的文件系统相关软件包：
   - btrfs：
     ```bash
     sudo zypper install btrfsprogs libbtrfs-devel
     ```
   - XFS：
     ```bash
     sudo zypper install xfsdump xfsprogs-devel
     ```

### 构建并安装测试、库和工具
```bash
git clone git://git.kernel.org/pub/scm/fs/xfs/xfstests-dev.git
cd xfstests-dev
make
sudo make install
```

### 设置环境
1. 将 XFS、EXT4、BTRFS 等文件系统编译进内核或作为模块加载。例如，对于 XFS，在内核配置中启用 `XFS_FS`，或编译为模块并使用 `sudo modprobe xfs` 加载。大多数发行版已默认包含这些文件系统。

2. 创建 TEST 设备：
   - 格式化为要测试的文件系统类型。
   - 至少 10GB 大小。
   - 可选：填充可销毁数据。
   - 设备内容可能会被销毁。

3. （可选）创建 SCRATCH 设备：
   - 许多测试依赖 SCRATCH 设备。
   - 无需格式化。
   - 至少 10GB 大小。
   - 必须与 TEST 设备不同。
   - 设备内容会被销毁。

4. （可选）创建 SCRATCH 设备池：
   - 用于 BTRFS 测试。
   - 通过 `SCRATCH_DEV_POOL` 变量指定 3 个或更多独立 SCRATCH 设备，例如 `SCRATCH_DEV_POOL="/dev/sda /dev/sdb /dev/sdc"`。
   - 设备内容会被销毁。
   - SCRATCH 设备应留空，会被 `SCRATCH_DEV_POOL` 覆盖。

5. 复制 `local.config.example` 到 `local.config`，并根据需要编辑。`TEST_DEV` 和 `TEST_DIR` 是必需的。

6. （可选）创建 fsgqa 测试用户和组：
   ```bash
   sudo useradd -m fsgqa
   sudo useradd 123456-fsgqa
   sudo useradd fsgqa2
   sudo groupadd fsgqa
   ```
   如果系统不支持以数字开头的用户名，可跳过 `123456-fsgqa` 的创建，仅少数测试需要此用户。

7. （可选）如果要运行 UDF 测试组件，安装 `mkudffs`，并从 [Philips UDF 验证软件](https://www.lscdweb.com/registered/udf_verifier.html) 下载并构建 `udf_test`，然后将其复制到 `xfstests/src/`。

8. （可选）要进行 io_uring 相关测试，确保以下三点：
   - 内核已启用 `CONFIG_IO_URING=y`。
   - 执行 `sysctl -w kernel.io_uring_disabled=0`（或设为 2 以动态禁用 io_uring 测试）。
   - 在构建 `fstests` 前安装包含 `liburing.h` 的 `liburing` 开发包。

### 使用 loopback 分区运行测试示例：
```bash
xfs_io -f -c "falloc 0 10g" test.img
xfs_io -f -c "falloc 0 10g" scratch.img
mkfs.xfs test.img
losetup /dev/loop0 ./test.img
losetup /dev/loop1 ./scratch.img
mkdir -p /mnt/test && mount /dev/loop0 /mnt/test
mkdir -p /mnt/scratch
```

配置示例：
```bash
cat local.config
export TEST_DEV=/dev/loop0
export TEST_DIR=/mnt/test
export SCRATCH_DEV=/dev/loop1
export SCRATCH_MNT=/mnt/scratch
```

从这里开始，您可以运行一些基本测试，参见下文的“使用 FSQA 套件”。

### 额外设置
某些测试需要额外的 `local.config` 配置。将以下变量添加到 `local.config`，或在 `common/config` 中根据主机名添加条件赋值，或使用 `setenv` 设置。

**额外 TEST 设备配置**：
- `TEST_LOGDEV`：测试文件系统外部日志设备。
- `TEST_RTDEV`：测试文件系统实时数据设备。
- 如果设置了 `TEST_LOGDEV` 和/或 `TEST_RTDEV`，它们将始终被使用。
- `FSTYP`：要测试的文件系统类型，默认从 `TEST_DEV` 推导，可手动覆盖，默认为 `xfs`。

**额外 SCRATCH 设备配置**：
- `SCRATCH_LOGDEV`：SCRATCH 文件系统外部日志设备。
- `SCRATCH_RTDEV`：SCRATCH 文件系统实时数据设备。
- 如果设置了 `SCRATCH_LOGDEV` 和/或 `SCRATCH_RTDEV`，设置 `USE_EXTERNAL` 环境变量为 `yes` 以启用。

**xfsdump 测试的磁带设备配置**：
- `TAPE_DEV`：用于测试 xfsdump 的磁带设备。
- `RMT_TAPE_DEV`：用于测试 xfsdump 的远程磁带设备。
- 如果测试 xfsdump，确保磁带设备中有可覆盖的磁带。

**额外 XFS 配置**：
- `TEST_XFS_REPAIR_REBUILD=1`：使 `_check_xfs_filesystem` 运行 `xfs_repair -n` 检查文件系统，运行 `xfs_repair` 重建元数据索引，再次运行 `xfs_repair -n` 检查重建结果。
- `FORCE_XFS_CHECK_PROG` 选项已于 2024 年 7 月移除，`xfs_check` 支持已全部删除。
- `TEST_XFS_SCRUB_REBUILD=1`：使 `_check_xfs_filesystem` 以“force_repair”模式运行 `xfs_scrub` 重建文件系统，并运行 `xfs_repair -n` 检查结果。
- 如果 `xfs_scrub` 存在，测试结束时将始终检查 TEST 和 SCRATCH 文件系统（如果仍在线），无需设置 `TEST_XFS_SCRUB`。

**工具配置**：
- **dump**：
  - `DUMP_CORRUPT_FS=1`：如果文件系统检查失败，记录 XFS、ext* 或 btrfs 文件系统的元数据转储。
  - `DUMP_COMPRESSOR`：指定压缩程序以压缩文件系统元数据转储，必须支持 `-f` 和文件名参数，以及 `-d -f -k` 解压（模拟 gzip）。
- **dmesg**：
  - `KEEP_DMESG=yes`：保留测试后的 dmesg 日志。
- **kmemleak**：
  - `USE_KMEMLEAK=yes`：在每次测试后扫描内核内存泄漏（需内核支持 kmemleak）。
- **fsstress**：
  - `FSSTRESS_AVOID` 和/或 `FSX_AVOID`：在 `fsstress` 和 `fsx` 调用末尾添加选项，以排除某些操作模式。
- **core dumps**：
  - `COREDUMP_COMPRESSOR`：指定压缩程序以压缩崩溃转储，必须支持 `-f` 和文件名参数（模拟 gzip）。

**内核/模块相关配置**：
- `TEST_FS_MODULE_RELOAD=1`：在测试调用间卸载并重新加载模块，假设模块名与 `FSTYP` 相同。
- `MODPROBE_PATIENT_RM_TIMEOUT_SECONDS`：指定模块移除的等待时间，默认 50 秒，设为“forever”将无限等待。
- `KCONFIG_PATH`：指定内核配置文件路径，用于测试检查内核功能是否启用。
- `REPORT_GCOV`：设置为目录路径以生成 gcov 代码覆盖率报告，设为 1 时报告写入 `$REPORT_DIR/gcov/`。

**测试控制**：
- `LOAD_FACTOR`：非零正整数，增加系统负载的倍数。
- `TIME_FACTOR`：非零正整数，增加测试运行时间的倍数。
- 对于“soak”组测试，设置 `SOAK_DURATION` 指定测试运行时间，支持浮点数和单位后缀（m：分钟，h：小时，d：天，w：周），优先于 `TIME_FACTOR`。

**其他**：
- `DISABLE_UDF_TEST=1`：禁用 UDF 验证测试。
- `LOGWRITES_DEV`：指定用于电源故障测试的块设备。
- `PERF_CONFIGNAME`：为性能测试指定标识字符串（如“spinningrust”或“nvme”）。
- `MIN_FSSIZE`：指定可创建文件系统的最小大小（字节），低于此大小的测试将被跳过。
- `DIFF_LENGTH`：失败测试的差异行数，默认 10，设为 0 打印完整差异。
- `IDMAPPED_MOUNTS=true`：在 idmapped 挂载上运行所有测试，目前仅 overlay 无问题，其他文件系统可能需要额外补丁。
- `CANON_DEVS=yes`：规范化设备符号链接（如使用 `/dev/disk/by-id/nvme-*`），默认禁用。
- `REPORT_VARS_FILE`：指定包含冒号分隔的名称-值对的文件，记录在测试报告中，名称必须唯一，冒号周围的空格将被移除。

---

## 使用 FSQA 套件

### 运行测试
- 进入 `xfstests` 目录：
  ```bash
  cd xfstests
  ```
- 默认运行 `auto` 组的所有测试，这些测试预期作为回归测试正常运行，不包括可能导致机器故障的“危险”测试。
- 运行指定测试：
  ```bash
  ./check '*/001' '*/002' '*/003'
  ./check '*/06?'
  ```
- 按组运行测试：
  ```bash
  ./check -g [group(s)]
  ```
  构建 `xfstests` 后，查看 `tests/*/group.list` 文件以了解每个测试的组成员关系。
- 运行所有测试（包括危险测试）：
  ```bash
  ./check -g all
  ```
- 随机化测试顺序：
  ```bash
  ./check -r [test(s)]
  ```
- 显式指定文件系统类型（否则从 `TEST_DEV` 自动检测）：
  - NFS 测试：
    ```bash
    ./check -nfs [test(s)]
    ```
  - AFS 测试：
    ```bash
    ./check -afs [test(s)]
    ```
  - CIFS/SMB3 测试：
    ```bash
    ./check -cifs [test(s)]
    ```
  - overlay 测试：
    ```bash
    ./check -overlay [test(s)]
    ```
    TEST 和 SCRATCH 分区需预先格式化为其他基础文件系统，overlay 目录将在其上创建。

`check` 脚本检查每个脚本的返回值，并将输出与预期输出比较。如果输出不符合预期，将显示差异并生成 `.out.bad` 文件。

意外的控制台消息、崩溃或挂起可能被视为失败，但不一定被 QA 系统检测到。

---

## 添加到 FSQA 套件

### 创建新测试脚本
使用 `new` 脚本创建新测试脚本。

### 测试脚本环境
开发新测试脚本时，需注意以下事项。在引入 `common/preamble` 文件并调用 `_begin_fstest` 函数后，所有环境变量和 shell 过程均可用。

1. **文件系统操作**：
   - 在 `$TEST_DIR` 创建目录和文件，该目录位于 XFS 文件系统内且可写。测试完成后需清理（如通过 `_cleanup` 函数），参见测试 `001` 示例。`$TEST_DIR` 位于 `$TEST_DEV` 块设备上的文件系统内。
   - 在 `$SCRATCH_DEV` 上创建新的 XFS 文件系统并挂载到 `$SCRATCH_MNT`。如果需要 SCRATCH 分区，启动时调用 `_require_scratch` 函数，它会检查 `$SCRATCH_DEV` 和 `$SCRATCH_MNT` 并确保未挂载。测试完成后需清理，卸载 `$SCRATCH_MNT`。
   - 测试可使用 `$SCRATCH_LOGDEV` 和 `$SCRATCH_RTDEV` 测试外部日志和实时卷，但在未设置这些变量的常见情况下，测试需通过（如输出 `$seq.out` 并退出）。

2. **临时文件**：
   - 在 `$tmp.<任意名称>` 创建非文件系统测试相关的临时文件（如捕获输出、准备操作列表）。`new` 脚本创建的标准框架会初始化 `$tmp` 并在退出时清理。

3. **用户权限**：
   - 默认以运行 `check` 脚本的用户的 UID 执行测试。

4. **常用 shell 过程**：
   - `_get_fqdn`：输出主机完全限定域名。
   - `_get_pids_by_name`：输入进程名称，返回匹配的 PID。
   - `_within_tolerance`：数值“足够接近”过滤器，用于确定性输出，参见 `common/filter` 注释。
   - `_filter_date`：将 ctime(3) 格式日期转换为 `DATE` 字符串，确保确定性输出。
   - `_cat_passwd`, `_cat_group`：输出密码或组文件内容（包括 NIS 数据库内容，如果存在）。

5. **通用建议和惯例**：
   - 获取密码或组文件内容时，使用 `_cat_passwd` 和 `_cat_group` 函数，确保包含 NIS 信息。
   - 调用 `getfacl` 时，使用 `-n` 参数以输出数字而非符号标识。
   - 创建新测试时，可输入自定义文件名，格式为 `NNN-custom-name`，其中 `NNN` 由 `./new` 脚本自动添加，`custom-name` 为可选字符串，仅限字母、数字和破折号。
   - 测试组成员关系：通过 `_begin_fstest` 函数参数指定组名，组名仅限 `[:alnum:_-]` 字符。组用于选择测试子集，构建系统扫描 `_begin_fstest` 调用以生成组列表。需遵循格式：
     - 单行，无多行续行。
     - 组名以空格分隔。
     - `#` 表示注释，之后内容被忽略。
     - 示例：`_begin_fstest auto quick subvol snapshot # metadata` 将测试关联到 `auto`、`quick`、`subvol` 和 `snapshot` 组，`metadata` 被忽略。
     - 无需指定 `all` 组，运行时自动计算。

### 验证输出
每个测试脚本（如 `007`）有对应的验证输出文件（如 `007.out`）。验证输出需确定性，需过滤以下内容：
- 日期
- PID
- 主机名
- 文件系统名称
- 时区
- 可变目录内容
- 不精确的数字（如大小和时间）

### 通过/失败
`check` 脚本用于运行一个或多个测试。测试 `$seq` 通过的条件：
1. 未生成 `core` 文件。
2. 未创建 `$seq.notrun` 文件。
3. 退出状态为 0。
4. 输出与验证输出匹配。

如果测试未运行（情况 2），`$seq.notrun` 文件应包含一行简短说明。标准输出不检查，可用于详细解释。

强制非零退出状态：
```bash
status=1
exit
```
注意：`exit 1` 因退出陷阱机制无效。

最近的通过/失败历史记录在 `check.log` 中，最新通过的测试用时记录在 `check.time` 中。

使用 `tools/compare-failures` 脚本可比较多次运行的失败输出。

---

## 提交补丁
将补丁发送至 `fstests@vger.kernel.org` 邮件列表。

---

以上为 `xfstests` README 的完整中文翻译，涵盖了所有章节和技术细节，语言简洁且符合技术文档风格。
