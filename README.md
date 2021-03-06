# A case for TLS: Backdooring Linux installation media

# Problem description:
  
   Servers and workstations with Gnu/Linux or bsd family unix operating systems have a well designed,tested and thought out security architecture.
   Administrators of these systems spend hours if not days of their lives securing and hardening their operating system installs.

   However a system cannot be secured if the installation of that system to begin with, has been compromised.
  
   This proof of concept project defines the threat model to be a man in the middle (MITM) attacker who has already compromised
   any number of devices between the victim's computer (or virtual-machine) and the server hosting the operating system's installation media.
   
   In this particular scenario the compromised computer is a linux machine that forwards and routes network traffic for the victim's computer.
 
   Using the proof of concept script and/or the well known open source tools used by the script, the attacker will be able to insert a revrese shell
   backdoor that he/she can later use to insert a more complex and targeted payload(s) to further compromise the victim's machine.

# Proposed solution

   Open source distribution maintainers should be using HTTPS (or any other connection with third party domain or server validation) for their installation media downloads.

   At the time of this writing most if not all major Linux distributions host installation media over HTTP and FTP connections.
  
   Their reasoning is this: 
   users can use cryptographic hash verification or GPG public key verification to validate the integrity and authenticity of the installation 
   media.

   Their approach has a few problems
   + It requires users to take the extra and often [relatively] complex set of steps to validate the installation media
   + It assumes users are aware of the security concerns and dangers at hand
   + The operating system distributions often host the PGP fingerprint along with cryptographic hash files over HTTP OR FTP.
       if the attacker can modify the download media,so can he/she modify these signatures
   + most current gpg installations use the HKP protocol to "refresh" and fetch the public key corresponding with the fingerprint,
       the HKP protocol in turn uses plain-text HTTP over a non-standard port for this action - the attacker who already has access
       to network traffic can simply serve the user a PGP fingerprint as well as corresponding public key over HKP to validate 
       the compromised installation media.

# The many imprefections of TLS

   TLS and the CA system are far from perfect. however simply using HTTPS will force the attacker to compromise the CA's or the user's computer 
   (to inject a malicious CA  certificate) before he/she can attempt the type of attack described above.

   HTTPS/TLS is not mutually exclusive with GPG verification or cryptographic hash verification.
   Using TLS does not protect against a compromised server or a malicious mirror-server.

   Performance over HTTPS  compared to HTTP is not significant enough to overlook the security beneifits of the protocol. 
   if performance is a problem there is nothing preventing distribution maintainers from offering HTTP side-by-side with HTTPS.

   It is not all that difficult to imagine (as the PoC will show) for a linux firewall or router to be compromised (does not need to be linux obviously).
   however there is a significant challenge and difficulty for a majority of malicious actors on the internet to compromise a Certificate Authority.

   Absolute security is neither purused or expected as a result of using TLS or HTTPS. the only thing expected is a significant improvement in integrity
   and authenticity validation  of installation media. 


# PoC usage:

Please bear in mind, the PoC is still under continued development.

Usage: `poc.sh <Victim's IP>  <Command and control IP>`


