# rsa_pwn_ideas


```
from qiskit import QuantumCircuit, Aer, transpile, assemble
from qiskit.algorithms import Shor
from qiskit.utils import QuantumInstance
from Crypto.Util.number import inverse

def reverse_rsa(ciphertext, n, e):
    # Use Shor's algorithm to factor n to find p and q
    shor = Shor(quantum_instance=QuantumInstance(Aer.get_backend('aer_simulator')))
    result = shor.factor(N=n)
    factors = result.factors
    if len(factors) != 2:
        raise ValueError("Failed to factor n")
    p, q = factors
    
    # Compute phi(n) = (p-1) * (q-1)
    phi_n = (p - 1) * (q - 1)
    
    # Compute the private exponent d
    d = inverse(e, phi_n)
    
    # Decrypt the ciphertext to find the original message
    message = pow(ciphertext, d, n)
    return message

# Example usage
n = 187  # Public modulus (product of two primes p and q)
e = 3    # Public exponent
ciphertext = 25  # Ciphertext

# Reverse RSA to find the original message
original_message = reverse_rsa(ciphertext, n, e)
print("Original message:", original_message)
```
