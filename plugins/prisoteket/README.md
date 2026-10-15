# Prisoteket som plugin för Claude

Pluginet får Claude att använda Prisotekets publika verktyg självmant: jämförelsen av elavtal för hushåll
och sökningen bland begagnat till salu i Sverige. Det består av två delar.

- `.mcp.json` pekar på Prisotekets publika MCP-server. Servern lämnar bara ut verktygen i
  `MCP_PUBLIKA_VERKTYG`, och kräver ingen inloggning.
- `skills/elavtal-och-begagnat/SKILL.md` säger när verktygen ska användas och att källraden alltid återges.

Pluginet delas som marknadsplats för Claude i ett eget publikt repo (`plugin_repo` i `config/site.toml`),
som byggs ur den här mappen med `scripts/bygg_pluginmarknad.py`. Ändra här, inte i det repot.

## Varifrån uppgifterna kommer

Adressen i `.mcp.json` är `STANDARDADRESS` i `scripts/kontrollera_publik_adress.py`, följd av `/mcp`. Namnet
och domänen i `.claude-plugin/plugin.json` är `namn` och `doman` i `config/site.toml`. Filerna här är
statiska, eftersom Claude läser dem som de är. `tests/test_plugin.py` fäller om de skiljer sig från källorna.
Ändra alltså där först, och sedan här.

## Data

En fråga om elavtal skickar elområde och, om användaren har angett det, förbrukning och elhandlare till
`mcp.prisoteket.se`. En sökning efter begagnat skickar sökord och valda filter. Pluginet sparar inget självt.

## Kontrollera och packa

```powershell
claude plugin validate ./plugin
.venv\Scripts\python -m pytest -q tests/test_plugin.py
.venv\Scripts\python -m zipfile -c underlag/prisoteket-plugin.zip plugin/.claude-plugin plugin/.mcp.json plugin/README.md plugin/skills
```

Zip-filen hamnar i `underlag/`, som aldrig checkas in. Manifestet och mapparna ligger i zip-filens rot,
inte under en mapp `plugin/`.

## Installera för ett eget test

1. Kontrollera att den publika adressen svarar: `.venv\Scripts\python scripts/kontrollera_publik_adress.py`.
2. Packa pluginet med kommandot ovan.
3. Öppna claude.ai eller Claude Desktop och gå till **Customize** → **Plugins** → **Add** →
   **Upload plugin**. Välj `underlag/prisoteket-plugin.zip`.
4. Öppna pluginet och fliken **Connectors**. Lägg till eller anslut `prisoteket` där. Finns adressen redan
   som egen connector räcker den: samma adress ger en uppsättning verktyg.
5. Starta ett nytt samtal och ställ en fråga utan att nämna Prisoteket.

I Claude Code går det utan uppladdning: `claude --plugin-dir ./plugin`.

## Byta till en ny version

Höj `version` i `.claude-plugin/plugin.json`, packa om, ta bort det gamla pluginet under **Customize** →
**Plugins** och ladda upp den nya zip-filen på samma sätt. Kontrollera versionen på pluginets sida, och
starta ett nytt samtal: ett pågående samtal har kvar den gamla skillen.
