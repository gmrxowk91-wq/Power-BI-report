### Database design and modelling

- Producing an **entity relationship diagram (ERD)** in **draw.io** to model tables, fields and the links between them. The worked example models a student results schema and resolves a many-to-many relationship explicitly:

| Table | Fields |
|---|---|
| `Student` | `id` (PK), `name` |
| `Class` | `class_id` (PK), `class_name` |
| `Results` | `result_id` (PK), `student_id` (FK), `class_id` (FK), `mark` |

A student can take many classes and a class holds many students — so `Results` sits between them as the linking table, carrying both foreign keys plus the attribute that only exists at the intersection (`mark`). That last point is the reason a linking table is not merely plumbing: a mark belongs to the *pairing* of a student and a class, not to either one alone.
