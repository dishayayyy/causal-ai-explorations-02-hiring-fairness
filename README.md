# Causal AI Explorations — Part 2: Is Mandatory Overtime Causally Unfair?

> This is Part 2 of my Causal AI Explorations series, where I work through real datasets to build intuition for causal reasoning. Each entry picks a question that correlation alone can't answer.

→ [Part 1: Does High Blood Sugar Cause Heart Disease?](https://github.com/yourusername/causal-ai-explorations-01-healthcare)

---

## What I Wanted to Learn

Part 1 showed me how causal adjustment can deflate a spurious correlation. I wanted to explore the other direction — a case where the causal effect is real and survives adjustment — and add a fairness dimension to it.

Specifically, I wanted to understand:
- What counterfactual fairness means and how to test it empirically
- How to ask "what would have happened if this person were different"
- How causal reasoning goes beyond prediction into questions of equity

---

## The Question

**Does mandatory overtime causally drive employees to quit — and does it affect men and women equally?**

A counterfactually fair policy is one where, if you changed a person's gender while holding everything else constant, the outcome would be the same. If overtime pushes women out more than men (or vice versa), the policy is causally discriminatory even if no one intended it to be.

---

## What I Did

**Dataset:** IBM HR Analytics Employee Attrition (Kaggle) — 1,470 records

**Setup:**
- Treatment: Overtime assignment
- Outcome: Employee attrition
- Protected attribute: Gender
- Confounders: Age, job level, monthly income, job satisfaction

**Method:**
1. Built a causal DAG with gender included as a confounder
2. Estimated the overall ATE of overtime on attrition
3. Estimated the ATE separately for male and female subgroups to test fairness
4. Built a second model to test whether gender causally determines overtime assignment
5. Ran refutation tests on the main estimate

---

## What I Found

**Main result:**

| | Effect Size |
|---|---|
| Naive correlation | 0.200 |
| Causal ATE (after adjustment) | 0.207 |
| p-value | ~0 (6.5e-24) |

Unlike Part 1, the causal effect held up strongly after adjustment. Overtime genuinely causes attrition — a 21 percentage point increase in probability of leaving.

**Fairness result:**

| Group | Causal ATE |
|---|---|
| Female | 0.2069 |
| Male | 0.2069 |
| Fairness gap | 0.0000 |

The effect is identical across genders. The overtime policy is counterfactually fair.

**Overtime assignment:**

Gender does not causally determine who gets assigned overtime once job level and income are controlled for (p = 0.10). The raw gender gap in overtime rates was confounded.

---

## What Surprised Me

The fairness gap being exactly zero was unexpected. I went in expecting to find some differential effect — the EDA showed females were on overtime slightly more, which suggested possible unfairness. Causal analysis showed the opposite: the policy itself is fair, even if the raw numbers look uneven.

This was the clearest demonstration I've seen of why you need causal methods for fairness questions. Observational comparisons can mislead in both directions.

---

## Tools Used
- Python, Jupyter Notebook
- DoWhy, pandas, matplotlib, seaborn

---

## Series
This is Part 2 of Causal AI Explorations.

→ [Part 1: Does High Blood Sugar Cause Heart Disease?](https://github.com/yourusername/causal-ai-explorations-01-healthcare)
