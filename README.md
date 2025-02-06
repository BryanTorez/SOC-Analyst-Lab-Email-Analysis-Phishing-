Emails are one of the most popular methods used by an attacker to achieve initial access. Specifically, using the technique of phishing. As a SOC analyst, you must understand how to analyze emails. In today's lesson, I'll walk you through a lab from Blue Team Cyber Range called, "The Planet's Prestige" and I would encourage you to follow along to get the most out of this lesson. Let's get started.

All right, heading over to Blue Team labs Online and if you don't have an account yet you can register for one here, but if you do you can go ahead and just hit log in and log into your account. Once you're logged in, you want to go and select "Challenges" which is located at the top.

Next, you want to search "planet" and hit "Enter". You should see "The Planet's Prestige". So we'll go ahead and hit "Start Challenge". Very quickly we'll go over the scenario here. "CoCanDa, a planet known as 'The Heaven of the Universe' has been having a bad year. A series of riots have taken place across the planet due to the frequent abduction of citizens, known as CoCanDians, by a mysterious force. CoCanDa's Planetary President arranged a war-room with the best brains and military leaders to work on a solution. After the meeting concluded the President was informed his daughter had disappeared. CoCanDa agents spread across multiple planets were working day and night to locate her. Two days later and there's no update on the situation, no demand for ransom, not even a single clue regarding the whereabouts of the missing people. On the third day a CoCanDa representative, an Army Major on Earth, received an email." 


This is a pretty interesting scenario where we will go into the email that the Army Major had received. So let's go ahead and start downloading the email itself. I'll click on "Download File" and you do want to keep in mind of the password. The password is "btlo". For my machine, I'll be using a Windows Virtual Machine to perform my analysis using notepad++, but you can use whatever text editor you like.

I'll also use an application called 'HXD'. Which is what I'll use to view some files in HEX, and lastly, I'll have 7-zip installed as well. Once the file is downloaded just right-click it and click on "Extract All..." We'll go ahead and extract it using the password "btlo" and now we have the email file. I'll go ahead and right-click this email file and click on "Edit with Notepad++".

If this is your first time analyzing emails, it can be quite confusing. Especially, with the amount of information you see here, but let's just take it slow. I'll start from the top. The first field is "Delivered-To". This field is who received the email, which in our case, it is "themajoronearth@gmail.com". 

Next, we have "Received", these are email servers that have received the email. Similar to how post office delivers their mail. The mail will end up in multiple post offices, before reaching the post office that is closest to the recipient. So the first "Received: by" from the top is the closest mail server to the recipient. Whereas if we were to scroll down just a little bit. The first received field is going to be the one closest to the sender. 

So just so we're all on the same page at the bottom, the very first received field is going to be the mail server that's closest to the sender. Whereas all the way at the top, the first received field from the top is going to be closest to the recipient. Next, are headers that have an "x" appended to the front. These are called 'X-Headers' and they are optional and included by tools or mail servers. 

Then, we have what are called "ARC Headers", AKA 'Authenticated Received Chain Headers'. These are used for verification purposes by having a trusted intermediate email server digitally sign the header. Scrolling down just a bit, passing all of the ARC Headers, we get a "Return-Path". This is the email address that will be used if the email fails to send, which is typically used for troubleshooting purposes.

Now, I'm sure you've experienced this once upon a time where you tried to send an email, but then you got an email failed to deliver. Since the email failed to deliver, your email address was likely put into the return path. Moving down just a bit, we have "Authentication-Results". This is where we can see the statuses for email protection such as 'SPF', 'DKIM' and 'DMARK'.

As we can see, SPF is currently set to fail. Meaning that the domain of "microapple", does not permit the mail server of '93.99.104.210' as a permitted sender. In other words, MicroApple.com does not know who '93.99.104.210' is. 

When you're performing email analysis, looking at the 'SPF', 'DKIM', and 'DMARK' statuses is a good indicator of suspicious emails. If SPF is set to fail, you might want to take a deeper look into it. We have "To", which is the recipient, the subject, and the from. So who sent the email.

Now if we were to just scroll down a little bit, we have the "Importance" set to "Normal". These are quite self-explanatory. Then, we have "Reply-To". So the "Reply-To" email address that is listed here is what will be used when the recipient clicks on "Reply" to the email.

If you also notice, the email has a domain of "pashter.com" and then if we look at the "From" email, it is from "microaapple.com". Since there's a discrepancy between the two, that I pretty suspicious. Not only did SPF fail, but the email addresses in the "From" and "Reply-To" field are different.


For "Content-Type", this field is to instruct the mail server on how to render the content of the email, with it being multi-art/mixed. That means there are multiple formats and it also has a boundary set for it. Where the boundary is "BOUND_600". A boundary is used to let the mail server know when to start and stop rendering the contents using a certain format.

