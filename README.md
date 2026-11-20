# klingaimcp

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


IntegraÃ§Ã£o do **Kling AI** (MCP + CLI) no pipeline de geraÃ§Ã£o automÃ¡tica de imagens e vÃ­deos.

- **[GUIA-KLING-MCP-CLI.md](GUIA-KLING-MCP-CLI.md)** â€” guia completo: instalaÃ§Ã£o (MCP e CLI), comandos, modelos disponÃ­veis, limites/crÃ©ditos e o fluxo de automaÃ§Ã£o "imagens â†’ cenas animadas â†’ vÃ­deo final".

## Resumo rÃ¡pido

```bash
npm i -g @klingai/cli-global@latest   # 0.2.1: --model obrigatÃ³rio
kling login                       # OAuth via browser
kling who_am_i                    # modelos e parÃ¢metros
kling text_to_image --model kling-image-v3_0 "..." --poll
kling image_to_video --image cena.png --model kling-video-v3_0_turbo "..." --poll
```

**Kling 4.0** (atÃ© 30 s, 10 quadros-chave) estÃ¡ em preview e chega conta por conta â€” ver o guia.

URLs de resultado expiram em 24h â€” baixar na hora. Fala em idioma nÃ£o-inglÃªs: prompt inteiro no idioma alvo; para voz prÃ³pria, dublagem por cima com ffmpeg (ver guia â€” o MCP/CLI nÃ£o aceita Ã¡udio de entrada).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>