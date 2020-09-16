---
layout: post
title:  "Password Cracking (Adopting a reasonable password hygiene)"
author: Didier Guillevic
date:   2020-09-13
categories: cybersecurity password
---

The main idea is about adopting a better password hygiene.
But first, as a motivation, it is important to understand how passwords are stored,
leaked and eventually cracked. In short, how vulnerable our various password
secured accounts can be.

As an introduction to the field of password cracking, one could mention an interesting
[post](https://medium.com/hackernoon/20-hours-18-and-11-million-passwords-cracked-c4513f61fdb1)
on how an "everyday developer" can crack millions of passwords in one day for less than 20$.
One should also watch a very entertaining [video](https://youtu.be/yzGzB-yYKcc) with Edward Snowden on passwords.
Finally, one can check if our information (web accounts, email addresses and passwords) has been pawned
(offered for sale on the [dark web](https://en.wikipedia.org/wiki/Dark_web)) by visiting
[Have I Been Pwned](https://haveibeenpwned.com). That site has information on
over 9 billion pawned accounts and half a billion unique cracked passwords (as of 2019-01).
We can check if our [email address](https://haveibeenpwned.com) has been pawned,
and whether our [passwords](https://haveibeenpwned.com/Passwords) are already among
the lists of cracked passwords, in which case they offer zero protection if targeted.

### Summary - [TL;DR](https://en.wikipedia.org/wiki/Wikipedia:Too_long;_didn't_read)

**How passwords are stored**

1. Plain text: "secret password" &rarr; "secret password"
2. [Hashed](https://en.wikipedia.org/wiki/Cryptographic_hash_function): [SHA256](https://en.wikipedia.org/wiki/SHA-2)("secret password") &rarr; "1ba133eccdfc4e5ca3...504a4b14c94b8557"
3. [Salted](https://en.wikipedia.org/wiki/Salt_(cryptography)) Hash: sha256("secret password" + ”B7aU3L") &rarr; "a1296cc48338...045136420"
4. [Key stretching](https://en.wikipedia.org/wiki/Key_stretching): pbkdf2( “sha256”, “secret password”, ”B7aU3L”, 100000 ) &rarr; “0274d5...82a4”

**How passwords are cracked**

1. Brute force (short password, network of computers, ...)
2. Use of lexicons + hand crafted rules on human behaviour when creating passwords
3. Language models (deep neural networks) (to generate candidate passwords)
4. Generative Adversarial Networks (to generate candidate passwords)

**Advice**

1. Use a different password for each account / web site
2. Use a passphrase instead of a password ([Snowden](https://youtu.be/yzGzB-yYKcc),
   [Better passwords](https://blog.1password.com/toward-better-master-passwords/),
   [Information entropy](https://blog.1password.com/better-master-passwords-the-geek-edition/))
3. Use a [password management tool](https://www.howtogeek.com/141500/why-you-should-use-a-password-manager-and-how-to-get-started/)
   (e.g. [Bitwarden](https://bitwarden.com) (open source),
   [NordPass](https://nordpass.com), [1Password](https://1password.com))
4. Use [two factor identification](https://www.howtogeek.com/117047/htg-explains-what-is-two-factor-authentication-and-should-i-be-using-it/)
   (e.g. [Authy](https://authy.com),
   [Google Authenticator](https://en.wikipedia.org/wiki/Google_Authenticator),
   [FreeOTP](https://freeotp.github.io))
5. Use a VPN app on public Wi-Fis:
   [ProtonVPN](https://protonvpn.com), [NordVPN](https://nordvpn.com),
   [ExpressVPN](https://www.expressvpn.com)

### Commonly used passwords

Wikipedia has a list of the [10,000 most commonly used passwords](https://en.wikipedia.org/wiki/Wikipedia:10,000_most_common_passwords), the first five of which are (in alphabetical order):
_123256_, _123245678_, _123456789_, _password_ and _qwerty_. Needless to day, those are not
very secure passwords.

I went on to a get a relatively small list of cracked passwords from [hashes.org](https://hashes.org)
corresponding to a given hacked web site and displayed the first few hundred most
frequent ones with a [word cloud](https://amueller.github.io/word_cloud/).

<center>
<img src="/assets/images/DG_passwords.jpg" title="Cracked passwords visualization" width="600" />
</center>

That small list, corresponding to one given hacked site, night not be representative of
the passwords in use worldwide, but it is still informative.
We can see the usual suspects (those that are part of the [10,000 most commonly used passwords](https://en.wikipedia.org/wiki/Wikipedia:10,000_most_common_passwords)
such as: _12345678_, _123456789_ and _password_.
We can also spot some passwords that seem very hard for a human to remember such as
"e10adc3949ba59abbe56e057f20f883e" and "5f4dcc3b5aa765d61d8327deb882cf99".
Those passwords only give a false sense of security to the people who use them,
as they are only one known function call away ([MD5](https://en.wikipedia.org/wiki/MD5))
from extremely trivial passwords, namely the first two most commonly used passwords:
_123456_ and _password_ respectively.
```
123456 --- MD5 ---> e10adc3949ba59abbe56e057f20f883e
password --- MD5 ---> 5f4dcc3b5aa765d61d8327deb882cf99
```

### Password storage

One can consider that there are four password storage schemes, namely plain text,
hashed, salted hash and finally key stretching (in order of increased strength).
The first three schemes are depicted in the diagram below.

<center>
<img src="/assets/images/DG_password_storage_schemes.png" title="Password Storage Schemes" width="800" />
</center>


#### 0. Password: Plain text

The first option, plain text, is no scheme at all and should never be used.
As the passwords are stored in clear (plain text) in the database,
in case of data breaches, all of the login information is readily available for criminals.

When used, it is mostly unintentional (a software [bug](https://en.wikipedia.org/wiki/Software_bug)),
like in the case of
[Twitter](https://krebsonsecurity.com/2018/05/twitter-to-all-users-change-your-password-now/) in 2018,
[Facebook](https://about.fb.com/news/2019/03/keeping-passwords-secure/) in
[2019](https://krebsonsecurity.com/2019/03/facebook-stored-hundreds-of-millions-of-user-passwords-in-plain-text-for-years/) and [Google GSuite](https://cloud.google.com/blog/products/g-suite/notifying-administrators-about-unhashed-password-storage) in 2019.

Unfortunately, sometimes, plain text is used out of over-confidence in one's ability as in the case of
[TMobile Australia](https://www.theverge.com/2018/4/10/17219118/t-mobile-austria-password-plaintext-fixed-security) in [2018](https://www.cnet.com/news/t-mobile-hack-may-have-exposed-2-million-customers-data/)
or [Adobe](https://www.neowin.net/news/adobe-leaks-150-million-passwords-facebook-and-others-impacted)
in [2013](https://nakedsecurity.sophos.com/2013/11/04/anatomy-of-a-password-disaster-adobes-giant-sized-cryptographic-blunder/) (password hints stored in plain text).

#### 1. Password: Hashed

A better solution, the first viable one, is to store the result of passing the
password through a [cryptographic hash function](https://en.wikipedia.org/wiki/Cryptographic_hash_function),
which is a *one-way* function that converts any string into gibberish.
One such function is the Secure Hash Algorithm 256 ([SHA-256](https://en.wikipedia.org/wiki/SHA-2))
that converts any string (e.g. a password) into 256 [bits](https://en.wikipedia.org/wiki/Bit) of information,
which are represented as 64 characters in the range [0..9a..e].

With this scheme, the passwords are never stored in clear plain text.
Instead, it is the hashed versions of those passwords (gibberish) that get
stored in a database. When a user wishes to log on, the typed password is hashed
and then compared with the value stored in the database for that given user.
The passwords are never explicitly compared; it is the hashes that are compared.

Note that it is a key requirement for a cryptographic hash function that two different
passwords are extremely unlikely to be converted to the same hash value.
Such an event would be called a [collision](https://en.wikipedia.org/wiki/Collision_(computer_science)).

The key advantages of hashing are that there is no need to ever store the
passwords in clear text. A very good cryptographic hash function is required in
order to ensure that the probability of a collision (two passwords hashing to the same value)
is extremely low.

While it is a major step up from storing passwords in clear text, and the first
viable option, this scheme is still vulnerable to various known attacks.
For example, an attacker could pre-hash a vast amount of passwords (say 1 billion)
using a list of cracked passwords as well as generating new passwords using some
pre-defined rules. Armed with that list of known hash values, an attacker
could then verify if a leaked password's hash is present in his database.
It is feasible, even with a modest laptop to compare billions of hash values in a few seconds.
For more information, the interested reader could refer to
[rainbow tables](https://en.wikipedia.org/wiki/Rainbow_table).

#### 2. Password: Salted Hash

An improvement over the previous scheme, and to counter
[rainbow table attacks](https://en.wikipedia.org/wiki/Rainbow_table),
is to use a cryptographic [salt](https://en.wikipedia.org/wiki/Salt_(cryptography)) value.
A salt is a, preferably large, random value assigned to each user of a given site (value different for each user).
That salt is added to the user's password before being sent to the hash function.
In doing so, even if two users have the same password, their respective
password hash values will be different (since their respective salt values are different).
The goal is to slow down a potential attacker as he/she will not be able to
use a database of pre-computed hash values for billions of passwords.
Instead, the attacker will need to call the cryptographic hash function
billions of time for each user, which will slow the attack by several order
of magnitude. For example, instead of a day of compute time to crack a million
passwords, the attacker might need a few years instead.

While it is still possible to target one given user account, the cracking of
millions of passwords from a leaked database becomes a very very lengthy task.

Note however that using a salted hash scheme is crippled if one uses the same
salt value for each user. To summarize, the salt values should be long,
randomly generated and different for each user account.

#### 3. Password: Key stretching

One further step up is to use [key stretching](https://en.wikipedia.org/wiki/Key_stretching)
which further slows down a potential attack by several order of magnitude.
With key stretching, the cryptographic hash function is called recursively
a significant amount of times (e.g. 100,000 times) before generating the
final hash value for a given password.

Key stretching, used in combination with a long random cryptographic salt different for each user,
renders an attack on a leaked database of password hashes extremely hard to perform.

<center>
<img src="/assets/images/DG_password_key_stretching.png" title="Password Key Stretching Scheme" width="800" />
</center>

#### 4. Yet another scheme: encryption

Actually, there is yet another scheme, although not an entirely advisable one.
Some companies such as [Adobe](https://krebsonsecurity.com/2013/11/facebook-warns-users-after-adobe-breach/),
[encrypt](https://en.wikipedia.org/wiki/Encryption) (not hash) all of the user information
(e.g. passwords) with a single encryption key.
If an attacker manages to get a hold of the decryption key, then he/she gets immediately
access to *all* passwords in the database. Another security / privacy issue is that
the database administrator would have the ability to see all the passwords in clear text
(by decrypting them).

### Data Breaches

Data breaches occur rather frequently. For news about the latest breaches, one
could refer to the [Twitter account](https://twitter.com/haveibeenpwned)
for the site Have I Been Pwned.

[Have I Been Pwned](https://haveibeenpwned.com) gathers information about those
breaches and offers services on its web site that allows us to check if information
about our email accounts has been offered for sale on the dark web,
or if our passwords, or the ones we are planning on using, are already on a list
of known passwords. Have I Been Pwned maintains a database of known cracked password hashes
without revealing the plain text passwords.
The site has information on 10 billions pawned accounts and half a billion real world
passwords previously exposed in data breaches.

Another site gathering information on data breaches is [Hashes.org](https://hashes.org)
which maintains a database of 3.5 billions cracked password hashes as well as
1 billion uncracked hashes.

Data breaches are doomed to happen. One issue that renders a breach problematic is that
cryptographic hash functions that are known to be insecure are still in use today.
For example, the [MD5](https://en.wikipedia.org/wiki/MD5) function has been
demonstrated to be insecure as early as
[2005](https://www.keepersecurity.com/blog/2017/04/18/whats-the-big-deal-with-hashing-and-why-should-i-care/).
It was shown that collisions (text that hash to the same value as the unknown password)
could be [found in seconds](https://searchsecurity.techtarget.com/definition/MD5).
With such an exploit, the real password is not needed
anymore as the new collision text found is sufficient to gain access to the desired account
(since its hash will match the one stored in the targeted system).
Even though MD5 should have been abandoned shortly after 2005, some companies,
such as Yahoo, [Cisco, McAfee and Symantec](https://jumpespjump.blogspot.com/2014/03/stop-using-md-5-now.html)
did not update their system. In 2015, some
[200 million Yahoo credentials](https://www.keepersecurity.com/blog/2017/04/18/whats-the-big-deal-with-hashing-and-why-should-i-care/) went up for sale.

Another popular cryptographic hash function, [SHA-1](https://en.wikipedia.org/wiki/SHA-1),
has been deprecated in 2011 by NIST, with an [insecurity demonstrated in 2017](https://shattered.io).
Consequently, it should also not be used anymore.

The largest known data breaches are from [Quora in December 2018](https://en.wikipedia.org/wiki/Quora#2018–present:_Further_growth_and_data_breach), with 100 million
user accounts affected by a data breach,
[Facebook saw 90 million accounts](https://www.wired.com/story/facebook-security-breach-50-million-accounts/)
affected in September 2018 and the largest was
[3 billion Yahoo user accounts](https://en.wikipedia.org/wiki/Yahoo!_data_breaches)
being breached in 2017. For Yahoo, the information released consisted in names,
email addresses, telephone numbers, encrypted and unencrypted security questions
and the corresponding answers, dates of birth and finally hashed passwords.

### Password Cracking

When a data breach occurs, the released information usually consists in a file,
potentially with millions of entries, with each line representing information about a user:
his/her user name (such as an email to gain access to a given site) and the
corresponding hash value of the password.

Cracking passwords consists in recovering the text (password) corresponding to a given hash value.
With such information, one can access the user account on the targeted website.

#### 1. Brute Force

The first basic straightforward approach is [brute force](https://en.wikipedia.org/wiki/Brute-force_attack),
which consists in trying out all possible combinations of characters.
This is feasible, and can be performed extremely fast, for passwords up to 8 characters.
This approach would be used when targeting one given user account and is
adequate when the length of the password is relatively small (e.g. 8 characters)
or alternatively when having access to large computing power (e.g. a network of computers).

Some 20 years ago, in 1998, a single machine called
[Deep Crack](https://en.wikipedia.org/wiki/EFF_DES_cracker) was designed to
test the robustness of the DES standard. At that time, it took less than 3 days
to crack a given message, and the machine was able to test 90 billion keys per second.

More recently, in 2012, a [25 GPU cluster](https://arstechnica.com/information-technology/2012/12/25-gpu-cluster-cracks-every-standard-windows-password-in-6-hours/) could crack every standard Windows
password in less than 6 hours.

There were also distributed efforts like [distributed.net](https://www.distributed.net),
a distributed computing project, where volunteers donate the power of their home
computers when not in use (nights) to tackle a given project, such as testing
the strength of a given encryption standard like [RC5](https://en.wikipedia.org/wiki/RC5).

#### 2. Lexicon + rules

The brute force approach is not feasible when one wishes to attack the millions
of passwords that can be found in a typical data breach. Therefore, the second approach
uses our domain knowledge in order to build up a list of feasible passwords to try
(rather than an exhaustive search of *all* the possible passwords).

That list is first initialized with real life passwords that have been leaked to the
outside world. Remember that companies such as
[T-mobile Australia](https://www.theverge.com/2018/4/10/17219118/t-mobile-austria-password-plaintext-fixed-security)
did not bother, until recently (2018), to hash/encrypt the passwords.
Also due to some software bugs, some companies may inadvertently store passwords
in clear text (such as [300 million Twitter passwords](https://krebsonsecurity.com/2018/05/twitter-to-all-users-change-your-password-now/)). One can start with, in the order of, 1 billion
real world passwords with data gathered from leaks by sites such as [hashes.org](https://hashes.org).
One can also gather ordinary language dictionaries.

Next is using knowledge on human behaviour in order to design some rules to
transform each and every of those words present in the initial list.
Rules include capitalizing the first and/or last letter of a word
(secret --> Secret, secreT, SecreT), adding numbers at the end of a word
(secret --> secret123), and replacing some well defined characters with their
identical looking digit (hello --> he11o, h3llo, hell0).
Those transformations yield some additional potential passwords to check.

There are open source tools, such as [John the Ripper](https://en.wikipedia.org/wiki/John_the_Ripper)
and [Hashcat](https://en.wikipedia.org/wiki/Hashcat), that offer, among other types,
those dictionary based attacks.

#### 3. Language models

As humans are keen on re-using passwords and are predictable in the transformations
they apply to existing dictionary words, the lexicon based approaches as
implemented in John the Ripper and Hashcat are very efficient in retrieving
(cracking) a large proportion of the password hashes found in data breaches.

For those remaining passwords that are not found by 'basic' dictionary + rules
approaches, there is a need to generate large amount of feasible passwords.
However, recall that we do not wish to make an exhaustive search (as in brute-force),
as this would be prohibitively expensive with respect to computing resources.

One solution is to train a [statistical language model](https://en.wikipedia.org/wiki/Language_model)
using, as training data, the vast amount of cracked leaked passwords at our disposal.
Once trained, the model is then used to generate huge quantities (in the billions)
of feasible passwords (according to the language model).
Note that a lot of those passwords will not be present in existing lexicons.
Consequently, they can complement the lexicon based approaches by providing
additional passwords to check.

Previously (not so long ago), language models were based on
[n-grams](https://en.wikipedia.org/wiki/N-gram) and
[Markov models](https://en.wikipedia.org/wiki/Markov_model)
(e.g. [Language modelling notes](http://www.cs.columbia.edu/~mcollins/courses/nlp2011/notes/lm.pdf)).
Nowadays, language models are built using [artificial neural networks](http://www.scholarpedia.org/article/Neural_net_language_models).
For an interesting article on building a language model to generate passwords,
the reader might look into this
[2016 paper](https://www.usenix.org/conference/usenixsecurity16/technical-sessions/presentation/melicher)
and its associated [code](https://github.com/cupslab/neural_network_cracking) implementation.
As for a tutorial on building a character level recurrent neural networks
(that would be used to train a password language model), there is a interesting
[PyTorch notebook](https://pytorch.org/tutorials/intermediate/char_rnn_generation_tutorial.html) available.

#### 4. Generative Adversarial Networks (GANs)

With regard to [generative models](https://en.wikipedia.org/wiki/Generative_model)
(systems able to generate new unseen data), one should not omit the relatively new scheme
that are [Generative Adversarial Networks](https://en.wikipedia.org/wiki/Generative_adversarial_network) (GANs).
Originally, and usually, GANs are trained on a set of images and subsequently
used to generate new unseen images that "live" in the same subspace as the training images.
A typical example is generating new human faces such as the paper from NVIDIA on
[Progressive GANs](https://arxiv.org/pdf/1710.10196.pdf) published in 2018, and
[StyleGANs](https://github.com/NVlabs/stylegan2) in early 2020.
Some of those randomly generated images are displayed below.
Using [StyleGANs](https://github.com/NVlabs/stylegan2), you can see additional
randomly generated and realistic images of persons and cats using the websites
[thispersondoesnotexist.com](https://thispersondoesnotexist.com) and
[thiscatdoesnotexist.com](https://thiscatdoesnotexist.com).

{% include image.html
  file="/assets/images/Progressive_GANs_NVIDIA_ICLR_2018.png"
  alt="Progressive Growing of GANs - NVIDIA - ICLR 2018"
  caption="Progressive Growing of GANs - NVIDIA - ICLR 2018"
  max-width="800"
  url="https://arxiv.org/pdf/1710.10196.pdf"
%}

Lately, GANs have also been applied to natural language processing tasks, with papers
such as [SeqGAN](https://arxiv.org/abs/1609.05473) in 2017 and
[Adversarial Text Generation](https://arxiv.org/abs/1810.06640) in 2018.
In 2019, [PassGAN](https://arxiv.org/abs/1709.00440) ([code](https://github.com/brannondorsey/PassGAN)),
used such an approach to model and consequently generate realistic passwords that
could then be fed to a dictionary based attack.

To summarize, generative models (language models and GANs) are used to augment
dictionary tools such as John The Ripper and Hashcat by providing billions of
unseen and realistic passwords.

### Toward Better Passwords

Now towards generating better passwords for ourselves that are less likely to
be cracked by the aforementioned tools. The picture from [xkcd.com](https://xkcd.com/936/)
(shown below) summarizes the issues and the solution extremely well.

{% include image.html
  file="/assets/images/xkcd.com_936_password_strength.png"
  alt="Password Strength (xkcd.com/936)"
  caption="Password Strength (xkcd.com/936)"
  max-width="800"
  url="https://xkcd.com/936/"
%}

One issue is that we are often asked (required) to create passwords containing
uppercase letters, numbers and special characters, while sometimes limiting the
password's length to be between 8 and 20 characters. Both of those constraints are wrong.

First, on the issue of password length, while it is a no-brainer that passwords
have to be longer than 8 characters, there should not be any restrictions on the
upper limit for that length. Certainly, 20 characters is way too short for the
kind of strong passwords we wish to generate for ourselves: [passphrases](https://en.wikipedia.org/wiki/Passphrase).

Second, on the issue of requiring uppercase letters, digits and special characters,
humans are highly predictable. This predictability is translated into rules
for dictionary based attacks. For instance, most of the time, people will uppercase
the first or last letter of the word. As for numbers, they will either add a number after
a word or change certain letters (in a predictable way) for certain
[similar looking](https://wmich.edu/arts-sciences/technology-password-tips) numbers.
Regarding special characters, only [a handful](https://i.imgur.com/aoIa6UX.png)
represent the vast majority of occurrences in passwords, and these are usually found
in predictable positions, following [well defined rules](https://wmich.edu/arts-sciences/technology-password-tips)
such as replacing a letter 'a' with the '@' character, or a letter 'l' with
the exclamation point.

As depicted in the diagram from xkcd.com above,
a [password strength](https://en.wikipedia.org/wiki/Password_strength) is a function
of length and unpredictability. The length of a password should [definitely be
longer than 8 characters](https://youtu.be/yzGzB-yYKcc).
If that's not the case, by a brute force attack, it can be cracked in seconds.
The unpredictability of a password refers to the notion of
[entropy](https://en.wikipedia.org/wiki/Entropy_(information_theory)) in information theory.
(For a great visual introduction to entropy, Christopher Olah's
[blog post](https://colah.github.io/posts/2015-09-Visual-Information/) is highly recommended.)

In order to create a very strong password (that will also be easy to remember for a human),
one can visualize some image and describe it with at least 4 words.
Those words should not be expected to occur together in a "normal" text
(not very likely to be generated by a language model).
From the xkcd diagram above, one can create the passphrase "correct horse battery staple".
Notice that those words do not usually appear together in a sentence, and are not
really related. The word '[stable](https://www.merriam-webster.com/dictionary/stable)'
would have been more related to 'horse' than '[staple](https://www.merriam-webster.com/dictionary/staple)'.
Hence 'horse staple' has a higher entropy (more of a surprise effect, less likely) than 'horse stable'.
Another example would be "submarine balloon cloud goeland" describing the image below:

{% include image.html
  file="/assets/images/dreamstime.com_submarine.png"
  alt="Submarine Cloud Balloon"
  caption="dreamstime.com"
  max-width="200"
  url="dreamstime.com"
%}

This idea is relatively old ([Arnold Reinhold's Diceware](http://world.std.com/~reinhold/diceware.html) - 1995).
Such passphrases have a high entropy and are consequently very hard to crack.
Note that no capital letters, digits and special characters were used when creating those passphrases.
They are simply not required for a very strong and easy to remember password.

#### Password Management Tool

Part of a good password hygiene is to use a different password for each site / account
that we have. It is hard (impossible) to do so without resorting to a password manager.

Password managers range from open source, freemium and commercial.
Some examples applications are [BitWarden](https://bitwarden.com) (open source),
[NordPass](https://nordpass.com) (freemium), [1Password](https://1password.com) (commercial)
and Apple's [iCloud Keychain](https://support.apple.com/en-ca/HT204085) functionality on iOS and macOS devices.
They have extension for the popular browsers which makes it practical and effortless
to log onto our accounts. Furthermore, some also offer two-factor authentication
for increased security. Do your research, and choose one that you trust.

#### Multi-factor authentication

[Multi-factor authentication](https://en.wikipedia.org/wiki/Multi-factor_authentication)
refers to the notions of:
- Something you have: a physical object like a bank card, a key, a [USB stick](https://www.yubico.com), ...
- Something you know: a password, a Personal Identification Number (PIN), ...
- Something you are: physical characteristics of a person (biometrics) like a fingerprint,
  a [face](https://en.wikipedia.org/wiki/Facial_recognition_system) (scan),
  voice print, [typing speed](https://www.typingdna.com), ...
- Somewhere you are: GPS signal to identify your location, a specific computing network connection, ...

#### Two-factor identification

Access to our accounts should not rely solely on a password (something we know).
It should also require at least a second authentication factor.
Nowadays, a lot of sites offer the option of requiring two factor authentication
to log onto their services like for example, password managers ([BitWarden](https://bitwarden.com)),
email accounts ([Gmail](gmail.com), [ProtonMail](https://protonmail.com)).

The dominant multi-factor authentication method for consumer facing accounts
is a code sent to an email address or an SMS sent to our phone.
While those are better than no second factor at all, as always, there are some issues.
If we give our main email account for second factor authentication and its
login details have just been leaked, then the attacker will get access to *all*
of our accounts for which we gave that email address as a two-factor.
For SMS, someone can ([unfortunately quite easily](https://www.cbc.ca/marketplace/episodes/2018-2019/hack-off-how-hackers-take-over-your-accounts-using-social-engineering))
'steal' our phone number.
This attack is known as [SIM porting fraud](https://www.cbc.ca/news/canada/toronto/number-porting-fraud-advice-1.5377237).

A better option could be to use [one time password](https://en.wikipedia.org/wiki/One-time_password)
generators as a
[two-factor authentication](https://www.howtogeek.com/117047/htg-explains-what-is-two-factor-authentication-and-should-i-be-using-it/).
With such a scheme, a different 6 digit code is generated every 30 seconds.
Twenty years ago, those codes were generated by physical devices such as
the [RSA SecurID](https://en.wikipedia.org/wiki/RSA_SecurID)
([still in use](https://www.rsa.com/content/dam/en/data-sheet/rsa-securid-hardware-tokens.pdf)).
Today, one has the option of using free software apps that we can install on
our mobile devices (phones, [AppleWatch](https://apps.apple.com/us/app/twilio-authy/id494168017#?platform=appleWatch))
such as [Authy](https://authy.com), [Google Autheticator](https://en.wikipedia.org/wiki/Google_Authenticator)
or [FreeOTP](https://freeotp.github.io) to name a few.

#### Virtual Private Networks (VPNs)

Once we have crafted a strong password, it is important not to reveal that password in the open.
One situation where that might happen is when using free open public Wi-Fis such as when
visiting our favourite coffee shop.
It is [rather](https://youtu.be/kRTLlXaUcV4) [really](https://www.youtube.com/watch?v=XcghUy-8VRA)
[easy](https://www.vpnmentor.com/blog/10-reasons-really-need-stop-using-public-wifi/),
even for a [7-year old](https://www.dailymail.co.uk/sciencetech/article-2919762/Hacking-Wi-Fi-s-child-s-play-Seven-year-old-shows-easy-break-public-network-11-minutes.html)
to snoop on our web traffic (e.g. typing) and steal our valuable information
(e.g. login information, passwords, etc...).

Therefore, when logging into public Wi-Fi, one should use a
[Virtual Private Network](https://en.wikipedia.org/wiki/Virtual_private_network) (VPN) software.
Nowadays, they are extremely easy to use (just an app installed on the phone).
There are free options (sufficient when logging onto public Wi-Fis) such as
[ProtonVPN](https://protonvpn.com) (which also offers a paid version for more options
on server locations), or paid options such as
[NordVPN](https://nordvpn.com) or [ExpressVPN](https://www.expressvpn.com).

### Resources

- Credentials compromised?: [Pawned email addresses](https://haveibeenpwned.com), [Pawned passwords](https://haveibeenpwned.com/Passwords)
- Leaked passwords lists: [hashes.org](https://hashes.org/leaks.php),
  [Openwall wordlists collections](https://www.openwall.com/wordlists/)
- Tools:
  [John the Ripper](https://en.wikipedia.org/wiki/John_the_Ripper),
  [Hashcat](https://en.wikipedia.org/wiki/Hashcat),
  [Aicrack-ng](https://en.wikipedia.org/wiki/Aircrack-ng),
  [Neural Language Model](https://github.com/cupslab/neural_network_cracking),
  [PassGAN Language Model](https://github.com/brannondorsey/PassGAN)
- Advice:
  1. Use a different password for each account / web site
  2. Use a passphrase instead of a password ([Snowden](https://youtu.be/yzGzB-yYKcc),
   [Better passwords](https://blog.1password.com/toward-better-master-passwords/),
   [Information entropy](https://blog.1password.com/better-master-passwords-the-geek-edition/))
   3. Use a [password management tool](https://www.howtogeek.com/141500/why-you-should-use-a-password-manager-and-how-to-get-started/)
   (e.g. [Bitwarden](https://bitwarden.com) (open source), [NordPass](https://nordpass.com),
   [1Password](https://1password.com))
   4. Use [two factor identification](https://www.howtogeek.com/117047/htg-explains-what-is-two-factor-authentication-and-should-i-be-using-it/)
   (e.g. [Authy](https://authy.com),
   [Google Authenticator](https://en.wikipedia.org/wiki/Google_Authenticator),
   [FreeOTP](https://freeotp.github.io))
   5. Use a VPN app on public Wi-Fis:
   [ProtonVPN](https://protonvpn.com), [NordVPN](https://nordvpn.com),
   [ExpressVPN](https://www.expressvpn.com)
- Watch [Edward Snowden's video on passwords](https://youtu.be/yzGzB-yYKcc)
- Bonus: Get familiar with entropy (information theory)
  - Entropy: measure of how surprised we are about the next character or word
  - Chris Olah’s post (highly recommended): [Visual Information Theory](https://colah.github.io/posts/2015-09-Visual-Information/)
  - Better Master Passwords: [The geek edition](https://blog.1password.com/better-master-passwords-the-geek-edition) (1Password)
