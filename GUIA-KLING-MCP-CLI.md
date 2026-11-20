# Kling MCP & CLI — Guia

> Fonte: https://kling.ai/app/mcp/guide (capturado em 2026-07-16) + README da CLI `@klingai/cli-global` **0.2.1** (28/09/2026). Atualizado em 2026-10-09.

> **Novidades (out/2026):** Kling **4.0** em preview (vídeos de até 30 s, 10 quadros-chave, Flash e Omni-Ref) e CLI 0.2.1 com `--model` obrigatório, Elements, Motion Control e Omni-Ref. Ver [Kling 4.0](#kling-40-preview) e [O que mudou na CLI 0.2.1](#o-que-mudou-na-cli-021).

## Antes de começar

### 1. O que é o Kling MCP?

O Kling MCP é um adaptador de protocolo que permite que assistentes de IA de propósito geral (como Cursor, Claude) invoquem diretamente as capacidades de geração do Kling.

Uma vez configurado e autorizado, você pode usar linguagem natural para instruir seu assistente de IA a criar imagens e vídeos com o Kling, recebendo os resultados direto no assistente.

Exemplo: *"Transforme esta imagem em um vídeo cinematográfico de 5 segundos com movimento de câmera."*

### 2. O que é o Kling CLI?

O Kling CLI é uma ferramenta de linha de comando que expõe as capacidades de geração do Kling via terminal. Também pode ser instalado por assistentes de IA.

Com o Kling CLI você pode:
- Gerar vídeos em lote a partir do terminal, scripts ou pipelines de CI
- Integrar o Kling nos seus próprios workflows e toolchains (nós de ComfyUI, Makefiles, pipes de shell)
- Instalar e usar o Kling através de assistentes de IA

### 3. Escolhendo entre MCP e CLI

> **Nota:** para um mesmo assistente de IA, instale **ou** MCP **ou** CLI — nunca os dois. Com ambos instalados, o assistente pode escolher entre eles de forma imprevisível.

| Dimensão | MCP | CLI (via assistente) | CLI (terminal direto) |
|---|---|---|---|
| Suporte a linguagem natural | ✓ | ✓ | – |
| Workflows multi-etapa numa conversa | ✓ | ✓ | – |
| Setup fácil (uma URL) | ✓ | ✓ | – |
| Funciona sem assistente de IA | – | – | ✓ |

## I. MCP

### 1. Instalação rápida

> "Please help me install the Kling MCP service via https://kling.ai/mcp, and complete the authorization process."

### 2. Configuração manual por cliente

**Cursor:** Settings → Tools & MCPs → Add Custom MCP → `"klingai": {"url": "https://kling.ai/mcp"}` → Connect → login com conta Kling.

**Claude:** Claude desktop ou claude.ai → Customize → Connectors → adicionar conector custom `"klingai": {"url": "https://kling.ai/mcp"}` → Add → Connect → login com conta Kling.

### 3. Capacidades do MCP

| # | Tool | Categoria | Descrição |
|---|---|---|---|
| 1 | `who_am_i` | Discovery | Retorna identidade do usuário, modelos disponíveis e especificação de parâmetros de cada tool de geração; chamar primeiro no primeiro uso. |
| 2 | `text_to_video` | Geração | Texto → vídeo. Retorna `generation_id`; fazer poll em `query_tasks`. |
| 3 | `image_to_video` | Geração | Imagem → vídeo. Retorna `generation_id`; fazer poll em `query_tasks`. |
| 4 | `text_to_image` | Geração | Texto → imagem. Retorna `generation_id`. |
| 5 | `image_to_image` | Geração | Imagem de referência + prompt → imagem. Retorna `generation_id`. |
| 6 | `query_tasks` | Task Query | Consulta status e URL final do asset pelo `generation_id`. |
| 7 | `file_upload` | Upload | Solicita ticket de upload one-time; bytes enviados pelo cliente. |
| 8 | `query_membership_and_credits` | Monetização | Status de assinatura e créditos disponíveis. |
| 9 | `omni_ref_video` | Geração | Mistura imagens, vídeos e Elements num vídeo novo (Omni-Ref). |
| 10 | `element_create` / `element_list` / `element_get` / `element_update` / `element_delete` | Elements | Personagens/objetos reutilizáveis (capa + vistas extras) para manter consistência entre gerações. |
| 11 | `motion_library_list` / `motion_control` | Motion Control | Aplica um movimento salvo (ou um vídeo de movimento) a uma imagem de sujeito. |
| 12 | `feedback` | Suporte | Reporta tarefa travada, cobrança estranha ou resultado inesperado. |

## II. CLI

### 1. Instalação rápida

> "Please run the following command to install the Kling AI Skill: `npx skills add klingai-tech/skills`, then complete the authorization process."

Dica: em plataformas como OpenClaw e Hermes, a CLI é a opção preferida.

### 2. Instalação manual

```bash
npx skills add klingai-tech/skills
# ou
npm i -g @klingai/cli-global --registry=https://registry.npmjs.org
```

Atualizar (e conferir a versão):

```bash
npm i -g @klingai/cli-global@latest
kling --version          # 0.2.1 em 2026-10-09
```

Conta do site da China: pacote `@klingai/cli-cn` (mesmo comando `kling`).

Após instalar:

```bash
kling login
```

### 3. Comandos da CLI

| # | Comando | Categoria | Descrição |
|---|---|---|---|
| 1 | `kling --help` | Discovery | Lista comandos e instruções de uso. |
| 2 | `kling who_am_i` | Discovery | Identidade, modelos disponíveis e parâmetros de cada tool; chamar primeiro no primeiro uso. |
| 3 | `kling text_to_image <prompt>` | Imagem | Texto → imagem. Retorna `generation_id`; poll via `query_tasks` ou `--poll` para aguardar. |
| 4 | `kling image_to_image --image <url\|path> <prompt>` | Imagem | Imagem de referência + prompt (arquivos locais sobem automaticamente). |
| 5 | `kling text_to_video --model <m> <prompt>` | Vídeo | Texto → vídeo. Retorna `generation_id`. |
| 6 | `kling file_upload <path>` | Upload | Sobe um arquivo. |
| 7 | `kling image_to_video --model <m> --image <url\|path> [prompt]` | Vídeo | Anima uma imagem. `--tailImage` = último quadro; `--input <slot>=<arquivo>` preenche qualquer quadro declarado (ex.: `keyframe_1`). |
| 7b | `kling omni_ref_video --model <m> [--image …] [--video …] <prompt>` | Vídeo | Omni-Ref: mistura imagens, vídeos e Elements. |
| 7c | `kling element_create --name … --cover … [--secondary …]` | Elements | Cria personagem/objeto reutilizável; `element_list/get/update/delete` para gerenciar. |
| 7d | `kling motion_library_list` / `kling motion_control --model <m> --image … --motionId <id>` | Motion | Aplica um movimento salvo a uma imagem de sujeito. |
| 7e | `kling feedback` | Suporte | Reporta tarefa travada ou cobrança estranha. |
| 8 | `kling query_tasks <generationId>` | Task Query | Status e URLs finais (`works[].url`) pelo `generation_id`. |
| 9 | `kling account` | Monetização | Plano e créditos disponíveis (ex.: `membershipType`, `availableRemainCredits`). |
| 9b | `kling tool_list` | Discovery | Lista as tools que o servidor MCP oferece agora. |
| 10 | `kling login` | Login | OAuth via browser. Credenciais salvas em `~/.kling/`. |
| 11 | `kling logout` | Logout | Revoga a sessão e limpa auth local (preserva o client ID registrado). |

### O que mudou na CLI 0.2.1

- **`--model` é obrigatório** em todo comando de geração; use o nome do `who_am_i` (ex.: `kling-video-v3_0_turbo`). Os atalhos `--omni` / `--flash` foram removidos. Script antigo sem `--model` agora falha.
- Parâmetros que você não passa são preenchidos pelo servidor; parâmetro desconhecido ou valor inválido é **recusado antes de cobrar**.
- **Quadros por nome:** `--image` preenche os quadros comuns, `--tailImage` o último; `--input <slot>=<arquivo>` endereça qualquer slot declarado (não combina com `--image`/`--tailImage`).
- Resultado sem marca d'água, quando houver: `works[].url_without_watermark`.
- Comandos novos: `omni_ref_video`, `element_*`, `motion_library_list`, `motion_control`, `feedback`, `tool_list`.
- `kling <comando> --help` busca a declaração ao vivo do servidor (parâmetros e limites atuais).
- Saída JSON no stdout (`--quiet` = uma linha); códigos de saída 0/1/130. Em agente/pipe a CLI nunca para pedindo dado: falta de valor = erro.

## Kling 4.0 (preview)

> Estado em **2026-10-09**: aparece na documentação da CLI 0.2.1, mas **ainda não foi liberado em todas as contas** — numa conta Pro testada, o `who_am_i` não listava nenhum modelo v4. A liberação é por conta: rode `kling who_am_i` (não gasta crédito) e procure `kling-video-v4_0`.

| Modelo | Comandos | Duração | Resolução | Quadros |
|---|---|---|---|---|
| `kling-video-v4_0` (Standard, preview) | `text_to_video`, `image_to_video`, `omni_ref_video` | **3–30 s** | 720p/1080p (conforme a conta) | `first_image`, `keyframe_1..10`, `tail_image` — todos opcionais, **até 10 imagens** no total |
| `kling-video-v4_0_flash` | `image_to_video`, `omni_ref_video` | 3–20 s | só 720p | só o primeiro quadro (`--tailImage` é recusado) |

```bash
# primeiro + meio + último quadro (v4 Standard)
kling image_to_video --model kling-video-v4_0 \
  --input first_image=./first.jpg --input keyframe_1=./meio.jpg \
  --input tail_image=./last.jpg --poll 300

# Flash: só o primeiro quadro
kling image_to_video --model kling-video-v4_0_flash --image ./first.jpg --poll 300

# Omni-Ref v4 mantendo o som do vídeo de referência
kling omni_ref_video --model kling-video-v4_0 --image ./ref.jpg --video ./clip.mp4 \
  --keepOriginalSound true --enableAudio false "segue o movimento de câmera da referência" --poll 300
```

- Com primeiro quadro, omitir `--aspectRatio` (essa entrada não aceita `auto`).
- **Omni-Ref v4:** até 10 imagens, 5 vídeos e 7 Elements (até 3 de vídeo), 15 referências no total; não aceita áudio avulso (`--audio`); `enable_audio` e `keepOriginalSound` não podem ser `true` juntos.
- **Créditos:** a Kling **ainda não publicou** o custo por segundo da 4.0 nem da 4.0 Flash — números que circulam em blogs não estão confirmados. Referência da 3.0: ~6 créditos/s (720p sem som) a ~30 créditos/s (4K). O Omni-Ref v4 é **cobrado por segundo, contando também a duração do vídeo de entrada**. A API para desenvolvedores (kling.ai/dev) diz que ainda não oferece a 4.0 — pela assinatura (CLI/MCP) ela chega conforme a liberação da conta.
- Medir o custo real quando chegar: um teste curto em 720p e `kling account` antes/depois.

## FAQ

1. **Créditos:** MCP e CLI usam a mesma precificação/consumo de créditos das plataformas oficiais Kling. **Geração gratuita off-peak NÃO está disponível** via assistentes de IA.
2. **Workspaces:** só créditos **pagos** do workspace **Pessoal**; workspace Team ainda não é suportado (virá em release futura).
3. **Créditos bônus:** válidos só nas plataformas oficiais; não funcionam em MCP/CLI.
4. **Rate limits:** 5 QPS. Não-assinantes: 1 tarefa de vídeo concorrente (mesmo limite do cliente Kling). Assinantes: sem limite de tarefas.
4b. **Omni-Ref v4:** cobrado por segundo, incluindo a duração do vídeo de entrada.
5. **Validade das URLs:** arquivos gerados ficam válidos por **24 horas** — baixar e salvar imediatamente.
6. **Cancelamento:** tarefas não podem ser canceladas após envio.
7. **Suporte:** Customer Support Center, no canto inferior direito do site do Kling.

---

## Modelos disponíveis (exemplo — capturado via `kling who_am_i` em 2026-10-09, conta Pro; a lista varia por conta/plano)

### Vídeo (`text_to_video` / `image_to_video`)

| Modelo | Alias | Destaque | Duração | Resolução | Extras |
|---|---|---|---|---|---|
| `kling-video-v3_0_turbo` | `kling3.0-turbo`, `v3-turbo` | Melhor custo-benefício; **1ª escolha p/ animar 1 imagem** | 3–15s | 720p/1080p | — |
| `kling-video-v3_0_omni` | `kling3.0-omni`, `v3-omni` | Multimodal all-in-one, **personagens com voz**, áudio nativo; default p/ multi-imagem ref | 3–15s | até **4K** | `prefer_multi_shots`, `enable_audio`, `aspect_ratio` |
| `kling-video-v3_0` | `kling3.0` | Multi-shot storytelling, áudio nativo, consistência | 3–15s | até 4K | `prefer_multi_shots`, `enable_audio` |
| `kling-video-v2_6` | `v2.6` | Vídeo + som, rápido | 5/10s | 720p/1080p | `enable_audio` |
| `kling-video-v2_5` | `v2.5Turbo` | Barato e rápido | 5/10s | 720p/1080p | `enable_audio`, `enable_asmr`, `audio_prompt`, `music_prompt` |
| `kling-video-o1` | `omni1`, `video-o1` | Consistência excepcional | 3–10s | 720p/1080p | — |

Defaults do servidor: `text_to_video` → `kling-video-v3_0_omni`; `image_to_video` → `kling-video-v3_0`. `omni_ref_video` hoje: `kling-video-v3_0_omni` e `kling-video-o1` (v4 quando liberada). `motion_control`: `kling-video-v3_0` e `kling-video-v2_6`.

Todos aceitam `imageCount` 1–4 (variações por request) e `aspect_ratio` 16:9 / 9:16 / 1:1 (t2v; no i2v o AR vem da imagem, exceto omni/o1).

### Imagem (`text_to_image` / `image_to_image`)

| Modelo | Alias | Destaque | Resolução |
|---|---|---|---|
| `kling-image-v3_0_omni` | `kling3.0-omni`, `image-o3` | Default Kling; narrativa, série de imagens (`story_mode` no i2i) | 1k/2k/**4k** |
| `kling-image-o1` | `image-o1` | Consistência de features, edição precisa, style transfer | 1k/2k |
| `kling-image-v3_0` | `image3.0` | Multi-referência livre, consistência | 1k/2k |
| `kling-image-v2_1` | `image2.1` | Aderência a prompt | 1k/2k |
| `gemini-3-pro-image` | **Nano Banana Pro** | Alta fidelidade, texto em imagem, cenas complexas | 1k/2k/**4k** |
| `gemini-3.1-flash-image` | **Nano Banana 2** | Rápido, bom texto em imagem (substituiu o Nano Banana 2.5 Flash) | conforme `who_am_i` |
| `gpt-image-2` (t2i) / `gpt-image2` (i2i) | GPT-Image-2 | Instrução forte, texto confiável (substituiu o GPT Image 1) | conforme `who_am_i` |

Imagens: `image_count` até 9 por request; ARs de 9:16 a 21:9.

### Dicas de automação

- `imageCount: 4` → 4 variações da mesma cena numa geração só.
- `aspect_ratio: 9:16` nativo → direto pra Shorts/Reels.
- `prefer_multi_shots: true` (v3.0) → vídeo multi-planos numa única geração.
- `enable_audio: true` → som nativo; v2.5 aceita `music_prompt`/`audio_prompt` separados.
- Conta assinante → sem limite de tarefas concorrentes; rate 5 QPS.
- **Custo**: `720p` consome bem menos créditos que 1080p/4k — usar 720p como padrão e subir só quando necessário.
- **Fala em idioma não-inglês**: escrever o prompt **inteiro** no idioma alvo, com a fala entre aspas (ex.: tudo em PT-BR) — prompt-quadro em inglês degrada a articulação labial e a pronúncia.
- **Robustez**: salvar a resposta bruta de cada submit em arquivo antes de parsear — jobs pagos não são canceláveis e um `generationId` perdido = créditos perdidos.

---

## Exemplo de pipeline de automação

Fluxo "imagens → cenas → vídeo":

1. **Gerar cenas** (imagem): `kling text_to_image --model kling-image-v3_0 "<prompt>" --poll` (ou qualquer gerador de imagem externo).
2. **Animar cada cena**: `kling image_to_video --model kling-video-v3_0_turbo --image cena1.png "<prompt de câmera>" --poll`.
3. **Baixar na hora** (URLs expiram em 24h): `kling query_tasks <id>` → `works[].url` → `curl -o`.
4. **Montar**: ffmpeg (concat de clipes com specs iguais dispensa re-encode: `-f concat -c copy`).

### Voz em português — o que fazer (resumo das lições)

O áudio nativo do Kling (`enable_audio`) em PT-BR sai com sotaque forte e palavras trocadas (transcrição real de um teste: *"não adianta só torcer vídeos de demontagem... é assim que se aprende Meijun"*). O tratamento em 3 camadas:

1. **Prompt 100% em português** (instrução + fala entre aspas): não conserta a voz, mas faz a **boca articular as frases certas nos tempos certos** — pré-requisito de qualquer dublagem.
2. **Voice conversion** (troca só o timbre): lip-sync perfeito, mas herda a pronúncia ruim. Use apenas se a fala original já saiu aceitável.
3. **Dublagem por compasso** (o método vencedor): regrava a fala com voz clonada em português correto e assenta cada pedaço na batida da boca. Ferramenta pronta: projeto **[dublagem-compasso](https://github.com/inematds/dublagem-compasso)** (e skill de mesmo nome no Claude Code).

```bash
# dublar um clipe do Kling com voz clonada, alinhado ao compasso da boca
conda run -n chatterbox python3 ~/projetos/dublagem-compasso/dublar.py \
  --video clip.mp4 --texto "<texto correto da fala>" \
  --ref voz-clonada.wav --out clip-dublado.mp4 --cauda 2
```

O `--cauda 2` congela o último frame por 2s — sem isso o vídeo termina seco na última sílaba.

### Narração / voz própria (dublagem)

O MCP/CLI **não aceita áudio de entrada**, e o áudio nativo (`enable_audio`) em idiomas não-inglês sai com pronúncia fraca. Receita de dublagem que preserva o lip-sync:

1. Gerar o clipe com prompt 100% no idioma alvo + fala entre aspas + `enable_audio true` — a boca articula as frases certas e o áudio nativo vira o **guia de tempo**.
2. Transcrever o áudio nativo com timestamps de palavra (ex.: faster-whisper) → mapa fala/silêncio.
3. Gerar cada frase num TTS de qualidade (voz clonada), em múltiplas tomadas, escolhendo a que preenche a janela da boca (ajuste fino de velocidade ≤ ±20% — **nunca esticar além disso**: se não coube, gere outra tomada ou corte por pedaços).
4. **Alinhar por compasso**: se a boca faz pausas dentro da frase, cortar a locução nova nas fronteiras de palavra (transcrevendo-a também) e assentar **cada pedaço** na batida exata da boca — alinhar só início/fim da frase deixa o meio fora de ritmo.
5. Montar a trilha com os pedaços nos offsets exatos (`adelay` + `amix`) e trocar o áudio: `ffmpeg -map 0:v -map 1:a -c:v copy`.
6. Verificar SEMPRE antes de entregar: energia de áudio presente do início ao fim de cada janela da boca (boca aberta sem som é o defeito mais visível) e pausas internas alinhadas.

Alternativa mais simples (timbre novo, prosódia original): **voice conversion** (ex.: Chatterbox VC) sobre o áudio nativo — lip-sync perfeito, mas herda a pronúncia do original.
