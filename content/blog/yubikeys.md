---
title: "Using Yubikeys for PGP with secure key backups"
date: 2020-09-07T12:21:04-07:00
draft: false
---

This article was originally written in 2018.

First, I really ought to say that I'm not recommending people do this, because everyone has their own appetite for risk, and everyone faces different threats online. That said, I did want to write up my own approach to keeping PGP keys safe and secure while ensuring I can restore a backup if I ever need to.

## Premise
I recently got some Yubikeys for to use as U2F devices to lock down my Google, Facebook, Github and Twitter accounts. I got two so that I'd have N+1 redundancy in case I lose one or one of them fails in a few years. I'm thinking about getting more, because I really don't want to lose access to my accounts, but that's beside the point. After playing around with them for a bit, I started thinking about using them for PGP. I'm no expert on PGP, but it seems like a good way to encrypt files that I only want myself to be able to decrypt. It's "supposed" to be used for email, but I have exactly one friend who uses PGP for email and I just use Signal to talk to her anyway, because it's 2018.

## Concerns
I like that I can put a private key onto the YubiKey and know that it's nominally impossible to export it from the device, but that poses problems if I want N+1 redundancy, since I'll need to either store a backup (encrypted or otherwise) or just trust that I'll never lose both keys. Unfortunately, I'm an expert at losing small things. I'm also very uncomfortable with the idea of storing a password-protected key and just trusting that I'll both remember the password and never use the password for something else. I could put it in a password manager, but at that point the password manager is my weakest link. What I want out of PGP is to have a way to encrypt something that cannot be cracked without both physically robbing me and figuring out my password via malware, phishing or coersion.

So passwords are out for encrypting my backup, which leaves PKI as the only viable encryption scheme, as far as I know. After thinking for a bit, I had the idea of encrypting the backup with my own public key. At first glance this idea seems very silly, since I'll need my private key to decrypt my own private key. But as I thought about it more, I realized that's exactly what I want. If I lose one YubiKey, all I have to do is decrypt my backup with the other and export the keys onto a newly-purchased YubiKey to get back to N+1.

## Security Implications
This is the one section you should probably take with several grains of salt, or maybe a salt lamp, since those are kinda cool. I'm not a security engineer. I know the basics of crypto but that's about it.

Basically, I claim that an attacker would need to get access to one of my YubiKeys, my PIN for that YubiKey, and my encrypted backup in order to get a copy of my private key. But at that point, if she has my YubiKey and my PIN, my private key is already compromised. So to my untrained eye, it seems like I have absolutely nothing to lose by uploading an encrypted backup of my private key to Google Drive or wherever else I want to put it. Which is wonderful because it means I have the security offered by keeping my keys off my filesystem and out of my safe, and the durability offered by having a backup of my key on Google Drive.

Besides, even if my key is compromised, I don't actually use PGP for anything important so I'll be fine :)