# Manual de Banners — Facility

Página estática do guia de produção de banners para desktop e mobile.
Implementada a partir do design no Figma (`Facility`, nó `4265:1556`).

## Estrutura

```
index.html                      página completa (HTML + CSS + JS inline)
vercel.json                     headers de download e de cache
assets/
  ├── Banner-Desktop.psd        template do banner desktop
  ├── Banner-Mobile.psd         template do banner mobile
  └── img/                      imagens da página (WebP + fallback PNG)
```

Sem build step e sem dependências. Abra o `index.html` no navegador.

## Imagens

Todas locais — nenhuma URL externa e nada que expire.

Cada imagem tem três arquivos: `nome.webp` (1x), `nome@2x.webp` (retina) e
`nome.png` (fallback para navegadores sem WebP). O `<picture>` escolhe sozinho.

Para regerar após trocar um original, use `assets/img/` como destino e mantenha
o padrão de nomes — o HTML não precisa mudar.

## Especificações documentadas

| Formato | Dimensões | Área segura |
|---|---|---|
| Desktop | 2560 × 660 px | 1100 × 494 px |
| Mobile | 991 × 690 px | 342 × 524 px |

Desktop reserva 134px no topo para o menu e 62px na base para indicadores.
No mobile são 104px e 62px.

## Fontes

Gotham e Arkipelago são licenciadas e não foram incluídas. A página usa
Montserrat e Caveat como substitutas, isoladas em variáveis CSS:

```css
--font-titulo: "Montserrat", "Gotham", system-ui, sans-serif;
--font-texto:  "Montserrat", "Gotham", system-ui, sans-serif;
--font-script: "Caveat", "Arkipelago", cursive;
```

Trocar essas três variáveis é suficiente para aplicar as fontes reais.

## Notas de implementação

- **Arte do hero** fica invisível até estar totalmente decodificada (`img.decode()`),
  para não aparecer pintando por partes. Um placeholder desfocado ocupa o espaço
  enquanto isso, e há um limite de 1,8s como rede de segurança.
- **O arquivo `hero-familia` já vem espelhado** do Figma. Não aplique `scaleX(-1)`
  no CSS, ou a família aparece invertida.
- **Animações de entrada** via `IntersectionObserver`, com `prefers-reduced-motion`
  respeitado e fallback para navegadores sem suporte.
- **Download dos PSDs** usa o atributo `download`, que só funciona same-origin.
  O `vercel.json` já envia `Content-Disposition: attachment`.

## Deploy

```bash
vercel --prod
```
