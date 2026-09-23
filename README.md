# PCB Design Review

A single-file, offline tool for running structured PCB design reviews: customer intake, a schematic review checklist, and sheet-by-sheet findings with customer replies. Each review saves as its own self-contained HTML file that you can reopen, archive or send.

Built by Marcus Lechner at Lechnology Engineering.

<h2 align="center">
  <a href="https://marcuslechner.github.io/PCB-Design-Review-Document/pcb-design-review.html">Click Here to Try It<br><sub>(Opens in your browser)</sub></a>
</h2>

https://github.com/user-attachments/assets/070b4f75-8d94-4626-b8a6-fc492cd98896



## Features

**Runs anywhere, no install**
- One HTML file with no build step, server, account or dependencies. Open it in a browser and start reviewing.
- Works offline. Web fonts load when online and fall back to system fonts when not.
- Follows your system's light or dark mode and works on desktop, tablet and phone.

**Customer intake**
- **"What needs to be reviewed?"** covers five areas: Schematic, PCB layout, BOM, Firmware review and Physical build. Each is a yes/no tick, has a notes box, and all details are always visible.
  - Schematic sub-items: verify environmental constraints, power consumption analysis, supporting components, signal line setup, symbols, IC selection, formatting, anything to ignore, additional notes.
  - BOM sub-items: component match, packaging and availability, counterfeit parts, anything to ignore, additional notes.
  - Physical build sub-items: enclosure design, enclosure fitment.
- **Out of scope:** any area or sub-item can be marked out of scope. It's struck through and asks for a reason (e.g. "customer request by email, 12 Sep"), so there's a record of what wasn't reviewed and why. Marking an area out of scope greys out everything under it.
- **Project details** are free-text answers for operating environment, thermal range, intended use (production vs demo unit or devkit), compliance standards, supporting documentation, and general notes.

**Schematic review checklist**
- One checklist for the whole schematic, in three groups:
  - **Boundary scan:** inputs and outputs, power, main ICs.
  - **Page-by-page review:** symbols, power, supporting components, relevant notes, inputs, outputs.
  - **System view:** connectivity, TX/RX crossing, net label consistency, duplicate power rail names.
- Tick items as you go, or mark them out of scope with a reason. Ticking a group ticks everything under it.
- A progress count shows how many items are done and how many are out of scope.

**Sheet-by-sheet findings**
- **Add sheet** creates a card for each schematic sheet, with sheet name and number. New sheets start with a Reviewer Notepad row.
- **Add note** adds a finding row with a severity dropdown, an automatic reference, your notes and the customer's reply, all on one line.
- Rows are colour coded by severity (see the table below).
- References are generated automatically, e.g. `3-W2` is the second Error/Warning on sheet 3. They update when you change a severity or sheet number.
- Tallies by severity are shown for each sheet and for the whole review.
- The ↑ / ↓ buttons on a sheet move it up or down, and **Sort by sheet number** puts every sheet in number order.
- Critical findings get a **Customer notified** date and time field, so there's a record of when the customer was told.
- Deleting a sheet or note needs a second click to confirm.

**Link chips**
- Paste a web address anywhere in a notes box and a clickable chip appears below it. Addresses starting with `www.` work too.
- File links are labelled with the file name, e.g. `https://example.com/datasheets/LDO-regulator.pdf` shows as **LDO-regulator.pdf**. Other links show the site name.

**Saving**
- **Autosave:** every change is saved in the browser as you type.
- **Save review** downloads a copy of the page with the whole review built in. It's named from the header, e.g. `PCB-Review_Acme-Robotics_Motor-Controller_Rev-B.html`.
- Saved reviews open fully filled in on any computer, with no extra files needed.
- A status line shows whether everything is in the saved file, and the browser warns before you close with unsaved changes.

## Severity levels

| Severity | Ref code | Colour | Use it for |
|---|---|---|---|
| Critical | C | Red | Issues the customer must hear about immediately. Log when they were notified. |
| Error/Warning | W | Amber | Errors and things that need fixing. |
| Note/Flag | N | Blue | Worth a look. Suggestions and nice-to-haves. |
| Review Later | L | Purple | Private reminder to come back and check something. |
| Reviewer Notepad | R | Grey | Private working notes, calculations, datasheet links. |

Review Later and Reviewer Notepad rows have one wide notes box and no customer reply column, since they're for you, not the customer.

## Quick start

