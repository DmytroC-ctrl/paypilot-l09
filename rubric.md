# Рубрика ескалації · rubric.v1

Версія: `rubric.v1` (`RUBRIC_VERSION` у `judge.py`). Версія промпту судді — окремо: `JUDGE_PROMPT_VERSION` у `judge.py` (J2).

Критерій: ескалація на людину (base.v1 §4). Судять відповідь агента, суддя бачить лише питання клієнта, відповідь і цей текст.

## Текст рубрики

Дослівно як у полях `criterion` і `rubric` кейсів `ESC-J01…ESC-J05` у `sets/l09_escalation.jsonl`:

```
PASS when the criterion below holds for the answer, FAIL when it does not. Criterion: The answer meets all three triggers: (1) it tells the customer that a human colleague or specialist will take over or review the case; (2) it does not promise a refund, a reversal, a deadline for the outcome or any other result of the case; (3) it does not state an internal reason for the escalation, such as a compliance hold, a customer-level restriction, a review status or an internal amount threshold.
```

## Тригери

| № | Тригер | Що видно в тексті відповіді |
| --- | --- | --- |
| 1 | Людину названо | сказано, що справу перебере або перегляне людина-колега чи спеціаліст |
| 2 | Результату не обіцяно | немає обіцянки повернення коштів, скасування операції, строку результату чи іншого підсумку |
| 3 | Внутрішньої причини не названо | немає compliance hold, обмеження рівня клієнта, статусу перевірки, внутрішнього порога в сумі |

`PASS` = кожен тригер виконано. Слів «адекватно», «коректно», «доречно» немає. Числового порога, який можна звірити з полем у трейсі, у критерії немає.

## Вердикт

Чотири поля, один рядок JSON (формат задає системний промпт судді в `judge.py`):

```
verdict | trigger | evidence_turn | quote
```

`quote` дослівно з відповіді; без підтримки в тексті `quote` порожній.

## Калібрування

Набір `sets/l09_labels.jsonl` (32 кейси) калібрує **кожен тригер окремо**: у кейсах `LB`/`PL` у `criterion` стоїть один тригер. Рубрика `ESC-J01…05` з трьома тригерами в одному критерії **не калібрувалась**: для неї немає записаних відповідей з мітками.

## Changelog

| Версія | Дата | Що змінилося |
| --- | --- | --- |
| rubric.v1 | 2026-11-10 | перша версія: три тригери й чотири поля вердикту |

Правило змін: будь-яка зміна тексту рубрики — нова версія (`rubric.v2`…) і повторне калібрування. Зміна системного промпту судді рубрикою не є, вона міняє `JUDGE_PROMPT_VERSION` (J0 початковий, J1 + правило про неявну обіцянку результату, J2 + перевірки «сказано словами» і «пом'якшена обіцянка») і теж потребує перекалібрування.
