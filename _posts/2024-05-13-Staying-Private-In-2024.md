---
layout: post
title: "Stayng Private Online in 2024"
categories: guide
author:
- th4ntis
---
First off, Privacy is what YOU want it to be. There's a lot of ways to go about, some are more simple while others are more indepth and require more effort. If you want to use social media that targets you with ads and tracks and listens to you, but you are aware of that, then by all means! But you have control over how much of your data they get and the ways they track you. Privacy can be a rabbithole but take it as far as you want, not what others may tell you is/isn't enough.

While some of this might be at the slight cost of convenience, it ends up being better for you overall in most cases. Keep in mind that **Privacy ≠ anonymity**.

Anonymity is about keeping yourself unidentified or unidentifiable. The important idea about anonymity is that a person be non-identifiable, unreachable, or untrackable.

While privacy on the other hand is about it not being important if they know who you are but hiding what you are doing. In my case, it’s about hiding it from big tech. My data is for sale as much as any one else, I’m just making sure as little of it as possible is out there for sale. If the profuct is free, there’s high chances that you are what they are selling to others.

There’s a good article breakdown between the two [here](https://proton.me/blog/anonymity-vs-privacy) by [Douglas Crawford](https://proton.me/blog/author/dcrawford) at Proton.

Most of these tools are free or have very small price points. The hardware will come down to your personal phone and/or computer.

# Starting with creating accounts
I use the [Proton Suite](https://proton.me/)(Mail, VPN, Calendar, Drive) but there's other alternatives such as [Tutanota mail](https://tutanota.com/) and [MullvadVPN](https://mullvad.net/). I personally like and prefer proton as they go for privacy as the basis of all of their products. They are a bit slower to release/update things for Linux and iOS, but they do get released for them. Proton is free with options to upgrade into paid plans for more storage, custom domains, additional emails, etc.

Next would be login to [SimpleLogin](https://simplelogin.io/) with your Proton account or you can make you own account without using Proton. You'll be using this to create emails for individual accounts/services you'll be using and making. An alternative to SimpleLogin is [Anonaddy](https://anonaddy.com/). This is insanely helpful when signing up for services or websites as the email alias created will forward all them to your real email. So in the event of a data breach, they don't get your real email address. You can also disable the alias temporarily and turn it back on whenever you want as to not get spam mail.

Now when making online payments use [Privacy.com](https://privacy.com/). This lets you create virtual payment cards for one-time purchases or subscriptions. **NOTE:** Some services will not let you or will fight you on using these virtual cards.

This doesn't need to be in order but essentially you create a new email account, use a VPN(or use a VPN then create the new email account), create new email aliases, change your important accounts over to your new email alias. Put all usernames and passwords in a password manager. When buying things online use Privacy.com for virtual payment cards.

# Software
First we can start with browsers, [Firefox](https://firefox.com/), [LibreWolf](https://librewolf.net/), or [Brave](https://brave.com/). I myself am a fan of [Hardened Firefox](https://www.privacyguides.org/en/desktop-browsers/#firefox), due to the ability to have [Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/). "Each container is isolated from the others, so you can access sites logged into different accounts.". So this will keep sites isolated from each other as to not share cookies and other info with each other. Additionally using [uBlock Origin](https://github.com/gorhill/uBlock#ublock-origin) with any browser you do use.

A must have, password managers. There's a few out there, there isn't an exact wrong answer but some have poor reputation from being breached. I myself use [Bitwarden](https://bitwarden.com/). This is free, and if you want to pay to self-host(recommended) and other features, it's only $10 a year. Super cheap and worth it. There alternatives are [ProtonPass](https://proton.me/pass), or [1Password](https://1password.com/).

Onto keeping in contact with others. For Texting/Calling there is [Signal](https://signal.org/en/), [Element](https://element.io/), or [Session](https://getsession.org/) for chat messaging, though you will need other people on the platform as well or your message will not encrypted. I have all 3, I use singal the most, especially now that they have added in [Signal Usernames](https://www.signal.org/blog/phone-number-privacy-usernames/), so you don't need to give people your phone # anymore.

[MySudo](https://mysudo.com/) for creating phone numbers, emails, etc. to use instead of your real personal information. **Note:** You _may_ need a stock android/iPhone device to sign up for a plan. I was unable to while using CalyxOS. So I had an older stock android phone around, downloaded the app, signed up for the service and logged into the account on my non-stock android phone.

Using [Cryptomator](https://cryptomator.org/) when using any cloud storage that you don't sync. This will encrypt files that are put onto any oneline cloud storage.

# Hardware
I personally recommend a Google Pixel Phone - Unlocked, and installing [CalyxOS](https://calyxos.org/) or [GrapheneOS](https://grapheneos.org/). iPhones are good alternative. Using [MintMobile](https://www.mintmobile.com/) instead of a normal carrier. So you'll be getting a new phone number. When giving out your new phone number to family/friends, make sure you trust them to not give it out to others.

Most machines work but will depend on your use case, but rather than using Windows or Mac OS, go with [QuebesOS](https://www.qubes-os.org/)(for more advanced) or base [Debian](https://www.debian.org/). I typically use Debian for ease as most things just work with it.

If you use Windows10/11, you'll want to take some extra steps like running [O&O ShutUp10++](https://www.oo-software.com/en/shutup10) and [O&O AppBuster](https://www.oo-software.com/en/ooappbuster). These will remoev unnecessary bloat apps and software, as well disable/enable certain features that are built into Windows that collects your data. You can use [Chris Titus's Windows Utility](https://github.com/ChrisTitusTech/winutil) for additional options.

# Conclusion
There's multiple options out there when it comes to your online privacy. Take it as far as you want. There's social media, and services that you are the product they are selling, and plenty of others that you still are the product even if you're paying. I myself use Social Media in their own container on Firefox, but I don't have the apps installed on my phone, I just access them via my phones Browser(Firefox) if I want/need. I use apps like Discord that collects your data but it's hard not to use it when a vast majority of people use it but the ones who are privacy concious/respecting will use the other apps with me.
