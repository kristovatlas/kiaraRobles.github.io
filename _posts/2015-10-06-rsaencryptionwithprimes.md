---
layout: post
title: RSA Encryption With Primes
---
# RSA Encryption With Primes
##### An Objective-C Implementation

Security is about layers of difficulty. Nothing is full proof. But some methods of encryption have more layers of difficulty than others. Before computers, simply shifting the characters in the alphabet over some spaces was sufficient to stump most prying eyes. This is known as the Caesar cipher method, its implementation on in most computer programing languages uses the modulus operator to shifts the message over by a factor of a key and rotates around the alphabet in ASCII values. 

![](/images/cy.jpg)

You can think of modulus as an operator that counts for a given base (as in counting in base 12 instead of 10), the 26 letters in the alphabet requires counting in base 26.

        NSUInteger key = 5;
        key = key % 26;

        NSString *message = @"What would Caesar do?";
        ...
        NSLog(@"%@", encodedMessage);
        // Bmfy btzqi Rw. Wtljwx it?

If it seems like shifting letters in the alphabet is simple, consider that it's how the Germans encrypted their correspondence in WWII. They shifted every letter through three different rotors of encryption, refactored it, then fed the message in backward to the further complicate the algorithm. The Enigma machine that eventually cracked this code, could calculate 150,738,274,937,250 possible combinations of input, which would have been impossible by hand.

##### Symmetric Encryption
Both the Enigma machine and the Caesar cipher method are implementations of symmetric encryption. Meaning that the same key is used for both encryption and decryption. This is ideal for documents that you're both encrypting and decrypting. But for messages being sent, the issue of sending the key securely with the message is problematic, and subject to a mand in the middle attack.
![](/images/skey.jpg)

##### Asymmetric Encryption
Asymmetric Encryption means never having to send a decryption key.
![](/images/pkey.jpg)
If Caesar wants to send a message to Alice, he would use her public key for encryption and she would use her private key for decryption.

##### A Simple Implementation of RSA encryption:

Person A selects two prime numbers: P and Q

    NSUInteger privateKeyValueP= 23;
    NSUInteger privateKeyValueQ = 41;

Person A multiplies both numbers to get their "public key": PQ = P x Q

    NSUInteger publicKeyValuePQ = privateKeyValueP * privateKeyValueQ; 
    
Person A chooses a number "e" that is prime relative to the first two numbers:
    
    NSUInteger publicKeyValueE  = 7;

And what's called the Tolent is calculated by:
    
    NSUInteger totient = (privateKeyValueP - 1) * (privateKeyValueQ - 1);

##### Why prime Numbers?
![](/images/primeComposites.jpg)
Prime numbers are positive integers have no positive factors other than 1 and itself. This is useful because numbers that have no prime factors have a multiplicative inverse. 

##### Multiplicative Inverse
In RSA, the property of multiplicative inverses create a "trap-door" algorithm, meaning that the forward encryption is easy to preform but the inverse decryption is more difficult unless you know the totient of the two prime numbers.

    23 * 23^-1 = 1 mod 41
             1 = 1

##### RSA Algorithm
The RSA method has a similar mathematical formula for both encryption and decryption. Given a plaintext (message), M, represented as a number.

The cyphertext C, is found by:

![](/images/equn6786.png)

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

Decrypting the message with d is simplified to:

![](/images/equn6785.png)

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

The result of these numbers multiplied by the mod of the public key returns the original message number.
![](http://imgur.com/r5LF1Xy.png)

This method of using the public key is the pair {e,n} and the private key as the pair {d,n} works for encryption because factoring large numbers is incredibly difficult.

![](/images/producttree.png)

To compute a product of all numbers N=N1,N2,Nm in a 4096 bit keys and would require such an unbelievable about of computing power no one has cracked it yet.

2^4096=
![](/images/4096_bit_encryption_keys.gif)
