# Ferla Outlet - реорганизация listing page и product page

Предложение перед разработкой: структура страниц, UX, план внедрения.

К документу приложен кликабельный прототип страниц и скриншоты.

## 0. Коротко - подход и главные выводы

Outlet сегодня - 9 уникальных юнитов, которые лежат в WooCommerce как 9 обычных товаров: без атрибутов, SKU, short description и учета остатков. Предлагаю не редизайн, а: единая фотосъемка, структурированные данные в WooCommerce, перенос цены / скидки / состояния / CTA в первый экран.

| Что не так | Проверено на сайте | Решение |
|---|---|---|
| Порядок на listing - WordPress default (дата добавления) | Bikes и carts вперемешку, like-new рядом с new | §1 |
| Карточка не различает юниты | Фото + title + две цены одного размера. Ни condition, ни скидки, ни electric / non-electric | §1 |
| Скидка не названа | Ни "Save $1,000", ни "-14%". Покупатель вычитает в уме | §1, §2 |
| Цена на странице продукта проигрывает каталогу на главной | Mini - $4,999, при "Starts At $4,499"; Vending Black - $4,199 при $3,499; X Wood $8,999 при $6,499 (судя по всему, это base + add-ons, но страница этого не говорит) | §2, §6 |
| Condition - свободный текст | 5 написаний для трех реальных состояний; schema.org отдает NewCondition даже на "Like New" | §2, §3 |
| Product page: CTA ("Add to cart") | Кнопка стоит внизу на desktop и mobile; объем описания отличается в 100 раз - от 5 до 500 слов | §2 |
| Фото - главный разрыв с брендом | 5 разных пропорций в 9 карточках; склад, коробки, фургон; AI-картинка вместо Ice Cream (Yellow); один и тот же кадр у двух Promo Bike | §3, §5 |
| На PDP нет фактов, снимающих риск покупки | Только "Shipping and Taxes not included". Нет про crate ($1,200), final sale, warranty, "в наличии" | §2 |

**Три решения нужны до макетов**:

1. **Цена трех юнитов выше нового base.** Дефолт: Like new / Used - ниже текущего "Starts At" модели; New (previous generation) - ниже новой.
2. **Модель CTA.** По FAQ сайта заказ идет через Get a Quote или по телефону, а на странице продукта "Add to cart".
3. **Где стоят юниты, pickup, warranty.** Footer - Gardena, FAQ - "visitors to our location in Azusa". Дефолт: pickup разрешен (экономит покупателю $1,200 - больше, чем скидка), warranty задается по грейду.

## 1. Предлагаемая структура Outlet listing page

Что остается: header, footer, 3-колоночная сетка, скругленные карточки, механика WooCommerce. Редизайн - это новый loop item и новый archive template, а не пересборка всего сайта.

### Секции сверху вниз

| # | Секция | Что | Зачем |
|---|---|---|---|
| 1 | H1-блок | H1 "Ferla Outlet - Bikes & Carts"; под ним строка "9 units in stock / ships now (new builds 2-4 weeks) / flat $1,200 crate shipping or pick up". Белый фон вместо черной полосы | Это единственный темный блок в верхней части сайта. Строка с count дает карту страницы: на mobile 9 карточек - это ≈4,300 px |
| 2 | "How Outlet works" | "In stock - ships in N business days" / "Inspected and serviced in our Southern California warehouse" / "Flat $1,200 crate shipping - or pick up and save it" / плитка warranty (где есть) | Немедленная доступность - главный USP outlet, который нигде не указан |
| 3 | Condition | Одна строка: "New (previous generation) - never used, prior production run" / "Like new - demo unit, minor marks, mileage listed" / "Used - visible wear, documented in photos" | Не chips и не tooltips: chips выглядят как фильтр, tooltips не работают на mobile |
| 4 | Grid **Bikes (7)** | Centered H2 в стиле "Why Ferla X?", 3 колонки, карточки 4:3 | Тип (bike или cart) - первое решение покупателя |
| 5 | Grid **Carts (2)** | То же | - |
| 6 | Lifestyle-band | Переиспользуем financing блок как на /ferla-x: "Need a different configuration?" + "Build new in 3D" + "Talk to sales". | Ловим тех, кому не подошел ни один из 9 |
| 7 | "Recently sold" | До 4 карточек: badge "Sold", без цены, 60% opacity, 30 дней | Social proof: юниты реально продаются |
| 8 | Buying guide: accordion, collapsed | Текущая статья ужата | URL /outlet сохраняет SEO-ценность (§5) |

