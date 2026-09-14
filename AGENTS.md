# Память проекта

## Правило общения
Мыслить и отвечать по-русски. Весь вывод для пользователя — на русском языке (имена переменных, комментарии и код остаются на английском).

## О проекте

3D Tower Defense игра в одном HTML-файле (Three.js через CDN, всё остальное инлайн: CSS, JS). Запуск — простое открытие файла в браузере, без сборки и сервера. Арт-стиль: low poly.

Рабочая папка: `D:\game`
Ожидаемый результат: один файл, предположительно `D:\game\index.html`.

## Обязательные правила (источник: `docs/Правила проекта.md`)

Перед любой работой читать `D:\game\docs\Правила проекта.md` и соблюдать конвейер:
**Понять → описать → проанализировать → согласовать → получить разрешение → реализовать → протестировать → принять.**

Ключевые правила:
1. Новую фичу проектируем в спеке, код до фиксации спеки не пишем.
2. Реализация не начинается без явного разрешения пользователя.
3. Существенные решения не принимаются вместо пользователя.
4. Саб-агенты запускаются только по прямому указанию пользователя.
5. Спека — единый источник истины.
6. Максимум 3 итерации проектирования.
7. Спека прогоняется через каждую роль (Аналитик → Системный аналитик → Дизайнер* → Разработчик → Тестировщик → Финальный аналитик); каждая роль ревьюит спеку и вносит свои замечания; проходы повторяются, пока замечаний не останется, но не более 3 раз.
8. После фиксации спрашивать: «Спека зафиксирована. Реализация не начинается автоматически. Ожидаю явного указания пользователя на реализацию.»

Явное разрешение — команды: «реализуй», «начинай разработку», «начинай реализацию», «делай», «можно писать код».
Не разрешение: «хорошо», «согласен», «всё понятно», обсуждение.

## Статус конвейера

- **Конвейер пройден (итерация 1):** Аналитик → Системный аналитик → Дизайнер → Разработчик → Тестировщик → Финальный аналитик.
- Полная спецификация — в файле `D:\game\docs\СПЕКА.md` (единый источник истины).
- **Реализация завершена.** Все 3 карты реализованы, 694/694 автотестов PASS (Node) + 715/715 (DOM-mock).
- Открытых вопросов по ТЗ нет.

## Спека

Полный текст: `D:\game\docs\СПЕКА.md` (конвейер пройден, все разделы: цель, требования, сценарии, архитектура, UX/UI+responsive, план реализации, Feature Tests, UI Tests, тестовые данные, критерии приёмки).

Краткая память (детали — в СПЕКА.md):
- Один файл `D:\game\index.html`: Three.js r184 (importmap, jsdelivr), код в `<script type="module">`, UI русский.
- Сетка 16×12, **3 карты** (Классика/Зигзаг/Дуга) с ротацией при старте/рестарте; дорожки 3–4 поворота, по 22 слота каждая (списки клеток — в СПЕКА.md п. 2.10).
- 5 башен × 3 уровня (лёд/огонь/молния/яд/пушка), 5 монстров (слизь/бегун/гигант/летун/дракон), волны бесконечные (6+N×2, HP×(1+0.10N)).
- 400 монет старт, награда только за убийство, 20 жизней.
- Автотесты: встроенный харнесс `index.html?test=1` (console.assert) без внешних фреймворков.

### Скилы проекта (установлены)
- `.opencode/skills/threejs-scene-setup/SKILL.md` — сцена, import map, камера, OrbitControls, resize.
- `.opencode/skills/threejs-materials-lighting/SKILL.md` — материалы, flatShading, свет, тени.
- `.opencode/skills/tower-defense/SKILL.md` — волны, таргетинг, экономика, жизни.
- `.opencode/skills/game-ai/SKILL.md` — movement, pathfinding, steering, dt.
- `.opencode/skills/game-ui-ux/SKILL.md` — HUD, оверлеи, responsive.
- `.opencode/skills/create-game-assets/SKILL.md` — арт-направление: палитра, shape language, детализация, стиль; референс `references/art-direction.md`.

### Референсы (web)
- Crystal Defense (aguier, itch.io) — 3D TD в одном HTML-файле на Three.js: CatmullRom S-path, chain lightning, canvas damage numbers.
- Полный разбор найден в этой сессии; можно использовать как образец техник.

## Принятые решения
- Папка пустая, начинаем с нуля.
- Визуальный стиль low-poly — по решению агента (референс-изображений не прикреплялось, пользователь разрешил свой выбор).
- Установлены 6 скилов из awesome-gamedev-agent-skills (в т.ч. `create-game-assets` — арт-направление).
- Игра в одном файле: Three.js через `importmap` (CDN jsdelivr), код в `<script type="module">`.

