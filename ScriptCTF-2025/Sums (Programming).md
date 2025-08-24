> Find the sum of nums[i] for i in [l, r] (if there are any issues with input/output format, plz open a ticket)
> 
> Attachments
> server.py
```
Instance Info

#!/usr/bin/env python3
import random
import subprocess
import sys
import time

start = time.time()

n = 123456

nums = [str(random.randint(0, 696969)) for _ in range(n)]

print(' '.join(nums), flush=True)

ranges = []
for _ in range(n):
    l = random.randint(0, n - 2)
    r = random.randint(l, n - 1)
    ranges.append(f"{l} {r}") #inclusive on [l, r] 0 indexed
    print(l, r)

big_input = ' '.join(nums) + "\n" + "\n".join(ranges) + "\n"

proc = subprocess.Popen(
    ['./solve'],
    stdin=subprocess.PIPE,
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE,
    text=True
)

stdout, stderr = proc.communicate(input=big_input)

out_lines = stdout.splitlines()
ans = [int(x) for x in out_lines[:n]]

urnums = []
for _ in range(n):
    urnums.append(int(input()))

if ans != urnums:
    print("wawawawawawawawawa")
    sys.exit(1)

if time.time() - start > 10:
    print("tletletletletletle")
    sys.exit(1)

print(open('flag.txt', 'r').readline())
```

The server generates n = 123,456 random integers (nums).
Then it generates n queries, each query is a pair of indices (l, r) representing a range in the array.
For each query, we need to return the sum of the elements in nums between indices l and r (inclusive).
After submitting all sums, if correct and within time limit, the server returns the flag.

The server.py script shows the generation of the input (the numbers and the queries) and how it runs a solution executable ./solve by feeding it all input at once.
We have to implement a solve program that reads the input from stdin and outputs the sums for each query.

Problem Analysis
Input size is large: 123,456 numbers and 123,456 queries.
Naively summing nums[l:r+1] for each query would be O(n²) — way too slow.
Need a data structure or technique to answer range sum queries efficiently.

With the prefix sums technique:
Build an array prefix where prefix[0] = 0 and for i from 1 to n:
prefix[i] = prefix[i-1] + nums[i-1]

Then the sum for any range [l, r] is:
prefix[r+1] - prefix[l]

This answers each query in O(1) time, making the entire solution efficient

Implementation of solve.py which : 
Reads the single line containing all nums.
Builds the prefix sums array.
Reads each (l,r) query line.
Prints the sum for that query immediately.

To interact with the remote challenge server (import.py):
We connected via a TCP socket to play.scriptsorcerers.xyz:10481.
We implemented a helper function recv_line to read lines terminated by newline.
The server first sends a line with all nums.
Then it sends 123,456 lines with queries.
For each query, we calculate the sum using prefix sums and send the result back immediately.
After all answers are sent, the server returns the flag string.


<details>
<summary>Flag</summary>

`scriptCTF{1_w4n7_m0r3_5um5_f156061d77c1}`

</details>
