# Repository Skills

Complete skill packages for repository-level agent work, one directory per skill under
`.agents/skills/<skill-name>/`, kept in their original form with their upstream license.

Skills here are local-only guidance: instructions the agent applies itself. Do not add skills
that run scripts calling external models or services, or that keep backup or validation state.

| Skill | Use it when | Source |
| --- | --- | --- |
| [`ponytail/`](ponytail/SKILL.md) | Simplifying a larger body of project guidance or code toward the minimum that works. | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) `skills/ponytail` @ `c982cd4`, MIT |
| [`ponytail-audit/`](ponytail-audit/SKILL.md) | Auditing a whole subsystem or body of guidance for over-engineering, as a read-only ranked report. | Same repository, `skills/ponytail-audit` @ `c982cd4`, MIT |
| [`caveman/`](caveman/SKILL.md) | Tightening agent-facing documentation to cut token count while keeping the necessary guidance. | [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) `skills/caveman` @ `6571943`, Apache-2.0 |
