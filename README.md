### EX3 Implementation of GSP Algorithm In Python
### DATE: 17/11/25
### AIM: To implement GSP Algorithm In Python.
### Description:
The Generalized Sequential Pattern (GSP) algorithm is a data mining technique used for discovering frequent patterns within a sequence database. It operates by identifying sequences that frequently occur together. GSP works by employing a depth-first search strategy to explore and extract frequent patterns efficiently.
### Steps:
1. <strong>Database Scanning:</strong> GSP scans the sequence database to determine the support of each item in the dataset.
2. <strong>Candidate Generation:</strong> It generates a set of candidate sequences using frequent items found in the previous step.
3. <strong>Pattern Growth:</strong> It extends the candidate sequences by merging them to form longer patterns, checking their support against a user-defined minimum support threshold.
4. <strong>Repeat:</strong> The process continues until no new sequences meet the minimum support threshold.
<p align="justify">
GSP finds application in various domains such as market basket analysis, web usage mining, bioinformatics, and more. For instance, in retail, GSP can identify common purchasing patterns, helping businesses understand customer behavior for targeted marketing or inventory management.
</p>

### Procedure:
<p align="justify">
1. From collections import defaultdict, from itertools import combinations: Imports necessary libraries/modules. defaultdict is
used to create a dictionary with default values and combinations generates all possible combinations of a sequence.</p>
<p align="justify">
2. generate_candidates(dataset, k): Function to generate candidate k-item sequences from a dataset. It loops through each sequence in the
dataset and finds combinations of length k for each sequence, updating their counts in a dictionary.</p>
<p align="justify">
3. gsp(dataset, min_support): Function that implements the Generalized Sequential Pattern (GSP) algorithm. It iterates through increasing
sequence lengths (k) until no new frequent patterns are found. It calls generate_candidates() to find patterns of varying lengths.</p>
<p align="justify">
4. Example dataset for each category: Defines example sequences for top wear, bottom wear, and party wear categories.</p>
<p align="justify">
5. Minimum support threshold: Sets the minimum support count required for a pattern to be considered frequent.</p>
<p align="justify">
6. Perform GSP algorithm for each category: Applies the GSP algorithm for each category using the defined example datasets and the
minimum support threshold.</p>
<p align="justify">
7. Output the frequent sequential patterns for each category: Prints the frequent sequential patterns 
    along with their support counts
for each wear category.</p>
<p align="justify">
8. Visulaize the sequence patterns using matplotlib.
</p>

### Program:

```python
from collections import defaultdict
from itertools import combinations
from tabulate import tabulate

# Function to generate candidate k-item sequences from previous frequent patterns
def generate_candidates(prev_frequent, k):
    candidates = set()
    items = set(item for pattern in prev_frequent for item in pattern)
    for combo in combinations(sorted(items), k):
        candidates.add(combo)
    return candidates

# Function to calculate support of a candidate in the dataset
def calculate_support(candidate, dataset):
    count = 0
    for sequence in dataset:
        if all(item in sequence for item in candidate):
            count += 1
    return count

# GSP algorithm implementation
def gsp(dataset, min_support):
    frequent_patterns = {}
    k = 1

    # First pass - single item sequences
    items = set(item for sequence in dataset for item in sequence)
    current_frequent = []
    for item in sorted(items):
        support = calculate_support((item,), dataset)
        if support >= min_support:
            frequent_patterns[(item,)] = support
            current_frequent.append((item,))

    # Generate longer sequences
    while current_frequent:
        k += 1
        candidates = generate_candidates(current_frequent, k)
        current_frequent = []
        for candidate in candidates:
            support = calculate_support(candidate, dataset)
            if support >= min_support:
                frequent_patterns[candidate] = support
                current_frequent.append(candidate)

    return frequent_patterns

# Example datasets
top_wear_data = [
    ["blouse", "t-shirt", "tank_top"],
    ["hoodie", "sweater", "top"],
    ["hoodie"],
    ["hoodie", "sweater"]
]

bottom_wear_data = [
    ["jeans", "trousers", "shorts"],
    ["leggings", "skirt", "chinos"]
]

party_wear_data = [
    ["cocktail_dress", "evening_gown", "blazer"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress", "formal_dress", "suit"],
    ["party_dress"],
    ["party_dress"]
]

# Minimum support
min_support = 2

# Run GSP on each category
top_wear_result = gsp(top_wear_data, min_support)
bottom_wear_result = gsp(bottom_wear_data, min_support)
party_wear_result = gsp(party_wear_data, min_support)

# Print results
def print_table(result, category):
    print(f"\nFrequent Sequential Patterns - {category}:")
    if result:
        table_data = [(f"{pattern}", support) for pattern, support in result.items()]
        print(tabulate(table_data, headers=["Pattern", "Support Count"], tablefmt="fancy_grid"))
    else:
        print(f"No frequent sequential patterns found in {category}.")

print_table(top_wear_result, "Top Wear")
print_table(bottom_wear_result, "Bottom Wear")
print_table(party_wear_result, "Party Wear")
```
### Output:
![image](https://github.com/user-attachments/assets/d610aeb0-717e-4dca-89aa-d8c3d23c7bf4)

### Visualization:
```python
import matplotlib.pyplot as plt

# Function to visualize frequent sequential patterns with a line plot
def visualize_patterns_line(result, category):
    if result:
        patterns = list(result.keys())
        support = list(result.values())

        plt.figure(figsize=(10, 6))
        plt.plot([str(pattern) for pattern in patterns], support, marker='o', linestyle='-', color='blue')
        plt.xlabel('Patterns')
        plt.ylabel('Support Count')
        plt.title(f'Frequent Sequential Patterns - {category}')
        plt.xticks(rotation=90)
        plt.tight_layout()
        plt.show()
    else:
        print(f"No frequent sequential patterns found in {category}.")

# Visualize frequent sequential patterns for each category using a line plot
visualize_patterns_line(top_wear_result, 'Top Wear')
visualize_patterns_line(bottom_wear_result, 'Bottom Wear')
visualize_patterns_line(party_wear_result, 'Party Wear')
```
### Output:
![image](https://github.com/user-attachments/assets/eb1ce927-a759-4d17-9d29-95dc277e3e70)

![image](https://github.com/user-attachments/assets/26556381-2196-4837-80dd-d38e9922c334)

### Result:
Thus, The Implementation of GSP Algorithm In Python is successfully executed

