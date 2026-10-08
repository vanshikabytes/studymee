DAY 2 — TUESDAY
October 7, 2026
Total study time today: ~4 hours

Topics Covered
🐧 File Operations 🔐 Linux Permissions 🗄️ SQL: CREATE & INSERT 💪 LeetCode #217
Time Breakdown
Linux file ops + chmod	~1.5 hrs
SQL: tables, keys, INSERT	~1.5 hrs
LeetCode #217 (Contains Duplicate)	~45 min
Updating notes & log	~15 min
What I Studied
Linux File Operations: Practiced creating folders with mkdir -p bootcamp/week1/linux — the -p flag creates parent folders automatically which is really useful. Practiced cp, mv, and rm. The rm -rf command is scary — it deletes without asking at all, need to be very careful.

chmod Permissions: This was the most confusing part of the day. The number system took time to sink in. Spent 30 minutes just practicing calculating permissions:
— 7 = 4+2+1 = read+write+execute
— 5 = 4+0+1 = read+execute (no write)
— 4 = read only
So chmod 755 = Owner gets everything, group and others can only read and run.

Mental model that helped: Think of permissions as a 3-digit padlock. Each digit (Owner, Group, Others) can be 0–7. Higher number = more access.
SQL Basics: Practiced on db-fiddle.com (PostgreSQL). Created Department and Employee tables with Primary Key and Foreign Key constraints. The order matters — you MUST create the Department table first before Employee, because Employee references it. Made that mistake and got an error that helped me understand FK constraints better.

LeetCode #217 Contains Duplicate: First attempted with a nested loop (O(n²)) — it worked but was slow. Then looked up that using a HashSet is O(n) and makes way more sense. Rewrote it with HashSet. Took about 45 minutes total.

Errors / Confusion Faced
Issue 1 — SQL: Got error "relation department does not exist" when creating Employee table. Root cause: I ran the CREATE TABLE Employee before running CREATE TABLE Department. Fixed by running in correct order. Learned that FK dependencies must be created in parent-to-child order.
Issue 2 — Java: When checking if HashSet contains an int from an int[], had a type mismatch. Had to use Integer (wrapper class) instead of int as the HashSet generic type: HashSet<Integer> not HashSet<int>.
Both resolved. Googled the Java one — found that Java's generics don't work with primitive types (int, double, etc.), you have to use wrapper classes (Integer, Double, etc.).
Key Takeaways
1. Always create parent table before child table when FK is involved.
2. chmod 755 is the standard for scripts you want to run but protect from editing.
3. HashSet is the go-to when you need to track "have I seen this before?" — it's O(1) lookup.
4. Java generics require wrapper classes (Integer, not int).

