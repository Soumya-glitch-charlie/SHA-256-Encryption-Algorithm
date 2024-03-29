# SHA-256-Encryption-Algorithm
A comprehensive collection of robust and efficient hash function implementations for secure data encryption and authentication using the SHA_256 algorithm.

SHA256 algorithm
The SHA256 hash is a block transformation algorithm based on LFSR message expansion.
The algorithm has 2 parts: the Message Schedule and the Hash Core.
The message schedule can be implemented as a compact 16-word circular buffer, that is cycled for the 64 clock cycles.
Here is a simplified diagram of implementing the message schedule datapath, with the circular buffer (r0 ... r15) and the combinational functions (σ0, σ1).



<br>

##
![Msg Schedule Logic](https://github.com/Soumya-glitch-charlie/SHA-256-Encryption-Algorithm/assets/127016329/c4ae5375-1be1-478e-b26a-410c5d34ec4a)
<br>
<i><b>Preview:</b> SHA-256 Message Schedule Circuit Diagram </i>

<br>



We implement the core logic as a word-shift register with combinational logic that computes the next state for the word-shifter registers.
Here is a simplified depiction of the hash core logic datapath, with the core registers (a .. h) and the combinational functions (Maj, Ch, ∑0, ∑1).
The whole block is processed in a single clock cycle for each of the 64 cycles of the algorithm.


<br>

##
![HASH Core Logic](https://github.com/Soumya-glitch-charlie/SHA-256-Encryption-Algorithm/assets/127016329/67c5088b-2e07-48d0-a92d-fdf0a2e8d3bc)
<br>
<p align="center"><i><b>Preview:</b>SHA-256 HASH Core Logic</i></p>

<br>


Note on the combinational functions (Maj, Ch, ∑0, ∑1, σ0, σ1): The Sigma function shifters are implemented as fixed bit remaps (zero-logic), and the Maj and Ch are canonical implementations of the FIPS-180-4 description. These boolean functions can be rearranged to match the target gate library. The boolean remap saves no area for LUT-based FPGA targets, but for ASIC targets and the more limited FPGA sea of gates, the remap can hold combinational depth.

Note on the wide port adders: The hash algorithm is heavily dependent on adder architecture. When implemented with 2-port 32+32 bit adders, a cascade of adders can be used for the adder chain, reducing interconnect complexity. However, aggressive implementation with wide port parallel adders can reduce combinational path length and increase top clock rates, at the expense of interconnect and area constraints.

