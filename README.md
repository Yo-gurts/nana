# Nana project

Chip: cv1801b
Toolchain: musl_riscv64
Board: y24 alarm

## 代码拉取

‼️不要尝试更改相对路径，按照步骤来！
‼️使用reproduce切换到特定版本的SDK，否则可能出现patch冲突！

```bash
mkdir V410 && cd V410

# 拉取项目代码
git clone git@github.com:Yo-gurts/nana.git

# 拉取 SDK 代码
./nana/scripts/repos.sh --gitclone ./nana/scripts/sdk-v410.xml --reproduce ./nana/scripts/git_version_github_v410_2025-11-04.txt

# 打上额外的patch到SDK代码（修复该版本已知的bug或者添加新的功能）
./nana/scripts/repos.sh --applypatch ./nana/scripts/git_version_github_v410_2025-11-04.txt

# 同步板卡配置到 SDK （注意这个脚本的运行位置需要固定）
./nana/scripts/sync.sh
```

✨可以使用下面的命令在检查SDK的状态：

```bash
# 检查SDK本地提交和远端的差异
./nana/scripts/repos.sh --run ./nana/scripts/sdk-v410.xml st

# 在每个git仓库中执行命令
./nana/scripts/repos.sh --run ./nana/scripts/sdk-v410.xml git status
```

## 代码更改与脚本使用

- 板卡特定的所有的代码更改都应该在 `nana` 目录下进行，不要直接在 `V410` 目录下进行更改。
  ```bash
  # 在sophcam_bsp 目录下进行代码更改后，需要执行下面的命令来同步到SDK
  ./nana/scripts/sync.sh
  # 使用 -c 参数可以检查是否有未同步的更改
  ./nana/scripts/sync.sh -c
  ```
- 所有的脚本都应该在 `V410` 目录下运行，不支持在其他目录下运行。
- 此项目下的配置是基于算能的demo板子，若硬件有差异，建议更具实际情况修改配置。可以选择fork此项目，然后根据实际情况修改配置，可以使用`scripts/rename.sh`脚本将项目名称改为自己的项目名称，原理是遍历所有文件内容、文件名、目录名，做简单的替换，大小写敏感。
  ```bash
  # 重命名项目名称为自己的项目名称，大小写都需要更改
  cd nana
  ./scripts/rename.sh sophcam projectname
  ./scripts/rename.sh SOPHCAM PROJECTNAME
  ```
- 如果需要修改SDK本身，建议修改后，将patch保存到`nana/patches`目录下，并以`xxx--0001-xxx.patch`格式命名，其中`xxx`为文件夹的名称，以`--`分隔。这样其他人拉代码之后，可以方便的应用patch。以linux_5.10为例：
  ```bash
  cd linux_5.10
  # 假设有两笔patch
  git format-patch -o ../nana/patches/ -2
  # 重命名patch
  cd ../nana/patches/
  mv 0001-xxx.patch linux_5.10--0001-xxx.patch
  mv 0002-xxx.patch linux_5.10--0002-xxx.patch
  ```
- 使用`applypatch`命令应用patch：
  ```bash
  cd V410
  ./nana/scripts/repos.sh --applypatch ./nana/scripts/git_version_github_v410_2025-11-04.txt
  ```
  该命令会自动提取patches目录下文件的前缀，确定是哪个仓库的patch，并进行应用。如果仓库当前存在未提交的改动或者与指定的txt文件中的commit-id不匹配，会提示用户是否继续应用patch。如果用户选择继续，会先重置到指定的commit-id，然后再应用patch。

## SDK 编译

```bash
# TDL SDK 编译有报错
export TPU_REL=0; source build/envsetup_soc.sh
defconfig cv1801b_y24_spinand

clean_all && build_all
```
