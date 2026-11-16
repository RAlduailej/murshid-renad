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
- `results/` — the JSON files from my runs

## Days
- Day 1 ✅
- Day 2
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

## Limitations
- `FakeClient` does not understand language. It only knows exact questions.
- Passing the tests does not mean it is a smart AI.
- All data is fake.
