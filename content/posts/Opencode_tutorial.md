---
title: "Opencode tutorial for beginners"
date: 2026-01-31T00:00:00+08:00
draft: false
tags: ["Opencode", "Coding Agent", "LLM", "Tutorial"]
---
# Opencode tutorial for beginners

## 1. 介绍

Opencode [1] 定位是Claude Code 的开源替代品。

但在目前LLM发展的趋势下，Claude Code很难独占市场，同时我并不看好A\实施的技术封锁。世面上Coding Agent普遍装配有现在最火的Skills、MCP servers、Agents、Subagents，单一使用Claude Code、Codex、Gemini CLI面临的问题就是锁定该CLI采用的模型。虽然现在[2-3]能够实现在CLI级别下，通过Skills方式在三家CLI之间进行联动，但流程繁琐、上下文管理不够精细、不适用于科研，同时[4]这类切换中转站管理的方式也能解决短期额度不够的问题。

上述讨论的问题，并不在该教程中深入讨论，Opencode通过后续安装插件能更好解决上述调用多个模型的痛点问题。

> 开源软件注定拥有更高的可玩性，但是配置门槛相应会高一些。
  
### 1.1 部署

[1]中仓库给出了opencode的多个平台的Desktop和tui的安装方式，根据喜好进行安装。

推荐安装：
- Oh-my-opencode[5]
- Opencode-antigravity-auth[6]

后续会给出推荐理由

### 1.2 优势

  在介绍中，大概提了两点目前御三家的痛点问题，这在Opencode中正好是完美符合，同时对学生党更加友好，相应的至少有两个优势：
  - 能够灵活使用多个且多家模型；
  - 对使用API调用模型的用户更友好，配置文件写好即可使用slack command进行模型切换。
  - 上下文窗口有百分比显示，容易对单个对话进行控制。
  - (补充) 如今已经支持github copilot登陆，通过Opencode-antigravity-auth可使用Google one pro用户在antigravity的额度，不需要通过反代。

## 2. 配置

开源社区中有丰富的docs，[7]中有详细的配置流程可供参考，官方并未支持中文，差评！A\的docs都支持中文。[8]中有非官方的中文docs，谨慎阅读，容易夹带私货。

接下来，直接进入穷学生省流配置环节

### 2.1 github copilot

在安装好Opencode后，建议不要直接在终端直接运行，而是先运行下面命令。

```bash
opencode auth login
```

选择对应的model provider进行OAuth登陆，这里穷学生直接选择github copilot的学生包，添加好认证后，就已经可以使用opencode随意探索了。

*注意*
- 这里面有一个对于我这种强迫症患者必须要做的，不用听有些教程上说的，在`auth login`选择`other`手动添加自己的中转站。
- 对于添加了自己不用的provider，使用`opencode autho logout`进行相应删除，不然翻遍配置文件，都不知道自己添加的provider到底放在哪个位置。
- (重点)：如果发现登陆过后模型不可用，请打开github，在头像中点击`settings->copilot->Fetures`将需要用的模型打开，同时可以查看当前premium requests查看用量。

### 2.2 Google one pro

相信很多穷学生都是美国留学生，拥有了Google one pro订阅的账号，虽然额度一调再调，但是对于科研党是完全够用的，不讨论如何获取Google one pro(养美区号，1key认证，咸鱼一刀虚拟卡)。

在[6]中安装Opencode-antigravity-auth插件，该插件可以将Google one pro订阅用到极致。

#### 2.2.1 多账户设置

使用上述登陆命令，可以添加实现多账户登陆，一个家庭组就是六个人。
```bash
2 account(s) saved:
  1. user1@gmail.com
  2. user2@gmail.com

(a)dd new account(s) or (f)resh start? [a/f]:
```

*注意*： 这个插件下使用Gemini模型可以先使用antigravity的额度，限额后可以自动切换到Gemini CLI的额度。

配置文件存储在`~/.config/opencode/antigravity-accounts.json`，以明文形式储存账户凭证，涉及敏感信息。

#### 2.2.2 模型配置

目前在antigravity中支持的模型

