# BAM Fantasy: setup and editing guide

BAM Fantasy is a free website for the Gridiron, Augustus and Big Ten leagues. Anyone can view it, with no account needed.

- **The website** is hosted for free on **GitHub Pages**.
- **All the stats** come from a **Google Sheet** that you own. Edit the sheet and the site updates by itself. You never touch code.

You do the setup once (about 20 minutes). After that, the only thing you ever edit is the sheet.

---

## What's in this folder

| File | What it is |
|---|---|
| `index.html` | The website itself. You don't need to edit it. |
| `404.html` | Makes page links like `/augustus/2025` work. Don't delete it. |
| `config.js` | One setting: your Google Sheet's ID. |
| `data/` | Starter copies of the data as CSV files. The site only uses these if `config.js` has no sheet ID. |
| `README.md` | This guide. |

`BAM-Fantasy-Sheet-Template.xlsx` comes with this folder (it sits next to it, not inside). It's the starting point for your Google Sheet.

---

## Part 1: Create your Google Sheet (5 minutes)

1. Go to **sheets.google.com** and sign in with your Google account.
2. Open a blank spreadsheet. Then go to **File → Import → Upload** and choose `BAM-Fantasy-Sheet-Template.xlsx`.
3. When it asks, pick **Replace spreadsheet** and click **Import data**. You'll see 10 tabs along the bottom: **Leagues, Managers, Members, Seasons, Standings, Games, Lineups, Draft, Trades, Awards**.
4. Rename the file at the top left, for example "BAM Fantasy Data".
5. Click **Share** (top right). Under **General access**, change "Restricted" to **Anyone with the link**, and make sure the role says **Viewer**. Click **Done**.
   - This lets the website read the sheet. Nobody can edit it unless you add them as an editor.
6. Copy the sheet's ID from the address bar. It's the long code between `/d/` and `/edit`:
   `https://docs.google.com/spreadsheets/d/`**`1AbCdEfGh...xyz`**`/edit#gid=0`

> ⚠️ Don't rename the tabs or the column headers in row 1. The site finds the data by those names. You *can* reorder columns or make them wider.

## Part 2: Connect the site to your sheet (1 minute)

1. Open `config.js` in Notepad (right-click → Open with → Notepad).
2. Paste your sheet ID between the quotes:
   ```js
   window.BAM_CONFIG = {
     sheetId: "1AbCdEfGh...xyz"
   };
   ```
3. Save the file.

## Part 3: Put the site online with GitHub Pages (10 minutes)

1. Go to **github.com** and create a free account. Your username becomes part of the web address.
2. Click the **+** at the top right → **New repository**.
   - Repository name: `bam-fantasy` (or anything you like).
   - Choose **Public**. Free GitHub Pages needs a public repository. The site is public anyway.
   - Click **Create repository**.
3. On the next page, click **uploading an existing file**.
4. Open the `bam-fantasy-site` folder on your computer, select **everything inside it** (`index.html`, `404.html`, `config.js`, `README.md` and the `data` folder), and drag it onto the GitHub page. Then click **Commit changes**.
   - Upload the *contents* of the folder, not the folder itself. `index.html` must sit at the top level of the repository.
5. Go to the repository's **Settings → Pages** (in the left sidebar).
6. Under **Build and deployment → Source**, choose **Deploy from a branch**. Set the branch to **main** and the folder to **/ (root)**, then click **Save**.
7. Wait 1–2 minutes and refresh the page. A box appears saying **"Your site is live at https://YOUR-USERNAME.github.io/bam-fantasy/"**. That's the link to share with your leagues.

That's the whole setup. 🎉

---

## Sharing links to one league, season or person

Every page has its own address, so you can send someone exactly the part you mean. With the site at `https://YOUR-USERNAME.github.io/bam-fantasy/`:

| Page | Address (add it after the main address) |
|---|---|
| The hub | `/bam-fantasy/` |
| A league (pick All time or a year) | `/bam-fantasy/augustus` · `/bam-fantasy/gridiron` · `/bam-fantasy/big-ten` |
| A league's all-time page | `/bam-fantasy/gridiron/all-time` |
| One season (champion, standings, weeks) | `/bam-fantasy/augustus/2025` |
| A season's draft / trades / lore | `/bam-fantasy/augustus/2025/draft` · `…/trades` · `…/lore` |
| All players | `/bam-fantasy/players` |
| One player | `/bam-fantasy/players/italian-vino` |

- League addresses come from the **Short Name** column on the Leagues tab: lowercase, with spaces turned into dashes. "Big Ten" becomes `big-ten`. The full name works too: `/bam-fantasy/augustus-league` opens the same page.
- Player addresses come from the name the same way: "John Lee" becomes `players/john-lee`.
- League pages and player pages have a **Copy link** button, so you never have to type these.
- With a custom domain, drop the `/bam-fantasy` part: `bamfantasy.com/augustus/2025`.

---

## Updating the site (the part you'll do all season)

Edit the Google Sheet. The website picks up changes the next time someone loads the page. Google can take up to about 5 minutes to pass along a change, so if an edit doesn't show yet, wait a moment and refresh.

