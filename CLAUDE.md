# Diretrizes de migração do ECO: Fragmento do Amanhã

## Objetivo
Refatorar o ECO para uma publicação melhor, sem bugs de lógica e sem precisar do vídeo
de instalação (hoje o jogo é marcado como PUA). Qualquer tecnologia serve, desde que rode
bem e seja divertido.

## Estado atual
- Aventura narrativa de terminal em PT-BR, com 4 puzzles.
- Node.js puro, sem dependências, só Windows.
- Instalador Inno Setup (instala Node e Windows Terminal via winget).
- Áudio via `mpg123.exe`; save criptografado em `save/save.dat`.
- Site em `frontEnd/` (vídeo "Como instalar o ECO" em `frontEnd/index.html`, ~linha 165).

## Causas do PUA a eliminar
- Instalador mexendo no Defender.
- PowerShell oculto.
- O jogo apagar o próprio `game.js`.
- Reescrever atalhos e fechar o terminal à força (`src/chapters/final.js` ~677-681).
- Criar arquivos na Área de Trabalho, home e AppData.
- Nome do jogador obtido via `net user` (`src/utils/nameGetter.js`).

## Bugs conhecidos a não reproduzir
- Resposta "sim" sem `trim` fecha o jogo (`src/utils/inputManager.js` ~798).
- Atividade 4 deixa o leitor de teclado ativo após o tempo acabar (~820-824); a resposta é
  ficar 3 min sem digitar, sem nenhuma pista.
- Marcador `<SLEEP:>` tratado como `<PAUSE:>` (`src/utils/textManager.js` ~211).
- Jogo inutilizável após o final.
- Aviso obrigatório de 45 s antes do início.

## Diretrizes de migração
1. Plataforma: portar para terminal no navegador (GitHub Pages/itch.io). Opcionalmente
   empacotar com Tauri/Electron para um `.exe` limpo.
2. Sem instalador e sem mexer no sistema do usuário. Nada de winget, Defender, atalhos
   ou arquivos fora do app.
3. Save em `localStorage` (sem criptografia obscura; versionar o formato).
4. Entrada única e normalizada (`trim`, minúsculas, sem acentos quando fizer sentido).
5. Capítulos como dados (texto, pausas, escolhas, efeitos), não como código espalhado.
6. O "sequestro" do final deve ser simulado dentro da janela do jogo.
7. Puzzle 1: sistema de arquivos falso (`ls`/`cd`/`cat`).
8. Puzzle 2: popup do próprio jogo.
9. Atividade 4: adicionar pista e encerrar o leitor de teclado ao fim do tempo.
10. Permitir pular/acelerar texto.
11. Após o final: replay e escolha de capítulo.
12. Configurações de volume e velocidade do texto.
13. Remover avisos obrigatórios e esperas forçadas.

## Convenções
- Todo texto do jogo em PT-BR.
- Preservar a narrativa e o tom originais.
- Sem alterações de lógica narrativa durante a portabilidade; correções só nos bugs listados.
