# ☕ Configuração do Ambiente Java no VSCode

Este guia orienta os desenvolvedores na configuração de um ambiente de desenvolvimento Java completo no Visual Studio Code (VSCode), incluindo suporte ao Lombok e Spring Boot.

---

## ✅ Pré-requisitos

### 1. Instalar o Java JDK

Baixe e instale a versão desejada do Java JDK (recomenda-se Java 21 para compatibilidade com diversos frameworks):

👉 [Download do JDK 21](https://www.oracle.com/java/technologies/javase-jdk21-downloads.html)

### 2. Instalar o Apache Maven

Instale o Maven para gerenciamento de dependências e builds:

👉 [Download do Maven](https://maven.apache.org/download.cgi)

Após instalar, certifique-se de adicionar o `bin` do Maven ao `PATH` do sistema.

![image](https://github.com/user-attachments/assets/a7b09e87-4f56-48f7-ba89-734d51516772)

![image](https://github.com/user-attachments/assets/f258d965-54ce-414a-9872-b94c2f2b87fe)

Verifique a instalação:

```bash
mvn -v
```

---

## 🛠️ Configuração do VSCode

### 3. Extensões Recomendadas

Instale as seguintes extensões no VSCode:

- ✅ [Lombok Annotations Support for VS Code](https://marketplace.visualstudio.com/items?itemName=GabrielBB.vscode-lombok)

![_lombok](https://github.com/user-attachments/assets/09f7a8c1-e024-4a01-9fd4-dab2221a100e)

- ✅ [Extension Pack for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)

![_extension_java_pack](https://github.com/user-attachments/assets/0d5a48ef-aa34-44ae-b168-2ba59ad94ab4)

- ✅ [Spring Boot Extension Pack](https://marketplace.visualstudio.com/items?itemName=Pivotal.vscode-spring-boot)

![_extension_spring_boot](https://github.com/user-attachments/assets/8acf2e72-8fe6-4008-bc8f-eec55a27a6b7)

---

### 4. Configuração do Java Runtime (nesse projeto foi utilizado Java 11, com outra versão é o mesmo passo-a-passo)

1. Pressione `Ctrl + Shift + P`

![Captura de tela 2025-06-12 101856](https://github.com/user-attachments/assets/c19ccd3d-ee43-49f9-828c-e3bdc296ddbb)

2. Digite e selecione `Java: Configure Java Runtime`

![_java_settings](https://github.com/user-attachments/assets/dfcac762-c357-42a5-9df3-4d07bda0c3aa)

---

### 5. Configuração do projeto com arquivos VSCode

#### 5.1 Criar pasta `.vscode`

Na raiz do projeto, crie uma pasta chamada `.vscode`.

#### 5.2 Criar `settings.json`

Crie o arquivo `.vscode/settings.json` com o conteúdo abaixo (ajuste o caminho do JDK conforme seu ambiente):

```json
{
  "java.configuration.runtimes": [
    {
      "name": "JavaSE-11",
      "path": "C:\\Program Files\\Java\\jdk-11.0.12",
      "default": true
    }
  ],
  "maven.settingsFile": "C:\\apache-maven-3.5.4\\conf\\settings.xml"
}
```
![_settings_json](https://github.com/user-attachments/assets/0ed5234c-9d29-4bf4-9b4e-68b77e28ada3)


#### 5.3 Criar `launch.json`

Crie o arquivo `.vscode/launch.json` com o conteúdo abaixo:

```json
{
    "version": "0.2.0",
    "configurations": [
      {
        "type": "java",
        "name": "Debug Spring Boot",
        "request": "attach",
        "hostName": "localhost",
        "port": 5005,
        "timeout": 120000,
        "preLaunchTask": "mvnDebug",
        "projectName": "application",
        "sourcePaths": [
          "${workspaceFolder}/application/src/main/java",
          "${workspaceFolder}/domain/src/main/java",
          "${workspaceFolder}/infrastructure/src/main/java",
          "${workspaceFolder}/usecase/src/main/java"
        ],
        "postDebugTask": "terminate mvnDebug"
      }
    ],
    "compounds": []
  }
```

#### 👉 Todos módulos DEVEM estar mapeados em sourcePaths para saber a raiz de execução de cada módulo "/src/main/java"
#### 👉 Application é a classe que inicia o spring boot, pois ela tem o decorator: @SpringBootApplication()

![image](https://github.com/user-attachments/assets/308dd609-fde3-4ec9-b5c3-dc3d1bf319de)


---

#### 5.4 Criar `task.json`

Crie o arquivo `.vscode/task.json` com o conteúdo abaixo:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "mvnDebug",
      "type": "shell",
      "command": "cd ${workspaceFolder}/application; mvn clean compile; Write-Host 'Iniciando aplicação em modo debug...'; mvn spring-boot:run '-Dspring-boot.run.jvmArguments=-agentlib:jdwp=transport=dt_socket,server=y,suspend=y,address=5005'",
      "isBackground": true,
      "problemMatcher": {
        "pattern": [
          {
            "regexp": ".",
            "file": 1,
            "location": 2,
            "message": 3
          }
        ],
        "background": {
          "activeOnStart": true,
          "beginsPattern": "Iniciando aplicação em modo debug...",
          "endsPattern": "Listening for transport dt_socket at address: 5005"
        }
      }
    },
    {
      "label": "terminate mvnDebug",
      "command": "echo ${input:terminate}",
      "type": "shell",
      "problemMatcher": []
    }
  ],
  "inputs": [
    {
      "id": "terminate",
      "type": "command",
      "command": "workbench.action.tasks.terminate",
      "args": "mvnDebug"
    }
  ]
}
```
#### 👉 Lembre-se que o projeto em questão (web) está no módulo application, por isso o mapeamento em: ${workspaceFolder}/application

## 🔎 Testando a Configuração

### 6. Testar Maven a partir do diretório raiz

Execute no terminal, na raiz do projeto (onde está o `pom.xml` principal):

```bash
mvn help:effective-settings
```

---

### 7. Limpar configurações antigas do Eclipse (se necessário)

Se houver resquícios de configuração do Eclipse, force o uso do `settings.xml` do Maven com o comando:

```bash
mvn clean install
```

```
👉 O local acima do maven na raiz é se você for administrador da máquina

👉 >> Recomendado << Se você não for administrador da máquina (ambiente de uma empresa que você loga com Active Directory) utilize SEMPRE ambiente do usuário ex: C:\Users\marcello.pedrosa\.m2
```

## ✅ Debug

Agora coloque a aplicação no modo debug para executar:

![image](https://github.com/user-attachments/assets/fc9674e7-ac8e-4e87-962a-cd9842f311cc)



