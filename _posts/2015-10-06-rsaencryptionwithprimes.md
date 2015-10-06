---
layout: post
title: RSA Encryption With Primes
---
# RSA Encryption With Primes
##### An Objective-c Implemantation

Security is about layers of difficulty. Nothing is full proof. But some methods have more layers of difficulty than others. Before computers simply shifting the characters in the alphabet over some spaces was sufficient to stump most prying eyes. This is known as the Caesar cipher method, its implementation in Objective-c shifts the message over by a factor of a key and rotates around the alphabet in ascii values using the mod operator. If you think of mod as an operator that counts for a given base (as in counting in base 12 instead of 10), the 26 letters in the alphabet requires counting in base 26.

        NSUInteger key = 5;
        key = key % 26;

        NSString *message = @"What would Caesar do?";
        ...
        NSLog(@"%@", encodedMessage);
        // Bmfy btzqi Rw. Wtljwx it?

If it seems like shifting letters in the alphabet is simple, consider that it's how the Germans encrypted their correspondence in WWII. They shifted every letter thru three different rotors of encryption, refactored it, then fed the message in backward to the further complicate the algorithm. The machine eventualy built to could crack this code, could calculate 150,738,274,937,250 possible combinations of input, which would have been impossible by hand.

Encryption has to with computing power. The problem with what's called symmetric cryptography, its that the key is embedded in the code and computers can crack this relatively easily. Brute force attack the algorithm with possible outputs and you've cracked the code. A more secure method of encryption is what's called asymmetric cryptography. This requires a decryption key that is not the reverse of the encryption.


##### A simple implementation of a symmetric RSA encryption looks like this:

Person A selects two prime numbers: P and Q

    NSUInteger privateKeyValueP = 23;
    NSUInteger privateKeyValueQ = 41;

Person A multiplies both numbers to get their "public key": PQ = P x Q

    NSUInteger publicKeyValuePQ = privateKeyValueP * privateKeyValueQ; 
    
Person A chooses a number "e" that is prime relative to the first two numbers:
    
    NSUInteger publicKeyValueE  = 7;

And what's called the Tolent is calculated by:
    
    NSUInteger totient = (privateKeyValueP - 1) * (privateKeyValueQ - 1);
  
We use prime numbers so that the two integers have no positive factors other than 1 and itself. This is useful because prime numbers have a multiplicative inverse, equal to the modulo of another prime number.

##### Multiplicative Inverse
Not all numbers have a multiplicative inverse, but when they do we can use them for RSA encryption. 

    23 * 23^-1 = 1 mod 41
             1 = 1

##### RSA Algorithm
The RSA method has the same mathematical form for both encryption and decryption. I.e. given a plaintext (message), M, represented as a number.

The cyphertext, C, is found by: C = M^e (mod PQ)

    NSUInteger plaintext = 35;
    
    NSUInteger plaintextPow = pow(plaintext, publicKeyValueE);
    NSUInteger ciphertext = plaintextPow % publicKeyValuePQ;
    
    NSLog(@"ciphertext = %ld", ciphertext);
    // 545

Likewise the decryption is found by: 

Solving for d where, ed = 1 mod PQ 

    // This algorithm iterates while ed is less than the totient, and saves the value of d when both sides of the equation are equal
    NSUInteger ed = 1;
    NSUInteger d = 0;
    NSUInteger i = 1;
    for (i = 1; ed < totient ; i++) {
        NSLog(@"d = %lu", ed);
        ed = (i * totient + 1) / publicKeyValueE;
        if (ed * publicKeyValueE == (i * totient + 1)) {
            d = ed;
        }
    }

Decrypting the message with d is simplified to, M = C^d (mod PQ):

    // This algorithm iterates, C to the power of i, when i is doubled each time, until M =  C^d (mod PQ) 
    NSUInteger ciphertextPow = 0;
	for (NSUInteger i = 2; i < ciphertext; i++){
		i--;
		ciphertextPow = pow(ciphertext,i);
		ciphertextPow = ciphertextPow2 % publicKeyValuePQ;
		
		NSLog(@"cyphertextPow = %llu", ciphertextPow);
		i = i * 2;
	}
	
	2015-10-06 11:39:14.965 rsaEncoder[95491:2712957] ciphertextPowJk = 545
    2015-10-06 11:39:14.965 rsaEncoder[95491:2712957] ciphertextPowJk = 923
    2015-10-06 11:39:14.965 rsaEncoder[95491:2712957] ciphertextPowJk = 400
    2015-10-06 11:39:14.965 rsaEncoder[95491:2712957] ciphertextPowJk = 633
    2015-10-06 11:39:14.966 rsaEncoder[95491:2712957] ciphertextPowJk = 857
    2015-10-06 11:39:14.966 rsaEncoder[95491:2712957] ciphertextPowJk = 795
    2015-10-06 11:39:14.966 rsaEncoder[95491:2712957] ciphertextPowJk = 215
    2015-10-06 11:39:14.966 rsaEncoder[95491:2712957] ciphertextPowJk = 18
    2015-10-06 11:39:14.966 rsaEncoder[95491:2712957] ciphertextPowJk = 324
    // ciphertextPow data type changed to accommodate for big numbers

The result of these numbers multiplied by the mod of the public key is the original message number.
 545 * 923 * 400 * 857 * 795 * 215 * 18 * 324 (mod 943) = 35

This method of using the public key is the pair {e,n} and the private key as the pair {d,n} works for encryption because factoring numbers is incredibly difficult. The difficulty of factoring n is what stops someone from finding d when given e and n. 

![](https://windowsontheory.files.wordpress.com/2012/05/producttree1.png)

To compute a product of all numbers N=N1,N2,Nm in a 4096 bit keys and would require such an unbelievable about of computing power, no ones done it yet.
