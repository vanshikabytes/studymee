
Executive Summary
Week 1 of the 3-month training program focused on establishing a strong conceptual foundation across three domains — Linux system navigation, relational database concepts with SQL, and Data Structures & Algorithms in Java. Since VM access for Oracle and PostgreSQL was not yet available, all database practice was carried out using db-fiddle.com (PostgreSQL) and Linux commands were practiced on Killercoda.com — both browser-based environments that provided hands-on experience without requiring local installation.

The week was productive. All four planned LeetCode problems were solved in Java, 15+ SQL queries were written and tested, and the key Linux command categories from Week 1's curriculum were covered in depth. Each day's learning was documented in a separate daily log with timestamps, error records, and self-ratings.

Key Win This Week: Understanding the why behind each concept — not just what the command does, but why it is designed that way. For example: understanding that HAVING exists because SQL's execution order runs WHERE before GROUP BY, so you need a separate clause to filter groups.
Topics Coverage Map
Domain	Topics Planned (Week 1)	Covered?	Depth
Linux	Filesystem navigation, file operations	✅ Yes	Thorough
Linux	File permissions (chmod, octal system)	✅ Yes	Thorough
Linux	Pipes, redirection (|, >, >>)	✅ Yes	Thorough
Linux	grep, find, file viewing (head/tail/less)	✅ Yes	Good
SQL	Relational DB concepts, data types, normalization	✅ Yes	Thorough
SQL	CREATE TABLE, Primary Key, Foreign Key	✅ Yes	Thorough
SQL	SELECT, WHERE, ORDER BY, GROUP BY, HAVING	✅ Yes	Thorough
SQL	JOINs (INNER, LEFT, RIGHT)	✅ Yes	Good
DSA	Big-O Notation, Time & Space Complexity	✅ Yes	Thorough
DSA	Arrays in Java (int[], ArrayList, 2D)	✅ Yes	Thorough
DSA	Strings, HashSet, HashMap in Java	✅ Yes	Good
DSA	4 LeetCode problems solved	✅ Yes	Thorough


Linux — What I Learned
1. Filesystem Navigation
Linux organizes everything as a tree starting from / (root). Practiced navigating using absolute paths (starting from /) and relative paths (relative to current location). Understood the difference between cd ~ (always goes home) versus cd .. (goes one level up).

# Key commands practiced pwd # tells you your current location ls -la # lists all files with permissions, hidden files included cd bootcamp/week1 # relative path cd /home/user/bootcamp # absolute path (same result)
2. File and Directory Operations
Created a complete project directory structure using mkdir -p. Practiced copying, moving, and deleting files. Learned the critical warning about rm -rf — it deletes permanently with no confirmation.

mkdir -p bootcamp/week1/linux bootcamp/week1/sql bootcamp/week1/dsa touch log.txt notes.txt echo "Day 1 notes" >> log.txt # append (safe) cp notes.txt notes_backup.txt mv notes.txt renamed_notes.txt rm notes_backup.txt
3. File Permissions (chmod)
Linux permissions are split across three entities — Owner, Group, Others. Each gets Read (4), Write (2), Execute (1) permissions. The values are added to get a single digit per entity.

# chmod [Owner][Group][Others] filename chmod 755 script.sh # 7=rwx, 5=r-x, 5=r-x — standard for executable scripts chmod 644 data.txt # 6=rw-, 4=r--, 4=r-- — standard for read-only files chmod 700 secret.sh # only owner can access — good for private scripts ls -l script.sh # verify: -rwxr-xr-x
4. Pipes and Redirection
The most powerful Linux concept this week. Redirection controls where output goes; pipes chain commands so the output of one becomes the input of the next.

# Redirection echo "start" > output.txt # OVERWRITE (dangerous — wipes old content) echo "line 2" >> output.txt # APPEND (safe — adds to end) # Pipes — chain multiple commands cat server.log | grep "ERROR" | wc -l # Read file → filter only ERROR lines → count them # Result: number of error entries in the log (real-world sysadmin use case!)
5. Searching and Viewing Files
# grep — search inside files grep "ERROR" app.log # basic search grep -i "error" app.log # case-insensitive grep -n "NullPointer" app.log # show line numbers grep -r "config" /etc/ # search inside all files in /etc/ # File viewing head -20 bigfile.txt # first 20 lines tail -20 bigfile.txt # last 20 lines tail -f app.log # LIVE follow — updates as new lines are added (key for server monitoring!) wc -l bigfile.txt # count total lines
Biggest Linux Insight: The pipe | is the foundation of how sysadmins analyze servers. Commands like ps aux | grep java | head -5 (find top 5 Java processes) show how you can build complex queries from simple building blocks. This is why Linux experts seem to do magic — they are just chaining simple commands together cleverly.