Next, we have "Message-Id", which is a unique identifier to help keep track of this email. Finally, we have the "Date", when the the recipient received the email. Now, do keep in mind that the date field can be spoofed.

We see our first boundary. Which will tell the mail server to start rendering the content using the following format. If we look at the "Content-Type", it says "text/plain". So it's telling the mail server to render this content using a plain text format. 

At the bottom, we see an encoding as well. Since you want to encode it using "Base64", so the mail server will render this using plain text alongside encoding it using "base64". Anytime we get hit with a base64, we want to start decoding it to see the contents of it. So we can highlight it. Right-click. Copy. Then we'll use a trusty tool called 'Cyberchef'.

Over on CyberChef, I'll go ahead and just paste that over into the input. Then, you can either search for Base64 or you click on 'From Base64' on the side. I'll drag that over and immediately at the bottom from the outputs we can see the output. It says, "Hi TheMajorOnEarth, The abducted CoCanDians are with me including the president's daughter. Don't worry. They are safe in a secret location. Send me 1 billion CoCanD's in cash, with a spaceship and my autonomous bots will safely bring back your citizens. I heard that CoCanDians have the best brains in the Universe. Solve the puzzle I sent as an attachment for the next steps. I'm approximately 12.8 light minutes away from the sun and my advice for the puzzle is, "Don't Trust Your Eyes". 

We can go ahead and click on "Save output to file". I'll name this as "email.txt". In the body of this email, they mentioned something about an attachment. So let's head back over to to our notepad++, and then we see the boundary again. Which will tell the mail server that this is the end for that particular piece of content.

If we scroll down, we see "application/pdf" with the name of "PuzzleToCoCanDa.pdf". So this is going to be the attachment that was mentioned in the body of the email. We do see the "Content-Transfering-Encoding" as "base64" and we see the base 64 content. 

Just like previously, I'll copy this. Head over to CyberChef, I'll add a new tab by clicking on the plus button. Let's paste that in. Now once we have the decoded base64, we can save out the output if we want, but before we do that. I'm going to select "To Hex" and convert this into hexadecimal. Now the reason I'm doing this is that if we take a look at the first couple bytes, '50', '4B' '03', and '04'. The first byte of a file makes up what is called a 'file signature', AKA a magic number or file header. 

Just because the file name right here is called "PuzzleToCoCanDa.pdf". It doesn't necessarily mean it is a PDF file. We want to always take a look at the file signature for a file to determine exactly what type of file it is. Going back over to CyberChef, I'll copy the first couple of bytes. Then, I'll go to use a site called "Gary Kesler file signature".

I'll press a cntrl+F and paste in those first couple bytes. Now we quickly see that the first couple of bytes relates to a zip extension. Now it did say "PDF" over to the file name, however, if we took a look at the file signature, it has a file extension of zip. What if searched for "PDF" instead? So I'll do that. 

The first couple of bytes for the PDF are '25', '50', '44', and '46'. If we were to go back over to our CyberChef, those are not the first couple of bytes. We can go and remove "To Hex" to have it all decoded. I'll save this file and call it "ATTACHMENT.ZIP". 

Remember, whenever you're doing any kind of analysis, especially, one that might contain malware always use a virtual machine and not your host. I'll go ahead and open this up. Right-click. Extract All. Now we have the "PuzzleToCanDa" directory. 

Let's open that up. We see two files here, but just in case there are any hidden files. I'm going to click on "View" and check off "Hidden items". Now we see an additional file called "Money.xlsx". Which is a file extension for Microsoft Excel. Is that actually Excel? That is something for us to figure out. 

Since I have HxD. I can view these files in their hex format. So I'll go ahead and drag "DaughtersCrown" over to HxD. Taking a look at the first couple bytes, the first ones are "FF D8 FF E0". Let's copy that. Head over to Gary Kesler. Press cntrl+F and paste.

We can see that the first couple of bytes relate to a JPEG file. I'll go back to the file and click on "Rename". Let's add ".jpeg" at the end of it. Once that's added double-click it and we get a picture.

Let's do the same for the other one. Just as an FYI, you want to always enable "File name extensions". If you don't, you won't be able to manually change the file extension. Open the HxD. Click on "New" and let's drag over the "GoodJobMajor" file. Taking a look at the first couple of bytes again, we see "25 50 44 46". So it is a PDF file.

I'll change the "GoodJobMajor" to "GoodJobMajor.pdf". Double-click. Now we see a picture. I'll open up another HxD and I'll drag in "Money.xlsx". The first couple of bytes are "50 4B 03 04". Copy and head over to file signatures. Paste that in. Now we see a zip file, which is what we saw earlier.

However, if we go down a little bit. We'll see a Microsoft Office XML format. We can safely say that this is an Excel document. Now what if we don't have Excel installed on a virtual machine? We can go ahead and install Excel or Open Office, but what I'll do is use a tool called "SquareX".