|**模型名称 (Model)**|**变体 (Variants)**|**说明 (Notes)**|
|---|---|---|
|`antigravity-gemini-3-pro`|low, high|Gemini 3 Pro (支持思维链/thinking)|
|`antigravity-gemini-3-flash`|minimal, low, medium, high|Gemini 3 Flash (支持思维链/thinking)|
|`antigravity-claude-sonnet-4-5`|—|Claude Sonnet 4.5|
|`antigravity-claude-sonnet-4-5-thinking`|low, max|Claude Sonnet (支持长文本思维链)|
|`antigravity-claude-opus-4-5-thinking`|low, max|Claude Opus (支持长文本思维链)|

下面提供一个配置文件可以叫AI帮忙写入。

```json
 {
  "google": {
    "name": "Google",
    "models": {
      "antigravity-gemini-3-pro-high": {
        "name": "Gemini 3 Pro High (Antigravity)",
        "thinking": true,
        "attachment": true,
        "limit": {
          "context": 1048576,
          "output": 65535
        },
        "modalities": {
          "input": ["text", "image", "pdf"],
          "output": ["text"]
        }
      },
      "antigravity-gemini-3-pro-low": {
        "name": "Gemini 3 Pro Low (Antigravity)",
        "thinking": true,
        "attachment": true,
        "limit": {
          "context": 1048576,
          "output": 65535
        },
        "modalities": {
          "input": ["text", "image", "pdf"],
          "output": ["text"]
        }
      },
      "antigravity-gemini-3-flash": {
        "name": "Gemini 3 Flash (Antigravity)",
        "attachment": true,
        "limit": {
          "context": 1048576,
          "output": 65536
        },
        "modalities": {
          "input": ["text", "image", "pdf"],
          "output": ["text"]
        }
      },
      "antigravity-claude-sonnet-4-5-thinking": {
        "name": "Claude Sonnet 4.5 Thinking",
        "limit": {
          "context": 200000,
          "output": 64000
        },
        "modalities": {
          "input": ["text", "image", "pdf"],
          "output": ["text"]
        },
        "variants": {
          "low": {
            "thinkingConfig": {
              "thinkingBudget": 8192
            }
          },
          "max": {
            "thinkingConfig": {
              "thinkingBudget": 32768
            }
          }
        }
      },
      "antigravity-claude-opus-4-5-thinking": {
        "name": "Claude Opus 4.5 Thinking",
        "limit": {
          "context": 200000,
          "output": 64000
        },
        "modalities": {
          "input": ["text", "image", "pdf"],
          "output": ["text"]
        },
        "variants": {
          "low": {
            "thinkingConfig": {
              "thinkingBudget": 8192
            }
          },
          "max": {
            "thinkingConfig": {
              "thinkingBudget": 32768
            }
          }
        }
      },
      "antigravity-claude-sonnet-4-5": {
        "name": "Claude Sonnet 4.5",
        "limit": {
          "context": 200000,
          "output": 64000
        },
        "modalities": {
          "input": ["text", "image", "pdf"],
          "output": ["text"]
        }
      }
    }
  }
}
```

而这个配置文件是放在`~/.config/opencode/opencode.json`文件中，如果出现安装了opencode但是没有找到这个文件，那么让AI建一个。

官方docs中给出了模版

```json
{
  "$schema": "https://opencode.ai/config.json",
  // Theme configuration
  "theme": "opencode",
  "model": "anthropic/claude-sonnet-4-5",
  "autoupdate": true,
}
```

### 2.3 中转站API

上述github copilot和Google one pro就已经覆盖当前市面上最强的三家模型，同时opencode中还提供Minimax 2.1和GLM-4.7的国产免费模型，如果这都还不够，那么就添加中转站API来满足用量。

还是在`~/.config/opencode/opencode.json`中直接写入下面模版内容，手动添加`baseURL`和`apiKey`，同时核对中转站模型名称。

```json
{
  "provider": {
    "custom-anthropic-provider": {
      "options": {
        "baseURL": "https://<YOUR_API_PROXY_ENDPOINT>/v1",
        "apiKey": "sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
      },
      "models": {
        "claude-opus-4-5-20251101": {
          "name": "Claude Opus 4.5"
        },
        "claude-sonnet-4-5-20250929": {
          "name": "Claude Sonnet 4.5"
        },
        "claude-haiku-4-5-20251001": {
          "name": "Claude Haiku 4.5"
        }
      }
    },
    "custom-openai-provider": {
      "name": "Provider Name",
      "options": {
        "baseURL": "https://<YOUR_API_PROXY_ENDPOINT>/v1",
        "apiKey": "sk-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
      },
      "models": {
        "gpt-5.2-codex": {
          "name": "GPT 5.2 Codex",
          "options": {
            "reasoningEffort": "high",
            "store": false
          },
          "variants": {
            "low": {
              "options": {
                "reasoningEffort": "low"
              }
            },
            "medium": {
              "options": {
                "reasoningEffort": "medium"
              }
            },
            "high": {
              "options": {
                "reasoningEffort": "high"
              }
            }
          }
        }
      }
    }
  }
}
```

