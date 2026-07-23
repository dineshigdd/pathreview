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

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/dineshigdd/pathreview/commit/d1bb46fb9fa2dfe219dda68c08766a321520b802

**Reproduction summary:**
Before reprodicing bugs, I analyzed the code in `skill_extractor.py` that detects JavaScript/TypeScript.

```python
 if ".js" in str(filename or "").lower():
            js_evidence.append("JavaScript file extension (.js)")
        if ".ts" in str(filename or "").lower():
            js_evidence.append("TypeScript file extension (.ts)")
        if re.search(r"\b(import|require)\s+", text):
            js_evidence.append("CommonJS or ES6 imports")
        if "package.json" in text_lower:
            js_evidence.append("package.json found")
```
In the above code, is `if re.search(r"\b(import|require)\s+", text):` the only code-level check that can trigger JavaScript detection? Therefore, any pattern that is not detected by the regex `\b(import|require)\s+` will not be counted as JavaScript. The following section shows four cases --with some edge cases-- in which JavaScript is not detected.


**Bug Reproduction Script:**
```python
from ingestion.parsers.skill_extractor import SkillExtractor

# Comprehensive Bug Reproduction Suite for JavaScript/TypeScript Detection
# This suite covers both standard JavaScript patterns and edge cases where 
# the current heuristics (file extensions, package.json, import/require regex) fail.

test_cases = [
    {
        "name": "Standard ES6 Modern Syntax (No extension, no import/require)",
        "code": """
        const calculateTotal = (items) => {
            let subtotal = 0;
            var taxRate = 0.05;
            return subtotal * taxRate;
        };
        """
    },
    {
        "name": "Traditional Function Declaration with Console Logging",
        "code": """
        function displayWelcomeMessage(username) {
            console.log("Welcome back, " + username);
        }
        """
    },
    {
        "name": "ES6 Class Definition without Module Imports",
        "code": """
        class ShoppingCart {
            constructor() {
                this.items = [];
            }
            addItem(item) {
                this.items.push(item);
            }
        }
        """
    },
    {
        "name": "Asynchronous Function Using Promise/Fetch Patterns",
        "code": """
        async function fetchData(url) {
            let response = await fetch(url);
            let data = await response.json();
            return data;
        }
        """
    }
]

e = SkillExtractor()

print("=== Running Skill Extractor Bug Reproduction Suite ===\n")
for index, test in enumerate(test_cases, start=1):
    detected_skills = e.extract_skills(test["code"])
    skill_names = [d.name for d in detected_skills]
    
    print(f"Test Case {index}: {test['name']}")
    print("Code Snippet:")
    print(test["code"].strip())
    print(f"Detected Skills: {skill_names}")
    print("-" * 50)

```
The following section shows the plan to fix the issue by detecting JavaScript/TypeScript for the optimal possible solution.

**PLAN.md link:** [link to PLAN.md in your fork]

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — recommended, not graded]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]