Reviews здесь не ставим.

### Карточка товара

| # | Элемент | Содержание | Визуальный вес |
|---|---|---|---|
| 1 | Фото 4:3, фон #F5F5F5, радиус 16px; badge condition в левом верхнем углу | Белая pill, текст #111, 12-13px, sentence case - не красная, не uppercase | Доминирует, ~70% карточки |
| 2 | Model name | "Ferla Mini", "Grande Cart" - одна строка | 20px / 600 |
| 3 | Spec line, ~40 символов | "Electric / Foldable frame / Black" | 14px, #7A7A7A |
| 4 | **Outlet Price** | "$4,999" без подчеркивания | 22-24px / 700, #111 - самый тяжелый текст карточки |
| 5 | **Original Price + Discount** | "Configured new ~~$5,499~~" + "Save $500" (фон #fdecea, текст #F93922) | 14-15px; chip 13px / 600 |
| 6 | Availability + link | "1 unit / ships now" + text-link "View unit ->" | 15px / 600 |

Три вопроса покупателя: *какие модели* - group header + model name; *сколько стоят* - bold Outlet Price + chip "Save"; *чем отличаются* - badge condition + spec line. Больше в карточке ничего не нужно.

**Убрано из карточки:** скрытая надпись "Available" (сидит в каждой из карточек, информации не несет); unit number из заголовка ("#008" etc.); конфигурация из title ("Base Pkg + No Freezer + Caster Wheels" -> в атрибуты); подчеркивание цены; H2 на title (-> H3).

**Фото 4:3, а не квадрат:** текущие карточки 340×265 уже почти 4:3, телефон снимает 4:3 нативно, а байк - длинный объект: квадрат режет переднее колесо или уменьшает байк до ~60% плитки.

### Порядок и группировка

**Правило: In stock -> тип (Bikes, затем Carts) -> Outlet Price по возрастанию.** Condition - badge, а не ключ сортировки; Reserved (deposit hold, §6 п.7) после In stock; Sold - в конец.

| # | Группа / юнит | Condition | Outlet | Regular сегодня | Save |
|---|---|---|---|---|---|
| 1 | **Bikes** - Vending Bike (Black) / Electric | New / previous gen | $4,199 | $4,899 | $700 |
| 2 | Ice Cream Bike (Yellow) / Non-electric / Freezer | New / previous gen | $4,699 | $5,399 | $700 |
| 3 | Ferla Mini / Electric / Foldable frame / #015 | Like new / 2 mi | $4,999 | $5,499 | $500 |
| 4 | Promo Bike (White) / Electric / #009 | Used | $5,399 | $6,399 | $1,000 |
| 5 | Promo Bike (White) / Electric | New / previous gen | $5,799 | $6,799 | $1,000 |
| 6 | Last Mile Delivery Bike / Prototype / #007 | Like new | $8,399 | $10,399 | $2,000 |
| 7 | Ferla X (Wood) / Electric / Freezer | New / previous gen | $8,999 | $9,999 | $1,000 |
| 8 | **Carts** - Grande Cart (White) / Caster wheels | New (поколение уточнить) | $4,799 | $5,799 | $1,000 |
| 9 | Metal Cart / Sink / Cash drawer / #008 | Like new | $6,399 | $7,399 | $1,000 |

### Sorting / filtering - решение

На 9 SKU не нужны UI sort / filter. Порядок пока автоматический. После >12 юнитов - sort dropdown (price по возрастанию по умолчанию / по убыванию / biggest saving); >20 - плашки-фильтры над сеткой (Type / Condition / Electric) и count "24 units"; >40 - sidebar и "load more" по 12. Атрибуты заведены, каждый шаг - настройка виджета.

### Mobile (390px)

| Блок | Поведение |
|---|---|
| H1 + count-строка | Строка переносится на две; это первое, что видно после header |
| 3 icon-tile | В один столбец, по одной строке текста на плитку |
| Condition | Одна строка текста с переносом, без интерактива |
| Grid | 1 колонка, карточка целиком; без кнопки |
| Lifestyle-band | Две кнопки full-width друг под другом |
| Recently sold | Горизонтальный scroll |
| Buying guide | Collapsed по умолчанию |

### Правила стиля Ferla:

- Шрифт VisbyRoundCF, sentence case; белый фон, текст #111, вторичный #7A7A7A, панели #F5F5F5 с чередованием белый -> серый -> белый.
- Радиусы: карточки 16px, кнопки 8px, pills 999px; контейнер и боковые отступы - те же переменные темы.
- Красный #F73821 (hover #DB2C16) - только одна primary CTA в viewport; secondary - белая; бейджи - белые pill с текстом.
- Cуществующие паттерны: 3 icon-USP, centered H2, Nested Accordion, "Explore Other Configurations", lifestyle-band с затемнением.
- Не используем: uppercase, черные боксы и кнопки, цветные бейджи.

## 2. Предлагаемая структура Outlet product page

Две колонки на белом: слева 60% галерея, справа 40% sticky buy column. Outlet-юнит - один SKU без вариантов; "варианты" - это cross-sells "Complete your setup" и "Ask about this unit".

### Верхний блок

| # | Элемент | Содержание | Визуальный вес |
|---|---|---|---|
| 1 | **Product** | Eyebrow "Outlet / Unit #015 / 1 in stock" + H1 "Ferla Mini (previous gen) - Black, Electric" (несколько слов). Title: Ferla {Model} ({generation}) - {Color / Drive / Key add-ons} - Outlet #NNN | H1 32-44px / 700; eyebrow 13px, #7A7A7A |
| 2 | **Images** | Hero 4:3 + thumbnail, swipe на mobile; фото дефектов в той же галерее с подписью "Condition". Стандартный Product Images виджет вместо пары Image + Gallery - убирает дубль фото и вертикальную простыню | 8-12 фото, hero ~100 KB WebP |
| 3 | **Outlet Price** | "$4,999" | 32-36px / 700, #111 - самый тяжелый элемент колонки |
| 4 | **Original Price** | На странице - "Configured new ~~$5,499~~" + строка-разбивка "= Ferla Mini $4,499 + Pedal Assist + Foldable Frame". Показывается при подтвержденном якоре, иначе - "Compare: new Ferla Mini from $4,499" | 16px, #7A7A7A, strikethrough |
| 5 | **Discount** | Chip "Save $500 (9%)" - доллары первичны | Pill #fdecea / #F93922, 13-14px / 600 |
| 6 | **Condition / Outlet info** | Панель #F5F5F5, радиус 12px: why outlet (demo / previous generation / prototype) / Inspected / Final sale. + "Ships in N business days / flat $1,200 crate / or pick up" | 15px, 50 - 100 слов |
| 7 | **Основные характеристики** | Сетка label / value: Drive / Range / Color / Add-ons / Generation / Mileage / hours. | 13px label / 15px value |
| 8 | **CTA** | Пара кнопок на всю ширину колонки: красная primary + белая secondary. Дефолт: primary "Buy now" (checkout, где $1,200 и tax уже посчитаны), secondary "Reserve / Ask about this unit" (заявка с депозитом или звонок). Если через корзину проходят единицы заказов, меняем их местами. Под кнопками: "or call +1 (213) 291-9070" и trust line "Flat $1,200 crate shipping / Arrives assembled / Final sale / Financing via Click Lease ->" | Один красный элемент |

### Нижние секции

Все ниже trust line - Nested Accordion; на desktop первые две секции открыты.

| Секция | Содержимое | Лимит |
|---|---|---|
| Overview | Один абзац про этот юнит (что, почему в outlet, кому) + 3 "Best for" | 100 слов + 3 буллета |
| Condition report | Чек-лист frame / motor & battery / brakes / tires / electrics / body / doors & locks / sink & tanks со статусом OK / note, дефекты со ссылками на фото, дата инспекции | ~100 слов + список |
| What's included | Base, add-ons, battery + charger, keys. Заменяет "Included Add-Ons" grid | ~10 буллетов |
| Specifications | Таблица из атрибутов: dimensions, weight, load, motor, battery, range, tank capacity | ~10 строк |
| Compare with new | 3 колонки: этот юнит / новая base-модель / новая в той же конфигурации - price, condition, lead time ("ships now" vs "2-4 weeks"), warranty | ~5 строк |
| Complete your setup | Cross-sells по ценам каталога: Standard Canopy $200, Solar Package $1,000, Freezer 70L $749 / 230L $1,149, Sink $129 + Water pump $189, Battery $599 (Mini / Ice Cream / Vending) или $799 (Ferla X) | 3-4 карточки |
| Shipping & pickup / Payment & returns / Warranty | Глобальные шаблоны: $1,200 per crate, arrives assembled, canopy disassembled, ships in N days, pickup; способы оплаты (card / ACH / Click Lease / PO - §6); CA sales tax при доставке или pickup в Калифорнии, tax-exempt - сертификат; "Outlet sales are final"; warranty текст | ~50 слов каждая |
| FAQ | Несколько Q&A: что значит previous generation, можно ли посмотреть вживую, можно ли добавить опции, повреждения | 10 - 20 слов на ответ |
| Reviews + Other outlet units | Site Reviews по родительской модели; 3 карточки того же компонента | - |

Итого описания на юнит не 5 - 500 слов, а ~250 (появляется шаблон).

**Убрано с PDP:** SEO-эссе из H3; quantity selector; форма "Talk To Sales" с "How did you hear about us?" (-> popup "Ask about this unit" с pre-filled unit и чекбоксами "financing quote" / "inspect in person"); generic "Included Add-Ons" grid (дублирует буллеты); дубль featured image. Блок "ANALYZE THIS PAGE WITH AI" (5 внешних ссылок над футером) - если это эксперимент по AI-visibility - переносим ниже CTA.

| Живет один раз на listing (и линкуется с каждой PDP) | На каждой PDP |
|---|---|
| Что такое Ferla Outlet; определения трех грейдов; warranty terms; final-sale policy; shipping $1,200 / assembled / pickup / international on request; financing; общий FAQ; архив Sold | Grade + причина попадания в outlet + mileage / hours; дефекты с фото и датой инспекции; ships-in days; исключения по warranty для этого юнита; состояние Sold |

### Грейды и правило якоря

Три грейда вместо пяти написаний; поколение и "прототип" - отдельные поля, а не грейды.

| Grade | Badge | Определение для покупателя | Обязательные поля | Юниты сегодня |
|---|---|---|---|---|
| New | "New" / "New / previous gen" | Never used; поколение указано явно | Generation; "what differs from current" | Vending Black, Ice Cream Yellow, Promo White, X Wood; Grande Cart White (поколение уточнить) |
| Like new | "Like new / demo" | Used by Ferla for demos, events or photos; inspected and serviced; minor cosmetic marks | Mileage / hours; marks + фото | Mini #015, Metal Cart #008, Last Mile #007 (+ флаг Prototype) |
| Used | "Used" | Previously operated; inspected and serviced; wear documented | Mileage / hours; wear-фото | Promo #009 |

**Правило якоря.** Original Price - зачеркнутой только если она воспроизводима как "текущий Starts At + текущие цены add-ons за эту конфигурацию". Иначе - Outlet Price, "Compare: new Ferla X from $6,499" и причина разницы (freezer, pedal assist, wood finish). Слово на странице - "Configured new", а не "Original price": strikethrough - это заявление о прежней цене, за которой должен стоять документ. Для прототипа #007 strikethrough не ставим - максимум "Comparable new build from $X".

## 3. Что убрать / изменить на текущих страницах

| Элемент | Сейчас | Действие | Почему |
|---|---|---|---|
| Порядок юнитов | WordPress default (date desc); bikes и carts вперемешку | Query "stock -> type -> price asc" | Порядок должен читаться как прайс-лист, а не как лог загрузок |
| H1-полоса | Почти черная (#181818) с H1 "Outlet Bikes" | Белый блок, H1 + count-строка | Единственный темный блок в верхней части страниц |
| "Available" | Скрытый статус в DOM | Удалить, вместо него badge condition + "1 unit" | Ноль информации |
| Цены в карточке | Обе 20px серые, outlet подчеркнута | Outlet 22-24px / 700 #111; "Configured new" 15px strikethrough; плашка "Save $X" | Покупатель не должен вычитать; новая цена - самый заметный текст |
| Title | "Grande Cart (White) - Base Pkg + No Freezer + Caster Wheels" - 3 строки | Правило: Ferla {Model} ({gen}) - {Color / Drive / Add-ons} - Outlet #NNN; конфигурация -> атрибуты; "No Freezer" -> Not included | Одна строка, ровная baseline по ряду |
| Фото | Разные пропорции; склад / коробки / фургон; заглушка у Ice Cream Bike (Yellow); один кадр у Promo #009 и Promo White; featured дублируется в галерее | Для каждого юнита один фронтальный кадр (три четверти), кроп 4:3 (800×600); Ice Cream Yellow - телефонная съемка у стены, затем фотосессия | Единая фото-рецептура; плейсхолдер "ChatGPT-image" подрывает доверие ко всей сетке |
| SEO-статья | ~1000 слов; на mobile 50% высоты | Сollapsed accordion; рерайт в Ferla-specific + FAQ | Ценовые диапазоны ($800-$6,000, "40-70% savings") противоречат реальным $4,199-$8,999 и 9-19% |
| H1 / <title> | "Outlet Bikes"; title обещает "Coffee Cart" и "Cargo Bikes" | H1 "Ferla Outlet - Bikes & Carts"; Coffee Cart из title убрать | 2 из 9 - carts; coffee cart в outlet нет |
| PDP CTA | Черная, ниже первого экрана; quantity перед ней | Пара full-width; mobile sticky bar; quantity скрыт | Черная кнопка - не паттерн Ferla; CTA после эссе не работает |
| Описание PDP | ~520 слов (#015), ~370 (#008); 4-5 слов (#007, #009, Promo White) | Шаблон ~250 слов; short description (сейчас пустая) | Непоследовательность |
| "Shipping and Taxes not included" | Единственная политика без цифры; $1,200 узнается только на checkout (10-30% цены юнита) | Trust line "Flat $1,200 crate shipping / Arrives assembled / Final sale / or pick up" | Известная цифра пугает меньше, чем неизвестная |
| Галерея | Все изображения столбиком 550×400, featured повторяется | Product Images widget: hero + thumbnails + lightbox | Один виджет решает дубль и простыню |
| "Included Add-Ons" grid | Generic карточки с основного каталога, дублируют буллеты | Секция What's included / Not included | Убирает дубль и generic-текст |
| Condition | Свободный текст, 5 написаний | Атрибут condition, 3 значения + поля generation / prototype | Нет badge, порядка, schema |
| Stock | Не управляется; sold individually выключен на всех | Manage stock, qty 1, sold individually; Sold вместо удаления | Юнит нельзя продать дважды; проданный не должен исчезать вместе с indexed URL |
| Schema | Два Product entity (WooCommerce + Rank Math), оба NewCondition | Один entity; itemCondition из грейда; shippingDetails $1,200; MerchantReturnNotPermitted | Google показывает condition и shipping вместо голой цены |
| Talk To Sales | Collapsed accordion без product context | Popup с unit / SKU / price в hidden fields; телефон рядом с CTA | Лид без указания юнита |
| SEO titles PDP | Metal Cart: "Pre-owned metal vending cart for sale >> Explore used metal cart ..."; остальные "<name> - Ferla Bikes" | Шаблон "Ferla {Model} Outlet #NNN - {Condition} \| Ferla Bikes" в Rank Math | Один шаблон вместо ручных title |
| Нет warranty / lead time / shipping / final sale / financing | Ни на одной из 9 PDP | Info box + глобальные шаблоны (§2) | Это и есть "Condition / Outlet information" из ТЗ |
| Alt text / SKU / short description | Пусто на всех 9 | Заполнить по шаблону | Доступность, feeds, GA4 item_id |
| FAQ vs footer | FAQ: "visitors to our location in Azusa"; footer и schema: 1100 W 135th St, Gardena | Привести к одному адресу после ответа CEO | Покупатель проверит адрес |

## 4. Дополнительные UX улучшения

**Quick wins (без глобальных изменений шаблона).**

1. Stock qty 1 + sold individually на всех товарах; проданный юнит -> Sold, не удаление (сохранение impressions проданных URL в Search Console).
2. Плашка "Save $X" через shortcode в каталоге и на странице продукта. Listing -> PDP CTR растет, сделка видна без арифметики.
3. Пересортировка через menu_order (type -> price) (scroll depth до 9-й карточки).
4. Статья -> collapsed accordion; фото-правила (высота страницы, bounce на /outlet).
5. Строка текстом в описании трех юнитов дороже нового base - "Configured new: $5,499 = Ferla Mini $4,499 + ..." ("почему дороже нового").
6. Trust line "Flat $1,200 crate shipping / Final sale / In stock" под ценой (гипотеза: меньше брошенных checkout из-за сюрприза доставкой; метрика: begin_checkout -> purchase / заявка).
7. Baseline: snapshot GA4 и Clarity за 2-4 недели до релиза по воронке view_item_list -> select_item -> view_item -> add_to_cart -> begin_checkout -> purchase плюс заявки. Без этого "после" не с чем сравнивать.

**Основной релиз (§5).**

8. Шаблоны v2 со sticky column и mobile bar (гипотеза: CTA в первом экране поднимает PDP -> действие; метрика: view_item -> add_to_cart / заявка, клики sticky bar отдельным событием).
9. Condition report с фото дефектов и датой инспекции (время до продажи Like new / Used против New).
10. Popup "Ask about this unit" с pre-filled unit (метрика: лиды с item_id).
11. "Compare with new" (гипотеза: честный якорь снимает возражение "дороже нового"; метрика: exit с outlet PDP на main PDP той же модели).
12. "Complete your setup" (attach rate, AOV outlet-заказа).

**Спрос и распродажа.**

13. Карточка "This model in stock now" на main PDP - рядом со строкой "Custom builds typically take 2-4 weeks", ссылкой на конкретный юнит: "Ferla Mini #015 / $4,999 / ships now".
14. Одно письмо / SMS по незакрытым quote-лидам той же модели (GoHighLevel).
15. Google Merchant Center free listings с itemCondition из нового атрибута.
16. Правило уценки: 30 дней без продажи - ревизия цены, 60 - шаг уценки. Цель: все 9 проданы к согласованной дате.
17. Pickup как оффер: "Pick up at our warehouse and save the $1,200 crate fee" - на плитке листинга и в info box, с таргетом на покупателей из LA.

**После запуска.**

18. "Hold this unit - 48h, $500 deposit" (метрика: доля резервов -> покупок).
19. Публичный чек-лист "Ferla outlet inspection" + PDF на юнит.
20. Per-unit monthly figure от Click Lease вместо общей строки.

## 5. Implementation на staging и запуск

**День 0 - проверки "до":** (1) число заказов через корзину против quote-лидов за 12 месяцев -> модель CTA; (2) outlet-юнит через checkout на staging - начисляются ли $1,200 и tax (если корзина отдает $0 доставки, это утечка маржи); (3) Search Console: клики /outlet по "used cargo bike / coffee cart" -> судьба статьи; (4) где физически стоят юниты; (5) baseline GA4 / Clarity.

| Этап | Содержание | Дни |
|---|---|---|
| 0 | Backup, staging clone, Stripe test mode, отключить почту, "Discourage search engines", etc. | 1 |
| 1 | Модель данных, stock settings, child theme, shortcode для скидок, query hook, sold-individually, schema, filters, image 800×600 | 3 |
| 2 | Listing: Outlet Card v2 + Archive v2 (дубликаты текущих шаблонов; старые - rollback) | 2 |
| 3 | PDP: Outlet Product v2 (текущий single-product template продолжает работать в основном каталоге) | 3 |
| 4 | Фото (обработка, WebP, alt, regenerate) * 9 юнитов | 2 |
| 5 | SEO: статья, titles, schema validation, sold-policy | 1 |
| 6 | Миграция на live: код через git / SFTP, шаблоны через Elementor Export / Import, данные через WooCommerce CSV "update existing" | 1 |
| 7 | Dark launch: шаблон на скрытой категории с одним клоном, реальный тест оплаты, переключение condition, regenerate CSS, очистка WP Rocket + Cloudflare, QA | 1 |
| 8 | GTM / GA4 / Clarity: события, отчеты | 1 |

Итого ~15 рабочих дней.

**Что нужно:** build sheets / quotes по всем юнитам; warranty по грейду; решения из §6; доступы (админка, хостинг, репозиторий, Cloudways, Search Console, Cloudflare, etc.); емкость / габариты / вес по юнитам.

**KPI.** На 9 единичных SKU A/B-тесты и недельные конверсии - шум, поэтому главные метрики: дни до продажи по юниту; лиды по юниту; sell-through к дате; listing -> PDP CTR; Clarity. Прогнозы "+ x %" дать не могу.

## 6. Решения, которые нужны от CEO

1. **Три юнита дороже нового base** - X Wood ($8,999 vs new X $6,499 / Glacier $7,499), Mini #015 ($4,999 vs $4,499), Vending Black ($4,199 vs $3,499). Рекомендую: Like new / Used - ниже текущего "Starts At" модели; New (previous gen) - ниже той же конфигурации новой с разбивкой; минимальный разрыв между грейдами одной модели. Что не проходит - в "Ready to ship" на main PDP.
2. **Модель CTA и оплата.** Рекомендую primary "Buy now" + secondary "Reserve / Ask about this unit"; если корзина не будет востребованной - наоборот. Способы оплаты: card, ACH / wire, Click Lease, PO?
3. **Warranty по грейду.** Рекомендую: New - как на новую; Like new / Used - limited на motor / battery / electrics, cosmetics as-is; текст - от вас. На сайте warranty не нашел.
4. **Где стоят юниты и pickup.** FAQ говорит Azusa, footer - Gardena. Рекомендую pickup разрешить: экономия $1,200 больше, чем скидка.
5. **Build sheets и #007.** Есть ли build sheets / quotes по 9 юнитам? Для прототипа #007 - существовал ли $10,399 как реальная цена или quote? Если нет - продаем без strikethrough.
6. **Два Promo Bike** - Used $5,399 и New $5,799 с одним кадром: это два физических юнита или одна карточка лишняя? Разрыв 7% между used и new - сам по себе вопрос к цене.
7. **Deposit hold и операционные дефолты.** Рекомендую "Hold 48h, $500 deposit" - да; ships-in по умолчанию - 5 business days; дефект-чек-лист заполняет склад, sales только подтверждает.
8. **"$199/month".** Одна и та же цифра стоит на Mini $4,499, Vending $3,499, Grande Cart $4,999, Ice Cream $4,999 и X $6,499 - она не масштабируется с ценой. Рекомендую запросить у Click Lease per-unit figures, а до этого показывать "Financing via Click Lease ->" без суммы - и на outlet, и на основном сайте.
9. **"ANALYZE THIS PAGE WITH AI".** Что это - эксперимент по AI-visibility? Переносим ниже футерного CTA или убираем с outlet?
10. **Canopy.** Стандартный canopy входит в outlet-юниты или продается отдельно за $200? От этого зависит "What's included" на всех 9 PDP.

---

## Модель данных (заполнять в админке)

| Поле | Значения | Использует |
|---|---|---|
| Атрибут type | Bike / Cart | group header, порядок |
| Атрибут condition | New / Like new / Used | badge, info box, schema itemCondition |
| Атрибут drive | Electric / Non-electric | spec line, характеристики |
| Meta: unit number, generation (current / previous), prototype flag | текст / выбор | eyebrow, H1, badge, блок Prototype |
| Meta: mileage / hours, inspected on, ships in days | число / дата | info box, condition report |
| Meta: configured breakdown, anchor verified | текст / да-нет | Original Price показывается только при "да" |
| Meta: included / not included, reserved | списки / да-нет | What's included; Reserved после In stock |
| Статус | WooCommerce stock (in stock / sold) - без дублирующего атрибута | порядок, Sold-state, sticky bar |

## Процесс "новый юнит за 15 минут"

**На складе:** (1) номер из реестра, (2) odometer / hours и поколение, (3) included / not included, (4) дефекты по чек-листу, (5) фото на стандартном месте. **Sales:** build sheet.

**В WooCommerce:** (1) title по конвенции и SKU OUT-MINI-015, (2) атрибуты и meta, (3) цены по правилу якоря, (4) stock 1, (5) фото без дубля featured, (6) cross-sells, (7) Rank Math title, (8) preview на mobile -> publish -> строка в shared sheet.

**При продаже:** stock 0 + дата, юнит не удалять. **Sold-state:** карточка с badge 30 дней, PDP 90 дней (social proof, SEO), затем noindex + 301 на /outlet.

## Фотосессия

Owner - warehouse lead, один день, один backdrop (чистая стена), одна высота камеры, 4:3. Shot list на юнит: 3/4 front (cargo box слева, saddle справа), side, rear, interior, defects - 6 кадров, ~15 минут на юнит. Первый - Ice Cream Yellow.

## О прототипе

ferla_outlet_prototype.html - самодостаточный файл: переключатель Listing / Product page (Mini #015), адаптив (3 / 2 / 1 колонки), рабочие аккордеоны и sticky bar на mobile. Шрифт - Nunito как ближайшая свободная замена VisbyRoundCF; на сайте будет VisbyRoundCF. Фото - текущие складские, до съемки; Ice Cream Yellow показан плейсхолдером "Photo coming" вместо "https://ferlabikes.com/wp-content/uploads/2025/09/ChatGPT-Image-Oct-7-2025-08_28_09-PM.jpeg". Strikethrough "Configured new" в прототипе стоит на всех юнитах как целевое состояние после подтверждения build sheets.