1. Download `pcb-design-review.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox or Safari).
3. Fill in the header, work through the intake and checklist, and add sheets and findings.
4. Click **Save review** to save the customer's review as its own file.

## Recommended workflow

1. **Keep the template blank.** Treat `pcb-design-review.html` as your starting point and don't save customer data into it.
2. **Intake:** go through the intake with the customer. Tick the areas they want reviewed, mark exclusions out of scope with who asked and when, and fill in the project details.
3. **Save:** click **Save review** to create that customer's file. The template goes back to blank the next time you open it.
4. **Review:** work through the schematic, adding a sheet card for each schematic sheet and logging findings. Tell the customer about any Critical finding straight away and fill in the notified date.
5. **Customer replies:** send the findings, record the customer's responses in the reply column, and save again.
6. **Continue later:** reopen the customer's own file, not the template.

When you save again, most browsers add "(1)" to the file name. Delete or rename the older copy so you know which is current.

## How saving works

Two layers keep your work safe:

| Layer | Where it lives | Portable? |
|---|---|---|
| Autosave | The browser's local storage, on that computer only | No |
| Saved file | The review data built into the downloaded `.html` file | Yes |

When a file is opened, the tool compares the data built into the file with anything autosaved in that browser for the same review, and loads whichever is newer. So if you forget to save, your latest edits are still there the next time you open that file in the same browser.

**Autosave limitations:**
- Autosaved work only exists in that browser on that computer.
- Clearing browser data (cookies and site data) erases anything you haven't saved to a file.
- Opening a file in a different browser only shows what was saved into the file.

**Privacy:** Reviewer Notepad and Review Later entries are included in saved files. Check them before sending a saved review to a customer.

**Hosted as a Claude artifact:** when the tool runs inside claude.ai, it also syncs to the artifact's database and saves files through the artifact's download feature. Offline, those features are skipped automatically.

## Updating the template

Each saved review contains its own copy of the tool, so it keeps working in the version it was saved with. When the template is updated, older saved reviews don't change.

## Data format

A saved review stores its data as JSON inside:

```html
<script type="application/json" id="review-data" data-own>{ ... }</script>
```

In the blank template this tag contains `null`.

```jsonc
{
  "meta": { "customer": "", "board": "", "rev": "", "reviewer": "", "date": "YYYY-MM-DD" },
  "intake": {
    "scope": {
      "schematic": {
        "on": true,            // ticked
        "x": false,            // out of scope
        "r": "",               // out-of-scope reason
        "notes": "",
        "subs": { "power": { "on": true, "x": false, "r": "", "notes": "" } }
      }
      // also: layout, bom, firmware, build
    },
    "answers": { "env": "", "thermal": "", "use": "", "standards": "", "docs": "", "notes": "" }
  },
  "checks": {
    "s0-1": { "d": true, "x": false, "r": "", "a": "" }  // schematic checklist item: done, out of scope, reason, answer
  },
  "pages": [
    {
      "id": "…", "name": "Power supply", "number": "3",
      "notes": [
        { "id": "…", "sev": "warning", "mine": "", "reply": "", "notified": "" }
        // sev: critical | warning | note | later | pad
      ]
    }
  ],
  "reviewId": "…",      // assigned on first save
  "editedAt": 0,        // ms timestamp of last edit
  "fileSavedAt": 0      // ms timestamp of last Save review
}
```

## Editing the template

Everything lives in `pcb-design-review.html`:

| To change | Look for |
|---|---|
| Intake review areas and sub-items | `SCOPE` |
| Project details questions and placeholders | `QUESTIONS` |
| Schematic checklist items | `SCHEM` |
| Severity names and reference codes | `SEV`, `SEV_ORDER` |
| Colours (light and dark) | CSS custom properties in `:root` |

Rules when editing:
- The `review-data` tag in the template must stay exactly `null`.
- Keep existing keys in `SCOPE`, `SCHEM` and `SEV` when renaming labels, so review data from older versions still loads.
- Test in both light and dark mode, and at phone width.

## Project structure

```
.
├── pcb-design-review.html   # The app (blank template)
├── docs/                    # Screenshots and extra documentation
├── README.md
└── LICENSE.md
```

Saved customer reviews (`PCB-Review_*.html`) are excluded by `.gitignore` and must never be committed.

## License

Source-available under the [Lechnology Source-Available License](LICENSE.md). You can use, modify and share this tool, including internally in your own business, but you can't sell it or offer it as a paid product or service. This is not an OSI-approved open source license.

The Lechnology Engineering name and logo are not included in the license. Please replace them with your own branding if you redistribute or build on this project.

And if you find it useful and we meet someday, a beer would be appreciated. 🍺
