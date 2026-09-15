# Biblioteca de elementos (Estúdios)

Peças reutilizáveis dos vídeos do Pixel, avaliadas pelo Charles com 0–5 estrelas na aba **Estúdios** do Daily Hub. Os arquivos moram aqui (`library/<categoria>/<slug>/`); estrelas e uso moram no Supabase do Hub, consultados via MCP `daily-hub` (tools `studio.*`).

## Regra de uso para o agente

1. **Antes de compor:** `studio.list` (padrão `min_stars=4`). Só peça aprovada entra em vídeo novo. Copiar os arquivos da pasta indicada em `path` para `assets/` do projeto e colar o `snippet.html`, preenchendo os campos listados em `element.json → fill`.
2. **Depois de renderizar:** `studio.markUsed` com `slug`, `project` (nome da pasta em `projects/`) e um `request_id` UUID novo por peça. Retry com o mesmo `request_id` não conta duas vezes.
3. **Peça nova aprovada pelo Charles:** criar `library/<categoria>/<slug>/` com os arquivos + `element.json` (`slug, name, category, tags, files, fill, notes, source, approved`) + `thumb.jpg` (≤ 300 KB, 480 px de largura) e chamar `studio.upsert` com a nota **dada pelo Charles** (nunca sem nota; `stars` omitido em update preserva a nota).
4. Peça com 1–2★ e sem uso há 30 dias aparece no Hub como "candidato a apagar": `studio.archive` só com o ok do Charles.

Categorias (fixas): `layouts, motion, infographics, hero_text, objects_3d, graphics, icons, backgrounds, broll, sfx, music`.

## Convenções dos snippets

- `snippet.html` guarda markup + CSS efetivo + GSAP com os tempos em origem local 0 s (`data-start="0"`); o agente soma o offset da cena.
- Fontes e mídias referenciadas como `assets/<arquivo>`: copiar para o `assets/` do projeto.
- Canvas 1080×1920 @ 30 fps. Quadro do Pixel em (700, 1225) 240×240; demos terminam antes de y=1225; legendas a partir de y=1490.

Esta pasta é versionada (não está no `.gitignore`); `projects/` continua fora do git.
