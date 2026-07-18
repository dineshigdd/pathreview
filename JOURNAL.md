## Week 7 — Issue selection

**Issue link:** (https://github.com/ascherj/pathreview/issues/148)

**Issue title:** Skill extractor fails to detect JavaScript and TypeScript

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The issue is located in the `extract_skills()` function within `ingestion/parsers/skill_extractor.py`. The function takes a text string as input for analysis. Currently, it fails to identify JavaScript-related work, and it fails to properly recognize TypeScript keywords or associated file extensions (.tsx/.ts). In the case of TypeScript, it incorrectly attributes the skills only to `React` rather than identifying the language itself. Essentially, the language detection for the JavaScript/TypeScript family is broken. 
I exectuted the tests and found that five tests are failing 
```bash
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_text_with_typescript_files - assert False
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_database_technology_detection - UnboundLocalError: cannot access local variable 'skill_names' where it is not associated with a value
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_devops_tool_detection - assert False
FAILED tests/unit/test_skill_extractor.py::TestSkillExtractor::test_javascript_detection - assert False
```

A successful fix would restore the ability of the SkillExtractor to accurately parse and categorize JS/TS-related content. Specifically, it would accomplish the following:

- **Comprehensive Detection:** Enable the function to recognize and return "JavaScript" as a detected skill when the input text mentions JS-related terminology or file extensions (e.g., index.js).

-  **Correct TypeScript Attribution:** Ensure that TypeScript keywords and files (e.g., .tsx, .ts) are correctly identified as "TypeScript" rather than defaulting only to "React.

- **Unit Tests:** The fix must successfully pass the specified unit test cases: `test_javascript_detection`, `test_text_with_typescript_files`, `test_devops_tool_detection`, and `test_docker_compose_detection`.

**Is this issue right for me:**  

- **Part 1 — Understanding the Issue**  
    As described in the problem summary section, I can explain the issue in my own words, locate the relevant files and functions affected, and clearly explain the expected outcome once the issue is fixed.

- **Part 2 — Tier Fit**  
    This is a localized issue that primarily affects the `skill_extractor.py` file. I chose a `Tier 1` issue because I have limited exposure to contributing to open-source projects, and this is the first AI-related codebase I have contributed to. The scope of the issue is a bug fix, and I am confident that my skill level and understanding of the subject are sufficient to resolve this issue.

- **Part 3 — Codebase Readiness**  
    I have found the specific parts of the codebase that cause the mentioned test cases to fail. I also looked into the test cases that pass. By comparing these tests, I have found the missing pieces of code that cause the test cases to fail. I believe at this point, I have sufficient contextual codebase knowledge to write a rough plan for fixing the issue.

-   **Part 4 — Scope and Time**  
    I checked the issue description and other related comments of this issue on the issue tracker. At the time of writing, several students are working on this issue. With my schedule, it might take me one to two weeks to complete, depending on the limited time available to me; however, I am confident I can make a PR before the deadline with relevant documentation. This issue has no open blockers or dependencies on other unresolved issues. It references only a PR that has failed to merge.

Therefore, with the above-mentioned reasoning, I decided to choose `issue #148` to be fixed.  

**Branch name:** fix/148-skill-extractor-fails

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger