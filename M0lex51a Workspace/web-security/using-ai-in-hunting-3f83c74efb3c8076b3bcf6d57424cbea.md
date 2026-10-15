# Using Ai In Hunting 3f83c74efb3c8076b3bcf6d57424cbea

## Using Ai In Hunting

## file structure

![image.png](<../.gitbook/assets/image (6).png>)

![image.png](<../.gitbook/assets/image 1 (4).png>)

### 1-AGENTS.md

1. scope lock : make ai focus on main target to not hunt another targets or third parties and if he will he should ask u
2. Skill gate : load specific skills when hunt on specific but not loading the whole skills when starting to not full the context window
3. FP gate : false positive gate —> when ai agent spot kind of finding he should ask himself 10 questions to validate the bug is it positive or false
4. evidence standard : to validate is the finding has impact or not
5.  Multi agent rules : to optimize the context widow we divide tasks on many agents every agent has his context window , it can done by customize every agent with specific rules such (main task , allowed tools , etc) , after every agent finished he will send the sorted output to main agent

    ![image.png](<../.gitbook/assets/image 2 (3).png>)
6. Exploitability gates : reach the the Hight impact of every finding
7.  workspace rules : save every output in files

    ![image.png](<../.gitbook/assets/image 3 (2).png>)

***

### 2-PRIORITIZATION.md

Rules to classify findings

1. p1—>p4 signal prioritization signal engine : rules to classify bugs from p1—>p4
2. Bypass ladder : to try bypass techniques before ignore finding
3. Chain board : chain more than one bug too reach high impact
4. Tested-variant ledger format : if ai ordered to test target had been tested before he retrieve previous test data so he can keep progress not to start form zero again
5. OOB workflow : for client vulnerability testin

### 3-agent-briefing.md

It describes a **closed-loop external knowledge / memory system** for an agent (apparently focused on security research / bug hunting):

* Real-world writeups are collected and processed offline into lessons, patterns, checklists, and an updated agent-briefing.md.
* The agent reads that briefing file at the start of every session (instant knowledge, no cold start).
* During a hunt it applies the knowledge, scores signals, saves findings, and receives triage feedback.
* Feedback updates a learning database, which eventually feeds the next rebuild of the briefing.
* Everything is kept outside the model weights (files + SQLite + a “Safety Gate”).

![image.png](<../.gitbook/assets/image 4 (1).png>)

### 4-OPENCODE.json

make ai can operate using browser by using mcp server
