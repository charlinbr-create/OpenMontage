# Biblioteca de elementos (Estúdios)

Peças reutilizáveis dos vídeos do Pixel, avaliadas pelo Charles com 0–5 estrelas na aba **Estúdios** do Agentic OS (`http://localhost:666/studio`). Não há banco: **o `element.json` de cada pasta é a fonte da verdade** — arquivos, nota (`stars`), usos (`uses`) e arquivamento (`archived`). O Agentic OS lê e escreve esse mesmo arquivo; o agente também.

## Regra de uso para o agente

1. **Antes de compor:** ler `library/*/*/element.json` e usar só peças com `stars >= 4` e `archived != true`. Copiar os arquivos da pasta para `assets/` do projeto e colar o `snippet.html`, preenchendo os campos listados em `fill`.
2. **Depois de renderizar:** registrar o uso na peça — anexar em `uses` um objeto `{ "request_id": "<uuid novo>", "project": "<pasta em projects/>", "at": "<ISO agora>" }`. Se o mesmo `request_id` já estiver lá, não repetir (retry idempotente). Alternativa pelo app local: `POST http://localhost:666/api/studio` com `{ "op": "markUsed", "category", "slug", "project", "request_id" }`.
3. **Peça nova aprovada pelo Charles:** criar `library/<categoria>/<slug>/` com os arquivos + `element.json` (`slug, name, category, tags, files, fill, notes, source, approved, stars`). Uma `thumb.jpg` (≤ 300 KB, 480 px de largura) é recomendada; o Estúdios também aceita `thumb.png`, `thumb.webp` ou usa a primeira imagem PNG/JPG/WebP/GIF/AVIF listada em `files`. `stars` é a nota **dada pelo Charles**, nunca inventada; sem nota, deixar `0` e avisar.
4. Peça com 1–2★ e sem uso há 30 dias aparece no Agentic OS como "candidato a apagar": marcar `"archived": true` só com o ok do Charles.

Categorias (fixas): `layouts, motion, infographics, hero_text, objects_3d, images, graphics, icons, backgrounds, broll, sfx, music`.

`objects_3d` recebe modelos e cenas (Blend, GLB, GLTF), acompanhados de uma capa. `images` recebe fotos, ilustrações e texturas. Os dois logos da Cunhas Runner estão em `objects_3d/cunhas-runner-horizontal` e `objects_3d/cunhas-runner-redondo`. Os modelos foram aprovados, mas ainda aguardam nota numérica; o teste de animação do aro não está aprovado. Antes de usar, ler `notes` além de `stars`.

Busca pelo app: `GET http://127.0.0.1:666/api/studio?category=objects_3d&q=cunhas%20runner`. A API retorna as mesmas peças do disco, com categorias e contagens. Use `approved=true` para filtrar as peças de 4–5 estrelas.

## Convenções dos snippets

- `snippet.html` guarda markup + CSS efetivo + GSAP com os tempos em origem local 0 s (`data-start="0"`); o agente soma o offset da cena.
- Fontes e mídias referenciadas como `assets/<arquivo>`: copiar para o `assets/` do projeto.
- Canvas 1080×1920 @ 30 fps. Quadro do Pixel em (700, 1225) 240×240; demos terminam antes de y=1225; legendas a partir de y=1490.

Esta pasta é versionada (não está no `.gitignore`); `projects/` continua fora do git.
