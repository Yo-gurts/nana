# Nana project

Chip: cv1801b
Toolchain: musl_riscv64
Board: y24 alarm

## 代码拉取

‼️不要尝试更改相对路径，按照步骤来！
‼️使用reproduce切换到特定版本的SDK，否则可能出现patch冲突！

```bash
mkdir Nana-v4.1.0 && cd Nana-v4.1.0

# 拉取项目代码
git clone git@github.com:Yo-gurts/nana.git

# 拉取 SDK 代码
./nana/scripts/repos.sh --gitclone ./nana/scripts/sdk-v410-y24.xml --reproduce ./nana/scripts/sdk-v410-2025-08-15.txt

# 同步板卡配置到 SDK
./nana/scripts/sync.sh
```