*注意*：
- 该配置文件就体现出它的优点了，可以写入多个provider的模型，而后面只需要`/models`就能轻松切换
- 这种方式是采用明文形式存储，在官方文档中给出另外两种更安全的方式。

```json
{
  "$schema": "https://opencode.ai/config.json",
  "model": "{env:OPENCODE_MODEL}", # ["./custom-instructions.md"],
  "provider": {
    "anthropic": {
      "models": {},
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}" # ["./custom-instructions.md"],
      }
    }
  }
}
```


## 3. 插件

opencode中可以安装各种plugins，可根据自己的需求进行安装，目前推荐的就是下面两个必装插件。

### 3.1 opencode-antigravity-auth

  在2.2中已经详细介绍了，这里仅为了保持行文的结构性。

### 3.2 oh-my-opencode

#### 3.2.1 安装

[5]中给了该插件的仓库，懒人方法是直接进入opencode，告诉它，在在全局或者在当前项目下安装，即可。可以预见的未来，github仓库就不再是放源码，而是放一堆markdown文件，AI已经具备复现项目的能力。

#### 3.2.2 配置

同样全局安装该插件后会生成`~/.config/opencode/oh-my-opencode.json`的配置文件

下面直接给配置模板

```json
{

  "$schema": "https://raw.githubusercontent.com/code-yeongyu/oh-my-opencode/master/assets/oh-my-opencode.schema.json",

  "google_auth": false,

  "agents": {

    "Sisyphus": {

      "model": "antigravity-claude-opus-4-5-thinking"

    },

    "oracle": {

      "model": "antigravity-claude-sonnet-4.5-thinking"

    },

    "librarian": {

      "model": "your_provider/claude-sonnet-4-5-20250929"

    },

    "explore": {

      "model": "google/antigravity-gemini-3-flash"

    },

    "frontend-ui-ux-engineer": {

      "model": "google/antigravity-gemini-3-pro-high"

    },

    "document-writer": {

      "model": "google/antigravity-gemini-3-flash"

    },

    "multimodal-looker": {

      "model": "google/antigravity-gemini-3-pro-high"

    }

  }

}
```

*注意*：
- 这里的model设置要与`opencode.json`文件对应，该问题可以直接通过AI进行校对。
- 每个模型对应的权限可以单独设置，这里采用项目的默认设置.
- (重点)安装这个插件会默认关闭opencode自带的两个Agents，`Plan` and `Build`，可以在[7]中重新开启。

后面的内容不是这次教程的重点，`oh-my-opencode`是一个非常强大的插件，内置了很多Agents，skills，MCP servers，Hooks。

## 4. 提示词

Opencode 支持三种作用域的规则

|**作用域**|**文件位置**|**适用场景**|
|---|---|---|
|**全局规则**|`~/.config/opencode/AGENTS.md`|**所有项目通用**的个人偏好（|
|**项目规则**|项目根目录 `AGENTS.md`|**特定项目专用**的规范|
|**配置文件**|`opencode.json` 的 `instructions` 字段|用于**引用多个规则文件**，或者配置更复杂的指令集。|

*注意*：同时兼容Claude Code的`CLAUDE.md`

## 5. 进阶

~~下次出一个进阶版教程。~~

## 参考文献

1. <https://github.com/anomalyco/opencode>
2. <https://github.com/GuDaStudio/skills>
3. <https://github.com/fengshao1227/ccg-workflow>
4. <https://github.com/farion1231/cc-switch>
5.  <https://github.com/code-yeongyu/oh-my-opencode>
6. <https://github.com/NoeFabris/opencode-antigravity-auth>
7. <https://opencode.ai/docs>
8. <https://www.opencodecn.com/>
