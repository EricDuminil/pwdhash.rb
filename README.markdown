# PwdHash.rb

Generates PwdHash passwords from the command line.

## What is this?

This is a simple Ruby command line tool to generate hashed passwords, using PwdHash's algorithm.

PwdHash was written as a browser plug-in, but sometimes we'd like to extend its use to desktop applications too. e.g. IM clients, Dropbox, etc. just to name a few.

This tool is essentially a *straight* copy of [Chris Roos' implementation][chris-roos-impl], with a couple of command line interface improvements:

1. The password is masked properly when the user is typing it.
2. The realm is specified as an argument instead of prompted.
3. The prompt text is sent to STDOUT instead of STDERR so the output can be piped to e.g. `xclip` or `putclip`.

## What is PwdHash?

[Standford PwdHash][stanford-pwdhash] helps you create theft-resistant passwords for each realm (domain name). In the case that your email password gets stolen, the attacker won't be able to log in your PayPal account to transfer all your money, even though you only have to remember one password for all logins. The [USENIX Security Symposium 2005 paper][usenix-paper] (PDF) explains it in details.

## Installation

    gem install pwdhash

## Example/Usage

    $ pwdhash example.com
    Password for example.com:
    5NBoCKraALBs

    $ pwdhash example.com | xclip # Put generated password to X clipboard (paste with middle-click)
    $ pwdhash example.com | xclip -selection c # Put generated password to X clipboard (paste with CTRL+V)
    $ pwdhash example.com | pbcopy # Or in OS X
    > pwdhash example.com | clip # Or in Windows
    $ pwdhash example.com | putclip # Or in Cygwin

## PwdHash2

Some vulnerabilities of PwdHash have been exposed in [Cracking PwdHash: A Brute-force Attack on Client-side Password Hashing][cracking-pwdhash], along with a [proof-of-concept][pwdhash-poc] update to PwdHash. The updated algorithm (with PBKDF2-SHA256 + salt + multiple iterations) has been implemented as a Firefox Add-On at [GWuk/PwdHash2][gwuk-pwdhash2].

The corresponding algorithm can be used with:

    $ pwdhash2 URI [SALT] [ITERATIONS]

    $ pwdhash2 example.com ReplaceThisSalt 50_000
    Password for example.com (salt = 'ReplaceThisSalt', iterations = 50000):
    IHC8WhmO40v

*SALT* can be defined in `PWDHASH2_SALT` environment variable.

*ITERATIONS* can be defined in `PWDHASH2_ITERATIONS` environment variable.

    $ pwdhash2 example.com | xclip -selection c # Put generated password to X clipboard (paste with CTRL+V)

## Attribution

The library part of this tool is a *straight* copy of [Chris Roos' implementation][chris-roos-impl]. The reason this git repository is set up is because: a) Chris Roos decided to put the code in an obscure corner in a pile of code which makes it hard for people to find and b) GitHub uses git :p

The PwdHash plug-in and the PwdHash algorithm is contributed by [Stanford PwdHash][stanford-pwdhash].

The PwdHash2 algorithm is contributed by [David Llewellyn-Jones and Graham Rymer][pwdhash-poc].

[chris-roos-impl]: http://chrisroos.co.uk/blog/2007-04-11-getting-to-grips-with-pwdhash
[stanford-pwdhash]: http://pwdhash.com
[usenix-paper]: http://crypto.stanford.edu/PwdHash/pwdhash.pdf
[cracking-pwdhash]: https://www.researchgate.net/publication/316267246_Cracking_PwdHash_A_Bruteforce_Attack_on_Client-side_Password_Hashing
[pwdhash-poc]: https://github.com/llewelld/pwdhash-poc
[gwuk-pwdhash2]: https://github.com/GWuk/PwdHash2/
