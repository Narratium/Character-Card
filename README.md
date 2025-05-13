# Character-Card

一个为 SillyTavern,Narratium 精心整理的公共角色卡片集合,可用于外部 API 资源访问请求
包含适用于奇幻、科幻、历史、情感旅程等场景的入门角色与故事种子。  

非常适合用户、创作者与开发者：
- 丰富冒险体验
- 融合进开源项目 Narratium.ai
- 快速搭建角色素材库

欢迎贡献更多角色卡片！

## API 使用示例

```javascript
"use client";
import { useEffect, useState } from "react";

const GITHUB_API_URL = "https://api.github.com/repos/Narratium/Character-Card/contents";

interface GithubFile {
  name: string;
  download_url: string;
}

interface CharacterInfo {
  displayName: string;
  author: string;
}

export default function FetchCharacterFilesExample() {
  const [characterInfos, setCharacterInfos] = useState<CharacterInfo[]>([]);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const fetchFiles = async () => {
      try {
        const res = await fetch(GITHUB_API_URL);
        const data = await res.json();
        if (Array.isArray(data)) {
          const pngFiles = data.filter((item: any) => item.name.endsWith(".png"));
          const infos = pngFiles.map((file: GithubFile) => extractCharacterInfo(file.name));
          setCharacterInfos(infos);
        } else {
          setError("Fetch Error: Unexpected response");
        }
      } catch (e) {
        setError("Fetch Error: " + (e as any).message);
      }
    };

    fetchFiles();
  }, []);

  const extractCharacterInfo = (fileName: string): CharacterInfo => {
    const nameWithoutExt = fileName.replace(/\.png$/, "");
    const parts = nameWithoutExt.split(/--/);

    if (parts.length === 2) {
      return {
        displayName: parts[0].trim(),
        author: parts[1].trim().length > 5 ? parts[1].trim().substring(0, 5) : parts[1].trim()
      };
    } else {
      return { 
        displayName: nameWithoutExt, 
        author: "Unknown" 
      };
    }
  };

  useEffect(() => {
    if (characterInfos.length > 0) {
      console.log("Character Infos:", characterInfos);
    }
  }, [characterInfos]);

  if (error) console.error(error);

  return null;
}
```

## 贡献方式

1. Fork 本仓库
2. 按照现有格式添加角色卡（推荐使用 `卡片名称--作者.png` 命名）
3. 提交 Pull Request

## 授权说明

所有角色卡片遵循各自附带的授权（如适用）。  
请尊重作者署名，尊重创作者权益，竞争一切形式的侵权行为，竞争未经允许的商业行为。

## 贡献者
贡献卡片即可加入贡献者列表，如暂未加入，请联系：qianzhang.happyfox@gmail.com

## 协议
> 协议全文参见：[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.zh)


