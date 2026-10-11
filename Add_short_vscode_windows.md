# Adicionar o VS Code ao menu de contexto do Windows

O arquivo [`Files/vsCodeOpenFolder.reg`](Files/vsCodeOpenFolder.reg) é a fonte canônica dos rótulos, ícones e comandos. Ele adiciona opções para abrir um arquivo, uma pasta ou o fundo da pasta no VS Code.

> **Atenção:** importar um arquivo `.reg` altera o Registro do Windows. Revise o conteúdo e faça backup das chaves que já existirem antes de importar.

## 1. Ajuste o caminho do VS Code

Abra `Files/vsCodeOpenFolder.reg` em um editor de texto e substitua todas as ocorrências do caminho padrão pelo caminho de `Code.exe` instalado na sua máquina.

Preserve as aspas internas escapadas na linha `command`; elas mantêm caminhos com espaços como um único executável. Por exemplo:

```reg
@="\"C:\\Users\\<SEU_USUARIO>\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%1\""
```

O arquivo também usa `%V` para abrir o fundo da pasta. Não altere `%1` nem `%V` ao substituir o caminho.

Os valores `Icon` também usam aspas internas para o caminho com espaços:

```reg
"Icon"="\"C:\\Users\\<SEU_USUARIO>\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\",0"
```

## 2. Faça backup das chaves existentes

Se alguma destas chaves já existir, exporte-a antes da importação. Pule o comando correspondente quando a chave ainda não existir.

```powershell
reg export "HKEY_CLASSES_ROOT\*\shell\Open with VS Code" "backup-arquivos.reg" /y
reg export "HKEY_CLASSES_ROOT\Directory\shell\vscode" "backup-pastas.reg" /y
reg export "HKEY_CLASSES_ROOT\Directory\Background\shell\vscode" "backup-fundo.reg" /y
```

## 3. Importe o arquivo

No PowerShell aberto na raiz deste repositório, execute:

```powershell
reg import "Files\vsCodeOpenFolder.reg"
```

Leia a confirmação do Windows antes de aceitar a alteração no Registro.

## 4. Verifique as chaves

Depois da importação, confirme que as três entradas existem:

```powershell
reg query "HKEY_CLASSES_ROOT\*\shell\Open with VS Code"
reg query "HKEY_CLASSES_ROOT\Directory\shell\vscode"
reg query "HKEY_CLASSES_ROOT\Directory\Background\shell\vscode"
```

O menu de contexto mostra **Abrir com VS Code** para arquivos e **Abrir pasta com VS Code** para pastas e para o fundo de uma pasta.

Para remover as entradas, consulte os comandos `reg delete` no [README](README.md#como-desfazer).
