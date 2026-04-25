# ComfyUI 本地部署完整指南

> 适用于 Windows 系统 + NVIDIA 显卡（6GB 显存及以上）  
> 本指南基于 RTX 1660 Super（6GB）实测通过

---

## 环境要求

| 项目 | 要求 |
|------|------|
| 操作系统 | Windows 10 / 11 64位 |
| 显卡 | NVIDIA GPU，显存 6GB 及以上 |
| 硬盘空间 | 至少 20GB 可用空间（建议放在 D 盘） |
| 网络 | 建议全程开启代理 |

---

## 第一步：安装 Python 3.10

ComfyUI 要求 **Python 3.10**，不支持 3.11 及以上版本。

1. 前往官网下载：https://www.python.org/downloads/release/python-31011/
2. 拉到页面底部，选择 **Windows installer (64-bit)**
3. 安装时注意：
   - 如果系统中**没有其他 Python 版本**：勾选 `Add Python to PATH`
   - 如果系统中**已有其他 Python 版本**：**不要勾选**，避免冲突

验证安装：

```bash
py -3.10 --version
# 输出：Python 3.10.x 即成功
```

---

## 第二步：安装 Git

用于下载 ComfyUI 代码及后续插件。

1. 前往官网下载：https://git-scm.com/download/win
2. 安装过程全部默认选项，一路点下一步

验证安装：

```bash
git --version
# 输出：git version x.x.x 即成功
```

---

## 第三步：克隆 ComfyUI

建议安装在 D 盘，模型文件较大，避免 C 盘空间不足。

```bash
git clone https://github.com/comfyanonymous/ComfyUI.git D:\ComfyUI
```

---

## 第四步：创建 Python 虚拟环境

使用虚拟环境隔离 ComfyUI 的依赖，避免与系统其他 Python 项目冲突。

```bash
py -3.10 -m venv D:\ComfyUI\venv
```

**每次使用前激活虚拟环境：**

```bash
D:\ComfyUI\venv\Scripts\activate
```

激活成功后，命令行最左边会出现 `(venv)` 标识。

> ⚠️ 后续所有 pip 命令都必须在激活状态下运行

---

## 第五步：安装 PyTorch（GPU 版本）

```bash
cd D:\ComfyUI
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

> 下载约 2.4GB，耐心等待，建议开代理

安装完成后验证 GPU 是否可用：

```bash
python -c "import torch; print(torch.cuda.is_available())"
# 输出 True 表示 GPU 可用
# 输出 False 表示只能使用 CPU，出图速度会很慢
```

---

## 第六步：安装 ComfyUI 依赖

```bash
pip install -r requirements.txt
```

---

## 第七步：下载模型

没有模型 ComfyUI 无法出图。推荐新手从以下网站下载：

- **哩布哩布 AI**（国内访问快）：https://www.liblib.art
- **Civitai**（模型最全）：https://civitai.com

**推荐新手第一个模型：DreamShaper v7**

- 搜索 `dreamshaper`，选择标签为 `Checkpoint`（不带 XL）的版本
- 下载 `.safetensors` 格式文件（约 2GB）

下载完成后，将文件放入：

```
D:\ComfyUI\models\checkpoints\
```

---

## 第八步：启动 ComfyUI

```bash
D:\ComfyUI\venv\Scripts\activate
cd D:\ComfyUI
python main.py --lowvram
```

> `--lowvram` 参数适用于 6GB 及以下显存的显卡，防止显存溢出

启动成功后，命令行会显示：

```
Starting server
To see the GUI go to: http://127.0.0.1:8188
```

浏览器访问 `http://127.0.0.1:8188` 即可进入 ComfyUI 界面。

---

## 一键启动脚本

每次启动不想手敲命令，可以新建一个 `start.bat` 文件，放在 `D:\ComfyUI\` 目录下：

```bat
@echo off
call D:\ComfyUI\venv\Scripts\activate
cd /d D:\ComfyUI
python main.py --lowvram
pause
```

以后双击 `start.bat` 即可一键启动。

---

## 常见问题

**Q：`py -3.10` 提示找不到命令？**  
A：Python 3.10 没有正确安装，重新安装并确认版本。

**Q：PyTorch 安装后 `cuda.is_available()` 返回 False？**  
A：检查显卡驱动是否为最新版，前往 NVIDIA 官网更新驱动后重试。

**Q：启动时显存溢出报错（out of memory）？**  
A：确认启动命令带了 `--lowvram` 参数。

**Q：模型加载失败？**  
A：确认 `.safetensors` 文件放在 `D:\ComfyUI\models\checkpoints\` 目录下，文件没有损坏。

---

## 显卡兼容参考

| 显存 | 推荐启动参数 | 支持模型 |
|------|------------|---------|
| 4GB | `--lowvram` | SD 1.5 |
| 6GB | `--lowvram` | SD 1.5，SDXL（较慢） |
| 8GB | 无需额外参数 | SD 1.5，SDXL |
| 12GB+ | 无需额外参数 | SD 1.5，SDXL，Flux |

---

## 参考资源

- ComfyUI 官方仓库：https://github.com/comfyanonymous/ComfyUI
- 哩布哩布 AI（模型下载）：https://www.liblib.art
- Civitai（模型下载）：https://civitai.com
- PyTorch 官网：https://pytorch.org
