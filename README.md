# Explain_Add_All_WorkSpace

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
2. **Substitua todos os caminhos** `C:\Users\Rodolfo\AppData\Local\Programs\Microsoft VS Code\Code.exe` pelo caminho do VS Code na sua máquina (o padrão costuma ser `C:\Users\<SEU_USUARIO>\AppData\Local\Programs\Microsoft VS Code\Code.exe`).
3. Importe o arquivo: dê duplo clique nele (aceitando o aviso do Windows) ou rode no terminal:

```powershell
reg import "Files\vsCodeOpenFolder.reg"
```

O guia detalhado com o passo a passo (incluindo a criação manual do arquivo pelo Notepad) está em [`Add_short_vscode_windows.md`](Add_short_vscode_windows.md).

### Como desfazer

Faça backup antes (`reg export`) e, para remover os atalhos criados, rode em um terminal **como administrador**:

```powershell
reg delete "HKEY_CLASSES_ROOT\*\shell\Open with VS Code" /f
reg delete "HKEY_CLASSES_ROOT\Directory\shell\vscode" /f
reg delete "HKEY_CLASSES_ROOT\Directory\Background\shell\vscode" /f
```

## Estrutura do projeto

```
.
├── Add_short_vscode_windows.md   # guia passo a passo (com trecho do .reg)
└── Files/
    └── vsCodeOpenFolder.reg      # arquivo para importar no Registro do Windows
```

## Observações

- O caminho do executável está fixado no exemplo original (`C:\Users\Rodolfo\...`) — obrigatório ajustar antes de importar.
- O guia em markdown e o `.reg` divergem só no rótulo do menu (`😍` no guia, `:)` no arquivo); ambos podem ser personalizados.
- Projetado para Windows; sem efeito em Linux/macOS.

## Licença

MIT — veja [LICENSE](LICENSE).
