# Murshid Mini — Renad

My project for the course **LLM Application Engineering (SDA-AIE-213)**
Instructor: **Meaad Al-Marri** · [SDAIA Academy](https://github.com/SDAIAAcademy)

## About the project
Murshid is a small, fictional assistant that answers simple questions about a service.
It uses `FakeClient`, which gives fixed answers. **It is not a real AI model** (mode = mock).
All data is fake (synthetic).

## Files
- `notebooks/murshid.ipynb` — the main project (I add to it every day)
- `homework/day1_real_model.ipynb` — Homework 1
- `homework/day2_tools.ipynb` — Homework 2
- `results/` — the JSON files from my runs

## Software structure

### Folders
```
murshid-renad/
├── README.md              # this file
├── notebooks/
│   └── murshid.ipynb      # main project (one notebook, all 4 days)
├── homework/
│   ├── day1_real_model.ipynb
│   └── day2_tools.ipynb
└── results/
    ├── core/              # JSON results from the main project
    │   ├── baseline_results.json
    │   └── day2_results.json
    └── day1/
        └── live_demo_result.json
```

### Parts of the program
| Part | Cell | What it does |
|---|---|---|
| `SERVICE_DIRECTORY` | 1 | Fake service answers (Arabic + English) |
| `FakeClient` | 1 | Pretends to be the AI model. Returns fixed answers |
| `save_json` | 1 | Saves results to a JSON file |
| `ask()` | 2 | Checks the question is not empty, then calls `FakeClient` |
| `RECORDS` / `SESSION` | 4 | Fake applications and the test user |
| `extract_ticket()` | 4 | Turns a question into a ticket (service, city, reference) |
| `validate_ticket()` | 4 | Checks the ticket is correct |
| `check_status()` | 5 | Read-only tool. Checks the owner, then returns the status |
| `route()` | 5 | Decides what to do with each question |

### How a question moves
```
Question → route() → refuse "cancel"?
                   → extract_ticket() → validate_ticket()
                   → no reference?  → ask for the reference number
                   → status request → check_status() → is it the user's? → answer
                   → service question → FakeClient → answer
```

## Days
- Day 1 ✅
- Day 2 ✅
- Day 3
- Day 4

## How to run
Open the notebook in Google Colab, choose CPU, and run the cells in order.

## Day 1 results
| Question | Answer |
|---|---|
| كيف أتابع طلبي؟ | يمكنك متابعة حالة طلبك باستخدام رقم المرجع. |
| How can I check my application status? | You can check your application status using its reference number. |
| ساعدني | ما الخدمة التي تحتاج مساعدة بشأنها؟ |
| (empty) | Please enter a valid question. |

All checks passed. The empty question is rejected before calling `FakeClient`.

## Homework 1 results
I ran a real small model (`google/flan-t5-small`) on CPU.
- Question: How can I check my application status?
- Answer: **Use its reference number** ✅ (correct, nothing made up)
- Note: the original install cell failed, so I installed the newer versions of the libraries.

**Difference:** the real model *creates* the answer from tokens. `FakeClient` only returns a saved answer.

## Day 2 results
Murshid now turns a question into a **ticket**, checks it, and then uses a **read-only tool** (`check_status`) to look up a fake application.
The user is `demo-user-1`.

| Question | Result | Tool calls |
|---|---|---|
| ما حالة طلبي REF-1001 في الرياض؟ | under_review | 1 |
| أريد حالة طلبي (no reference) | needs_clarification → ما رقم مرجع الطلب؟ | 0 |
| ما حالة REF-9999؟ | not_found | 1 |
| ألغ طلبي REF-1001 (cancel) | unsupported_action → nothing was changed | 0 |
| ما حالة REF-1002؟ (another user's request) | access_denied → status not shown | 1 |

All checks passed. The Day 1 question still works, and the records did not change.

## Homework 2 results
A small dispatcher that checks a tool call before running it: **7 / 7 cases passed**, records unchanged.
- Only the `check_status` tool is allowed. Other tools are refused.
- Wrong arguments or a wrong call shape are refused.
- Another user's request returns `unavailable`.

**Validation vs. authorization:** validation checks that the request is written correctly. Authorization checks that this user is allowed to see the result.

## Limitations
- `FakeClient` does not understand language. It only knows exact questions.
- Passing the tests does not mean it is a smart AI.
- All data is fake.
- `demo-user-1` is a test user, not a real login system.