SQL — What I Learned
1. Core Concepts: Tables, Keys, Relationships
A relational database stores data in tables (rows + columns). Tables are linked using Primary Keys (unique row identifier) and Foreign Keys (references another table's PK). Created two linked tables and practiced inserting data.

-- Parent table created first (no dependencies) CREATE TABLE Department ( DeptID INT PRIMARY KEY, DeptName VARCHAR(50) NOT NULL, Location VARCHAR(100) ); -- Child table created second (references Department) CREATE TABLE Employee ( EmpID INT PRIMARY KEY, EmpName VARCHAR(100) NOT NULL, Salary DECIMAL(10,2) DEFAULT 30000, HireDate DATE NOT NULL, DeptID INT, CONSTRAINT fk_dept FOREIGN KEY (DeptID) REFERENCES Department(DeptID) );
2. SELECT Queries (15 Written This Week)
-- Basic retrieval SELECT * FROM Employee; SELECT EmpName, Salary FROM Employee ORDER BY Salary DESC; SELECT DISTINCT DeptID FROM Employee; -- Filtering SELECT * FROM Employee WHERE Salary > 70000; SELECT * FROM Employee WHERE EmpName LIKE 'R%'; SELECT * FROM Employee WHERE DeptID IN (1, 3); SELECT * FROM Employee WHERE Salary BETWEEN 50000 AND 85000; SELECT * FROM Employee WHERE HireDate > '2023-01-01'; -- Aggregation SELECT COUNT(*) AS TotalEmployees FROM Employee; SELECT MAX(Salary), MIN(Salary), AVG(Salary) FROM Employee; SELECT DeptID, COUNT(*) AS NumEmp, ROUND(AVG(Salary),2) AS AvgSal FROM Employee GROUP BY DeptID; -- HAVING (filter groups) SELECT DeptID, COUNT(*) FROM Employee GROUP BY DeptID HAVING COUNT(*) > 1; -- JOIN SELECT e.EmpName, e.Salary, d.DeptName, d.Location FROM Employee e INNER JOIN Department d ON e.DeptID = d.DeptID ORDER BY e.Salary DESC;
3. JOINs — Understanding the Differences
JOIN Type	What It Returns	When to Use
INNER JOIN	Only rows that match in BOTH tables	Most reports — you only want matched data
LEFT JOIN	All rows from left table + matched rows from right (NULLs if no match)	When you want to include employees with no dept
RIGHT JOIN	All rows from right table + matched from left	When you want depts with no employees
4. Database Normalization (1NF → 3NF)
-- 1NF: Each cell holds ONE value (no lists/arrays in a column) -- BAD: -- EmpID | Skills → violates 1NF (multiple values) -- 101 | Java, Python, SQL -- 2NF: No partial dependencies on composite primary key -- 3NF: No transitive dependencies (City shouldn't depend on ZipCode if ZipCode isn't the PK) -- GOOD (3NF): CREATE TABLE ZipLocation ( ZipCode CHAR(6) PRIMARY KEY, City VARCHAR(50), State VARCHAR(50) -- City/State now live in their own table );
Key SQL Insight: The order SQL executes a query is different from how you write it: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY. This is why you can't use a column alias from SELECT in a WHERE clause — WHERE runs before SELECT!


DSA in Java — What I Learned
1. Big-O Notation — Time & Space Complexity
Complexity	Name	Java Example	Verdict
O(1)	Constant	arr[i], HashMap.get()	✅ Ideal
O(log n)	Logarithmic	Binary Search	✅ Great
O(n)	Linear	Single for-loop	✅ Acceptable
O(n log n)	Log-linear	Arrays.sort()	⚠️ OK for most
O(n²)	Quadratic	Nested loops	⛔ Avoid for large n
2. Java Syntax for DSA (Key Patterns Used This Week)
// HashSet — O(1) lookup, used to track "seen" elements HashSet<Integer> seen = new HashSet<>(); seen.add(5); seen.contains(5); // true — O(1) // HashMap — O(1) key-value storage HashMap<Integer, Integer> map = new HashMap<>(); map.put(num, index); map.getOrDefault(key, 0); // safe get with default value // Character to index trick (used in Valid Anagram) int[] count = new int[26]; count[ch - 'a']++; // 'a'-'a'=0, 'z'-'a'=25
3. Problems Solved — Full Explanation
#217 — Contains Duplicate
O(n) timeO(n) space
Problem: Return true if any value in the array appears more than once.

Approach: Use a HashSet. For each element, if it already exists in the set → duplicate found. Otherwise, add it. One pass through the array = O(n).

public boolean containsDuplicate(int[] nums) { HashSet<Integer> seen = new HashSet<>(); for (int num : nums) { if (seen.contains(num)) return true; seen.add(num); } return false; }
#1 — Two Sum
O(n) timeO(n) space
Problem: Find two numbers that add to target, return their indices.

Approach: HashMap of (value → index). For each element x, check if (target - x) already exists in the map. If yes, we found our pair.

public int[] twoSum(int[] nums, int target) { HashMap<Integer, Integer> map = new HashMap<>(); for (int i = 0; i < nums.length; i++) { int complement = target - nums[i]; if (map.containsKey(complement)) return new int[]{map.get(complement), i}; map.put(nums[i], i); } return new int[]{}; }
#242 — Valid Anagram
O(n) timeO(1) space
Approach: Use a 26-element int array (one per letter). Increment for each char in s, decrement for each char in t. If all zeros at the end → anagram.

public boolean isAnagram(String s, String t) { if (s.length() != t.length()) return false; int[] count = new int[26]; for (int i = 0; i < s.length(); i++) { count[s.charAt(i) - 'a']++; count[t.charAt(i) - 'a']--; } for (int c : count) if (c != 0) return false; return true; }
#121 — Best Time to Buy & Sell Stock
O(n) timeO(1) space
Approach: One pass — track the minimum price seen so far. At each step, calculate profit as (current price - min price) and update maxProfit if better.

public int maxProfit(int[] prices) { int minPrice = Integer.MAX_VALUE, maxProfit = 0; for (int price : prices) { if (price < minPrice) minPrice = price; else if (price - minPrice > maxProfit) maxProfit = price - minPrice; } return maxProfit; }
Pattern Recognition Learned: Most Week 1 problems follow 1 of 2 patterns: (1) "Have I seen this before?" → Use HashSet. (2) "What is the complement/pair needed?" → Use HashMap. Recognizing these patterns early makes future problems much easier.
