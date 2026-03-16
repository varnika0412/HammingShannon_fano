# Reg.no : 212224060292
# Name   :VARNIKA K

# Huffman-Shannon_fano
# Aim:
Consider a discrete memoryless source with symbols and statistics {0.125, 0.0625, 0.25, 0.0625, 0.125, 0.125, 0.25} for its output. 
Apply the Huffman and Shannon-Fano to this source. 
Show that by drawing the tree diagram, and 
Calculate the average code word length, entropy, variance, redundancy, and efficiency.
# Tools Required:
  * Google colab with computer
# Program:
# HUFFMAN CODING
```
# ==========================
# HUFFMAN CODING (Fixed Order Output)
# ==========================

import heapq
import math

# -------- Fixed Input --------
symbols = ['A','B','C','D','E','F','G']
probabilities = [0.125,0.0625,0.25,0.0625,0.125,0.125,0.25]

n = len(symbols)

# -------- Huffman Node --------
class Node:
    def __init__(self, prob, symbol=None, left=None, right=None):
        self.prob = prob
        self.symbol = symbol
        self.left = left
        self.right = right

    def __lt__(self, other):
        return self.prob < other.prob

# -------- Build Tree --------
heap = []
for i in range(n):
    heapq.heappush(heap, Node(probabilities[i], symbols[i]))

while len(heap) > 1:
    left = heapq.heappop(heap)
    right = heapq.heappop(heap)
    merged = Node(left.prob + right.prob, None, left, right)
    heapq.heappush(heap, merged)

root = heap[0]

# -------- Generate Codes --------
codes = {}

def generate_codes(node, code=""):
    if node.symbol is not None:
        codes[node.symbol] = code
        return
    generate_codes(node.left, code + "0")
    generate_codes(node.right, code + "1")

generate_codes(root)

# -------- Sort by Probability (Descending) --------
data = list(zip(symbols, probabilities))
data.sort(key=lambda x: x[1], reverse=True)

print("Huffman Codes:\n")
for sym, prob in data:
    print(f"{sym} : {codes[sym]}")

# -------- Calculations --------
L = 0
H = 0
var = 0

for sym, prob in data:
    length = len(codes[sym])
    L += prob * length
    H += prob * math.log2(1/prob)

for sym, prob in data:
    length = len(codes[sym])
    var += prob * (length - L)**2

L = round(L,3)
H = round(H,3)
eff = round(H/L,3)
red = round(1-eff,3)
var = round(var,3)

print("\nAverage Codeword Length:", L)
print("Entropy:", H)
print("Efficiency:", eff)
print("Redundancy:", red)
print("Variance:", var)
```

# Shannon-Fano Coding
```
# ==========================
# SHANNON-FANO CODING
# ==========================

import math

# -------- Fixed Input --------
symbols = ['A','B','C','D','E','F','G']
probabilities = [0.125,0.0625,0.25,0.0625,0.125,0.125,0.25]

# -------- Sort by Probability (Descending) --------
data = list(zip(symbols, probabilities))
data.sort(key=lambda x: x[1], reverse=True)

codes = {symbol: "" for symbol, _ in data}

# -------- Shannon-Fano Function --------
def shannon_fano(data):
    if len(data) <= 1:
        return

    total = sum([item[1] for item in data])
    acc = 0
    split_index = 0

    for i in range(len(data)):
        acc += data[i][1]
        if acc >= total/2:
            split_index = i
            break

    left = data[:split_index+1]
    right = data[split_index+1:]

    for symbol, _ in left:
        codes[symbol] += "0"
    for symbol, _ in right:
        codes[symbol] += "1"

    shannon_fano(left)
    shannon_fano(right)

# -------- Generate Codes --------
shannon_fano(data)

print("Shannon-Fano Codes:\n")
for sym, prob in data:
    print(f"{sym} : {codes[sym]}")

# -------- Calculations --------
L = 0
H = 0
var = 0

for sym, prob in data:
    length = len(codes[sym])
    L += prob * length
    H += prob * math.log2(1/prob)

for sym, prob in data:
    length = len(codes[sym])
    var += prob * (length - L)**2

L = round(L,3)
H = round(H,3)
eff = round(H/L,3)
red = round(1-eff,3)
var = round(var,3)

print("\nAverage Codeword Length:", L)
print("Entropy:", H)
print("Efficiency:", eff)
print("Redundancy:", red)
print("Variance:", var)
```

# Calculation:
![WhatsApp Image 2026-02-20 at 9 18 43 PM](https://github.com/user-attachments/assets/2ee0e975-620b-494a-819b-fab1ecb6b7de)
![WhatsApp Image 2026-02-20 at 9 19 12 PM](https://github.com/user-attachments/assets/a48817d6-6e3d-418a-8b1e-88172e34e475)
![WhatsApp Image 2026-02-20 at 9 21 42 PM](https://github.com/user-attachments/assets/c9c54903-6043-4265-bd88-7ff758f3011b)


# Output
# HUFFMAN CODING
<img width="395" height="346" alt="image" src="https://github.com/user-attachments/assets/575f5ee0-bf1b-4988-8f75-f44f98ad31ab" />

# Shannon-Fano Coding
![WhatsApp Image 2026-02-20 at 10 10 28 PM](https://github.com/user-attachments/assets/bcb3ae2f-966b-47db-b3ad-3a0bd5048f43)

# Results:
The Huffman and Shannon-Fano of the given statistics {0.125, 0.0625, 0.25, 0.0625, 0.125, 0.125, 0.25} using python are verified.

](https://github.com/varnika0412/IdealSampling)
