emails one of the most popular methods used by an attacker to achieve initial access specifically using the technique fishing as a stock analyst it is crucial for you to understand how to analyze emails in today's video I'll walk you through a CTF like lab from Blue Team cyber range called the planet's Prestige and I would encourage you to follow along to get the most out of this video let's get started all right heading over Walkthrough to Blue Team labs. online and if you don't have an account yet you can register for one here but if you do you can go ahead and just hit log in and log into your account once you're logged in you want to go and select challenges which is located at the top next you want to search planet and hit enter you should see the planet's Prestige so we'll go ahead and hit start challenge very quickly we'll go over the scenario here so Canda a planet known as the heaven of the universe has been having a bad year A Series of riots have taken place across the planet due to the frequent Abduction of citizens known as candians by a mysterious Force kanda's planetary president arranged a war room with the best brains and military leaders to work on a solution after the meeting concluded the president was informed his daughter had disappeared Canda agents spread across multiple planets were working day and night to locate at her 2 days later and there is no update on the situation no demand for ransom not even a single clue regarding the whereabouts of the missing people on the third day a Canda representative an Army Major on Earth received an email this is a pretty interesting scenario where we will go into the email that the Army Major had received so let's go ahead and start downloading the email itself I'll click on download file and you do want to keep in mind of the password the password is BT all lowercase for my machine I'll be using a Windows Virtual Machine to perform my analysis using notepad++ but you can use whatever text editor you like and I'll also have an application called hxd which is what I'll use to view some files in HEX and lastly I'll have seven zip installed as well once the file is downloaded just right click it and click on extract all there we'll go ahead and extract it using the password BT all lowercase and now we have the email file right here I'll go ahead and right click this email file and click on edit with notepad++ if this is your first time analyzing emails it can be quite confusing especially with the amount of information you see here but let's just take it slow and I'll start from the top the very first field is delivered to now this field is who received the email which in our case it is the major onar gmail.com next we have received or received buy these are email servers that have received the email similar to how post office delivers their mail the mail will end up in multiple post offices before reaching the post office that is closest to the recipient and the first received buy from the top is the closest mail server to the recipient whereas if we were to scroll down just a little bit the very first received field is going to be the one that is closest to the sender and just so we're all on the same page at the bottom the very first received field is going to be the mail server that's closest to the sender whereas all the way at the top the first reced field from the top is going to be closest to the recipient next are headers that have an x appended to the front these are called X headers and they are optional and included by tools or mail servers and then we have what are called Arc headers AKA authenticated received chain headers these are used for verification purposes by having a trusted intermediate email server digitally signed the header scrolling down just a bit passing all of the arc headers we get return path this is the email address that will be used if the email fails to send which is typically used for troubleshooting purposes now I'm sure you you've experienced this once upon a time where you tried to send an email but then you got an email failed to deliver and because you received the email failed to deliver your email address was likely put into the return path moving down just a bit we have authentication results this is where we can see the statuses for email protection such as SPF dkim and dmar as we can see SPF is currently set to fail meaning that the domain of my microApple which is this one right here micro apple.com does not permit the mail server of 9399 do1400 as a permitted sender in other words microapp ale.com does not know who the heck 9399 104210 is and when you're performing email analysis looking at the SPF dkim and dmark statuses is a good indicator for suspicious emails if SPF is set to fail you might want to take a deeper look into it and then we have the two which is the recipient the subject and the from so who sent the email now if we were to just scroll down a little bit here we have the importance that is set to normal these are quite self-explanatory but then we have reply to so the reply to email address that is listed here is what will be used the moment moment the recipient clicks on reply to the email and if you notice the email has a domain of pastor.com and then if we look at the from email it is from microa apple.com because there's a discrepancy between the two that I pretty suspicious not only did SPF fail but the email addresses in the from and reply to field are different and then we have content type this field is to instruct the mail server on how to render the content of the email with it being multi-art SL miixed that means there are multiple formats and it also has a boundary set for it where the boundary is bound uncore 600 a boundary is used to let the mail server know when to start and stop rendering the contents using a certain format next we have message ID which is a unique identifier to help keep track of this email and finally we have the date when the the recipient received the email now do keep in mind that the date field can be spoofed we then see our first boundary which will tell the mail server to start rendering the content using the following format if we look at the content type it says text slpl so it's telling the mail server to hey render this content using plain text format but at the bottom we see an encoding as well you want to encode it using B 64 so then the mail server is like sure why not let me render this using plain text and I'll encode it using B 64 and anytime we gethit with a B 64 we want to start decoding it to see the contents of it so we can highlight it right click go ahead and copy that and then we'll use a trusty tool called cyberchef over on cyberchef I'll go ahead and just paste that over into the inputs and then you can either search for base 64 or if you look at the bottom you can see from base 64 so I'll go ahead and just drag that over and immediately at the bottom from the outputs let me zoom in just a little bit we can see the outputs here and it says hi the major on Earth the abducted
8:15
candians are with me including the
8:18
president's daughter don't worry they
8:20
are safe in a secret location send me 1
8:23
billion candies in cash with a spaceship
8:26
and my autonomous Bots will safely bring
8:29
back your citizens I heard that candians
8:32
have the best brains in the universe
8:34
solve the puzzle I sent as an attachment
8:36
for the next step I'm approximately 12.8
8:39
light minutes away from the Sun and my
8:41
advice for the puzzle is don't trust
8:44
your eyes then we can go ahead and click
8:46
on Save output to file and I'll just
8:48
call this as email. text click on okay
8:52
and we got that saved out now in the
8:54
body of this email they mentioned
8:56
something about an attachment so let's
8:58
head back over to to our notepad++ and
9:01
then we see the boundary again which
9:02
will tell the mail server that this is
9:04
the end for that particular piece of
9:07
content now if we scroll down we do see
9:10
a content type of application SL PDF
9:13
with the name of puzzle to ca. PDF so
9:17
this is going to be the attachment that
9:19
was mentioned in the body of the email
9:22
we do see the content transfering coding
9:24
as base 64 and we do see the base 64
9:27
content here so just like previously
9:29
I'll go ahead and copy this out head
9:31
over to cyberchef I'll add a new tab by
9:34
clicking on the plus button then let's
9:36
go ahead and paste that in now once we
9:38
have the decoded base 64 we can go ahead
9:41
and save out the output if we want but
9:43
before we do that I'm actually going to
9:46
select two hex and convert this into
9:49
hexad decimal now the reason I am doing
9:51
this is because if we take a look at the
9:53
first couple bytes which is 50 4B
9:58
030 04 the first bite of a file makes up
10:03
what is called a file signature AKA a
10:06
magic number or file header and just
10:09
because the file name right here is
10:12
called puzzle to ca. PDF doesn't
10:15
necessarily mean it is a PDF file we
10:18
want to always take a look at the file
10:20
signature for a file to determine
10:22
exactly what that file is now going back
10:24
over to cyberchef and copying the first
10:27
couple bytes I am going to use a site
10:29
called Gary Kesler file signature and
10:33
I'll click on this one right here where
10:35
is Gary kessler. net and then just do a
10:38
crlf and paste in those first couple
10:41
bytes now we quickly see that the first
10:43
couple bytes relate to a zip extension
10:46
interesting now it did say PDF over to
10:49
the file name however if we took a look
10:52
at the file signature it has a file
10:55
extension of zip what if we did PDF
10:58
instead so if I were to just search up
11:00
pdf looking at the file extension I am
11:02
looking for PDF and not drmz let's just
11:05
keep on clicking next and finally we
11:07
have the PDF extension now the first
11:10
couple bytes for the PDF is 25 50 44 46
11:16
and if we were to go back over to our
11:18
cyberchef that is not the first couple
11:20
bites pretty interesting right we can go
11:23
and remove the two hex to have it all
11:26
decoded and I will save this file and
11:29
call it attachment. zip click on okay
11:33
and now we have our zipped file now
11:35
remember whenever you're doing any kind
11:37
of analysis especially one that might
11:40
contain mware always use a virtual
11:42
machine and not your host I'll go ahead
11:44
and open this up right click it and
11:47
extract all click on extract and we get
11:50
a puzzle to Canda directory so let's go
11:53
ahead and open that up and we see two
11:55
files here but just in case there are
11:57
any hidden files I'm going to click
11:59
click on view and check off hidden items
12:02
and now we see an additional file called
12:04
money. xlsx which is a file extension
12:07
for Microsoft Excel now is that actually
12:11
Excel that is something for us to figure
12:13
out now because I do have a tool called
12:16
hxd again link down below if you want to
12:19
download it I can view these files in
12:21
their hex format so I'll go ahead and
12:23
drag daughter's Crown over to hxd and
12:27
let's take a look at the first couple
12:28
bytes so the first one here is FF
12:32
d8 FF e0 let's go ahead and copy that
12:36
head over to file signatures from Gary
12:39
kler crlf and paste that in we can see
12:42
that the first couple bytes relate to a
12:44
JPEG file I am going to go back to the
12:48
file and I'll click on rename and let's
12:51
just add a JPEG extension to it and once
12:55
that's added double click it and we get
12:57
a picture nice let's do the same for the
13:01
other one now just as an FYI you want to
13:04
always enable file name extension
13:06
because if you don't you won't be able
13:08
to manually change the file extension
13:11
here now sometimes you can it's kind of
13:14
odd but just to make sure everything
13:16
works properly always have file name
13:18
extension checked open the hxt and then
13:21
I'll click on new and let's drag over
13:24
the good job major take a look at the
13:26
first couple bites again we have 25 50
13:30
44 and 46 now if that sounded familiar
13:33
to you that is because we checked this
13:36
earlier which turned out to be a PDF so
13:39
I'll go ahead and change good job major
13:41
and I'll add the extension. PDF now we
13:44
can double click it we see that it says
13:47
Hey candians are safe the proof is in
13:49
the file named daughter's Crown location
13:52
to send 1 billion candies is in the
13:55
money. xlsx file okay head over to the
13:59
directory I'll open up another
14:03
hxd and I'll drag in the money. xlsx the
14:07
first couple bytes is 504b
14:11
0304 copy that out head over to file
14:13
signatures and I'll paste that in now we
14:16
do see azip file which is what we saw
14:20
earlier however if we just kept going
14:23
down just a little bit we do see
14:25
Microsoft open Office XML format we can
14:29
safely say that this is an Excel
14:31
document now what if we don't have Excel
14:33
installed on a virtual machine which I
14:36
do not have now we can go ahead and
14:38
install Excel or open office or what
14:41
I'll do is use a tool called Square X
14:44
they have a file viewer that you can use
14:46
to drag and drop your files and then it
14:48
will open it for you now you can go
14:50
ahead and sign up with the link down
14:52
below it is a tool that I always use
14:55
during my analysis now that I opened up
14:57
the file viewer I'll go ahead and just
15:00
drag the money. xlsx file and into
15:04
square x with this file it says whatever
15:06
you have seen or read till now is fake
15:09
Our intention was not for money it is
15:11
the beginning of the war with Candian
15:14
it's not that easy to find this major I
15:16
will also stay in the same location but
15:19
I bet candians can't do anything in my
15:22
Planet find and come ASAP I'm waiting
15:26
okay with this Excel file we do have two
15:30
sheets so this one is sheet one this one
15:32
is sheet three and Sheet three appears
15:35
to have nothing in it and because this
15:37
lab is like a CTF lab there might be
15:41
some trickery in here where the text is
15:44
Blended in using the colored white so
15:46
what I'll do is I'll select everything
15:49
right click and then I'll click on clear
15:51
format that way if there's any kind of
15:53
formatting happening I will see it right
15:56
here so sheet one seems like it's all
15:58
good but what what about sheet three
16:00
because it's blank and there's a sheet
16:02
it's kind of suspicious here so clear
16:05
format and we do see a base 64 so go
16:09
ahead and copy that head over to our
16:11
cyberchef open up another tab by
16:14
clicking on the plus and let's go ahead
16:16
and paste that in so it says the Martian
16:19
Colony nice okay I'll go ahead and copy
16:22
that out and put that into our notes now
16:24
let's do a quick recap before we head
16:26
over to the questions because we just
Recap
16:28
went over a a lot of things so we take a
16:30
look at our email and scrolling back up
16:32
the malicious actor sent an email from
16:35
Bill jobs at micro.com to the major onar
16:41
gmail.com with the subject of a hope to
16:45
Canda on January 26 2021 at
16:51
14118 Eastern Standard Time the return
16:54
path if we scroll up is set to Bill JW
16:59
at micro apple.com so this email is
17:02
going to receive any emails that were
17:04
failed to deliver however if we take a
17:06
look at the reply to it is a completely
17:09
different email one that is ending in
17:12
pastor.com if we scroll up just a little
17:15
bit we can see that the malicious actor
17:17
used an email service called mk. CZ and
17:21
if we were to just search that up to see
17:23
what that is and we can see that it is a
17:26
fake mailer mk's fake mailer is a
17:29
web-based tool that allows you to create
17:31
and send fake emails so we know that the
17:34
thread actor used an email service
17:36
called MK and the contents of the email
17:39
was a threat to the major on Earth
17:42
demanding 1 billion in cash along with
17:45
an attachment that was supposedly a PDF
17:48
file but it actually turned out to be a
17:50
zip file and within that zip it
17:52
contained three files one jpeg one PDF
17:56
and one Excel file the next thing I'm
17:58
I'm about to say is extremely important
18:00
so make sure that you take a note of it
18:02
when performing email analysis there are
18:05
some important fields that you should
18:07
put more focus on and those are the
18:09
following first and foremost received so
18:12
these are the mail servers that received
18:14
the email what you want to do is perform
18:16
ENT on these mail servers so for example
18:20
taking a look at the sender IP you want
18:23
to check out the IP reputation you also
18:26
want to check out the domain reputation
18:28
as well and of course what we just did
18:30
the email service this will provide you
18:32
with some context when you're doing your
18:34
analysis the next thing is return path
18:37
look at the email address itself because
18:39
if you recall the return path is the
18:41
email address that will receive a failed
18:43
or error in email delivery and then we
18:46
have authentication results this is
18:48
where you can find the status of SPF dkm
18:51
and dmark remember any time you see SPF
18:54
failed that is pretty weird and you
18:55
should put more effort into
18:57
investigating that email and then we
18:59
have the two so this is the recipient
19:02
who received that email then the subject
19:04
itself which is the subject of the email
19:07
if you have an email security Gateway or
19:09
if you have the ability to search emails
19:11
across your organization you can use the
19:14
subject field to see who else received a
19:16
similar email and then we have the from
19:19
so who sent that email in this case we
19:21
have the name Bill and the email of Bill
19:25
jobs micro.com you always want to focus
19:28
on the email address itself rather than
19:31
the name now this can be spoofed of
19:33
course but it's always a good idea to
19:35
keep note of it because again you can
19:37
always search your emails across your
19:39
organization searching specifically
19:41
emails that are sent from this user and
19:44
then we have reply to so this is the
19:47
email address that will be used if the
19:49
recipient clicks on reply it is a quick
19:52
win if you notice that the from email
19:55
address and the reply to email address
19:58
is different
19:59
because if you think about this
20:00
logically if I send you an email and you
20:02
reply I want to get that reply why is
20:05
that reply going to somebody else's
20:07
email something like that which is a
20:09
question that you should ask yourself
20:11
and then we have content type this is
20:14
how the mail server is going to render
20:15
the content if you do see boundary that
20:18
means you'll see multiple formats and
20:20
then you have message ID this is
20:23
incredibly useful again if you're
20:24
searching emails across the organization
20:27
this keeps track of where that email
20:29
went and finally you have the date do
20:31
keep in mind that the date can be
20:33
spoofed but take note of it nonetheless
20:35
as it is a good starting point again
20:37
anytime you receive an email you want to
20:40
focus on these fields as they can
20:42
provide you with quick wins now in my
20:44
course we go into more detail about
20:46
email analysis and I'll also show you
20:49
some other tools that you can use my
20:51
course is going to be a paid course but
20:53
it will be absolutely worth it trust me
20:57
I'll leave a link down for you to sign
20:59
up for the wait list if you're
21:00
interested now let's move on to the
Questions
21:03
questions so it says what is the email
21:05
service used by the malicious actor now
21:09
we know this by going back over we saw a
21:12
fake email service that was used which
21:15
is mk. CZ so I'll go ahead and copy that
21:18
and I'll paste it here click on submit
21:22
perfect what is the reply to email
21:25
address that's pretty easy I'll go ahead
21:27
and copy this one out now remember reply
21:30
to is whenever the recipient clicks on
21:32
reply that is going to be the email
21:34
address for that paste that in hit
21:37
submit what is the file type of the
21:39
received attachment that helped to
21:41
continue the investigation if we look at
21:43
our email scrolling down we do see the
21:46
content type as PDF but when we did our
21:49
analysis it wasn't actually a PDF right
21:51
it was
21:52
azip file what is the name of the
21:55
malicious actor hm this one's quite
21:58
interesting if we take a look at the
21:59
email let's see if there's a name here
22:02
so we see the from field and it is from
22:04
Bill but it's asking for a last name as
22:08
well so bill is probably not the one
22:11
unless you put in Bill jobs but if we
22:14
think about it the reply to field is a
22:16
different email address and because it
22:18
is a different email address it makes me
22:20
believe that this is actually the threat
22:23
actor or the malicious actor here what
22:25
we can do is use a tool called exit f
22:28
tool and take a look at the metadata of
22:31
the attachment itself because if the
22:33
thread actor was responsible for
22:35
creating it they might leave some clues
22:37
in there to take a look at the metadata
22:40
I'm going to be using a tool called XF
22:42
tool and again I'll leave the download
22:44
links down below so I'll use XF tool DF
22:47
to specify my files and I am going to
22:51
take a look at the attachments so I'll
22:53
go ahead and just copy the path and I'll
22:56
paste it in here back slash with an ASX
22:59
and using except tool will show us all
23:01
the interesting metadata for these files
23:03
here so we have the zip file name the
23:06
mime type let's see file modification
23:10
access and creation but nothing about a
23:12
name and oh right here author so we can
23:15
see a name
23:17
Pasto nea I I don't know if that's how
23:20
you pronounce it but there you go first
23:23
and last name I'm going to assume that
23:25
that is the one because it does match
23:28
our reply to email right the last name
23:33
Nea is right here paste that in hit
23:37
submit and we got it perfect what is the
23:41
location of the attacker in this
23:43
universe H what is the location of the
23:47
attacker in this universe okay um I do
23:52
recall seeing this the Martian Colony
23:56
this is the decoded Bas 64 that we we
23:58
found in the Excel document called money
24:01
so I'll go ahead and just copy that cuz
24:03
this sounds like a location to me paste
24:05
that in here and submit nice what could
24:09
be the probable CNC so command and
24:12
control domain to control the attacker's
24:15
autonomous Bots taking a look at the
24:18
email we do see
24:20
pure.com from this malicious actors and
24:23
there weren't any other URLs that we've
24:26
seen other than the fake email service
24:29
here but because this domain is tied to
24:31
the malicious actor I'm going to assume
24:34
that this is going to be the C2 domain
24:37
going to go ahead and pasted it and
24:39
submit and awesome we just completed it
24:42
email analysis is something that you
24:44
should frequently expect to see as a
24:46
sock analyst and by learning what the
24:49
important fields are will definitely
24:51
help you in your
24:52
investigations again if you're
24:54
interested in my course the weit list to
24:56
the link is down below I'll provide the
24:59
pricing to you shortly once I have
25:01
finalized all of the content and my goal
25:04
here is to try and release it in
25:06
sometime in May or by June but in the
25:09
meantime continue to level up your
25:12
skills and get better every single day
25:15
that is it for the video and I hope you
25:17
found that informative if you did please
25:19
share this with other aspiring sock
25:21
analysts And subscribe if you want to
25:25
remember to stay curious and do things
25:31
differently
