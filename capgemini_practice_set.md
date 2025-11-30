# 🚀 Capgemini Practice Set — 20 Questions with Solutions

This file contains the 20 practice problems (5 SQL, 5 Hands-On Programming, 5 Front-End, 5 DSA/Logical) with detailed solutions and runnable code snippets where applicable.

---

## Table of Contents

1. SQL (5)
2. Hands-On Programming (5)
3. Front-End Simulator (5)
4. DSA / Logical (5)

---

# Section 1 — SQL (5 Questions + Solutions)

### SQL 1 — Fetch employee names & salaries where salary is above company average

**Problem:** Return `name`, `salary` for employees whose salary is greater than the average salary, ordered descending by salary.

**Solution (SQL):**

```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees)
ORDER BY salary DESC;
```

**Explanation:** Use a subquery to compute average salary then filter. Time complexity is DB-dependent.

---

### SQL 2 — Products where stock < 10 and category = 'Grocery'

**Problem:** Return product_id, name, stock, category for products with low stock in Grocery.

**Solution (SQL):**

```sql
SELECT product_id, name, stock, category
FROM products
WHERE stock < 10 AND category = 'Grocery';
```

**Explanation:** Simple WHERE filters.

---

### SQL 3 — Customers who placed more than 3 orders

**Problem:** Using tables `customers(id,name)` and `orders(id, customer_id, order_date, amount)` return `customer_id`, `name`, and `order_count` for customers with more than 3 orders.

**Solution (SQL):**

```sql
SELECT c.id AS customer_id, c.name, COUNT(o.id) AS order_count
FROM customers c
JOIN orders o ON o.customer_id = c.id
GROUP BY c.id, c.name
HAVING COUNT(o.id) > 3;
```

**Explanation:** GROUP BY + HAVING filters aggregated results.

---

### SQL 4 — Top 5 highest selling products by total quantity

**Problem:** Assume `order_items(order_id, product_id, quantity)`. Return top 5 product_id and total_qty.

**Solution (SQL):**

```sql
SELECT product_id, SUM(quantity) AS total_qty
FROM order_items
GROUP BY product_id
ORDER BY total_qty DESC
LIMIT 5;
```

**Explanation:** Aggregate by product and sort.

---

### SQL 5 — Students whose marks improved compared to previous exam

**Problem:** Tables: `Students(id,name)`, `Marks(student_id, exam_no, score)` where exam_no = 1 or 2. Find students with score(exam2) > score(exam1).

**Solution (SQL):**

```sql
SELECT s.id, s.name, m2.score AS score_exam2, m1.score AS score_exam1
FROM Students s
JOIN Marks m1 ON s.id = m1.student_id AND m1.exam_no = 1
JOIN Marks m2 ON s.id = m2.student_id AND m2.exam_no = 2
WHERE m2.score > m1.score;
```

**Explanation:** Self-join Marks to compare rows for same student.

---

# Section 2 — Hands-On Programming (5 Problems + Solutions)

(All code given in Python with explanation and complexity.)

### H1 — Special days where cumulative sum divisible by 5

**Problem:** Alice adds 1,2,... each day. Count days <= N where cumulative sum (triangular numbers) is divisible by 5.

**Observation:** Total after day d = d*(d+1)/2. We need d*(d+1)/2 ≡ 0 (mod 5).
Solve for d mod 5 patterns. We can brute force O(N) or use pattern since modulo repeats every 5 or 10.

**Python solution (O(N) simple):**

```python
def count_special_days(N):
    count = 0
    total = 0
    for d in range(1, N+1):
        total += d
        if total % 5 == 0:
            count += 1
    return count

# Example
print(count_special_days(6))  # -> 2
```

**Optimized observation:** Since d*(d+1)/2 mod 5 depends on d mod 5, we can precompute for cycle of 10 or 5 and use division to scale up. But O(N) is fine for typical N ≤ 10^7 with optimization.

Complexity: O(N) time, O(1) space.

---

### H2 — Split students into two equal groups even vs odd IDs

**Problem:** Given list of IDs length N, group of evens and odds. Both groups must be same size — return maximum size (size per group).

**Solution:** Count evens and odds; answer = min(count_even, count_odd).

**Python:**

```python
def max_group_size(ids):
    evens = sum(1 for x in ids if x % 2 == 0)
    odds = len(ids) - evens
    return min(evens, odds)

# Example
print(max_group_size([5,2,3,6]))  # -> 2
```

Complexity: O(N) time.

---

### H3 — Spinner last-digit product (cycle detection)

**Problem:** Start with N, repeatedly multiply by K. Collect last digits forever — find distinct digits and product.

