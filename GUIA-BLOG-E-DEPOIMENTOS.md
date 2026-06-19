# Guia — Blog + Depoimentos gerenciáveis (Instituto Camilla Fernandes)

Versão do site: **v3** · Hospedagem: **GitHub Pages** · Painel de conteúdo: **Storyblok** (plano gratuito)

Este documento descreve, em ordem, como deixar o **Blog** e a seção de **Depoimentos**
gerenciáveis pela Dra. Camilla — com **login próprio (e‑mail/senha, sem GitHub)**,
publicando **artigos, imagens e vídeos do YouTube**, mantendo o site no GitHub Pages.

---

## Como vai funcionar (visão geral)

```
  Dra. Camilla  ──(login e-mail/senha)──►  Painel Storyblok (nuvem, grátis)
                                                  │  publica artigo / depoimento
                                                  ▼
   Visitante  ──────────►  Site no GitHub Pages  ──(busca via API)──►  exibe atualizado
```

- A Dra. **publica no painel do Storyblok** (não no GitHub).
- O site **lê o conteúdo via API** e mostra no Blog e nos Depoimentos.
- **Custo:** R$ 0 no plano gratuito *(limites a confirmar no momento da criação da conta)*.
- **Token usado no site é de leitura pública** — pode ficar no código sem risco.

---

## FASE 1 — Criar a conta e o espaço (VOCÊ faz)

1. Acesse **https://www.storyblok.com** e crie uma conta com o **e‑mail e senha** da Dra. Camilla.
2. Crie um **Space** (espaço) chamado, por exemplo, `Instituto Camilla Fernandes`.
3. Vá em **Settings → Access Tokens** e copie o token do tipo **Public** (leitura).
   - Guarde esse token; vou precisar dele para conectar o site.
4. (Opcional, recomendado) Em **Settings → Users**, confirme que só a Dra. tem acesso de administrador.

---

## FASE 2 — Modelar os conteúdos no Storyblok (VOCÊ faz, eu oriento)

No Storyblok, conteúdo é organizado em **Content Types** (modelos). Crie dois:

### 2.1 — Modelo `artigo` (Blog)
Crie um Content Type chamado **`artigo`** com os campos:

| Campo (nome técnico) | Tipo | Observação |
|---|---|---|
| `titulo` | Text | Título do artigo |
| `categoria` | Single-Option | Opções: `Artigo científico`, `Dica de saúde`, `Bem-estar`, `Mitos e fatos` |
| `resumo` | Textarea | Aparece na lista do blog |
| `capa` | Asset (image) | Imagem de capa (opcional) |
| `conteudo` | Richtext | Texto do artigo (negrito, listas, links…) |
| `video_youtube` | Text | **Link** do YouTube (opcional) |
| `data` | Date/Time | Data de publicação |

> Os artigos ficam dentro de uma pasta `artigos/` no Storyblok (ex.: `artigos/envelhecimento-facial`).

### 2.2 — Modelo `depoimento` (Avaliações)
Crie um Content Type chamado **`depoimento`** com os campos:

| Campo (nome técnico) | Tipo | Observação |
|---|---|---|
| `autor` | Text | Nome de quem avaliou |
| `nota` | Number | De 1 a 5 (vira as estrelas ★) |
| `texto` | Textarea | Depoimento escrito |
| `imagem` | Asset (image) | Foto opcional (paciente, resultado, etc.) |
| `video_youtube` | Text | **Link** do YouTube para depoimento em vídeo (opcional) |
| `data` | Date/Time | Opcional |

> Os depoimentos ficam dentro de uma pasta `depoimentos/` no Storyblok.

---

## FASE 3 — Conectar ao site (EU faço, no código)

Depois que você me passar o **Public Token**, eu:

1. Adiciono um pequeno trecho de JavaScript no `prototipo-instituto-camilla-fernandes-v3.html`.
2. Crio um único ponto de configuração (o token), assim:
   ```js
   const STORYBLOK_TOKEN = "COLE_AQUI_O_TOKEN_PUBLICO";
   ```
3. **Blog:** a lista de artigos e a leitura de cada artigo passam a vir do Storyblok
   (com capa, categoria, texto e o vídeo do YouTube embutido quando houver).
4. **Depoimentos:** os cards passam a vir do Storyblok, exibindo texto, estrelas,
   e — quando houver — **foto** ou **vídeo** do YouTube.
5. Mantenho o visual atual (mesmas fontes, cores e layout v3).

---

## FASE 4 — Depoimentos: enviar avaliação + foto/vídeo

Dois recursos na seção de Depoimentos:

### 4.1 — Botão "Deixe sua avaliação"
Um botão convidando o visitante a avaliar. Pode apontar para:
- **A página do Google** (já temos o link no site) — avaliações entram direto no Google, ou
- **Um formulário próprio** (ex.: Google Forms) onde a paciente envia depoimento + foto/vídeo,
  e a Dra. depois **aprova e publica** no painel.

*(Definir qual das duas opções — ver perguntas no fim deste guia.)*

### 4.2 — Avaliações com foto/vídeo
Curadas pela Dra. no painel do Storyblok (modelo `depoimento`):
- **Foto:** sobe a imagem no campo `imagem`.
- **Vídeo:** cola o **link do YouTube** no campo `video_youtube` → o site embute o player.

---

## FASE 5 — Rotina de publicação (depois de pronto)

Para publicar um artigo, a Dra. Camilla:
1. Acessa o painel do Storyblok e faz login (e‑mail/senha).
2. Clica em **+ Create** → escolhe `artigo` → preenche título, categoria, texto, capa e (se quiser) link do YouTube.
3. Clica em **Publish**.
4. Em segundos, o artigo aparece no blog do site. *(Mesmo processo para `depoimento`.)*

---

## Observações importantes (honestas)

- **SEO/carregamento:** como ficamos no GitHub Pages, o conteúdo carrega via JavaScript.
  Funciona bem, mas o texto dos artigos é indexado de forma um pouco mais fraca que num site
  "pré-montado". Isso só melhora migrando para Netlify/Vercel (opção que você preferiu não usar).
- **Vídeos:** sempre via **link do YouTube** (não subimos arquivos de vídeo no repositório).
- **Backup:** o conteúdo fica no Storyblok; vale exportar de tempos em tempos (o painel permite).

---

## O que preciso de você para começar a implementar

1. **Public Token** do Storyblok (após a Fase 1).
2. Decisão da Fase 4.1: botão de avaliação vai para o **Google** ou para um **formulário próprio**
   (com aprovação da Dra. antes de publicar)?
3. Confirmar os nomes dos campos acima (ou pedir ajustes).
