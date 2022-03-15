
# PicoCTF – keygenme-py

* **Category:** Reverse Engineering
* **Points:** 30

## Challenge
[keygenme-trial.py](https://mercury.picoctf.net/static/0c363291c47477642c72630d68936e50/keygenme-trial.py)

## Solution
This challenge gives you a python script. When running it, we see that we need to enter a license key to unlock the full version. This license key is the flag for the challenge.  

We can use cat to view the code, and see the first part of the flag, 
`picoCTF{1n_7h3_|<3y_of_` as well as a bunch of Xs which likely need to be replaced.
Scrolling down we see that the code is checking the key against indexes of an sha256 hash of username_trial, which is in this case "MORTON"

<code> if key[i] != hashlib.sha256(username_trial).hexdigest()[4]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[5]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[3]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[6]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[2]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[7]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[1]:
            return False
        else:
            i += 1

        if key[i] != hashlib.sha256(username_trial).hexdigest()[8]:
            return False



        return True
</code>
	Taking this part of the code and running it with python3 after importing hashlib and reassigning <code>username_trial = b"MORTON"</code> (the b so it is encoded as bytes instead of str) we get some letters and numbers. Combining them after the first part of our flag, and adding `}` we get the flag

FLAG
`picoCTF{1n_7h3_|<3y_of_75fc1081}`