Plan for Tomorrow
— Linux: Pipes (|), Redirection (>, >>), grep
— SQL: SELECT with WHERE, GROUP BY, JOINs
— DSA: Two Sum (LeetCode #1) and Valid Anagram (#242)



DAY 3 — WEDNESDAY
October 8, 2026
Total study time today: ~4.5 hours

Topics Covered
🔗 Pipes & Redirection 🔍 SQL SELECT & WHERE 📊 GROUP BY & Aggregates 💪 LeetCode #1 Two Sum
Time Breakdown
Linux: pipes, grep, redirection	~1.5 hrs
SQL: SELECT, WHERE, GROUP BY, HAVING	~1.5 hrs
LeetCode #1 Two Sum (+ revision of #217)	~1 hr
Reading about normalization 1NF/2NF	~30 min
What I Studied
Linux Pipes & Redirection: This is where Linux starts getting really powerful. The pipe | concept is elegant — take output of one command and feed it directly into another without saving to a file in between.

Best example I tried: cat logfile.txt | grep "ERROR" | wc -l — reads a file, filters only ERROR lines, then counts them. Three commands chained into one line.
Key difference between > and >>: I kept forgetting this so I made a note — > is destructive (overwrites), >> is safe (appends). Always use >> when adding to logs.

SQL - SELECT, WHERE, GROUP BY: Wrote about 12 queries on db-fiddle. The part that confused me was the difference between WHERE and HAVING. Cleared it up:
— WHERE: filters individual rows BEFORE grouping
— HAVING: filters the groups AFTER GROUP BY runs

Also practiced aggregate functions — COUNT(), SUM(), AVG(), MAX(), MIN(). Ran a query to find which department pays the most on average. Felt like actual data analysis.

LeetCode #1 Two Sum: Spent 40 minutes on this. First tried brute force (O(n²) with nested loops) — it worked but too slow for large inputs. Then implemented the HashMap approach — for each number, calculate what complement is needed and check if it was already stored. This is O(n) and much cleaner.

Errors / Confusion Faced
Issue 1 — SQL: Wrote SELECT DeptID, COUNT(*) FROM Employee WHERE COUNT(*) > 2 GROUP BY DeptID — got an error. Learned that you can't use aggregate functions in WHERE clause. Must use HAVING for that: GROUP BY DeptID HAVING COUNT(*) > 2.
Issue 2 — Linux: When I did echo "hello" > file.txt twice with different text, realized the second one wiped the first. Took me a moment to realize I needed >>. That's a dangerous habit to break.
Both resolved. SQL error message was actually helpful — "aggregate functions not allowed in WHERE". Will remember the rule: WHERE → rows, HAVING → groups.
Key Takeaways
1. Pipe | chains commands — incredibly powerful for filtering and processing.
2. NEVER use > when you mean to append — always double-check.
3. WHERE filters rows, HAVING filters groups — completely different things.
4. Two Sum HashMap solution: for every element x, check if (target - x) is in the map.

Plan for Tomorrow
— Linux: grep flags (-i, -n, -r), find command, viewing files (head, tail, less)
— SQL: JOINs (INNER, LEFT) and Normalization 1NF/2NF/3NF
— DSA: Valid Anagram (#242) and Best Time to Buy/Sell Stock (#121)
— Start preparing Week 1 deliverable document

Self-Assessment (out of 5)
Linux Pipes: ⭐⭐⭐⭐  |  SQL Queries: ⭐⭐⭐⭐  |  Two Sum: ⭐⭐⭐⭐  |  Overall energy: Very Good


DAY 4 — THURSDAY
October 9, 2026
Total study time today: ~4 hours

Topics Covered
👁️ File Viewing Commands 🔗 SQL JOINs 📐 Normalization 1NF–3NF 💪 LeetCode #242 & #121
Time Breakdown
Linux: head, tail, less, grep advanced	~1 hr
SQL: INNER JOIN, LEFT JOIN, normalization	~1.5 hrs
LeetCode #242 Valid Anagram + #121 Stock	~1 hr
Week 1 deliverable prep + reviewing notes	~30 min
What I Studied
Linux File Viewing: Covered cat, head, tail, less, wc. The most useful one for real work seems to be tail -f — it follows a log file in real time, which is exactly what you'd do when monitoring a live server. Also practiced grep with flags:
— grep -i for case-insensitive search
— grep -n to show line numbers
— grep -r to search inside all files in a folder

SQL JOINs: Practiced INNER JOIN and LEFT JOIN on the Employee/Department tables I created on Day 2. The visual I drew helped: INNER JOIN is the overlapping middle of a Venn diagram, LEFT JOIN keeps everything from the left table even with no match.

Insight: If an employee has no DeptID (NULL), they appear in a LEFT JOIN with NULL for department columns — but they completely disappear in an INNER JOIN. That matters a lot for reports.
Normalization: Read and took notes on 1NF, 2NF, 3NF. 1NF is the simplest (atomic values, no lists in a cell). 3NF is the one that matters most in interviews — eliminate transitive dependencies. The ZipCode → City → State example made it very clear.

LeetCode Problems:
#242 Valid Anagram — Used a 26-element int array indexed by character position (char - 'a'). Elegant solution without a HashMap.
#121 Best Time to Buy/Sell Stock — Track minimum price seen so far and calculate profit at each step. Clean one-pass O(n) solution.

Errors / Confusion Faced
Issue 1 — SQL JOIN: Initially wrote FROM Employee JOIN Department without specifying INNER. Realized SQL defaults to INNER JOIN when you just write JOIN, but it's better practice to be explicit. Also forgot the ON clause once and got a cross join (cartesian product) — got 15 rows for 5 employees × 3 departments.
Issue 2 — LeetCode #242: My first approach failed on strings with different lengths — forgot to add the early return if (s.length() != t.length()) return false. Added it and all test cases passed.
Both resolved. The cartesian product bug was actually a good learning — now I understand WHY the ON clause is critical in JOINs.
Key Takeaways
1. tail -f is the real-world tool for live log monitoring — bookmark this.
2. INNER JOIN removes unmatched rows completely; LEFT JOIN keeps them with NULLs.
3. 3NF: if a non-key column determines another non-key column → separate it into its own table.
4. For anagram problems: subtract char frequencies instead of using a HashMap for simplicity.

Week 1 Reflection
Did not expect to cover this much in 4 days. The biggest challenge was SQL — writing syntactically correct queries without a working database (waiting for VM access). db-fiddle.com saved me here. DSA in Java is getting more comfortable — the key is understanding WHY the optimal solution works, not just memorizing it.

One thing I want to improve next week: spending less time debugging syntax errors and more time understanding algorithm logic. I should review Java basics more thoroughly before jumping into problems.

Self-Assessment (out of 5)
JOINs: ⭐⭐⭐⭐  |  Normalization: ⭐⭐⭐  |  LeetCode: ⭐⭐⭐⭐  |  Overall energy: Satisfied ✅




Oct 6 – Oct 9, 2026   (4 Days)
Total hours logged: ~16 hours

Hours by Subject
Subject	Hours	Topics Covered
🐧 Linux	~5.5 hrs	Navigation, File ops, Permissions, Pipes, Grep, File viewing
🗄️ SQL	~5.5 hrs	Tables, Keys, SELECT, WHERE, GROUP BY, JOINs, Normalization
☕ DSA Java	~4 hrs	Big-O, Arrays, HashSet/Map, 4 LeetCode problems
📝 Review/Logs	~1 hr	Daily log updates, note organization
Problems Solved This Week
#	Problem	Approach	Complexity
217	Contains Duplicate	HashSet lookup	O(n) time, O(n) space
1	Two Sum	HashMap complement	O(n) time, O(n) space
242	Valid Anagram	Char frequency array	O(n) time, O(1) space
121	Best Time to Buy/Sell Stock	One-pass min tracking	O(n) time, O(1) space
What Went Well
SQL clicked faster than expected. The visual of table relationships helped. db-fiddle.com was a great substitute while waiting for VM access. DSA — solving all 4 problems in under a week feels good. Big-O understanding is solid now.
What Was Challenging
Linux permissions (chmod) took the longest to internalize. The octal number system is not intuitive at first. Also, HAVING vs WHERE distinction in SQL confused me for almost half a day.
Focus for Week 2
→ Go deeper into Linux: text processing with awk, sed, writing bash scripts
→ SQL: Subqueries, Views, more complex JOIN scenarios
→ DSA: Linked Lists, Stacks, Queues — move beyond arrays
