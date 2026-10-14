# AutoPano for PixInsight

**One-button seamless mosaic of linear deep-sky panels — with a seam brush when you want control.**

![AutoPano window: Leo Triplet, 6 colour panels, seam map on](screenshot.png)

Open the stacked panels of a mosaic (linear masters, colour or one mono filter), tick them and press **Build mosaic**. AutoPano:

1. **Places the panels.** Panels without an astrometric solution are plate-solved (ImageSolver); a panel whose header coordinates are far off is placed on a solved neighbour by its star patterns first. Any camera rotation works. Then the positions of all panels are refined **together** on the stars of every overlap (bright-star residuals typically 0.1–0.3 px), and each panel is reprojected onto one common grid.
2. **Matches the photometry.** Star fluxes give each panel's scale; the backgrounds are flattened and the remaining differences are removed by smooth surfaces fitted to all overlaps at once — no panel order, no accumulating error.
3. **Joins the panels along seams through the sky.** The seams avoid stars and galaxies and prefer the cleaner panel; multiband blending hides them: the background blends over hundreds of pixels, the fine detail over a few.
4. **Keeps every panel's signal-to-noise ratio.** A noisier panel is not cleaned up and the cleaner ones are not made worse everywhere: the grain changes **gradually** around the noisier panel (noise of the same texture is added in a transition zone), and bright parts of stars and galaxies are protected from it.

The preview always shows the final result (zoom in to 1:1 to check the grain). Sliders adjust the noise transition and the object protection; overlays show the object mask, the noise map and the **seam map** — which panel gives which part of the mosaic, with each panel's noise relative to the cleanest one. **Create image** makes the mosaic (32-bit, linear, with an astrometric solution) without closing the window, so you can try other settings.

**Seam editing** (*Edit seams*): pick a panel in the list or Ctrl+click it on the preview, then paint where this panel should be used — for example over a galaxy that a noisier panel covers. AutoPano routes the seams around your strokes through the background by itself. Alt+paint erases, Ctrl+Z / Ctrl+Shift+Z undo / redo, *Clear panel* removes the strokes of the active panel. *Apply* re-blends the changed part of the mosaic.

Panels that cannot be joined (no overlap, no common stars, or not plate-solvable) are left out with a clear message, and the mosaic is built from the rest.

## Installation

1. In PixInsight: **Resources › Updates › Manage Repositories › Add** and enter
   ```
   https://raw.githubusercontent.com/BananAndrews/AutoPano/main/repository/
   ```
2. **Resources › Updates › Check for Updates**, install, restart PixInsight.
3. Run it from **Script › Utilities › AutoPano**.

PixInsight 1.9.5 or newer. New versions arrive through **Check for Updates**. The repository is not signed yet, so PixInsight asks for confirmation once.

## Settings

| Setting | Default | Meaning |
|---|---|---|
| Use GraXpert (strong gradients only) | off | GraXpert background extraction (subtraction) on every panel first. Only for strong gradients (light pollution domes, Moon); AutoPano suggests it when it sees them. Needs GraXpert installed |
| Noise transition | 1.00 | width of the gradual grain change around noisier panels, relative to the automatic width |
| Object protection | 1.00 | how much bright parts of stars / galaxies are spared the added noise (0 = off) |
| Mask softness | 12 px | feather of the object mask |
| Crop to the filled area | off | the largest rectangle without empty corners (frame on the preview). The mosaic is always built at the angle that makes this rectangle largest — no extra resampling (the panels are reprojected once), the astrometric solution stays exact |
| Preview quality | Quarter | resolution of the preview (the final image is always full resolution) |

Mono mosaics: build each filter separately. Settings are remembered between runs.

## Tips

- Use linear (unstretched) masters; AutoPano warns about stretched ones.
- Overlaps of 15–20 % or more give the seams room around objects; the console log tells you if yours were small.
- Memory: about 4 GB for 6 colour panels of 3000 × 3000 px (a 7400 × 7500 px mosaic).

## Support

