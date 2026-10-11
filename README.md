# Explain_Add_All_WorkSpace

## ℹ️ Sobre este repositório

Notas de documentação sobre workspace.

Guia em português que ensina a adicionar a opção **"Abrir com VS Code"** ao menu de contexto do Windows Explorer, acompanhado do arquivo `.reg` pronto para importar.

![Markdown](https://img.shields.io/badge/Markdown-000000?style=flat&logo=markdown&logoColor=white)
![Windows](https://img.shields.io/badge/Windows-Registro-0078D6?style=flat&logo=windows&logoColor=white)
![Licença](https://img.shields.io/badge/licen%C3%A7a-MIT-green)
![Status](https://img.shields.io/badge/status-utilit%C3%A1rio-blue)

## Sobre

Repositório de documentação criado em abril de 2023 para registrar uma dica de produtividade no Windows: incluir atalhos do VS Code no menu de clique direito (para arquivos, para pastas e para o fundo de uma pasta). É útil para quem trabalha com Windows e quer abrir projetos sem navegar até o terminal.

> ⚠️ **Atenção: o arquivo `.reg` altera o Registro do Windows** (chaves em `HKEY_CLASSES_ROOT`). Importar um `.reg` modifica o sistema — exporte um backup antes e só execute se souber o que está fazendo. Nenhuma alteração é feita automaticamente por este repositório: a importação é uma ação manual sua.

## O que o `.reg` faz

O arquivo `Files/vsCodeOpenFolder.reg` cria três chaves:

| Chave | Onde aparece no menu de contexto |
| --- | --- |
| `HKEY_CLASSES_ROOT\*\shell\Open with VS Code` | Clique direito em **arquivos** |
| `HKEY_CLASSES_ROOT\Directory\shell\vscode` | Clique direito em **pastas** |
| `HKEY_CLASSES_ROOT\Directory\Background\shell\vscode` | Clique direito no **fundo da pasta** |

Cada chave define um rótulo (`@`) e um ícone (`Icon`) e registra o comando que chama `Code.exe` com o item selecionado (`%1` para arquivo/pasta e `%V` para o fundo da pasta).

## Como usar

1. Abra `Files/vsCodeOpenFolder.reg` em um editor de texto.
2. **Substitua todos os caminhos** `C:\Users\Rodolfo\AppData\Local\Programs\Microsoft VS Code\Code.exe` pelo caminho do VS Code na sua máquina. Preserve as aspas internas escapadas no valor `command`: elas mantêm o caminho com espaços como um único executável. Por exemplo:

```reg
@="\"C:\\Users\\<SEU_USUARIO>\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%1\""
```

1. Antes de substituir chaves que já existam, exporte cada chave afetada para um arquivo de backup. Ignore as chaves que ainda não existem:

```powershell
reg export "HKEY_CLASSES_ROOT\*\shell\Open with VS Code" "backup-arquivos.reg" /y
reg export "HKEY_CLASSES_ROOT\Directory\shell\vscode" "backup-pastas.reg" /y
reg export "HKEY_CLASSES_ROOT\Directory\Background\shell\vscode" "backup-fundo.reg" /y
```

1. Importe o arquivo: dê duplo clique nele (aceitando o aviso do Windows) ou rode no terminal:

```powershell
reg import "Files\vsCodeOpenFolder.reg"
```

Se o Windows negar acesso às chaves, abra o PowerShell como administrador e repita o comando.

1. Confirme que as três chaves foram criadas:

```powershell
reg query "HKEY_CLASSES_ROOT\*\shell\Open with VS Code"
reg query "HKEY_CLASSES_ROOT\Directory\shell\vscode"
reg query "HKEY_CLASSES_ROOT\Directory\Background\shell\vscode"
```

O guia detalhado está em [`Add_short_vscode_windows.md`](Add_short_vscode_windows.md). `Files/vsCodeOpenFolder.reg` é a fonte canônica dos rótulos, ícones e comandos; o guia orienta a personalização sem duplicar o arquivo inteiro.

### Como desfazer

Faça backup antes (`reg export`) e, para remover os atalhos criados, rode em um terminal **como administrador**:

```powershell
reg delete "HKEY_CLASSES_ROOT\*\shell\Open with VS Code" /f
reg delete "HKEY_CLASSES_ROOT\Directory\shell\vscode" /f
reg delete "HKEY_CLASSES_ROOT\Directory\Background\shell\vscode" /f
```

## Estrutura do projeto

```text
.
├── Add_short_vscode_windows.md   # caminho, backup e verificação do Registro
└── Files/
    └── vsCodeOpenFolder.reg      # arquivo para importar no Registro do Windows
```

## Observações

- O caminho do executável está fixado no exemplo (`C:\Users\Rodolfo\...`) — ajuste-o antes de importar e preserve as aspas internas escapadas do comando.
- Os rótulos e o formato de `Icon` estão padronizados entre as três chaves do arquivo `.reg`.
- Projetado para Windows; sem efeito em Linux/macOS.

## Licença

MIT — veja [LICENSE](LICENSE).
