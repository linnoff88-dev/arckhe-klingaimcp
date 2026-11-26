ï»¿# klingaimcp

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


IntegraêÂ§êÂ£o do **Kling AI** (MCP + CLI) no pipeline de geraêÂ§êÂ£o automêÂ¡tica de imagens e vêÂ­deos.

- **[GUIA-KLING-MCP-CLI.md](GUIA-KLING-MCP-CLI.md)** ê¢â¬â guia completo: instalaêÂ§êÂ£o (MCP e CLI), comandos, modelos disponêÂ­veis, limites/crêÂ©ditos e o fluxo de automaêÂ§êÂ£o "imagens ê¢â â cenas animadas ê¢â â vêÂ­deo final".

## Resumo rêÂ¡pido

```bash
npm i -g @klingai/cli-global@latest   # 0.2.1: --model obrigatêÂ³rio
kling login                       # OAuth via browser
kling who_am_i                    # modelos e parêÂ¢metros
kling text_to_image --model kling-image-v3_0 "..." --poll
kling image_to_video --image cena.png --model kling-video-v3_0_turbo "..." --poll
```

**Kling 4.0** (atêÂ© 30 s, 10 quadros-chave) estêÂ¡ em preview e chega conta por conta ê¢â¬â ver o guia.

URLs de resultado expiram em 24h ê¢â¬â baixar na hora. Fala em idioma nêÂ£o-inglêÂªs: prompt inteiro no idioma alvo; para voz prêÂ³pria, dublagem por cima com ffmpeg (ver guia ê¢â¬â o MCP/CLI nêÂ£o aceita êÂ¡udio de entrada).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>