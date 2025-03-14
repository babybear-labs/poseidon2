# Poseidon

Poseidon is a cryptographic hash function optimized for zero-knowledge proof (ZKP) systems. 
It operates using the sponge construction, which processes input data through a fixed-size permutation. 

Below are the key steps in its operation

- State Initialization
  - Poseidon uses a state vector of t field elements (e.g., t = 3).
  - The state splits into:
    - Rate (r): The portion that absorbs input (e.g., r = 2).
    - Capacity (c): Reserved for security (e.g., c = 1).
  - The state starts as all zeros, such as [0, 0, 0] for t = 3.
    
- Input Absorption
  - Input data is divided into blocks of r field elements.
  - Each block is added to the rate portion of the state.
  - After adding a block, a permutation is applied to mix the entire state.

- Permutation:
  - The permutation consists of multiple rounds, each with three steps:
  - AddRoundKeys (ARK): Adds precomputed constants to the state.
  - S-Box Layer: Applies a nonlinear function (often x^5) to
    - All elements in full rounds.
    - Only one element in partial rounds (for efficiency).
  - Mix Layer: Multiplies the state by a matrix (e.g., an MDS matrix) to ensure diffusion.
  - The number of full and partial rounds is tuned for security and performance.

- Output Squeezing
  - After all input blocks are processed, the hash is taken from the rate portion.
  - For longer outputs, additional permutations "squeeze" more data from the state.

### Example

Consider a small Poseidon instance:

- Field: `F_p` where p = `2^64 - 2^32 + 1` (Goldilocks prime).
- State Size: `t = 3` (rate r = 2, capacity c = 1).
- Input: Two field elements `[a, b]`.

Steps:
- Initialize state: `[0, 0, 0]`.
- Absorb input: Add `[a, b]` to the rate → `[a, b, 0]`.
- Apply permutation: Run several rounds (e.g., 8 full + 20 partial) of ARK, S-box, and Mix operations.
- Output: Extract the first element of the state (or more if a longer hash is needed).