### General rules
- **Names must match.** Type each person's name the way it appears on the **Managers** tab ("Italian Vino", "John Lee"). Capital letters don't matter, but spelling does. "Italian Vinno" would show up as a new, separate person.
- **Leagues** can be typed as the short name (`Gridiron`, `Augustus`, `Big Ten`) or the full name.
- **Blank cells are fine.** Anything you leave empty shows as "coming soon" on the site.
- **One row = one thing.** One season, one team's standing, or one game. Add rows at the bottom.

### The tabs

**Leagues**: one row per league.

| Column | Example | Notes |
|---|---|---|
| League | Augustus League | Full name, shown as the big title |
| Short Name | Augustus | Used in the menu and in the web address |
| Founded | 2019 | Optional. If blank, the first season's year is used |
| Type | Dynasty | `Redraft` or `Dynasty`. Changes the wording on the Draft tab |
| Weeks | 17 | How many weeks (playoffs included) each season page lists |
| Color | #8B8BFF | Optional. The league's glow color (a hex code) |
| Order | 2 | The order leagues appear in (1, 2, 3…) |

**Managers**: one row per person, across all leagues.

| Column | Example | Notes |
|---|---|---|
| Name | Italian Vino | |
| Nickname | | Optional |
| Photo | images/italian-vino.jpg | Optional headshot. Leave blank to show their initials |

**Adding headshots (with the neon blue background):**
1. Put the new photos in the `headshots` folder inside **BAM Fantasy** on your Desktop. Name each one after the person, like `johnnfl.png` or `Italian Vino.jpg`. Capital letters, spaces and an "nfl" ending don't matter.
2. Open **PowerShell** (Start menu → type "PowerShell" → Enter) and paste this one line:
   ```
   powershell -ExecutionPolicy Bypass -File "$HOME\OneDrive\Desktop\BAM Fantasy\tools\make-headshots.ps1" -Source "$HOME\OneDrive\Desktop\BAM Fantasy\headshots" -Out "$HOME\OneDrive\Desktop\BAM Fantasy\bam-fantasy-site\images" -Managers "$HOME\OneDrive\Desktop\BAM Fantasy\bam-fantasy-site\data\Managers.csv" -NeonBackground
   ```
   It turns every photo into a square 512×512 PNG with the neon blue background and prints a line like `OK johnnfl.png -> images/john.png (John)` for each one. Photos that are already done just get redone, which is harmless.
3. In the Google Sheet's **Managers** tab, put that path in the person's **Photo** column, for example `images/john.png`.
4. On GitHub, open the `images` folder in your repository, click **Add file → Upload files**, and drag in the new PNGs from `bam-fantasy-site\images`.
5. Optional: so everyone's browser shows the new photos right away, open `index.html` on GitHub (pencil icon), find `const IMG_VERSION = "..."`, change the text inside the quotes to anything new (for example today's date), and commit.

The background swap works on plain white studio backgrounds like the NFL-style headshots. Always look at the result before uploading. On a busy photo (outdoors, a colored wall) it may not find the background cleanly.

A full web link to a photo (starting with `https://`) also works in the Photo column, but it keeps whatever background it has.

**Members**: who plays in which league. One row per person per league. This handles people who aren't in every league: Italian Vino gets an Augustus row but no Big Ten row.

| League | Manager |
|---|---|
| Augustus | Italian Vino |
| Big Ten | Brian |

**Seasons**: one row per league per year. **Each row creates a season tab** on that league's page.

| Column | Example | Notes |
|---|---|---|
| League | Augustus | |
| Year | 2025 | |
| Status | Final | `Final` or `In progress` |
| Teams | 8 | Optional. How many teams that season (Gridiron 2019 had 8) |
| Note | Gridiron's first season. | Optional. A one-line note shown at the top of that season's page |
| Champion | Sage | Leave blank until the title is decided |
| Runner Up | Lucas | |
| Championship Week | 17 | Optional |
| Champion Score | 141.6 | Optional. Shows the championship scoreboard |
| Runner Up Score | 128.2 | Optional |
| Lore | The season recap… | As long as you like. Press **Ctrl + Enter** inside a cell for a new line |

**Standings**: one row per team per season (the final regular-season table).

| League | Year | Manager | Wins | Losses | Ties | Points For | Points Against | Finish | Made Playoffs | Playoff Wins |
|---|---|---|---|---|---|---|---|---|---|---|
| Augustus | 2025 | Sage | 10 | 4 | 0 | 1650.4 | 1500.2 | 1 | Yes | 2 |

- **One row per team that played that season.** The Gridiron rosters for 2019–2026 are already filled in with the records left blank. Fill in Wins, Losses and the rest as you go. Until then the site shows "—" instead of a made-up 0-0.
- **Co-managed team:** put both names in the Manager cell joined with `&`, like `Tristan & Juan` (Gridiron 2022). Both people get credit for that team's record, playoff runs and any title. Use the same `Tristan & Juan` in Games, Champion or Runner Up for that season too.
- These rows also feed the **All time** page (seasons played, record, points for and against, playoff numbers) and each person's page on **Players**.
- **Made Playoffs** (`Yes`/`No`) and **Playoff Wins** are optional. Fill them in for old seasons where you don't have every game. Leave them blank and the site works them out from the **Games** tab: any game with a playoff **Round** counts.
- **Titles** and **Finals** (championship appearances) come from the Champion and Runner Up columns on **Seasons**. You don't enter them here.

