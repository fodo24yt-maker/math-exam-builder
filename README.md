# math-exam-builder

A Claude plugin that prepares you for math exams. You give it your material (syllabus, notes, past exams) and it builds a realistic study path: what to study, in what order, and how to practice it with exams that look like the real one.

> **Status:** initial commit. Only the project structure and this README exist so far. The skills are not implemented yet.

## The idea

Studying for a math exam usually fails in one of three ways: you don't know what matters most, you don't know how to spread your time, or you never practice under exam-like conditions. This plugin tackles each of them with one skill:

| Skill | Question it answers |
|---|---|
| `visionary` | *What is most likely to appear on the exam?* |
| `plan-maker` | *What do I study each day, given my time and level?* |
| `exam-generator` | *How do I practice with something that feels like the real exam?* |

## The three skills

### 1. `visionary` (exam analyst)

Reads everything you provide (past exams, class notes, syllabus, any other material) and studies it to find patterns: which topics come up most often, how they are weighted, what types of problems the professor likes, and how difficulty is distributed. The output is a ranked list of what is most probable to be on the test and therefore most worth studying.

- **Input:** past exams, notes, syllabus, other course material.
- **Output:** topic priority ranking and an estimate of what the exam will look like.
- **Feeds:** `plan-maker` (what to prioritize) and `exam-generator` (what the exam should contain).

### 2. `plan-maker` (study planner)

Builds a day-by-day study plan. It takes into account:

- your starting point (what you already know),
- the level the exam requires,
- how much time you have until the exam and how much you can study per day,
- the priorities coming from `visionary`.

The plan concentrates effort on the most important topics and schedules practical exercises, including practice exams from `exam-generator`, so you apply what you learn instead of only reading it.

- **Input:** your current level, target level, exam date, daily availability, `visionary` output.
- **Output:** a day-by-day plan.
- **Feeds:** `exam-generator` (which topics to practice and when).

### 3. `exam-generator` (practice exams)

Generates exams that make sense for your course: based on the material you uploaded, the study plan, and the `visionary` analysis. The questions follow the style, difficulty and topic weighting expected on the real exam, and they focus on the part of the syllabus you are working on in the plan.

- **Input:** your material, the current study plan, `visionary` output.
- **Output:** a practice exam (and its solutions).

## How they fit together

```
          material (notes, past exams, syllabus)
                          │
                          ▼
                   ┌─────────────┐
                   │  visionary  │  what is likely to appear
                   └──────┬──────┘
                          │ priorities
                          ▼
                   ┌─────────────┐
                   │ plan-maker  │  day-by-day study plan
                   └──────┬──────┘
                          │ topics + schedule
                          ▼
                   ┌────────────────┐
                   │ exam-generator │  practice exams
                   └────────────────┘
```

`visionary` also feeds `exam-generator` directly so the generated exams mirror the probable real exam.

## Project structure

```
math-exam-builder/
├── .claude-plugin/        # Plugin manifest (plugin.json)
├── skills/
│   ├── exam-generator/    # Skill: generates practice exams
│   ├── plan-maker/        # Skill: builds the day-by-day study plan
│   └── visionary/         # Skill: analyzes material and predicts what matters
├── data/                  # Your input material
│   ├── syllabus/          # Course syllabus / official topic list
│   ├── notes/             # Class notes, books, summaries
│   └── past-exams/        # Previous exams (the most valuable input)
├── output/                # Everything the skills produce
│   ├── analysis/          # visionary reports
│   ├── plans/             # study plans
│   └── exams/             # generated exams and solutions
├── docs/                  # Design notes and documentation
├── LICENSE
└── README.md
```

Empty folders contain a `.gitkeep` file so Git tracks them; it can be deleted once the folder has real content.

## Roadmap

- [x] Project structure and README
- [ ] `visionary` skill
- [ ] `plan-maker` skill
- [ ] `exam-generator` skill
- [ ] Plugin manifest (`.claude-plugin/plugin.json`)
- [ ] Usage guide and examples in `docs/`

## License

See [LICENSE](LICENSE).
