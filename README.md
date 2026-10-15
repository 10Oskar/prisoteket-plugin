# Prisoteket för Claude

Prisoteket jämför elavtal för hushåll per elområde och söker bland begagnat till salu i Sverige, direkt i
din AI-assistent. Det här repot är en marknadsplats för Claude med pluginet Prisoteket. Pluginet har
connectorn till Prisotekets server och en skill som får Claude att använda verktygen självmant vid frågor
om elavtal och begagnat.

## Installera i Claude

1. Öppna **Customize**, sedan **Plugins**, **Add** och **Add marketplace**.
2. Skriv in `10Oskar/prisoteket-plugin` och klicka **Add**.
3. Välj **Prisoteket** och klicka **Add**.
4. Öppna pluginet, gå till fliken **Connectors** och lägg till Prisoteket.

Ingen inloggning och ingen nyckel behövs. Att lägga till pluginet lägger inte till connectorn, därför steg 4.

Mer om att lägga till Prisoteket i Claude och ChatGPT: https://prisoteket.se/lagg-till/

## Innehåll

- `.claude-plugin/marketplace.json`: marknadsplatsen `prisoteket`.
- `plugins/prisoteket/`: pluginet, med connectorn i `.mcp.json` och skillen under `skills/`.

Repot byggs ur Prisotekets egna källor. Ändringar som görs direkt här skrivs över.

Integritet: https://prisoteket.se/integritetspolicy/
