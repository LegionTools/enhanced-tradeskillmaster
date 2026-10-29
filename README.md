# TradeSkillMaster (Enhanced)

An enhanced fork of TradeSkillMaster's core addon. All credit for the original addon goes to the TradeSkillMaster team; this fork fixes an auction house query bug specific to this server's emulated API.

## What changed

- **Fixed: Normal-mode auction house name search returning zero results.** `QueryAuctionItems`'s `qualityIndex` argument was being sent `query.quality` literally, including its default value of `0` for "no quality filter" — the convention used everywhere else in this codebase (e.g. `AuctionTab_Util.lua`'s own record-validity check treats `quality == 0` as unfiltered). On this server, a literal `qualityIndex` of `0` is apparently treated as an actual filter for Poor-quality items rather than as a "no filter" sentinel, so any plain name search — which always defaults quality to `0` — came back with zero results server-side, even though an identical search in Blizzard's own default AH Browse tab found matches fine. Confirmed via diagnostic logging that the zero result was a deterministic, repeatable server response (not a scan-timing race), consistent across both English and Cyrillic search terms. The fix omits the quality constraint entirely when none was requested, matching the "`0` means unfiltered" convention already used elsewhere in this codebase.

## Installation

Drop this folder into `Interface/AddOns/TradeSkillMaster`, replacing the stock core addon.

---

# TradeSkillMaster (улучшенная версия)

Улучшенный форк базового аддона TradeSkillMaster. Вся заслуга за оригинальный аддон принадлежит команде TradeSkillMaster; этот форк исправляет ошибку в запросах к аукциону, специфичную для серверного эмулятора этого API.

## Что изменено

- **Исправлено: поиск по названию на аукционе в режиме Normal возвращал пустой результат.** Аргумент `qualityIndex` функции `QueryAuctionItems` получал значение `query.quality` буквально, включая его значение по умолчанию `0`, означающее "без фильтра по качеству" — это соглашение используется везде в этом коде (например, проверка валидности записи в `AuctionTab_Util.lua` трактует `quality == 0` как отсутствие фильтра). На этом сервере буквальный `qualityIndex`, равный `0`, по всей видимости трактуется как настоящий фильтр "только предметы Плохого качества", а не как признак "без фильтра" — поэтому любой обычный поиск по имени (который всегда оставляет качество равным `0`) возвращал ноль результатов на стороне сервера, хотя тот же поиск через стандартную вкладку "Обзор" аукциона Blizzard находил совпадения без проблем. Через диагностический вывод было подтверждено, что нулевой результат — детерминированный, воспроизводимый ответ сервера (а не гонка состояний при сканировании), одинаково для запросов на английском и кириллице. Исправление полностью не передаёт ограничение по качеству, если оно не было запрошено, в соответствии с уже принятым в этом коде соглашением "`0` значит без фильтра".

## Установка

Поместите эту папку в `Interface/AddOns/TradeSkillMaster`, заменив штатный базовый аддон.
