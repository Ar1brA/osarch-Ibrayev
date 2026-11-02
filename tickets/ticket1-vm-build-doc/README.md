# Ticket 1 — VM build document

## 1. What was asked

From: Aigerim T. (Office Manager) Subject: test machine for new dev

Hi, we have someone starting on Monday and he needs a test computer set up. Nothing special, just so he can try things without breaking anything. Can you write down how to do it so I can do the next one myself? Thanks

## 2. What was actually needed

What the request left out

Which operating systems. "Nothing special" does not tell us what a developer needs. I assumed two guests: Ubuntu Server (Linux dev/test target) and Windows 11 Enterprise evaluation (Windows testing). To test his knowledge.
Host resources. The request does not say how much RAM/disk the office computer has. Everything depends on it (see calculation below).
Network. Not stated whether the guests need internet or need to reach each other. I assumed both: internet for updates and package installs, guest-to-guest for testing client/server software.
Hypervisor, licence, ISO. Aigerim must not buy anything by mistake. I chose Newest VirtualBox because it is free and easy to use, and the evaluation edition of Windows 11 Enterprise because it is free from Microsoft and legal for testing (it is time-limited, about 90 days, and not for production).
Recovery. "Without breaking anything" is only true if there is a way back, so snapshots are part of the document, not an extra.

Where the request is imprecise. 
"Without breaking anything" suggests the developer might break the computer. A VM isolates the guest from the host: if the guest is wrecked, the host is unaffected, and a snapshot returns the guest to a known-good state in minutes. The thing that can actually be broken is the guest, and the thing at risk is time, not hardware.



## 3. What I did

## 4. Evidence

## 5. What I would do differently

At least one thing. "Nothing, it all went well" is not an answer.
