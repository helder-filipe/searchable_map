# Mapa pesquisável da Infralobo

Abra a aplicação através de um servidor HTTP (por exemplo, GitHub Pages). Para executar localmente: `python -m http.server 8000`.

## Delimitar urbanizações e zonas

1. Escolha o tipo (**Urbanização** ou **Zona**), clique em **Nova área** e indique o nome e a cor.
2. Clique no mapa para marcar os pontos do contorno, pela sua ordem. Arraste o fundo para deslocar o mapa e utilize a roda do rato para ampliar.
3. São necessários pelo menos três pontos que formem uma área. Evite cruzar as linhas do contorno.
4. Use **Retirar ponto** para corrigir o último ponto ou arraste qualquer ponto branco para o ajustar.
5. Clique em **Guardar área** para fechar o contorno.
6. Para alterar uma área, selecione-a na lista e escolha **Editar limites**. Também pode alterar o nome e a cor durante a edição. **Cancelar** preserva a versão guardada.
7. Desative **Delimitar áreas** para voltar a marcar moradias.
8. Use **Exportar JSON** para descarregar o projeto. Guardar área mantém os dados apenas na sessão: exporte antes de fechar ou atualizar a página.

## Formato JSON

A exportação usa um objeto com `version: 3`, `mapa`, `moradias`, `urbanizacoes` e `zonas`. Cada área contém `id`, `tipo`, `nome`, `cor` e `points`, uma lista de objetos `{x, y}`. O contorno fecha implicitamente do último ponto para o primeiro.

As coordenadas dos contornos pertencem ao SVG original `MAPA Infralobo.svg`; **não são latitude/longitude nem GeoJSON**. O campo `mapa.viewBox` identifica a referência do desenho. As coordenadas GPS existentes das moradias continuam incluídas.

**Importar JSON** aceita:

- Projetos versão 2 (urbanizações) e versão 3 (urbanizações e zonas): após confirmação, substituem as moradias mapeadas e as áreas da sessão.
- Ficheiros antigos com uma lista de moradias: atualizam as moradias correspondentes e preservam as áreas existentes.

As aplicações externas que esperavam uma lista na raiz devem passar a ler `dados.moradias` ao utilizar a nova exportação. Os ficheiros JSON antigos do repositório não foram alterados.

## Exportar uma área para imagem

Guarde a área e selecione-a na lista. Utilize **Exportar SVG** ou **Exportar PNG**. A imagem contém o mapa original e o contorno da área selecionada; exclui as outras delimitações e os marcadores da interface.

- **SVG:** preserva os vetores, os estilos e as imagens incorporadas no mapa, sem depender do zoom do ecrã. As imagens raster existentes mantêm a resolução original.
- **PNG:** escolha 2048, 4096 (predefinição) ou até 8192 píxeis no lado maior. A proporção é mantida. Para limitar a memória, imagens próximas de um quadrado são reduzidas a um máximo de 32 megapíxeis; a dimensão efetiva aparece após a exportação.
- **Recortar pelo contorno:** inclui apenas o interior da área. Desative para obter o recorte retangular com o mapa envolvente.
- **Fundo branco:** preenche as regiões transparentes do recorte; não remove fundos já existentes no mapa original.

Para voltar a editar, conserve também o JSON. SVG e PNG são exportações para utilização gráfica.

## Verificação no navegador

Com Node.js, Playwright e Chromium instalados no ambiente de testes, sirva esta pasta em `http://127.0.0.1:8765` e execute `node tests/urbanizacoes.cjs`. Pode definir `MAP_TEST_URL` para outro servidor. O teste verifica desenho, ajuste de pontos, importação/exportação JSON e exportação de uma zona em SVG e PNG.