AutoPano is free. If it helped you — **[♥ support the author via PayPal](https://www.paypal.com/donate/?business=contact%40bluebelleweddings.com&no_recurring=0&item_name=AutoPano)** (the same heart is in the script window, where you can also pay by card).

## License

Free to use; all rights reserved. Modification and redistribution of modified versions are not permitted — see [LICENSE](LICENSE).

---

# AutoPano для PixInsight

**Бесшовная мозаика из линейных панелей одной кнопкой — и кисть швов, когда нужен контроль.**

Откройте сложенные панели мозаики (линейные мастера, цветные или одного моно-фильтра), отметьте их и нажмите **Build mosaic**. AutoPano:

1. **Расставляет панели.** Панели без астрометрии решаются ImageSolver; если координаты в заголовке сильно сбиты, панель сначала ставится на решённого соседа по рисунку звёзд. Поворот камеры — любой. Затем положения **всех** панелей уточняются вместе по звёздам всех перекрытий (невязки ярких звёзд обычно 0.1–0.3 px), и каждая панель перепроецируется на общую сетку.
2. **Выравнивает фотометрию.** Масштаб каждой панели — по потокам звёзд; фон выравнивается, остаточные различия убираются гладкими поверхностями, подобранными по всем перекрытиям сразу — без порядка склейки и накопления ошибки.
3. **Склеивает панели по швам через небо.** Швы обходят звёзды и галактики и тяготеют к более чистой панели; многополосное смешивание их прячет: фон смешивается на сотнях пикселей, мелкие детали — на нескольких.
4. **Сохраняет отношение сигнал/шум каждой панели.** Шумную панель не «замыливают», а чистые не портят целиком: зерно меняется **плавно** вокруг шумной панели (в переходной зоне добавляется шум той же фактуры), яркие части звёзд и галактик от него защищены.

Превью всегда показывает итог (приблизьте до 1:1, чтобы проверить зерно). Ползунки — ширина перехода шума и защита объектов; слои — маска объектов, карта шума и **карта швов**: какая панель какую область дала, с шумом каждой панели относительно самой чистой. **Create image** создаёт мозаику (32 бит, линейную, с астрометрией), не закрывая окно, — можно попробовать другие настройки.

**Правка швов** (*Edit seams*): выберите панель в списке или Ctrl+щелчком на превью и закрасьте, где брать эту панель — например, галактику, попавшую на более шумную панель. Швы вокруг мазков AutoPano проводит сам, по фону. Alt — ластик, Ctrl+Z / Ctrl+Shift+Z — отмена / повтор, *Clear panel* убирает мазки активной панели. *Apply* пересобирает изменённую часть мозаики.

Панели, которые не удалось присоединить (нет перекрытия, нет общих звёзд или не решается астрометрия), пропускаются с понятным сообщением, а мозаика собирается из остальных.

## Установка

1. В PixInsight: **Resources › Updates › Manage Repositories › Add**, адрес
   ```
   https://raw.githubusercontent.com/BananAndrews/AutoPano/main/repository/
   ```
2. **Resources › Updates › Check for Updates**, установить, перезапустить PixInsight.
3. Запуск: **Script › Utilities › AutoPano**.

Нужен PixInsight 1.9.5 или новее. Новые версии приходят через **Check for Updates**. Репозиторий пока не подписан — PixInsight один раз спросит подтверждение.

## Настройки

| Настройка | По умолчанию | Что делает |
|---|---|---|
| Use GraXpert (strong gradients only) | выкл. | сначала GraXpert (вычитание фона) на каждой панели. Только для сильных градиентов (засветка, Луна); AutoPano сам подскажет. Нужен установленный GraXpert |
| Noise transition | 1.00 | ширина плавного перехода зерна вокруг шумных панелей относительно автоматической |
| Object protection | 1.00 | насколько яркие части звёзд / галактик защищены от добавляемого шума (0 — выкл.) |
| Mask softness | 12 px | мягкость краёв маски объектов |
| Crop to the filled area | выкл. | максимальный прямоугольник без пустых углов (рамка на превью). Мозаика всегда собирается под углом, при котором этот прямоугольник наибольший, — без лишней интерполяции (панели перепроецируются один раз), астрометрия остаётся точной |
| Preview quality | Quarter | разрешение превью (итог всегда в полном разрешении) |

Моно-мозаики — каждый фильтр отдельным запуском. Настройки запоминаются.

## Советы

- Нужны линейные (не растянутые) мастера; на растянутые AutoPano предупредит.
- Перекрытие 15–20 % и больше даёт швам место обойти объекты; если было мало — скажет лог в консоли.
- Память: около 4 ГБ на 6 цветных панелей 3000 × 3000 px (мозаика 7400 × 7500 px).

## Поддержка

AutoPano бесплатен. Если он вам помог — **[♥ поддержите автора через PayPal](https://www.paypal.com/donate/?business=contact%40bluebelleweddings.com&no_recurring=0&item_name=AutoPano)** (такое же сердечко есть в окне скрипта, там можно оплатить и картой).

## Лицензия

Бесплатное использование; все права защищены. Изменение и распространение изменённых версий запрещены — см. [LICENSE](LICENSE).
