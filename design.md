# Political Tendency App - Design Document

---

## Purpose
Build a single-page fun quiz app that asks users fundamental moral/philosophical questions to infer their political tendencies. Instead of directly asking political questions, the app subtly reveals user leanings across multiple ideological dimensions. At the end, users will see their profile visualized with a football-player-style radar (spider) chart.

---

## Core Concepts

- **Non-direct questions**: No explicit political party or ideology mentions.
- **Subtle moral and societal questions**: Challenge users' views on leadership, tradition, security, etc.
- **Simple answer choices**: Each question has 3 options: **Yes**, **No**, **Neutral**.
- **Non-negative scoring**: Only positive weights are assigned to attributes.
- **Balanced dimensionality**: Every question should affect an even number of attributes (4 or more), focusing on opposing pairs.
- **Question pools per step**: Each step (question slot) will have a pool of possible questions, and the app will randomly pick one for each step. All questions in a given step pool must impact the same attribute pairs to maintain scoring balance.

---

## Key Political Dimensions

Opposing axes (each forming a pair):

| Dimension A         | Dimension B       |
|:--------------------|:------------------|
| Authority           | Liberty            |
| Security            | Openness           |
| Traditionalism      | Progressivism      |
| Nationalism         | Globalism          |
| Collectivism        | Individualism      |
| Equality            | Meritocracy        |

These 12 traits can be visualized on a 6- or 12-spoke radar chart.

---

## Question and Answer Structure

### Questions
- 8 total steps (questions asked).
- Each step pulls one question randomly from a **step-specific question pool**.
- Each question covers 4 (or more) attributes.
- Cross-dimensional design: a single answer impacts multiple traits.

### Answers
- Three choices: **Yes**, **No**, **Neutral**.
- Each choice has a predefined, non-negative weight per attribute.
- **Yes** boosts one side of each pair; **No** boosts the opposite side.
- **Neutral** lightly boosts both sides equally.

### Improved JSON Structure
```json
{
  "1": {
    "questions": [
      {
        "question": "Strong leaders should have the power to override courts in emergencies.",
        "answers": {
          "Yes": {
            "Authority": 3,
            "Liberty": 0,
            "Security": 3,
            "Openness": 0
          },
          "No": {
            "Authority": 0,
            "Liberty": 3,
            "Security": 0,
            "Openness": 3
          },
          "Neutral": {
            "Authority": 1,
            "Liberty": 1,
            "Security": 1,
            "Openness": 1
          }
        }
      }
      // ... more questions for step 1
    ]
  },
  "2": {
    "questions": [
      // questions for step 2 impacting another set of attributes
    ]
  }
  // and so on for each step
}
```

---

## Scoring and Calculation

### Per Question:
- Based on user selection, add the corresponding attribute scores.

### After All Questions:
- Sum total scores for each attribute.
- Normalize scores to a 0-100 scale:

```
Normalized Score = (User Score / Maximum Possible Score) * 100
```

- Each attribute is normalized **independently**.

### Visualization:
- Display final results on a **radar chart**.
- One spoke per attribute (or per opposing dimension pair).
- Smooth animation of chart reveal after quiz completion.

---

## Additional Notes

- **Question shuffling**: Within step-specific pools, randomize question selection.
- **Balance per step**: Ensure each pool targets the same dimension pairs.
- **Wildcard questions**: Future expansion could include "spice" questions.
- **Extensible**: Easily update/add new questions without major system changes.

---

## Next Steps

- Write 8+ question pools, each pool focused on a consistent set of attribute pairs.
- Build frontend components: question card, answer buttons, result chart.
- Connect score calculation engine to final chart visualization.
- Polish UX: transitions, hover effects, progress bar.

---

# End of Design Document

---

