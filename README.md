# Vila Fonseca Planejados — Site

Home + 5 landing pages em **HTML plano**.

Cada página depende da sua pasta `assets/`. Os arquivos **não** são
autocontidos: subir um `index.html` sem a pasta `assets/` correspondente
quebra a página.

## Estrutura

```
/index.html                          <- home
/assets/                             <- imagens da home (58 arquivos)
/lp-cozinhas-planejadas/index.html
/lp-cozinhas-planejadas/assets/
/lp-quartos-e-closets/index.html
/lp-quartos-e-closets/assets/
/lp-londrina/index.html
/lp-londrina/assets/
/lp-cambe/index.html
/lp-cambe/assets/
/lp-moveis-planejados/index.html
/lp-moveis-planejados/assets/
```

Cada pasta de LP tem sua própria cópia de `assets/`. É intencional — as
páginas são independentes entre si.

## Deploy

⚠️ **Merge na `main` publica em produção automaticamente.**

Este repositório alimenta dois destinos a partir da branch `main`:

| Destino | Endereço | Mecanismo |
|---|---|---|
| Produção | https://vilafonsecaplanejados.com | Hostinger, integração Git, deploy automático em `public_html` (~6s) |
| Espelho | https://estevangui.github.io | GitHub Pages, branch `main`, `/(root)` |

Não há etapa de revisão entre o merge e o ar.

### Fluxo de trabalho

1. Trabalhar sempre em **branch separada** — enquanto não for a `main`,
   nada é publicado
2. Abrir Pull Request e revisar
3. Merge na `main` = publicação imediata nos dois destinos

Para mudanças de risco, desligar a implantação automática em
hPanel → Avançado → Git antes do merge, e reativar depois.

## Incluído em cada página

- Google Ads `AW-18124116382` + evento de conversão nos cliques de WhatsApp
  (sem `value` fixo)
- WhatsApp (43) 99663-6876 nos CTAs, com mensagem pré-preenchida por contexto
- SEO: title, description, canonical, Open Graph, viewport, um único H1
- JSON-LD LocalBusiness
- Imagens WebP com fallback JPEG, `srcset` responsivo, `width`/`height`,
  `loading="lazy"` (exceto hero, com `fetchpriority="high"`)
- Favicon
- Fontes com `preconnect` e `display=swap`
- Layout responsivo com barra fixa de WhatsApp no mobile

## Histórico

As páginas foram originalmente geradas em formato standalone empacotado
(`super_inline_html`): o HTML real ficava serializado dentro de
`<script type="__bundler/template">`, as imagens em base64 no
`__bundler/manifest`, e um script montava o DOM no `DOMContentLoaded`.
Até isso terminar, a página exibia apenas "Unpacking..." e o `<title>`
era o placeholder "Bundled Page".

Peso: ~1,8 MB por LP e ~5,8 MB na home. LCP ruim, sem cache de imagem
separado, e `<head>` invisível para leitores que não executam JavaScript.

Esta versão substitui aquele formato por HTML servido direto.

## Depois de publicar

- Conferir que a imagem hero aparece em cada página
- Testar a conversão em Google Ads → Ferramentas → Diagnóstico da tag
- Rodar PageSpeed Insights nas 6 URLs
- Reenviar as URLs ao Search Console e pedir reindexação (o título antigo
  "Bundled Page" pode estar indexado)

## Pendências

**Conteúdo** (bloqueiam escalar investimento em Ads)
- Prova social: depoimentos, tempo de atuação, avaliações
- Razão social, CNPJ e endereço no rodapé
- `streetAddress` e `postalCode` no JSON-LD
- `sameAs` com links reais de Instagram e Perfil da Empresa no Google
  (hoje são placeholders)

**Técnicas**
- Carrossel da home: intervalo de 1800 ms é curto e faz baixar as 25
  imagens (~976 KB) em cerca de 9 segundos, anulando o `loading="lazy"`.
  Subir para 4000 ms e iniciar a rotação só quando o cartão entrar na
  viewport
- `data-cta` nos botões da home — as LPs já identificam cada CTA, a home
  usa um rótulo único
- `alt` genéricos na home ("Cozinhas", "Banheiro"), um vazio
- GitHub Pages ativo cria uma cópia pública indexável em
  `estevangui.github.io`. O `canonical` já protege, mas considerar
  desativar o Pages depois que o deploy pela Hostinger estiver validado
