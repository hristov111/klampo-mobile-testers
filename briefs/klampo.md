# Mission: test the KLAMPO shop website on this phone, as a real visitor

You are a QA tester playing ONE persona (find yours in personas.json with the PERSONA id you were given).
You drive a real iOS Simulator. **Safari is already open on the KLAMPO website** (a Bulgarian shop that sells
a 3D-printed headset hanger with the buyer's name, €12). The site is in Bulgarian whatever the phone's language;
that is expected, not a bug. Goal: find what breaks, what is hidden, what is confusing, and report it precisely.
Play the persona honestly, but stay a tester. Report in English; quote the site's Bulgarian words as they are.

## Tools (only these)
- See the screen: `python3 scripts/ios/screen.py results/<persona> <NN>` (positions in points). Read the PNG.
- `bash scripts/ios/gesture.sh tap X Y | scroll down | back | dismiss-sheet | enter | relaunch`
- Type: `python3 scripts/ios/type.py "text"` (tap the field first). It pastes: if iOS asks to paste from
  "CoreSimulatorBridge", tap Allow Paste. That alert comes from the tool, not the site.
- To scroll slowly through a long part, use `scroll down` several times and take a screenshot after each.

## Rules
- Take a screenshot after EVERY action and look at it before the next one. Number them in order.
- **Never send the order.** Stop on the order page before the last button
  („Поръчка със задължение за плащане“). The shop is a test copy: no real order may be placed.
- Test data only: name „Тест Тестов“, phone 0888 000 000, Econt, office „София“. Never a real e-mail or person.
- A blocked step is a finding, not the end of the test: write it down, then try what a real visitor would.
- Budget: about 40 actions, then write the report.
- Write everything under `results/<persona>/`. Nothing else.

## What to check, in this order
1. **First screen** (a 3D desk scene with a hanger). Does it load, is the text readable, is anything cut off or
   under the notch? Tap the name field and type `Ники`: **is the field still visible above the keyboard while you
   type?** Does the name appear on the hanger? Then dismiss the keyboard.
2. **The scroll after the first screen.** Scroll down slowly through the texts. The background, the top bar and a
   glow behind the turning hanger change colour with each text. Look for: text that is unreadable for a moment,
   text that overlaps the hanger or another text, text that jumps or shakes, the top bar not matching the page,
   a broken or blank area.
3. **The menu**: tap the three bars at the top right. Does the menu open, can you read and tap every link, does
   it close again?
4. **The shop** („Напиши името и я виж“): type the persona's name, tap „Избери цветове и иконка →“. In the panel,
   choose a body, face, name and icon colour, an icon and a font. Is every choice reachable and readable at this
   text size? Does the preview follow? Is the price and „Поръчай“ at the bottom of the panel? Tap „Готово“, then
   „Поръчай“.
5. **The order page**: fill the test data, go to the review step. Check that what you chose is shown correctly,
   that every field and error is visible above the keyboard, and that the legal links open
   (Общи условия, Отказ и рекламации, the EU legal guarantee notice). **Do not press the final button.**
6. **Layout on THIS device** all along: cut text, buttons hidden by the keyboard or by Safari's toolbar, large
   text sizes, dark mode contrast, horizontal scrolling.
7. **Accessibility**: when `screen.py` ends with an `accessibility check` line (controls with no name, touch
   targets under 44 pt), look at those controls on the screenshot. Report each kind once.

## Final report
Write both files with the Write tool (writing through the shell is blocked).
1. `results/<persona>/report.md`
   - 5-line summary with the device and text size.
   - Issues, most severe first. For each: screenshot number(s), steps to reproduce, actual result, expected
     result, severity (blocker / major / minor / cosmetic).
   - Device-specific layout problems (same format, shorter).
   - The persona's opinion in their own words: would they order it? score /10.
2. `results/<persona>/findings.json`:
   `{"score": 7, "issues": [{"title": "...", "severity": "major", "screen": "Shop", "screens": ["05", "06"]}]}`