## Дизайн-итерация (арт-направление, реализована)
- Детализация геометрий: `geoSphere` 12→16 сегм, `geoIco` detail 0→1, `geoCone`/`geoCyl` 10/12→14/16, `geoCap` 4/8→6/12.
- Поле: волнистый `PlaneGeometry(16,12,24,18)` со смещением вершин + цветовые пятна травы + бордюр.
- Дорожка: сегменты-плиты со скошенными кромками (roadEdgeMesh) + тёмные швы (roadDark).
- Башни: гексагональные основания-плиты (geoHex), кольцо-перило (TorusGeometry), каменные швы (darkMat).
- Палитра PALETTE: grassBase/grassPatch/grassEdge/roadBase/roadDark/slotBase/towerWarm/towerCool/stoneDark/magicAccent/skySoft.
- Монстры: нагрудник гиганта (светлая пластина + плечи + ремень), чешуя дракона (тетраэдры scaleMat).
- Декорации: кусты 3 сферы (вариативные тона), камни 3 icos (три тона), холмы 2 сферы, сосны 3 конуса (вариативные зелёные).
- Скай-фон: skySoft (0xcfe8ff).

**Спека:** `D:\game\docs\specs\визуальное-улучшение-дизайна-2026-09-14.md` — реализована.

## Текущая задача (перенос UI + bake-рендер)

**Спека:** `D:\game\docs\specs\перенос-ui-и-bake-рендера-2026-09-14.md` — конвейер пройден
(2 прохода ролей Аналитик → Системный аналитик → Дизайнер → Разработчик → Тестировщик →
Финальный аналитик), **реализована** (по явному разрешению). Автотесты: **694/694 (Node) +
727/727 (DOM-mock) PASS.** Браузерный визуальный чек/FPS-смоук — предстоит.

Реализовано в `D:\game\index.html`:
- UI из `tower-defense-astra.html`: topbar, command-бар с dock из 5 карточек (не плавающая
  панель), инспектор справа (`#inspector`), нативные `<dialog>` (game-over/справка), toast,
  footer-hints, автопауза по `visibilitychange`, keyboard (1–5/Пробел/Esc/R), старт-экран.
- Док-флоу: `chooseType` → build по слоту через raycast-пикер; floaters-слои и счётчики
  визуализации удалены вместе с `#build-panel`/`#tower-panel`, `positionPanel`/`screenPos`.
- SVG-иконки башен: в `TOWERS[]` добавлены `css` (hex) и `tag`; иконки — inline `icon(id)`.
- Bake-рендер: `worldMat` (MeshStandardMaterial vertexColors flatShading), `bake()` на
  `toNonIndexed`/`clone(true)`; кеши `towerGeo` (15: база+топ раздельно, turret у cannon),
  `actorCache` (5: static-bake + pivots leg/arm/wing/jaw вне bake), `mapGeo` (3 по `map.id` +
  GEO-по-кэшу, `_cached` в `disposeObj`), призрак `buildTowerMesh(td,1,'ghost')` с прозрачными
  материалами 0.44; частицы `particles` → `InstancedMesh` (200, `DynamicDrawUsage`).
- Инварианты API сохранены: `tower.mesh.userData.ring` (+`userData.height`, `userData.turret`),
  `monster.userData.parts` (+`hp`), HP-бары Sprite CanvasTexture, range-ring 0x3ddc63 op .18.
- Тесты адаптированы под bake: model smoke (buildMonsterMesh/buildTowerMesh/buildMap), кеши
  стабильны, `fireProjectileTest` (children/типы 4 снарядов), UI smoke (dock 5 карточек,
  элементы существуют, toast `.show`), карты/пути, чистые функции (без изменений).
- DOM-mock в `verify-models.cjs` расширен: Geo (`index`, `toNonIndexed`, `applyMatrix4`, `setAttribute`,
  `computeBoundingSphere`, `clone(deep)`), `Float32BufferAttribute`, `MeshStandardMaterial`,
  `InstancedMesh`/`setMatrixAt`/`setColorAt`, `DynamicDrawUsage`, document с registry
  `getElementById` + Set-based classList stubs, `setTimeout`/`performance`/`window`.
- **Исправленный баг старта** (нашёл headless-прозвон): `btn.dataset = { type: t.id }` в `buildDock`
  бросал `TypeError` в реальном браузере (`dataset` — read-only getter), module-скрипт падал до `init()`,
  кнопка «Старт» не имела обработчика. Заменено на `btn.dataset.type = t.id`. DOM-mock этот баг
  пропустил, т.к. `dataset: {}` был обычным объектом (разрешал перезапись) — учёт на будущее:
  DOM-mock stubs должны повторять read-only геттеры реальных HTMLElement.
- Прозвон: `C:\Users\Dominic\AppData\Local\Temp\opencode\cdp-test.cjs` (headless Chrome + CDP)
  подтверждает: dock=5 карточек, клик по «Старт» → старт-экран скрыт, волна идёт (отсчёт 12→8 с).

## Следующие шаги
1. Браузерный чек: `?test=1` в реальной странице, FPS-смоук ≥50, визуальная отладка
   (dock каретки, hover, ghost, инспектор, диалоги, автопауза, responsive ≤1100/≤760).
2. Возможные мелочи: пустая заглушка `updateMapNameUI_(){}` в index.html (безвредна, удалить при случае).
3. Ожидаем явного указания пользователя для дальнейших задач.