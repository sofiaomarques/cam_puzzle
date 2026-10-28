# Quebra-Cabeça por Gestos

Um quebra-cabeça 3×3 controlado só com as mãos, direto no navegador. Você tira uma foto de si mesma fazendo uma "moldura" com os dedos, a imagem vira um quebra-cabeça embaralhado e você o resolve com gestos, sem encostar no mouse ou no teclado.

O reconhecimento das mãos usa o [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands) e roda inteiramente no navegador. O vídeo da câmera não é enviado para nenhum servidor.

## Como rodar

O projeto é um único arquivo HTML, sem build nem dependências para instalar.

1. Abra `index.html` em um navegador moderno (Chrome, Edge ou Safari).
2. Permita o acesso à câmera quando o navegador pedir.
3. Aguarde o carregamento do MediaPipe e do detector de mãos.

> Requer conexão com a internet: as bibliotecas do MediaPipe são carregadas via CDN (jsDelivr).
>
> Se o navegador bloquear a câmera ao abrir o arquivo direto (`file://`), sirva a pasta localmente:
>
> ```bash
> python3 -m http.server 8000
> ```
>
> e acesse `http://localhost:8000`.

## Como jogar

| Gesto | O que faz |
| --- | --- |
| 📷 **Moldura** | Junte polegar e indicador (pinça) nas **duas mãos** ao mesmo tempo para capturar a foto e criar o quebra-cabeça |
| ☝️ **Apontar** | Estenda só o indicador: seleciona a peça sob o cursor |
| ✊ **Punho** | Com uma peça selecionada, feche a mão apontando para uma peça **adjacente** para trocá-las |
| ✋ **Mão aberta** | Deseleciona a peça atual |

Quando todas as peças voltam ao lugar, aparece "🎉 Resolvido!". Faça a moldura de novo para começar com outra foto.

### Botões

- **Embaralhar**: embaralha novamente a foto atual.
- **Nova foto**: volta ao modo de captura.

## Como funciona

- **Detecção de mãos:** MediaPipe Hands, com até 2 mãos e 21 pontos de referência por mão.
- **Gestos:** são deduzidos comparando a posição da ponta de cada dedo com a articulação anterior (apontar, punho, mão aberta). A moldura é detectada quando a distância entre polegar e indicador fica pequena nas duas mãos.
- **Cursor:** a ponta do indicador é espelhada e mapeada para a grade do quebra-cabeça. Um ponto vermelho mostra essa posição sobre a câmera.
- **Captura:** o quadro atual do vídeo (espelhado) é desenhado em um canvas e dividido em 3×3 peças.
- **Troca de peças:** só é possível trocar peças vizinhas (acima, abaixo, esquerda ou direita).

## Dicas

- Use um ambiente bem iluminado e deixe as mãos inteiras visíveis na câmera.
- Mantenha a mão "dominante" (a primeira detectada) como a que controla o jogo.

## Tecnologias

- HTML, CSS e JavaScript puros
- [MediaPipe Hands](https://www.npmjs.com/package/@mediapipe/hands), `camera_utils` e `drawing_utils` via CDN
- Canvas 2D e `getUserMedia`
