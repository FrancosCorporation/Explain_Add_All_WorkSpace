Para adicionar ao menu do windows abrir a pasta diretamente no vs code voce precisara:

1) Abra um novo bloco de notas (notepad) e salva o documento com o nome vsCodeOpenFolder.reg e o tipo como todos os arquivos

2) Cole o seguinte script dentro desse documento em branco SUBSTITUINDO O CAMINHO PELO SEU VSCODE ! :

Windows Registry Editor Version 5.00
; Open files

[HKEY_CLASSES_ROOT\*\shell\Open with VS Code]
@="Abrir com Vscode 😍"
"Icon"="C:\\Users\\Rodolfo\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe,0"

[HKEY_CLASSES_ROOT\*\shell\Open with VS Code\command]
@="\"C:\\Users\\Rodolfo\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%1\""

; This will make it appear when you right click ON a folder
; The "Icon" line can be removed if you don't want the icon to appear

[HKEY_CLASSES_ROOT\Directory\shell\vscode]
@="Abrir Pasta com Vscode 😍"
"Icon"="\"C:\\Users\\Rodolfo\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\",0"

[HKEY_CLASSES_ROOT\Directory\shell\vscode\command]
@="\"C:\\Users\\Rodolfo\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%1\""

; This will make it appear when you right click INSIDE a folder
; The "Icon" line can be removed if you don't want the icon to appear

[HKEY_CLASSES_ROOT\Directory\Background\shell\vscode]
@="Abrir Pasta com Vscode 😍"
"Icon"="\"C:\\Users\\Rodolfo\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\",0"

[HKEY_CLASSES_ROOT\Directory\Background\shell\vscode\command]
@="\"C:\\Users\\Rodolfo\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe\" \"%V\""

3) Clique no arquivo 2 vezes e execute:

4) Feito isso será exibido a opção de "Open Folder as VS Code" ao seu windows explorer:

Pronto, tudo certo para usufruir.