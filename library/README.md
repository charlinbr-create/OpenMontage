# Biblioteca de elementos (Estúdios)

Peças reutilizáveis dos vídeos do Pixel, avaliadas pelo Charles com 0–5 estrelas na aba **Estúdios** do Agentic OS (`http://localhost:666/studio`). Não há banco: **o `element.json` de cada pasta é a fonte da verdade** — arquivos, nota (`stars`), usos (`uses`) e arquivamento (`archived`). O Agentic OS lê e escreve esse mesmo arquivo; o agente também.

## Regra de uso para o agente

1. **Antes de compor:** ler `library/*/*/element.json` e usar só peças com `stars >= 4` e `archived != true`. Copiar os arquivos da pasta para `assets/` do projeto e colar o `snippet.html`, preenchendo os campos listados em `fill`.
2. **Depois de renderizar:** registrar o uso na peça — anexar em `uses` um objeto `{ "request_id": "<uuid novo>", "project": "<pasta em projects/>", "at": "<ISO agora>" }`. Se o mesmo `request_id` já estiver lá, não repetir (retry idempotente). Alternativa pelo app local: `POST http://localhost:666/api/studio` com `{ "op": "markUsed", "category", "slug", "project", "request_id" }`.
3. **Peça nova aprovada pelo Charles:** criar `library/<categoria>/<slug>/` com os arquivos + `element.json` (`slug, name, category, tags, files, fill, notes, source, approved, stars`) + `thumb.jpg` (≤ 300 KB, 480 px de largura). `stars` é a nota **dada pelo Charles**, nunca inventada; sem nota, deixar `0` e avisar.
4. Peça com 1–2★ e sem uso há 30 dias aparece no Agentic OS como "candidato a apagar": marcar `"archived": true` só com o ok do Charles.

Categorias (fixas): `layouts, motion, infographics, hero_text, objects_3d, graphics, icons, backgrounds, broll, sfx, music`.

## Convenções dos snippets

- `snippet.html` guarda markup + CSS efetivo + GSAP com os tempos em origem local 0 s (`data-start="0"`); o agente soma o offset da cena.
- Fontes e mídias referenciadas como `assets/<arquivo>`: copiar para o `assets/` do projeto.
- Canvas 1080×1920 @ 30 fps. Quadro do Pixel em (700, 1225) 240×240; demos terminam antes de y=1225; legendas a partir de y=1490.

Esta pasta é versionada (não está no `.gitignore`); `projects/` continua fora do git.
