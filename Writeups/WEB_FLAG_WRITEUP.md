FLAG - PClub{Easy LFI}

Well this one was a nightmare(I used to run away from web).

So my first instinct after opening the website was well /sitemap.xml and /robots.txt which well did not work.

then i thoroughly checked the source and inspected all the related webpages but still nothing.

next I did steg analysis on all the photos available on the whole website and found nothing in them also.

then I found the link for the localhost of grafana and decided to try 1=1 and random break-throughs until user admin and pass admin worked.

When I saw the option to change password for the admin , I was reluctant to do that as I thought that this might destroy the integrity of the challenge but after waking up next day I find myself logged out with the password changed and then I was well ready to war.

Found various grafana vulnerabilities due to it being the older version 8.3.0 and found various CVE's which explained the vulnerabilities 
and I had to basically find a exploit to them. 

Used nmap to scan all the available ports and googled a lot to find ways to use any port if possible.

Used nikto to scan the website and also find any sub-domains which could be of use.

tried sqlmap and burpsuite and still did not get anything(elaborate more on this)

By now I had already found the reddit account used in the OSINT challenge and was busy doing steg and connecting those numbers with rowing statistics and anything that I really saw related to rowing which wasted quite a lot of time.

when i got free from all this and decided to go back to Grafana and found the website was on maintenance and hoped that the server was getting restarted and that there might be a chance for me to use that admin admin credentials again and this time I would be the one who will change the password.

Kept checking constantly and as soon as it came back online i logged in ( thankfully admin credentials were working ) and i changed the password forcing others to try and breach through the login page.

It was not really needed at this point for me to do that cause i had already found a pdf which gave detailed step by step on how to exploit and login but well why not .
(insert ambassador.pdf )

found myself doing random tries after logging in , like looking at cookies and decrypting possibly anything which had (=) in the end 
(i had lost my mind at this point)

At one point i was so close that i had visited (http://13.126.50.182:3000/public/plugins/alertlist/..%2f..%2f..%2f..%2f..%2f..%2f..%2f..%2fetc/passwd/)

but got annoyed and kept trying to still find exploits in plugins or literally anywhere and the breakthrough came with the hint which indicated the temporary directory.

used ffuf to find the number of levels down we have to go to get to the root and when simple path traversal didn't work , did it with .%2f url encoding at finally got the flag after so many days.
(insert flag photo)