**Observation:** Last digit of numbers under repeated multiplication depends only on last digit. There are at most 10 possible digits → cycle detection with a set.

**Python:**

```python
def spinner_product(N, K):
    seen = set()
    cur = N
    while True:
        d = cur % 10
        if d in seen:
            break
        seen.add(d)
        cur = cur * K
    prod = 1
    for x in seen:
        prod *= x
    return prod

# Example
print(spinner_product(11, 3))  # -> 189
```

Edge case: if seen contains 0, product becomes 0 (correct).
Complexity: O(10) iterations worst-case.

---

### H4 — Generate GP until term ≤ Z

**Problem:** Given A1, R, Z return series [A1, A1*R, ...] while term ≤ Z.

**Python:**

```python
def gp_series(a1, r, z):
    res = []
    val = a1
    if a1 <= 0 and r <= 0:
        return []
    while val <= z:
        res.append(val)
        val = val * r
        # avoid infinite loop when r==1
        if r == 1:
            break
    return res

print(gp_series(2,3,100))  # -> [2,6,18,54]
```

Complexity: O(log_r(Z/a1)).

---

### H5 — Repeated digit sum (digital root)

**Problem:** Reduce number by summing digits repeatedly until single digit remains.

**Observation:** Digital root formula: if n==0 -> 0 else 1 + (n-1) % 9.

**Python:**

```python
def digital_root(n):
    if n == 0:
        return 0
    return 1 + (n - 1) % 9

print(digital_root(9875))  # 2 (9+8+7+5=29 -> 2+9=11 -> 1+1=2)
```

Complexity: O(1).

---

# Section 3 — Front-End Simulator (5 tasks + solutions)

Each task includes HTML/CSS/JS snippet you can paste into an `index.html` or small project.

### FE1 — Password Strength Checker

**Goal:** Update strength indicator with weak/medium/strong colors and placeholder. Provided earlier; here's a concise working JS snippet.

