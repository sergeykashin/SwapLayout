# SwapLayout

> **SwapLayout is an unofficial fork of [RuSwitcher](https://github.com/rashn/RuSwitcher)** (MIT) by rashn — a different app with its own name and icon, built from the original's source — it is **not** the official app and has no affiliation with its author. The official app, website, Homebrew cask and update feed belong to the original project. This fork is distributed as **source code only**: build it yourself (see *Installation*).
>
> **SwapLayout — неофициальный форк [RuSwitcher](https://github.com/rashn/RuSwitcher)** (MIT) автора rashn: отдельное приложение со своим именем и иконкой, собрано из исходников оригинала — не официальное приложение и не связан с автором оригинала. Официальные сборки, сайт, Homebrew и ленту обновлений ведёт оригинальный проект. Форк распространяется **только как исходный код**: соберите его сами (см. «Установка»).

### What is different in this fork / Чем отличается форк

- **Hotkeys fire on key press**, not on release — fast typing right after the hotkey no longer cancels it (e.g. Option+Shift, the Windows-style layout switch, works the same on the Mac, in RDP and in Parallels). Trade-off: modifier-only hotkeys now also fire inside ordinary shortcuts such as ⌘⇧4.
- **Tap timing uses the event's own timestamp** — a busy main thread can no longer push a tap out of its time window.
- **The layout switches earlier** during a manual conversion, before the Accessibility checks and the retype.
- **Selection + trigger flips the whole selection** when smart conversion changed nothing (e.g. `Ai SDLC` ↔ `Фш ЫВДС`); mixed-script selections like `iPhone стоит` are still left alone.

Подробности и причины: [HOTKEY_NOTES.md](HOTKEY_NOTES.md) · Details and root causes: [HOTKEY_NOTES.md](HOTKEY_NOTES.md).

> ℹ️ The original's update feed is **disabled** in this fork: "Check for updates" just opens this repository's releases, and the app never installs the official build over itself. Update by pulling and rebuilding.
> ℹ️ Лента обновлений оригинала в форке **отключена**: «Проверить обновления» просто открывает релизы этого репозитория и не ставит поверх официальную сборку. Обновление — `git pull` и пересборка.

---

## English

Typed `ghbdtn` when you meant `привет`? Just tap **Option ⌥** and SwapLayout converts the last word into the right layout — typing it directly, no copy-paste. Works with any pair of installed keyboard layouts — Russian, Ukrainian, Belarusian, German, French, and more. The trigger is fully configurable (a single key or a two-key combo), it can also fix the layout **automatically as you type**, and it even works through **Apple Screen Sharing**.

### How it works

| Action | Result |
|---|---|
| Type a word, tap **Option ⌥** | Last typed word is converted |
| Tap **Option ⌥** again | Reverse conversion (undo) |
| Select text, tap **Option ⌥** | Selected text is converted |

The trigger is configurable — **Option**, **Command**, **Control** or **Shift** (left or right side, single or double-tap), or a **two-key combo** (⌘+⇧, ⌃+⇧, ⌘+⌥, ⌃+⌥) for the Windows-style Alt+Shift feel.

### Automatic conversion (beta)

SwapLayout can also fix the layout **automatically as you type**, with no key press. Turn it on in **Settings → Auto-conversion** (off by default). When you finish a word (space), it checks the word against the macOS system dictionary and — only when confident — converts it and switches the layout for you.

Words with trailing punctuation are handled too *(new in 2.7.0)*: `ghbdtn,` becomes `привет,` — the punctuation stays exactly as typed. Genuinely ambiguous tails are left alone on purpose: on the Russian layout the EN keys `. , ; :` are the letters `ю б ж Ж`, so `levf.` could mean either «думаю» or «дума.» — such words are yours to convert with the manual trigger.

Precision-first: to avoid false fixes it deliberately **skips** short words (< 3 letters), words with digits / URLs / punctuation in the middle, ALL-CAPS acronyms typed with Shift, camelCase / mixed-script code identifiers, terminals / IDEs / password managers, and password fields. It targets layout pairs that have a macOS system dictionary (English ↔ Russian / Ukrainian / German / French… are reliable); languages without one (Belarusian, Armenian, Georgian) keep using the manual trigger.

Because the check relies on the **macOS system dictionary** — which is less complete than the real vocabulary of the languages it converts — some compound, rare or slang words won't auto-convert on their own. That's exactly what the built-in exception lists are for: add words you type often to **Always convert** (or **Never convert**) and SwapLayout will handle them the way you want, no dictionary needed.

**Three exception lists** let you tune it (Settings → Auto-conversion):
- **Apps** — where auto-conversion stays off (terminals, IDEs and password managers are pre-filled; password managers can't be removed).
- **Never convert** — words it must never touch (nicknames, logins, brands). After a wrong fix, tap the trigger to undo and SwapLayout offers to add the word here.
- **Always convert** — words to always fix even if they aren't in the dictionary (compound words, slang). Add the **target** word — the result you want.

### Hebrew — right-to-left, experimental (new in 3.0)

SwapLayout's **first right-to-left layout**. Any layout pair involving Hebrew now converts: typed `akuo` in the wrong layout? One tap makes it `שלום`, in either direction, with any second layout. Because conversion is keycode-based, right-to-left text is handled safely and niqqud marks are never reordered.

**Automatic conversion for Hebrew pairs is deliberately conservative** — the macOS Hebrew dictionary accepts any letter sequence, so auto only fires on a positive signal (you typed in Hebrew but the word is a real word of your other layout's language). The reverse direction is left to the manual trigger — no corrupted words. This is a new, experimental feature; **the interface itself is not yet localized into Hebrew (RTL UI is planned separately)**, and reports from Hebrew typists are very welcome.

### Layout-switch hotkey (new in 3.0)

Besides the conversion trigger, you can set a **second hotkey that only switches the layout** — no conversion — in Settings. It supports modifier-only combos like **Ctrl+Shift** that macOS system settings can't assign, plus **right-key-only** and **double-tap** options (e.g. double-Shift to switch). Off by default.

### Remote desktop (beta — new in 2.5)

SwapLayout works through **Apple Screen Sharing**. Type into a remote Mac's session and fix wrong-layout text right there — by trigger or automatically — just like on your local machine. Run SwapLayout on **both** Macs and turn on **Remote Desktop mode** (beta, marked in the menu). Conversion happens on the Mac you're controlling, where the text actually lives.

### Layout flag at the cursor (beta — new in 2.6)

After you switch layout, SwapLayout can briefly show the layout flag **right next to the text cursor** — so you see which layout you're in without glancing at the menu bar. It hides as you start typing. Turn it on in the menu or Settings (off by default). It works wherever the app exposes the cursor position via Accessibility (native apps and most text fields); a few apps that draw their own text (e.g. the VS Code editor) don't expose it — there macOS's own input indicator covers the gap.

### Features

- **Any two layouts** — configure any pair from your installed system layouts. No hardcoded tables.
- **Hebrew — right-to-left (experimental, new in 3.0)** — the first RTL layout; convert to/from Hebrew with any second layout. Auto-conversion is conservative by design; the manual trigger works both ways.
- **Switch layout from the menu** *(new in 2.6.1)* — pick any installed layout right from the menu-bar menu (flag, name, a check on the current one) and click to switch.
- **Configurable trigger** — Option, Command, Control or Shift (left/right, single/double-tap), or a two-key combo like ⌘+⇧.
- **Layout-switch hotkey** *(new in 3.0)* — a separate hotkey that only switches the layout (no conversion), including Ctrl+Shift and other modifier-only combos macOS can't assign; right-key-only and double-tap options. Off by default.
- **Hotkeys tab** *(new in 3.5)* — all hotkeys in one place: "Always layout 1/2" (say, left ⌘ for English and right ⌘ for Russian), Fn (🌐) and Fn+Shift as keys, and a hotkey that adds a word to exceptions.
- **Automatic conversion (beta)** — optionally fix the layout as you type, with a precision-first system-dictionary check. Off by default.
- **Extended dictionary (optional)** *(new in 3.5)* — a download of about 250 KB with slang, names and new words; brand names like ChatGPT or GitHub typed in the wrong layout are recognized too.
- **Enter, Tab and text fixes** *(new in 3.5)* — auto-conversion can also trigger on Enter and Tab; abbreviations (brgds → Best regards), two capitals in a row and obvious typos get fixed as you type. All off by default.
- **Remote desktop (beta)** — fix the layout over Apple Screen Sharing, on the Mac you're controlling.
- **Exception lists** — a per-app exclusion list plus never-convert and always-convert word lists.
- **Layout sound (optional)** — a short sound on the first letter after a layout change, so you *hear* which layout you're in.
- **Layout flag at the cursor (beta)** — briefly show the layout flag next to the text cursor right after a switch.
- **Monochrome menu-bar icon (optional)** *(new in 2.6.1)* — a system-style `РУ/EN` badge instead of the colored flag; adapts to light/dark automatically. Off by default.
- **Universal binary** — runs natively on both Apple Silicon and Intel Macs.
- **Clipboard-free** — the converted word is typed directly via synthesized Unicode, so it works even in Electron / VS Code / Atom-class editors. Your clipboard is never touched (it's only a fallback for unusual apps).
- **Smart word detection** — converts the last typed word, including punctuation.
- **Selected text** — select any text and tap the trigger to convert it in place.
- **Tap again to undo** — reverse conversion if you changed your mind.
- **Per-app layout memory** — remembers the active layout for each application and restores it when you switch back.
- **16 interface languages** — English, Русский, Українська, Беларуская, Deutsch, Français, Español, Português, Polski, 中文, 日本語, 한국어, Ελληνικά, Български, Հայերեն, ქართული.
- **Auto-start at login** — set and forget.
- **Minimal footprint** — no Electron, no web views, pure Swift + AppKit.
- **No telemetry** — your keystrokes stay on your Mac.

### Download

Get **`SwapLayout-arm64.zip`** from the [Releases page of this fork](https://github.com/sergeykashin/SwapLayout/releases/latest). Apple Silicon only (arm64), macOS 13+.

1. Unzip and move `SwapLayout.app` to `/Applications`.
2. The build is **ad-hoc signed and not notarized**, so Gatekeeper blocks the first launch. Either run `xattr -dr com.apple.quarantine /Applications/SwapLayout.app`, or open *System Settings → Privacy & Security* and click *Open Anyway*.
3. Grant **Accessibility** and **Input Monitoring** when asked (and again after every update — the ad-hoc signature changes).
4. Quit the original RuSwitcher first — two event taps conflict.

The official app is published only by the original project (see its [website and Releases](https://github.com/rashn/RuSwitcher/releases)) — **do not use the official Homebrew cask or DMG if you want this fork's behaviour**, they install the original build.

### Installation

Building from source instead:

**Build from source (for macOS)**

```bash
git clone https://github.com/sergeykashin/SwapLayout.git
cd SwapLayout
git checkout swaplayout
cd macos
swift build -c release            # current architecture (arm64 on Apple Silicon)
```

`build_app.sh` builds a **universal** binary, which needs the full Xcode (`xcbuild`) and signs with the original author's Developer ID. For a local build either run it as `RS_SIGN_ID="-" ./build_app.sh` with Xcode selected, or use `swift build` as above and assemble the bundle yourself (copy the binary, `Info.plist`, `RuSwitcher.icns`, then `codesign --force --deep --sign - --entitlements RuSwitcher.entitlements`). With the Xcode toolchain the stricter compiler may reject some existing code; the Command Line Tools toolchain builds it.

An ad-hoc signed build has a different signature from the official app, so macOS asks for **Accessibility** and **Input Monitoring** again. Quit the official app before running this one — two event taps conflict.

Requires macOS 13+ and Xcode Command Line Tools.

### Permissions

On first launch, SwapLayout requests two macOS permissions:

1. **Accessibility** — to read and modify text in applications.
2. **Input Monitoring** — to detect keyboard events.

The app adds itself to the permission lists automatically — you only need to flip the toggles. The built-in permission wizard walks you through it step by step.

### Technical details

- `CGEventTap` (passive, listen-only) for keyboard monitoring.
- `UCKeyTranslate` (Carbon) for dynamic character mapping between any layout pair.
- `CGEvent.keyboardSetUnicodeString` to type the converted text directly — no clipboard, no pasteboard side effects.
- `CGEventSource.userData` marker to filter the app's own simulated events.
- `AXUIElement` API for focused element detection.
- `SMAppService` for login item management.
- No hardcoded layout tables — works with any installed layouts.

### Settings

Access via the menu bar icon → **Settings** (⌘,).

- **General** — conversion trigger (single key or combo), per-app layout memory, launch at login, interface language, layout pair.
- **Auto-conversion** — automatic conversion, **Remote Desktop mode (beta)**, and the three exception lists (apps, never-convert, always-convert).
- **About** — version, donate, contact, check updates.
- **Advanced** — debug logging, log management.

The menu-bar menu also has quick toggles for Automatic conversion, Layout sound, Flag at cursor and Remote Desktop mode.

### Support the project

If you find SwapLayout useful, support the **original author** (not this fork):

- [**Boosty**](https://boosty.to/ruswitcher) — donate to the original project
- **Star** this repo on GitHub

### Code signing

Free code signing on Windows provided by [SignPath.io](https://about.signpath.io/), certificate by [SignPath Foundation](https://signpath.org/).

### License

[MIT](LICENSE) — free to use, modify, and distribute. The license covers the code; the SwapLayout name and icon are not part of it, see [TRADEMARKS.md](TRADEMARKS.md).

App icon designed by [kutawhat](https://github.com/kutawhat).

Word lists (the downloadable dictionaries and the built-in list of common Hebrew words) are derived from [FrequencyWords](https://github.com/hermitdave/FrequencyWords) by Hermit Dave, CC BY-SA 4.0.

---

## Русский

Набрали `ghbdtn` вместо `привет`? Просто нажмите **Option ⌥** — и SwapLayout сконвертирует последнее слово в правильную раскладку, печатая его напрямую, без копипасты. Работает с любой парой установленных раскладок — русская, украинская, белорусская, немецкая, французская и другие. Триггер настраивается (одна клавиша или комбо из двух), есть **автоматическая конверсия по ходу набора**, и всё это работает даже через **Apple Screen Sharing**.

### Как работает

| Действие | Результат |
|---|---|
| Набрать слово, нажать **Option ⌥** | Последнее слово сконвертировано |
| Нажать **Option ⌥** повторно | Обратная конвертация (отмена) |
| Выделить текст, нажать **Option ⌥** | Выделенный текст сконвертирован |

Триггер настраивается — **Option**, **Command**, **Control** или **Shift** (левый или правый, одиночный или двойной тап), либо **комбо из двух клавиш** (⌘+⇧, ⌃+⇧, ⌘+⌥, ⌃+⌥) — в стиле привычного Alt+Shift.

### Автоматическая конверсия (бета)

SwapLayout умеет исправлять раскладку **автоматически по ходу набора**, без нажатий. Включается в **Настройки → Автоконверсия** (по умолчанию выключено). Когда вы заканчиваете слово (пробел), приложение сверяет его с системным словарём macOS и — только при уверенности — конвертирует и само переключает раскладку.

Слова с прилипшим знаком препинания тоже обрабатываются *(новое в 2.7.0)*: `ghbdtn,` превратится в `привет,` — знак останется ровно как набран. По-настоящему неоднозначные хвосты не трогаем сознательно: клавиши `. , ; :` английской раскладки — это буквы `ю б ж Ж` в ЙЦУКЕН, так что `levf.` может означать и «думаю», и «дума.» — такие слова конвертируйте ручным триггером.

Точность важнее полноты: чтобы не сработать зря, авто-конверсия намеренно **пропускает** короткие слова (< 3 букв), слова с цифрами / URL / пунктуацией в середине, акронимы капсом через Shift, camelCase / смешанные алфавиты (идентификаторы кода), терминалы / IDE / менеджеры паролей и поля паролей. Работает для пар раскладок, у которых есть системный словарь macOS (английский ↔ русский / украинский / немецкий / французский… — надёжно); для языков без словаря (белорусский, армянский, грузинский) остаётся ручной триггер.

Поскольку проверка опирается на **системный словарь macOS** — а он не так богат, как реальный словарный запас конвертируемых языков — некоторые составные, редкие или сленговые слова сами не сконвертируются. Ровно для этого и нужны встроенные списки исключений: часто используемые слова добавляйте в **«Всегда конвертировать»** (или **«Никогда не конвертировать»**), и SwapLayout будет обрабатывать их как вам нужно, без словаря.

**Три списка исключений** для тонкой настройки (Настройки → Автоконверсия):
- **Приложения** — где авто-конверсия выключена (терминалы, IDE, менеджеры паролей уже в списке; менеджеры паролей удалить нельзя).
- **Никогда не конвертировать** — слова, которые трогать нельзя (ники, логины, бренды). После ошибочной замены нажмите триггер для отмены — SwapLayout предложит добавить слово сюда.
- **Всегда конвертировать** — слова, которые исправлять всегда, даже если их нет в словаре (составные слова, сленг). Добавляйте **целевое** слово — то, что должно получиться.

### Иврит — справа налево, экспериментально (новое в 3.0)

**Первая раскладка с письмом справа налево.** Теперь конвертируется любая пара раскладок с ивритом: набрали `akuo` не в той раскладке? Один тап — и это `שלום`, в обе стороны, с любой второй раскладкой. Конверсия кейкодная, поэтому RTL-текст обрабатывается безопасно, а огласовки (никуд) никогда не переставляются.

**Авто-конверсия для пар с ивритом сознательно консервативна** — системный ивритский словарь macOS принимает любой набор букв, поэтому авто срабатывает только при положительном сигнале (вы набрали в иврите, а слово — настоящее слово языка второй раскладки). Обратное направление — ручным триггером, никаких испорченных слов. Это новая, экспериментальная функция; **сам интерфейс пока не переведён на иврит (RTL-интерфейс — отдельным этапом)**, и отзывы от тех, кто печатает на иврите, очень приветствуются.

### Хоткей переключения раскладки (новое в 3.0)

Помимо триггера конверсии можно назначить **второй хоткей, который только переключает раскладку** — без конверсии — в Настройках. Поддерживает комбо из одних модификаторов вроде **Ctrl+Shift**, которые системные настройки macOS назначить не позволяют, плюс опции **только правая клавиша** и **двойной тап** (например, двойной Shift → смена). По умолчанию выключен.

### Режим удалённого стола (бета — новое в 2.5)

SwapLayout работает через **Apple Screen Sharing**. Печатаете в сессии удалённого Mac — и неправильная раскладка исправляется прямо там, по триггеру или автоматически, как на локальной машине. Запустите SwapLayout на **обеих** машинах и включите **Режим удалённого стола** (бета, помечен в меню). Конверсия происходит на управляемой машине, где и находится текст.

### Флаг у курсора (бета — новое в 2.6)

После переключения раскладки SwapLayout может ненадолго показать флаг раскладки **прямо у текстового курсора** — видно, в какой раскладке печатаете, не глядя в меню-бар. Прячется, как только начинаете печатать. Включается в меню или Настройках (по умолчанию выключено). Работает там, где приложение отдаёт позицию курсора через Accessibility (нативные приложения и большинство текстовых полей); некоторые приложения, рисующие текст сами (например, редактор VS Code), позицию не отдают — там раскладку показывает встроенный индикатор macOS.

### Возможности

- **Любая пара раскладок** — настраивается любая пара из установленных в системе. Без захардкоженных таблиц.
- **Иврит — справа налево (экспериментально, новое в 3.0)** — первая RTL-раскладка; конверсия в/из иврита с любой второй раскладкой. Авто-конверсия консервативна by design; ручной триггер работает в обе стороны.
- **Переключение раскладки из меню** *(новое в 2.6.1)* — выберите любую установленную раскладку прямо в меню-баре (флаг, имя, галочка на текущей) и кликните для переключения.
- **Настраиваемый триггер** — Option, Command, Control или Shift (левый/правый, одиночный/двойной тап), либо комбо из двух клавиш вроде ⌘+⇧.
- **Хоткей переключения раскладки** *(новое в 3.0)* — отдельный хоткей только для смены раскладки (без конверсии), включая Ctrl+Shift и другие комбо модификаторов, недоступные системным настройкам; опции «только правая» и «двойной тап». По умолчанию выключен.
- **Вкладка «Хоткеи»** *(новое в 3.5)* — все хоткеи в одном месте: «Всегда раскладка 1/2» (например, левый ⌘ для английской и правый ⌘ для русской), Fn (🌐) и Fn+Shift как клавиши, хоткей, который добавляет слово в исключения.
- **Автоматическая конверсия (бета)** — опционально исправляет раскладку по ходу набора, с проверкой по системному словарю. По умолчанию выключено.
- **Расширенный словарь (по желанию)** *(новое в 3.5)* — загрузка около 250 КБ: сленг, имена, новые слова; узнаются и бренды вроде ChatGPT или GitHub, набранные не в той раскладке.
- **Enter, Tab и правки текста** *(новое в 3.5)* — автоконверсия может срабатывать и на Enter и Tab; автозамена сокращений (сув → С уважением), исправление двух заглавных подряд и явных опечаток. Всё выключено по умолчанию.
- **Режим удалённого стола (бета)** — исправление раскладки через Apple Screen Sharing, на управляемой машине.
- **Списки исключений** — список приложений плюс словари never-convert и always-convert.
- **Звук раскладки (опционально)** — короткий звук на первой букве после смены раскладки, чтобы *на слух* понимать раскладку.
- **Флаг у курсора (бета)** — ненадолго показывает флаг раскладки у текстового курсора сразу после переключения.
- **Монохромная иконка в меню-баре (опционально)** *(новое в 2.6.1)* — системная плашка `РУ/EN` вместо цветного флага, сама подстраивается под светлую/тёмную тему. По умолчанию выключена.
- **Universal-сборка** — нативно на Apple Silicon и Intel.
- **Без буфера обмена** — конвертированное слово печатается напрямую через синтез Unicode, поэтому работает даже в Electron / VS Code / Atom. Буфер обмена не трогается (только как запасной вариант для нестандартных приложений).
- **Умное определение слова** — конвертирует последнее набранное слово, включая знаки препинания.
- **Выделенный текст** — выделите любой текст и нажмите триггер для конвертации на месте.
- **Повторное нажатие — отмена** — обратная конвертация, если передумали.
- **Память раскладки по приложению** — запоминает активную раскладку для каждой программы и восстанавливает при возврате.
- **16 языков интерфейса** — English, Русский, Українська, Беларуская, Deutsch, Français, Español, Português, Polski, 中文, 日本語, 한국어, Ελληνικά, Български, Հայերեն, ქართული.
- **Автозапуск при входе** — настроил и забыл.
- **Минимальное потребление** — без Electron и веб-вьюх, чистый Swift + AppKit.
- **Без телеметрии** — ваши нажатия остаются на вашем Mac.

### Скачать

Скачайте **`SwapLayout-arm64.zip`** на [странице Releases этого форка](https://github.com/sergeykashin/SwapLayout/releases/latest). Только Apple Silicon (arm64), macOS 13+.

1. Распакуйте и перенесите `SwapLayout.app` в `/Applications`.
2. Сборка подписана **ad-hoc и не нотаризована**, поэтому Gatekeeper заблокирует первый запуск. Выполните `xattr -dr com.apple.quarantine /Applications/SwapLayout.app` либо откройте *Системные настройки → Конфиденциальность и безопасность* и нажмите *Всё равно открыть*.
3. Выдайте **Универсальный доступ** и **Мониторинг ввода** по запросу (и заново после каждого обновления: ad-hoc подпись меняется).
4. Закройте оригинальный RuSwitcher: два event tap конфликтуют.

Официальное приложение выпускает только оригинальный проект (см. его [сайт и Releases](https://github.com/rashn/RuSwitcher/releases)) — **не ставьте официальный Homebrew-cask или DMG, если нужно поведение форка**: они установят оригинальную сборку.

### Установка

Или соберите из исходников:

**Сборка из исходников**

```bash
git clone https://github.com/sergeykashin/SwapLayout.git
cd SwapLayout
git checkout swaplayout
cd macos
swift build -c release            # текущая архитектура (arm64 на Apple Silicon)
```

`build_app.sh` собирает **universal**-бинарь, для чего нужен полный Xcode (`xcbuild`), и подписывает Developer ID автора оригинала. Для локальной сборки либо запустите `RS_SIGN_ID="-" ./build_app.sh` с выбранным Xcode, либо соберите через `swift build` и оформите бандл вручную (бинарь, `Info.plist`, `RuSwitcher.icns`, затем `codesign --force --deep --sign - --entitlements RuSwitcher.entitlements`). Компилятор из Xcode строже и может отвергнуть часть существующего кода; тулчейн Command Line Tools собирает без проблем.

Сборка с ad-hoc подписью отличается от подписи официального приложения, поэтому macOS снова запросит **Универсальный доступ** и **Мониторинг ввода**. Перед запуском закройте официальное приложение — два event tap конфликтуют.

Требуется macOS 13+ и Xcode Command Line Tools.

### Разрешения

При первом запуске SwapLayout запросит два системных разрешения macOS:

1. **Универсальный доступ (Accessibility)** — для чтения и изменения текста в приложениях.
2. **Мониторинг ввода (Input Monitoring)** — для отслеживания нажатий клавиш.

Программа автоматически добавляется в списки разрешений — вам нужно только включить тумблеры. Встроенный мастер разрешений проведёт по шагам.

### Технические детали

- `CGEventTap` (пассивный, только чтение) для мониторинга клавиатуры.
- `UCKeyTranslate` (Carbon) для динамического маппинга символов между любой парой раскладок.
- `CGEvent.keyboardSetUnicodeString` для прямой печати конвертированного текста — без буфера обмена и побочных эффектов с pasteboard.
- Маркер `CGEventSource.userData` для фильтрации собственных симулированных событий.
- `AXUIElement` API для определения сфокусированного элемента.
- `SMAppService` для управления автозапуском.
- Без захардкоженных таблиц — работает с любыми установленными раскладками.

### Настройки

Доступ через иконку в строке меню → **Настройки** (⌘,).

- **Общие** — триггер конвертации (одна клавиша или комбо), память раскладки по приложению, автозапуск, язык интерфейса, пара раскладок.
- **Автоконверсия** — автоматическая конверсия, **Режим удалённого стола (бета)** и три списка исключений (приложения, never-convert, always-convert).
- **О программе** — версия, донат, контакт, проверка обновлений.
- **Дополнительно** — режим отладки, управление логами.

В меню в строке меню также есть быстрые тумблеры: «Автоматическая конверсия», «Звук раскладки», «Флаг у курсора» и «Режим удалённого стола».

### Поддержать проект

Если SwapLayout вам полезен, поддержите **автора оригинала** (не этот форк):

- [**Boosty**](https://boosty.to/ruswitcher) — донат оригинальному проекту
- **Star** на GitHub

### Подпись кода

Бесплатную подпись Windows-сборок предоставляет [SignPath.io](https://about.signpath.io/), сертификат — [SignPath Foundation](https://signpath.org/).

### Лицензия

[MIT](LICENSE) — свободное использование, модификация и распространение. Лицензия относится к коду: название SwapLayout и иконка в неё не входят, см. [TRADEMARKS.md](TRADEMARKS.md).

Иконку приложения нарисовал [kutawhat](https://github.com/kutawhat).

Списки слов (скачиваемые словари и встроенный список частых ивритских слов) взяты из [FrequencyWords](https://github.com/hermitdave/FrequencyWords) Hermit Dave, CC BY-SA 4.0.
