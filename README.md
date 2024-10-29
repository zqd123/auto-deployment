# Auto Deployment

**一键部署前端项目。**

## Features

- 选择“Build & Deploy”来构建您的项目并将其部署到远程服务器。
- 选择“仅部署”以将项目构建目录部署到远程服务器。

![image](https://github.com/user-attachments/assets/701bece0-a702-4dc1-9412-4fa20cc1fb1b)



命令选项板中的命令：

![image](https://github.com/user-attachments/assets/e54bfe3d-8a1d-4508-8967-4579bcbf96ef)




## 扩展插件设置

两个settings.json示例：

- 第一种使用方法

> 选择“Build & Deploy”在本地构建项目并将dist上传到远程服务器。

```json
{
  "autoDeployment.config": {
    "configurations": [
      {
        "name": "dev",
        "local": {
          "projectPath": ".",
          "buildCmd": "yarn build",
          "outputDir": "dist"
        },
        "remote": {
          "deploymentPath": "~/nginx/html",
          "backupOriginalFiles": true,
          "backupTo": "~/backup",
          "deleteOriginalFiles": true
        },
        "ssh": {
          "host": "192.168.1.200",
          "port": 22,
          "username": "pi",
          "privateKey": "~/.ssh/id_rsa"
        }
      }
    ]
  }
}
```

- 第二种使用方法

> 选择“Deploy Only”将项目源代码上载到远程服务器，并执行脚本在远程服务器上进行构建和部署。

```json
{
  "autoDeployment.config": {
    "configurations": [
      {
        "name": "dev",
        "local": {
          "projectPath": ".",
          "outputDir": ".",
          "exclude": ["**/node_modules/**", "dist/**"]
        },
        "remote": {
          "deploymentPath": "~/web/projects/demo",
          "deleteOriginalFiles": true,
          "postCmd": "bash ./build_and_deploy.sh"
        },
        "ssh": {
          "host": "192.168.1.200",
          "port": 22,
          "username": "pi",
          "privateKey": "~/.ssh/id_rsa"
        }
      }
    ]
  }
}
```

**每个配置项的详细信息如下：**

- `local` 配置（本地项目的配置）：

  |  Key           | 默认值 | 必需 | 描述  |
  |  ----          | ----     | ----     | ----         |
  | `projectPath`  | .        | √        | 项目根路径（相对路径） |
  | `buildCmd`     |          |          | 本地项目的生成命令 |
  | `outputDir`    | dist     | √        | 编译后的产品输出路径。（相对于projectPath的路径） |
  | `exclude`      | []       |          | 部署时排除的文件。(path模式相对于outputDir） |

- `remote` 配置（远程服务器的配置）：

  |  Key                  | 默认值 | 必需 | 描述  |
  |  ----                 | ----     | ----     | ----         |
  | `deploymentPath`      |          | √        | 远程部署路径。（必须是绝对路径） |
  | `backupOriginalFiles` | false    |          | 是否需要备份原始文件? |
  | `backupTo`            | ~/backup |          | 原始文件的备份路径。(must绝对路径） |
  | `deleteOriginalFiles` | false    |          | 是否需要删除原始文件？ |
  | `postCmd`             |          |          | 部署后执行的命令 |

- `ssh` 配置（用于连接到远程服务器的SSH配置）：

  |  Key         | 默认值 | 必需 | 描述 |
  |  ----        | ----     | ----     | ----        |
  | `host`       |          | √        | 服务器的主机名或IP地址 |
  | `port`       | 22       | √        | 服务器的端口号 |
  | `username`   |          | √        | 用户名 |
  | `password`   |          |          | 密码 |
  | `privateKey` |          |          | 用于基于密钥或基于主机的用户身份验证的私钥（绝对路径）（OpenSSH格式）  |
  | `passphrase` |          |          | 对于加密的私钥，这是用于解密它的密码。 |