**HTML (index.html):**

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Password Strength</title>
  <style>
    .strength_indicator { padding: 12px; border-radius: 8px; font-weight: bold; }
    .weak { background-color: #e74c3c; color: white; }
    .medium { background-color: #f1c40f; color: white; }
    .strong { background-color: #2ecc71; color: white; }
  </style>
</head>
<body>
  <h1>Password Strength</h1>
  <input type="password" id="passwordInput" placeholder="Enter your password">
  <div id="strengthIndicator" class="strength_indicator"><span id="strengthText">Enter a password</span></div>

  <script>
    const passwordInput = document.getElementById('passwordInput');
    const strengthIndicator = document.getElementById('strengthIndicator');
    const strengthText = document.getElementById('strengthText');

    passwordInput.addEventListener('input', checkStrength);

    function getStrengthLevel(password){
      const hasLetters = /[a-zA-Z]/.test(password);
      const hasNumbers = /[0-9]/.test(password);
      const hasSpecial = /[^a-zA-Z0-9]/.test(password);
      if(password.length < 4) return 'weak';
      if((hasLetters && hasNumbers) || (hasLetters && hasSpecial) || (hasNumbers && hasSpecial)){
        if(password.length >= 8) return 'strong';
        return 'medium';
      }
      return 'weak';
    }

    function checkStrength(){
      const pwd = passwordInput.value;
      if(pwd.length === 0){
        strengthIndicator.className = 'strength_indicator';
        strengthText.textContent = 'Enter a password';
        return;
      }
      const s = getStrengthLevel(pwd);
      strengthIndicator.className = 'strength_indicator ' + s;
      strengthText.textContent = s.charAt(0).toUpperCase() + s.slice(1) + ' Password';
    }
  </script>
</body>
</html>
```

**Notes:** Placeholder already set; `.weak` color `#e74c3c` used as required.

---

### FE2 — Character counter with limit highlight

**HTML + JS:**

```html
<textarea id="textInput" maxlength="200"></textarea>
<div id="counter">0/200</div>
<script>
  const t = document.getElementById('textInput');
  const c = document.getElementById('counter');
  t.addEventListener('input', () => {
    const used = t.value.length;
    c.textContent = `${used}/200`;
    if(200 - used < 20) c.style.color = 'red'; else c.style.color = '';
  });
</script>
```

---

### FE3 — Theme toggle (Light/Dark) with localStorage

**HTML + JS:**

```html
<button id="toggle">Toggle Theme</button>
<script>
  const btn = document.getElementById('toggle');
  const root = document.documentElement;
  const apply = theme => document.documentElement.setAttribute('data-theme', theme);
  const saved = localStorage.getItem('theme') || 'light';
  apply(saved);
  btn.addEventListener('click', () => {
    const cur = document.documentElement.getAttribute('data-theme') || 'light';
    const next = cur === 'light' ? 'dark' : 'light';
    apply(next); localStorage.setItem('theme', next);
  });
</script>
```

Add CSS for `[data-theme="dark"]` accordingly.

---

### FE4 — Live search filter

**HTML + JS:**

```html
<input id="search" placeholder="Search...">
<ul id="list">
  <li>Apple</li><li>Banana</li><li>Cherry</li>
</ul>
<script>
  document.getElementById('search').addEventListener('input', function(){
    const q = this.value.toLowerCase();
    document.querySelectorAll('#list li').forEach(li => {
      li.style.display = li.textContent.toLowerCase().includes(q) ? '' : 'none';
    });
  });
</script>
```

---

### FE5 — OTP input boxes auto-jump

**HTML + JS:**

```html
<div id="otp">
  <input maxlength="1" />
  <input maxlength="1" />
  <input maxlength="1" />
  <input maxlength="1" />
</div>
<script>
  const inputs = document.querySelectorAll('#otp input');
  inputs.forEach((input, i) => {
    input.addEventListener('input', () => {
      if(input.value && i < inputs.length - 1) inputs[i+1].focus();
    });
    input.addEventListener('keydown', (e) => {
      if(e.key === 'Backspace' && !input.value && i > 0) inputs[i-1].focus();
    });
  });
</script>
```

---

# Section 4 — DSA / Logical (5 Problems + Solutions)

### D1 — First non-repeating character in a string

**Problem:** Return the first character that appears only once.

**Python solution:**

```python
from collections import Counter

def first_non_repeating(s):
    cnt = Counter(s)
    for ch in s:
        if cnt[ch] == 1:
            return ch
    return None

print(first_non_repeating('aabbcdde'))  # -> 'c'
```

Complexity: O(n) time, O(1) extra space for alphabet-limited.

---

### D2 — Stable partition: evens first preserving order

**Problem:** Rearrange array so evens appear first preserving relative order.

**Solution (Python stable):**

```python
from collections import deque

def evens_first_preserve(arr):
    ev = []
    od = []
    for x in arr:
        (ev if x % 2 == 0 else od).append(x)
    return ev + od

print(evens_first_preserve([3,2,5,6,9,4]))  # -> [2,6,4,3,5,9]
```

Complexity: O(n) time, O(n) extra space.

---

### D3 — Longest Increasing Subsequence length (LIS)

**Efficient solution O(n log n):** patience sorting technique.

**Python:**

```python
import bisect

def lis_length(nums):
    tails = []
    for x in nums:
        i = bisect.bisect_left(tails, x)
        if i == len(tails):
            tails.append(x)
        else:
            tails[i] = x
    return len(tails)

print(lis_length([10,9,2,5,3,7,101,18]))  # -> 4 (2,3,7,18)
```

Complexity: O(n log n).

---

### D4 — Valid palindrome (alphanumeric only)

**Problem:** Return True if ignoring non-alphanumeric and case the string is palindrome.

**Python 2-pointer solution:**

```python
import string

def is_palindrome(s):
    i, j = 0, len(s)-1
    while i < j:
        while i < j and not s[i].isalnum(): i += 1
        while i < j and not s[j].isalnum(): j -= 1
        if s[i].lower() != s[j].lower(): return False
        i += 1; j -= 1
    return True

print(is_palindrome('A man, a plan, a canal: Panama'))  # True
```

Complexity: O(n).

---

### D5 — Find missing number from 1..N

**Problem:** Given array of size N-1 containing distinct numbers from 1..N with one missing. Find missing number.

**Solutions:**

* Sum formula: missing = N*(N+1)//2 - sum(arr)
* XOR: compute xor 1..N and xor of array.

**Python (sum):**

```python
def find_missing(arr, N):
    return N*(N+1)//2 - sum(arr)

print(find_missing([1,2,4,5], 5))  # -> 3
```

Complexity: O(n) time, O(1) space.

---

# Final notes

* These solutions are concise and directly applicable for interview-style coding rounds.
* For front-end tasks run the HTML/JS in a browser.
* For SQL tasks drop into your DB environment and adapt table/column names as needed.

---

Good luck — practice these, implement them, and time yourself. If you want, I can also:

* Provide unit tests for each function.
* Provide full-length explanations and edge-case tests.
* Export a downloadable `.md` or `.zip` containing separate files for each problem.

If you want that, say `Export files` or `Add tests`.