They have a file viewer that you can use to drag and drop your files that they'll open for you. So I'll go ahead and just drag the "Money.xlsx" file into SquareX. It says "Whatever you have seen or read till now is fake. Our intention was not for money it is the beginning of the WAR WITH CoCanDians. It's not that easy to find this major! I will also stay in the same location but I bet CoCanDian's can't do anything in my planet. Find and come ASAP I'm Waiting!"

This Excel file has two sheets. Since this lab is a CTF lab, there might be some trickery in here. Where the text is blended in using the colored white. So what I'll do is select everything on sheet three. Right-click. Click on "Clear. Select "Format". Now we see a base64. Copy that out. Head over to our CyberChef. Open up another tab. Let's paste that in. So it says, "The Martian Colony". I'll copy that out and put that into our notes. 

Let's do a quick recap before we head over to the questions since we just did a lot. Scroll back up our email. We see that the malicious actor sent an email from "billjobs@microapple.com" to "themajoronearth@gmail.com". With the subject of "A Hope to CoCanDa" on January 26, 2021 at 1:41:18 (Eastern Standard Time). 

Scroll up to see that the "Return-Path" is set to "billjobs@microapple@gmail.com". So this email is going to recieve any emails that were failed to deliver. However, if we take a look at the "Reply-To", it is completely different email.

If we scroll up, we can see that the malicious actor used an email service called "emkei.cz". If we search that up we'll see that it's a tool that allows you to create and send fake emails with encryption. 

The next thing I'm about to say is extremely important so take a note of it. When performing email analysis, there are some important fields that you should put more focus on. The first is "Received". So these are the mail servers that received the email. What you want to do is perform OSINT on these mail servers. So for example, taking a look at the sender's IP, you want to check out the IP reputation. You also want to check out the domain reputation. And what we just did for the email service.

This will provide you with some context when you're doing your analysis. The next thing is "Return-Path", look at the email address itself. Since if you recall, the "Return-Path" is the email address that will receive a failed or error in email delivery. The "Authentication-Results" is where you can find the status of "spf", "dkm", and "dmark".

Remember, any time you see an "SPF failed", that is pretty weird and you should put more effort into investigating that email. For "To", this is the recipient who will receive that email. "Subject" is the subject of the email.

If you have an email security gateway, or if you have the ability to search emails across your organization, you can use the subject field to see who else received a similar email. "From" is who sent that email. In this case, we have the name "Bill" and the email of "billjobs@microapple.com".

You always want to focus on the email address itself, rather than the name. Now this can be spoofed of course, but it's always a good idea to keep note of it. "Reply-To" is the email address that will be used if the recipient clicks on "Reply". It is a quick win if you notice that the "From" email address and the "Reply-To" email address is different. If you think about this logically, if I send you an email and you reply, I want to get that reply. But why is that reply going to somebody else's email, right?

For "Content-Type", this is how the mail server is going to render the content. If you do see "boundary", that means you'll see multiple formats. "Message-Id" is incredibly useful if you're searching emails across the organization. Since this keeps track of where that email went. Finally, "Date". Do keep in mind that the date can be spoofed, and take note of it nonetheless as it is a good starting point.

Again, anytime you receive an email. You want to focus on these fields as they can provide you with quick wins. Let's move on to the questions.


For question one, "What is the email service used by the malicious actor?" We saw a fake email service that was used which is "emkei.cz". I'll copy that and I'll paste it here. 

For question two, "What is the Reply-To email address? That's pretty easy. I'll copy and paste. Now remember that "Reply-To" is used whenever the recipient clicks on "Reply".

For question three, "What is the filetype of the received attachment which helped to continue the investigation?" If we look at our "Content-Type" its a PDF. However, when we did our analysis, it wasn't actually a PDF. It was a zip file.

For question four, "What is the name of the malicious actor?" Remember the name "Bill Jobs". You would think it would be it, but it's probably not. If we look at "Reply-To", it has an email address that help give us a hint. What we can do is use a tool called Exiftool and take a look at the metadata of the attachment itself. Since if the threat actor was responsible for creating the email, they might have left some clues in there to take a look at. 

So I'll use "exiftool.exe -f" to specify my files. I'll copy and paste the address path of "ATTACHMENT". The tool will help show us all the interesting metadata for these files here. As we read "Author", it says the name Pestero Negeja.


For question five, "What is the location of the attacker in this universe?" Recalling back the output of "The Martian Colony, Beside Interplanetary Spaceport".

For question six, "What could be the probable C&C, command and control, domain to control the attacker's autonomous bots? Taking a look at "Reply-To", we can assume that its the email domain that it's asking for.

Email analysis is something that you should frequently expect to see as a SOC analyst. Learning what the important fields are will definitely help you in your investigations. I hope you found that informative if you did please share this with other aspiring SOC analysts. Remember to stay curious and do things differently.
