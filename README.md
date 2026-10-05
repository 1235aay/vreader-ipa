# 用 GitHub 打包 VReader，再用 SideStore 安装

适用条件：iPhone 为 iOS 17.0 或以上，SideStore 已安装并设置完成。

这套流程不需要付费 Apple 开发者账号。GitHub 负责编译未签名的真机 IPA，SideStore 使用你已设置的 Apple 账号签名安装。GitHub 工作流不需要填写 Apple 账号、密码或证书。

当前状态：已准备打包工作流并检查 YAML 和内嵌 Bash 语法；尚未在 GitHub 实际编译，尚未生成 IPA。这台 Mac 缺少完整 Xcode，无法在本地完成编译。

## 1. 创建自己的 GitHub 仓库

1. 在电脑浏览器登录 GitHub，打开 https://github.com/new 。
2. Repository name 填 `vreader-ipa`。
3. 可见性选择 **Public**。标准 GitHub 托管运行器在公开仓库中免费运行；产物存储受账号额度约束，这份工作流仅保留 7 天。
4. 勾选 **Add a README file**，其余保持默认。
5. 点击 **Create repository**。

不需要复制整份 VReader 源码，也不需要 Fork。下面的工作流会自行下载固定版本源码。

## 2. 添加打包文件

1. 用 Codex 或文本编辑器打开同目录的 `build-vreader-ipa.yml`，复制全部内容。
2. 回到刚创建的 `vreader-ipa` 仓库，打开 **Code** 页。
3. 点击 **Add file → Create new file**。
4. 文件名必须填写下面的完整路径（包含前面的点和文件夹）：

   ```text
   .github/workflows/build-vreader-ipa.yml
   ```

5. 把复制的内容粘贴进下方编辑框，保留原来的缩进。
6. 点击 **Commit changes…**，选择直接提交到 `main`，再点 **Commit changes**。

注意：把文件放在仓库根目录，GitHub 不会把它识别成打包工作流。

## 3. 开始打包

1. 点击仓库顶部 **Actions**。
2. 如果出现启用 Actions 的提示，按照页面提示启用。
3. 点击左侧 **Build VReader IPA**。
4. 点击右侧 **Run workflow**，分支选择 `main`，再点绿色 **Run workflow**。
5. 刷新页面，打开新出现的任务；黄色图标表示执行中，绿色对勾表示成功。

首次打包需要下载依赖并编译，通常需要等待十几分钟，也可能更久。不要因为页面暂时没变化而连续点运行。

如果没有 Run workflow 按钮：确认已登录仓库所有者账号、文件路径正确，而且文件已提交到默认分支 `main`。

## 4. 下载真正的 IPA

1. 在绿色成功的任务页面，滚动到下方 **Artifacts**。
2. 点击 **VReader-IPA**，下载 `VReader-IPA.zip`。下载时需要登录 GitHub。
3. 双击 ZIP 解压，得到 **vreader-unsigned.ipa**。
4. 通过 AirDrop 或 iCloud Drive 把 IPA 发到 iPhone，保存到“文件”App。

必须选择解压得到的 `.ipa`。不要把 `VReader-IPA.zip`、GitHub 的 `Source code.zip` 或源码文件夹导入 SideStore；也不要继续解压 IPA 本身。

也可以在 iPhone Safari 登录 GitHub 后直接下载 ZIP，再到“文件”App 点一下 ZIP 解压。

## 5. 在 iPhone 中安装

1. 连接 Wi-Fi。
2. 打开 **LocalDevVPN**，点 **Connect**，确认已连接。
3. 打开 **SideStore → My Apps（我的应用）**。
4. 点左上角 **＋**；如果出现安装来源菜单，选择从文件安装。
5. 在文件选择器中找到 `vreader-unsigned.ipa`，点选。
6. 等待 SideStore 签名和安装；按 App 的提示处理必要的确认。
7. 安装完成后，回到桌面打开 **vreader**。可以先导入一本 TXT 或 EPUB 检查阅读功能。

免费 Apple 账号安装的 App 通常需要在 7 天内刷新签名。之后使用 SideStore 刷新，通常不需要重新运行 GitHub 打包。

## 如果打包失败

打开红色失败的任务 → `build` → 展开报错步骤。若失败发生在编译阶段，任务底部会提供 `VReader-build-log`，下载解压可获得详细日志。

把失败步骤的报错或日志发给我，我可以按实际错误修正。YAML 和 Bash 语法检查不代表 Swift 源码一定能编译成功。

## 版本与来源

- 源项目：https://github.com/lllyys/vreader
- 本次固定源码：`b996ab4d828a180ae4b23d0b07d045b3d318c34f`，项目版本 `3.67.7`。
- 系统要求与构建说明：https://github.com/lllyys/vreader#requirements
- GitHub 产物下载：https://docs.github.com/en/actions/how-tos/manage-workflow-runs/download-workflow-artifacts?tool=webui
- GitHub Actions 计费规则：https://docs.github.com/en/billing/concepts/product-billing/github-actions
- SideStore 安装前提：https://docs.sidestore.io/zh/docs/installation/prerequisites
