Orko is a web application which provides a unified UI and web service API to numerous cryptocurrency exchanges, allowing you to trade and manage your portfolio, even if it is spread across multiple exchanges, all from one screen. It supports powerful background processes allowing you to set up stop losses, take profit orders, trailing stops and more, even on exchanges that don't support these types of advanced orders, as well as price and profit/loss level alerts.

## Why yet another trading system?

Orko is designed to solve a range of problems with existing solutions:

- Cryptocurrency exchanges' own features often leave a lot to be desired. Some tend to be missing important features such as stop losses or trailing stops, others are simply buggy, hard to use or suffer extreme lag during busy trading (with one or two notable exceptions).
- Many advanced trading techniques are often only available if you have the technical capability to write "bots" or scripts to access exchanges' APIs. While these are deceptively simple to write initially, it's extremely hard to write something reliable and stable.
- Traders who use multiple exchanges have log into them all separately. This can be onerous, particularly given that they have varying, and usually aggressive, security measures which take some time to get past, and even worse if a trader uses multiple devices. It also means that it is often hard to get a "single version of the truth".
- Existing solutions to these problems, for reasons of easy monetisation, are usually commercial, "multi-tenant" applications, where a trader pays for access to a shared server. There are two problems here:
  - The trader's API keys are potentially vulnerable to hacks by other users. There have been several instances where exchanges have blamed users who use shared trading applications for losses as a result of not properly securing their keys (that is, uploading them to a third party service).
  - Such services often get blocked by exchanges, due to having large numbers of users "spamming" exchange APIs from a small number of IP addresses. These can make it impossible to trade for extended periods.

## What Orko does differently

- Orko is a **strictly single-tenant** application. That is, each instance of the application services one, and only one user. You manage that application - the developers of Orko have no access to it. That means your API keys are never shared. You can run it on your own hardware in your own home or trust it to a cloud provider account to which only you have access.
- Orko's code is open source, so it's hard for anyone to add exploit code which could put you at risk. If you're really paranoid, you can fork the code to make sure it doesn't change, then audit it line-by-line. You don't have to trust your valuable API keys to unknown code.
- You can change it and improve it (and we'd be really grateful if you shared it with the rest of the community by submitting a [pull request](../pulls)).
- It's really easy to deploy and run. If you have a [Heroku](https://heroku.com/) account, You can deploy it [with just a few clicks](https://heroku.com/deploy?template=https://github.com/badgerwithagun/orko), or download it and run it in just a few commands.
- It's being constantly improved and extended. What you have here is a very early version, with only a fraction of the features we're intending to add, but it has been used as our main trading UI for over a year as it has been developed.

## Licence

Orko uses the [Affero General Public License v3](../blob/master/LICENSE), which gives you the right to use the application freely for private or commercial purposes, but requires that if you make changes to the application and then offer the application online as a service, you must make those changes publicly available.

This is specifically to avoid a commercial online service based largely on Orko from launching without contributing improvements back to the open source project. However, this should not discourage such a service from launching and differentiating itself in other ways (hosting, DR, support or SLAs, for example).
