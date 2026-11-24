# Luma Perfumaria

Site de uma perfumaria de nicho: vitrine animada, coleção com filtros, página de cada perfume, sacola e checkout.
Em português (pt-BR), pensado para desktop, TV e celular.

## Arquitetura

| Camada | Situação |
| --- | --- |
| **Front-end** | Uma única página estática: [index.html](index.html) com HTML, CSS e JavaScript juntos. Sem build, sem `npm`. |
| **Back-end** | **Não existe.** Nada de API, banco de dados, `fetch` ou armazenamento no navegador. |
| **Dados** | Os perfumes, preços e textos ficam em listas JavaScript dentro do `index.html` (`P` = clássicos, `CASA` = linha Luma). |
| **Hospedagem** | Pensado para a Vercel, como site estático. Configuração em [vercel.json](vercel.json) (cache de 1 dia nas fotos). |

O carrinho e o checkout funcionam só na tela: o pedido **não é enviado a lugar nenhum**. Para vender de verdade, é preciso ligar um back-end ou um serviço de pagamento. Isso ainda não foi feito.

## Estrutura

```
index.html              página inteira (estilos, marcação e scripts)
vercel.json             cabeçalhos de cache
favicon.svg / .ico / favicon-32.png / apple-touch-icon.png
fotos/webp/             todas as imagens do site
  <id>.webp             foto principal do perfume (usada na animação de produção)
  <id>-frasco(.webp|-400.webp)   frasco recortado dos clássicos (820 px e 400 px)
  <id>-640.webp         foto da linha Luma (versão leve)
  close-1..5.webp       close de cada nicho da abertura
  parede-render.webp    parede de pedra da abertura
  m_<matéria>.webp      textura de cada matéria-prima
  recorte/              recortes com fundo transparente (frascos Luma e matérias)
  pedra.webp, nicho-pedra.webp, sillage*.webp   fundos
```

### Bibliotecas (via CDN, sem instalar)

- [GSAP 3.12.5](https://gsap.com) com ScrollTrigger e Flip: animações ligadas à rolagem
- [Lenis 1.1.13](https://lenis.darkroom.engineering): rolagem suave
- Google Fonts: Six Caps, EB Garamond e Geist

## Escala por tela (TV, monitor, notebook)

O site é desenhado para **1366 px de largura (a tela onde o site foi montado)**. Em telas de **1100 px ou mais**, tudo é ampliado ou reduzido de forma proporcional, então a TV, o notebook e um monitor 4K mostram a mesma composição.

- A variável `--u` (`calc(100vw / 1366)`) vale 1 unidade de design. No CSS, todo tamanho em px é escrito como `calc(N * var(--u))`.
- Ao criar estilos novos, **use `calc(N * var(--u))` no lugar de `px`** (menos para linhas finas de 1 px). Se usar `px` puro, o elemento não acompanha a escala.
- Abaixo de 1100 px (celular e tablet) a escala é desligada (`--u: 1px`) e o layout é fluido, com os pontos de quebra de 1000, 860, 700 e 640 px.
- Só a altura pode variar: telas com proporção diferente de 16:9 mostram mais ou menos conteúdo na vertical, porque as seções usam `vh`.

## Como rodar

Não precisa instalar nada. Use qualquer servidor estático na pasta do projeto:

```bash
python -m http.server 8000
# abra http://localhost:8000
```

Abrir o `index.html` direto do disco (`file://`) também funciona, mas o frasco animado em WebGL pode não aparecer, porque o navegador bloqueia a imagem.

## Como editar

- **Trocar perfumes, preços e textos:** edite as listas `P` e `CASA` no JavaScript do `index.html`.
- **Desconto no Pix e cupom:** constantes `PIX` e `COUPON`, no mesmo bloco.
- **Adicionar um perfume:** inclua o item na lista e coloque as fotos em `fotos/webp/` com os nomes da tabela acima. O nome do arquivo vem do `id` do perfume.
- **Cores e fontes:** variáveis no início do CSS (`:root`).

## Deploy

O projeto tem `vercel.json`, então é publicado como site estático na Vercel. Não há etapa de build: basta publicar a pasta.

## Antes de publicar

Rode estas conferências no navegador, em tela de computador e de celular:

1. Nenhum erro no console e nenhuma imagem com 404.
2. Sem rolagem horizontal.
3. Menu, sacola, filtros e a página de um perfume abrem e fecham.

Imagens que o código não referencia devem ser apagadas, para o repositório não crescer à toa.
