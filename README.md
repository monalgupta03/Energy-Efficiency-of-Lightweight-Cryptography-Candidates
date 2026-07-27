# Energy-Efficiency-of-Lightweight-Cryptography-Candidates



### notes

Fix the include path
Right-click the project → Properties → C/C++ Build → Settings → Include paths (or MCU GCC Compiler → Include paths). Remove the old Inc/Ascon path if it's listed, and add Core/Inc/algorithms/ascon instead. This is the one setting that will later become your per-algorithm switch.
