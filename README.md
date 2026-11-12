# Algorithm-battle
To find which algorithm wins.
Here is the complete answer in the assignment's format. All figures come from my run of `algorithm_battle.py` (40 unique employee IDs, hash table size 61).

## Part A – Problem Selection

**Application: Employee ID Search**

- **a. Problem:** A company must store employee records (ID, name, department), add new employees, and retrieve any record by ID.
- **b. Why efficient searching:** Payroll, attendance and login systems look up IDs constantly. A linear scan costs O(n) per lookup, which becomes too slow as the employee count grows.
- **c. Why compare these three:**
  - BST: simple, O(log n) on average, but can degrade to O(n).
  - AVL: a BST that balances itself, giving guaranteed O(log n).
  - Hashing: O(1) on average, with linear probing to resolve collisions.

  Together they show the trade-off between simplicity, guaranteed performance and raw speed.

## Part B – Implementation

- **BST:** insert, search, inorder/preorder/postorder, height.
- **AVL:** insert with LL, RR, LR and RL rotations, search, height.
- **Hashing:** `key % 61`, linear probing `(i+1) % 61`, collision counter.

## Part C – Mini Experiment (random insertion order)

| Observation | BST | AVL | Hashing |
|---|---|---|---|
| Successful search, all 40 keys (total) | 225 | 184 | 56 |
| Successful search (avg / max) | 5.62 / 9 | 4.60 / 6 | 1.40 / 6 |
| Unsuccessful search, 9 absent keys (avg / max) | 4.67 / 7 | 5.44 / 6 | 3.78 / 7 |
| Insertion, building 40 keys (total) | 185 | 160 | 56 |
| Inserting 4 new keys (1111, 1222, 1333, 1555) | 6, 6, 4, 7 | 6, 6, 6, 5 | 5, 4, 1, 1 |

## Part D – Algorithm Battle

| Parameter | BST | AVL | Hashing |
|---|---|---|---|
| Search comparisons | Avg 5.62 | Avg 4.60 | **Avg 1.40** |
| Insertion observations | 185 comparisons, no rebalancing | 160 comparisons, 26 rotations | **56 probes**, no rotations |
| Height | 9 (40 on sorted input) | **6** (6 on sorted input) | – |
| Collisions | – | – | 16 (load factor 0.66) |
| Overall observation | Fine on random data, collapses on sorted data (avg 20.5 comparisons) | Stays balanced on any input | Fastest, but unordered and slows as the table fills |

## Part E – Final Verdict

**a. Winner: Hashing with linear probing.**

**b. Supporting observations:**
- It needed the fewest comparisons for successful search (1.40, against 4.60 for AVL and 5.62 for BST).
- It was cheapest to build (56 probes, against 160 and 185).
- It also did best on unsuccessful search (3.78, against 4.67 and 5.44).
- Its cost was 16 collisions at a load factor of 0.66, which is acceptable if you rehash above about 0.7.

**Runner-up: AVL.** With sorted input, BST reached height 40 while AVL stayed at 6, so AVL is the safe tree choice. If the application also needs sorted employee listings or ID range queries, choose AVL, because hashing has no ordering.
