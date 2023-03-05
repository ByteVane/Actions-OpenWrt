# Actions-OpenWrt

使用 GitHub Actions 构建 OpenWrt

## 使用方法

##### 一、有 .config 配置文件

- 点击`Create new file`（创建新文件）按钮
- 文件名填写为`.config`，把生成的`.config `文件的内容复制粘贴到下面的文本框中。
- 翻到页面最下方，点击`Commit new file`（提交新文件）按钮。
- 在 Actions 页面选择`Build OpenWrt`，然后点击`Run Workflow`按钮，即可开始编译。

##### 二、无 .config 配置文件，如何定制自己 .config文件

- 在 Actions 页面选择`Build OpenWrt`，然后点击`Run Workflow`按钮，把`SSH connection to Actions`的值改为`true`

- 在触发工作流程后，在 Actions 日志页面等待执行到`SSH connection to Actions`步骤，会出现类似下面的信息：

  ```
  To connect to this session copy-n-paste the following into a terminal or browser:
  
  ssh Y26QeagDtsPXp2mT6me5cnMRd@nyc1.tmate.io
  
  https://tmate.io/t/Y26QeagDtsPXp2mT6me5cnMRd
  ```

- 复制 SSH 连接命令粘贴到终端内执行，或者复制链接在浏览器中打开使用网页终端。（网页终端可能会遇到黑屏的情况，按 `Ctrl`+`C` 即可）

- ```
  cd openwrt && make menuconfig
  ```

- 定制好自己所需的配置后执行

  ```
  make defconfig
  ./scripts/diffconfig.sh > seed.config
  ```

- 这样配置的差异部分就写入`seed.config`这个文件了

- 输入`cat seed.config`查看自己的定制的 .config文件

- 完成后按`Ctrl`+`D`组合键或执行`exit`命令退出，后续编译工作将自动进行。

##### 三、源码更新自动编译

- [创建 Personal access token(PAT)](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL3NldHRpbmdzL3Rva2Vucy9uZXc) ，勾选`repo`权限，这将用于自动触发编译工作流程。

- 然后点击自己仓库的`Settings`选项卡，再点击`Secrets`。添加名为`ACTIONS_TRIGGER_PAT`的加密环境变量，保存刚刚创建的 PAT 。

- 在 Actions 页面选择`Update Checker`，点击`Run workflow`手动进行一次测试运行。如果没有报错且 OpenWrt 编译工作流程被触发，则代表测试通过。

- 最后编辑`Update Checker`的 workflow 文件（`.github/workflows/update-checker.yml`），取消注释（删除`#`）定时触发相关的部分。这里可以根据 cron 格式来设定检测的时间，时区为 UTC 。

  ```
  #  schedule:
  #    - cron: 0 */18 * * *
  ```

## License

[MIT](https://github.com/P3TERX/Actions-OpenWrt/blob/main/LICENSE) © ByteVane
