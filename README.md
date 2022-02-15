# Disecto_Assessment

In this repo, we go through an implementation of an homomorphic encryption scheme which is mainly inspired from BFV. We have split the whole scheme into basic functionalities, key-generation, encryption, decryption and evaluation (add and mul).

We start by importing the amazing Numpy library, then we define two helper functions (polyadd and polymul) for adding and multiplying polynomials.

We start by generating a random secret-key (sk) from a probability distribution, we will use the uniform distribution over R2, which means sk will be a polynomial with coefficients being 0 or 1. For the public-key (pk) we first sample a polynomial a uniformly over Rq and a small error polynomial e from a discrete normal distribution over Rq.

So let's first implement the *generation of polynomials* from different probability distributions.
def gen_binary_poly(size)
def gen_uniform_poly(size, modulus)
def gen_normal_poly(size)

Then we can define our *key-generator* using these functions
def keygen(size, modulus, poly_mod)
    Args:
        size: size of the polynoms for the public and secret keys.
        modulus: coefficient modulus.
        poly_mod: polynomial modulus.
    Returns:
        Public and secret key.
The public-key (b, a) can then be used for encryption, and the secret-key sk for decryption.

The *encryption algorithm* takes a public-key and a plaintext polynomial and outputs a ciphertext, which is a tuple of two polynomials ct0 and ct1.
def encrypt(pk, size, q, t, poly_mod, pt)
    Args:
        pk: public-key.
        size: size of polynomials.
        q: ciphertext modulus.
        t: plaintext modulus.
        poly_mod: polynomial modulus.
        pt: integer to be encrypted.
    Returns:
        Tuple representing a ciphertext.

We will *decrypt* to the correct value if the rounding to the nearest integer don't get impacted by the error terms.
def decrypt(sk, size, q, t, poly_mod, ct)
    Args:
        sk: secret-key.
        size: size of polynomials.
        q: ciphertext modulus.
        t: plaintext modulus.
        poly_mod: polynomial modulus.
        ct: ciphertext.
    Returns:
        Integer representing the plaintext.


One can easily choose the parameters that guarantee a correct decryption after a simple encryption, but the goal of HE isn't just to encrypt/decrypt data, but also to compute on encrypted data (add and multiply), and computation will enlarge the error terms as we will see next.

Having a ciphertext in hand, we can add or multiply it with other ciphertexts or plaintexts. In this repo we will focus on plain operations, which means that we will add the capability to our scheme to *add or multiply a ciphertext with plaintexts*.
def add_plain(ct, pt, q, t, poly_mod)
    Args:
        ct: ciphertext.
        pt: integer to add.
        q: ciphertext modulus.
        t: plaintext modulus.
        poly_mod: polynomial modulus.
    Returns:
        Tuple representing a ciphertext.

def mul_plain(ct, pt, q, t, poly_mod)
    Args:
        ct: ciphertext.
        pt: integer to multiply.
        q: ciphertext modulus.
        t: plaintext modulus.
        poly_mod: polynomial modulus.
    Returns:
        Tuple representing a ciphertext.

If we go into back of how this works we can see that:
Compared to the plaintext addition, our error terms get scaled up by our message which means that multiplying with big values might introduce some rounding errors during decryption.

# Having all functionalities implemented we just provide the values for variables/args and see the data being encrypted and decrypted

