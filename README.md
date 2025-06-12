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

### 4. Configuração do Java Runtime

1. Pressione `Ctrl + Shift + P`
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
      "name": "Debug Minha API",
      "request": "launch",
      "mainClass": "br.com.fachesf.Application",
      "projectName": "application",
      "cwd": "${workspaceFolder}/application",
      "args": "",
      "console": "integratedTerminal"
    }
  ]
}
```

![_launch json](https://github.com/user-attachments/assets/f82c7ea5-0080-4432-9887-d05a1cb51773)

---

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
mvn clean install -s "C:\apache-maven-3.5.4\conf\settings.xml"
```
#### O local acima do maven na raiz é se você for administrador da máquina 
#### Se você não for administrador da máquina (ambiente de uma empresa que você loga com Active Directory) utilize SEMPRE ambiente do usuário ex: C:\Users\marcello.pedrosa\.m2
---

## ✅ Pronto!

Seu ambiente está configurado para desenvolvimento com Java, Lombok e Spring Boot no VSCode.