**Games**: one row per matchup. These fill the **Weeks** dropdowns on each season page.

| League | Year | Week | Round | Team 1 | Team 1 Score | Team 2 | Team 2 Score |
|---|---|---|---|---|---|---|---|
| Augustus | 2025 | 1 | | Sage | 120.5 | Lucas | 98.2 |
| Augustus | 2025 | 16 | Semifinal | Sage | 130.0 | Justin | 110.4 |
| Augustus | 2025 | 17 | Championship | Sage | 141.6 | Lucas | 128.2 |

- Leave **Round** blank for regular-season games. For playoffs write `Quarterfinal`, `Semifinal` or `Championship`. Use `Consolation` for games that don't count as playoffs, like the third-place game or the sacko bracket.

**Lineups**: who started and who sat, for any matchup. One row per player per team per week. It shows up when someone opens that matchup.

| League | Year | Week | Manager | Slot | Player | Position | NFL Team | Points |
|---|---|---|---|---|---|---|---|---|
| Augustus | 2025 | 1 | Sage | QB | (player name) | QB | KC | 24.5 |
| Augustus | 2025 | 1 | Sage | FLEX | (player name) | WR | CIN | 12.0 |
| Augustus | 2025 | 1 | Sage | BN | (player name) | RB | ATL | 7.1 |

- **Slot** is the lineup spot: `QB`, `RB`, `WR`, `TE`, `FLEX`, `SUPERFLEX`, `K`, `DST`, or `BN` for bench (`IR` and `TAXI` also count as bench). Starters show first, in that order, with a starters total. The bench is listed below.

**Draft**: every pick of that year's draft. One row per pick.

| League | Year | Round | Pick | Manager | Player | Position | NFL Team |
|---|---|---|---|---|---|---|---|
| Gridiron | 2025 | 1 | 1 | Colby | (player name) | RB | ATL |

- In Gridiron (redraft) that's the full draft every year. In Augustus and Big Ten (dynasty) it's the startup draft in year one and the rookie draft after that.

**Trades**: every trade, one row per trade.

| League | Year | Week | Date | Manager 1 | Manager 1 Gets | Manager 2 | Manager 2 Gets | Note |
|---|---|---|---|---|---|---|---|---|
| Augustus | 2025 | 6 | Oct 12 | Sage | Player A; 2026 2nd | Lucas | Player B | Optional comment |

- Separate several items with a semicolon (`;`). Week and Date are both optional; use whichever you know.

**Awards**: optional fun extras for a season ("Highest single week", "Worst trade", "Sacko"). They show on the season's **Lore** tab.

| League | Year | Award | Manager | Note |
|---|---|---|---|---|
| Augustus | 2025 | Highest single week | Sage | 188.4 in week 6 |

### Common tasks
- **New season starting (e.g. 2027):** add a row to **Seasons** for each league with Status `In progress`. When it ends, change it to `Final` and fill in the champion, scores and lore.
- **New league:** add a row to **Leagues**, its people to **Members**, and at least one row to **Seasons**.
- **New person:** add them to **Managers**, then to **Members** for their league(s).
- **Fix a typo:** just fix the cell.

---

## Changing the design or wording later

To change the look or the text (not the stats), you need to edit `index.html`. On GitHub, open the file, click the ✏️ pencil icon, make the change and click **Commit changes**. The site updates in about a minute. Or ask Claude to make the change and upload the new `index.html` the same way.

## Optional: your own web address

You can buy a domain such as `bamfantasy.com` (about $10–15/year from Namecheap, Cloudflare, Google/Squarespace Domains, etc.). Then go to **Settings → Pages → Custom domain** in your repository and follow GitHub's instructions for pointing the domain at it.

## Troubleshooting

| What you see | Fix |
|---|---|
| "The league data didn't load" | Make sure the sheet is shared as **Anyone with the link → Viewer**, that the ID in `config.js` is right, and that the tab names are unchanged (Leagues, Managers and Seasons must exist; the others can be empty). |
| A headshot doesn't show | Check the Photo path matches the file name exactly, including capital letters and `.jpg` vs `.png`. |
| A person appears twice on Players | A name is spelled two different ways somewhere. Search the sheet for the misspelling and fix it. |
| A season tab is missing | Make sure **Seasons** has a row for that league and year, with the league name spelled right. |
| Change isn't showing | Wait a few minutes (Google caches for a short time), then refresh the page. |
| GitHub shows a 404 page | Make sure `index.html` and `404.html` are at the top level of the repository and Pages is set to **main / (root)**. |
| Links like `/augustus` open the hub or a blank page | Only if the repository is named `YOUR-USERNAME.github.io` (so the site has no `/bam-fantasy` part). Open `404.html`, find `var keep = …` and change that line to `var keep = 0;`. |
