---
createTime: 2025-12-19
---

## 前期准备

时间线的跨度和 div 条目（.ob-timelines 类）默认在预览中是隐藏的，不过如果你想展示内容，可以尝试在 Obsidian 设置-外观-CSS片段中添加一个类似这样的 CSS 片段：
```css
/* Render the ob-timelines span or div elements as inline blocks that use an italic font */
.ob-timelines {
  display: inline-block !important;
  font-style: italic;
}

/* Use the before pseudo element to display attributes of the span or div */
.ob-timelines::before {
  content: "🔖 " attr(data-start-date) ": " attr(data-title) ".";
  color: lilac;
  font-weight: 500;
}
```

Tag插件1：[Multi Properties](https://github.com/technohiker/obsidian-multi-properties)
批量增删标签到 frontmatter 区域（文件属性）

Tag插件2：[Multi Tag](https://github.com/technohiker/obsidian-multi-tag)
批量增删标签到文件夹中每个笔记的底部

## 增删标签

### 1、
采用[Multi Properties](https://github.com/technohiker/obsidian-multi-properties)给每个文件frontmatter区添加# timeline（必加标签）

### 2、
采用[Multi Tag](https://github.com/technohiker/obsidian-multi-tag)给每个文件底部添加# test，给后续批量化做准备

## VScode、Python批量化处理

### 1、
采用VScode搜索# test并替换为
```css
<div class="ob-timelines"
   data-start-date="{{currentDate}}"   
   data-title='{{title}}'                             
>
  简介: {{desc}}                         
</div>
```

### 2、
采用Python检测每个文件frontmatter 的属性来实现{{currentDate}}和{{title}}的一一对应

#### （1）创建.py代码

将以下代码完整复制，保存为 `replace_timeline_variables.py` 文件，并放置在你的笔记文件夹中。
```Python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
【Obsidian电影笔记批量更新工具】
功能：批量将 .md 文件中的时间轴块模板变量替换为对应 frontmatter 中的真实值。
使用方法：将此脚本放入笔记文件夹，直接运行。
"""

import os
import re
import sys

def update_timeline_blocks(folder_path='.'):
    """
    批量更新指定文件夹下所有.md文件中的时间轴块变量。
    """
    updated_count = 0
    error_files = []

    print("🎬 开始批量更新电影笔记时间轴变量...")
    print(f"📁 工作目录: {os.path.abspath(folder_path)}")
    print("=" * 50)

    # 遍历文件夹中的所有文件
    for filename in os.listdir(folder_path):
        if not filename.lower().endswith('.md'):
            continue  # 跳过非.md文件

        file_path = os.path.join(folder_path, filename)
        
        try:
            with open(file_path, 'r', encoding='utf-8') as f:
                content = f.read()

            # ----- 第一步：提取Frontmatter中的目标属性值 -----
            # 定义要查找的属性
            target_props = ['currentDate', 'title', 'desc']
            prop_values = {}

            for prop in target_props:
                # 正则表达式匹配 frontmatter 中的属性行
                # 例如匹配：currentDate: 2025-02-15
                pattern = rf'^{prop}:\s*(.+)'
                match = re.search(pattern, content, re.MULTILINE)
                if match:
                    # 成功获取，去除两端可能的空格和引号
                    prop_values[prop] = match.group(1).strip().strip('"\'')
                else:
                    # 未找到，使用占位符
                    prop_values[prop] = f'[未找到{prop}]'
                    print(f'  ⚠️  警告：文件【{filename}】中未找到属性【{prop}】')

            # ----- 第二步：替换时间轴块中的模板变量 -----
            # 定义要查找和替换的目标HTML代码块（使用多行模式）
            old_html_block_pattern = r'<div class="ob-timelines"\s*\n\s*data-start-date="{{currentDate}}"\s*\n\s*data-title=\'{{title}}\'\s*\n>\s*\n\s*简介: {{desc}}\s*\n</div>'
            
            # 构建新的、包含真实值的HTML代码块
            new_html_block = f'''<div class="ob-timelines"
   data-start-date="{prop_values['currentDate']}"
   data-title='{prop_values['title']}' 
>
  简介: {prop_values['desc']}
</div>'''

            # 执行替换
            if re.search(old_html_block_pattern, content):
                new_content = re.sub(old_html_block_pattern, new_html_block, content, flags=re.MULTILINE)

                # 检查内容是否真的发生了变化
                if new_content != content:
                    # 写回文件
                    with open(file_path, 'w', encoding='utf-8') as f:
                        f.write(new_content)
                    print(f'✅ 已成功更新: {filename}')
                    updated_count += 1
                else:
                    print(f'🔸 无需更新: {filename} (变量可能已被替换过)')
            else:
                print(f'⚠️  跳过: {filename} (文件中未找到指定的时间轴块)')

        except Exception as e:
            error_files.append((filename, str(e)))
            print(f'❌ 处理文件【{filename}】时出错: {e}')

    # ----- 输出报告 -----
    print("=" * 50)
    print("📊 批量更新完成!")
    print(f"  成功处理: {updated_count} 个文件")
    
    if error_files:
        print(f"  处理失败: {len(error_files)} 个文件")
        for fname, err in error_files:
            print(f'    - {fname}: {err}')
    else:
        print("  所有文件处理成功，无错误。")

    # 在Windows下运行完毕后保持窗口
    if sys.platform.startswith('win'):
        input("\n🔄 按回车键退出...")

if __name__ == "__main__":
    # 直接运行脚本，处理当前文件夹
    update_timeline_blocks()
```

#### （2）运行终端

Windows：在文件夹空白处，按住 Shift 键并点击鼠标右键，选择“在此处打开 PowerShell 窗口”或“打开命令窗口”。
运行命令：
```bash
python replace_timeline_variables.py
```

#### （3）运行结果

![[可视化时间轴-运行结果.png]]