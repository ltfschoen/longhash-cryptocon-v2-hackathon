# LongHash CryptoCon Vol.2 Hackathon

## Problem

Direct anonymous attestation of membership proof

## Requirements

### Manufacturer

* Embeds a pair of asymmetric encryption keys in each device
* Manufacturer will construct a temporary X.509 certificate so that a proof does not need to be provided every time, temporary because the end user might want to stop even anonymous data collections.
X.509 digital certificates use the international X.509 public key infrastructure (PKI) standard to verify that a public key belongs to the identity contained in the certificate
* Manufacturer is willing to disclose the public keys of all its products openly
* Hacking the device on-premise to obtain these keys is prohibitively expensive to do (remote is required)
* Collects data from anonymous devices
* Check data received not corrupt
* Check that they manufactured the device.
  * Device can prove to the manufacturer that it is indeed one of the devices it has created

### End User

* Cryptographically-guaranteed that data collection from user devices without their consent is anonymous (impossible to trace their identity)

## Challenge

### Core

* Demonstrate how to implement an anonymous data collection scheme that:
  * Allows manufacturer to anonymously collect data from its end devices without knowing exactly which device it came from

### Bonus

* Manufacturer and device both anchor the challenge and proof onto the blockchain
* Device anchors its data transmissions onto the blockchain with the temporary certificate

## References:

* Paper - https://infoscience.epfl.ch/record/128718/files/CCS08.pdf
* Code implementation - https://github.com/ing-bank/zkproofs
* Hackathon - https://hack.longhash.com

## Additional References: 

* Xain AG (use their business model as contents)
