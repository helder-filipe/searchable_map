# Mapa pesquisável da Infralobo

Abra a aplicação através de um servidor HTTP (por exemplo, GitHub Pages). Para executar localmente: `python -m http.server 8000`.

## Delimitar urbanizações

1. Escolha **Nova área** e indique o nome e a cor.
2. Clique no mapa para marcar os pontos do contorno, pela sua ordem. Arraste o fundo para deslocar o mapa e utilize a roda do rato para ampliar.
3. São necessários pelo menos três pontos que formem uma área. Evite cruzar as linhas do contorno.
4. Use **Retirar ponto** para corrigir o último ponto ou arraste qualquer ponto branco para o ajustar.
5. Clique em **Guardar área** para fechar o contorno.
6. Para alterar uma área, selecione-a na lista e escolha **Editar limites**. Também pode alterar o nome e a cor durante a edição. **Cancelar** preserva a versão guardada.
7. Desative **Delimitar urbanizações** para voltar a marcar moradias.
8. Use **Exportar JSON** para descarregar o projeto. Guardar área mantém os dados apenas na sessão: exporte antes de fechar ou atualizar a página.

## Formato JSON

A exportação usa um objeto com `version: 2`, `mapa`, `moradias` e `urbanizacoes`. Cada urbanização contém `id`, `nome`, `cor` e `points`, uma lista de objetos `{x, y}`. O contorno fecha implicitamente do último ponto para o primeiro.

As coordenadas dos contornos pertencem ao SVG original `MAPA Infralobo.svg`; **não são latitude/longitude nem GeoJSON**. O campo `mapa.viewBox` identifica a referência do desenho. As coordenadas GPS existentes das moradias continuam incluídas.

**Importar JSON** aceita:

- Projetos versão 2: após confirmação, substituem as moradias mapeadas e as áreas da sessão.
- Ficheiros antigos com uma lista de moradias: atualizam as moradias correspondentes e preservam as áreas existentes.

As aplicações externas que esperavam uma lista na raiz devem passar a ler `dados.moradias` ao utilizar a nova exportação. Os ficheiros JSON antigos do repositório não foram alterados.
